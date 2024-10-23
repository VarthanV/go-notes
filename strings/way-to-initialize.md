# Different ways to initialize Go structs

- Many languages like Java, C#, etc. have ``constructors`` that help you instantiate an object and its properties.

- Go does not have ``constructors`` in the pure sense of the word.

## Build in options

- Out of the box go gives two ways to initialize structs, structs literals and ``new`` function

```go
package people

type Person struct {
  age  int
  name string
}

// struct literal
person := &Person{
  age:  25,
  name: "Anton",
}

// new build-in
person := new(Person) // person is of type *Person
person.age = 25
person.name = "Anton"
```

## Constructor function

- Go does not have constructors. However, that does not mean we cannot define one ourselves. 

- We can define a constructor function where the struct fields are positional arguments of the function.

```go
package people

func NewPerson(name string, age int) *Person {
  if age < 0 {
    panic("NewPerson: age cannot be a negative number")
  }
  return &Person{name: name, age: age}
}
```

- This provides us  benefits
    - We have validation for the age field. If someone tried to create a Person with a negative age, they will get a panic with a descriptive error of what they did wrong (we could have also returned an error)
    - The Person internals are decoupled from the initialization logic. 
    - For example, we can add an ``isUnderage`` field to the struct and compute that based on age inside of the ``NewPerson`` function. 
    - The consumer of the function won’t have to be bothered with the logic behind this field, because we are taking care of it.

-  The downside is function becomes bloated , If our ``Person`` had many ``string`` field for example (multiple,names or address) this fn would have looked like this

```go
func NewPerson(firstName, lastName, addressLine1, addressLine2 string, age int) *Person
```

- And having a function with multiple parameters of the same type is a recipe for disaster. In this case, the danger is that the consumer of this function can easily misplace any of the parameters (for example, replace the first and last names) and they would have no compile-time check for that.

```go
// is it?
p := NewPerson("Anton", "Sankov", "Bulgaria", "Sofia", 25)
// or?
p := NewPerson("Anton", "Sankov", "Sofia", "Bulgaria", 25)
// or?
p := NewPerson("Anton", "Bulgaria", "Sofia", "Sofia str, number 25", 25)
```

- Another bigger issue is backward compatability lets say if we need to add fields to Person struct. We can add it without issue

```go
package people

type Person struct {
  age    int
  name   string
  salary float64
}

// change
// func NewPerson(name string, age int) *Person {
// to:
func NewPerson(name string, age int, salary float64) *Person {
  if age < 0 || salary < 0 {
    panic("NewPerson: age and salary cannot be negative numbers")
  }
  return &Person{name: name, age: age, salary: salary}
}
```


- This is a breaking change to ``NewPerson`` , all of the users would get compilation error when they upgrade to new version

## With Options struct

- We can define ``PersonOptions`` struct that mirrors the Person but with exported field and pass this to ``NewPerson``. Let’s see how this looks like.

```go
package people

type Person struct {
  age    int
  name   string
  salary float64
}

type PersonOptions struct {
  Age    int
  Name   string
  Salary float64
}

func NewPerson(opts *PersonOptions) *Person {
  if opts == nil || opts.Age < 0 || opts.Salary < 0 {
    panic("NewPerson: age and salary cannot be negative numbers")
  }
  return &Person{name: opts.Name, age: opts.Age, salary: opts.Salary}
}

```

- This allows us to introduce new fields to ``Person`` as much as we like without breaking the contract of ``NewPerson``.

- If we add a new field to ``Person``(and respectively ``PersonOptions``) we cannot easily enforce that all consumers of ``NewPerson ``will set the new properties.

- Of course, we can fail ``NewPerson`` if any of the new fields are missing, but this is actually a breaking change, not much better than the one in the first example (and even worse, because it will only be caught at runtime).

- Possible mitigation to this problem is if we define the required field in ``PersonOptions`` as ``value types`` and the optional ones as ``refs.`` For example:

```go
type PersonOptions struct {
  // Age and Name are required
  Age    int
  Name   string

  // Salary is optional
  Salary *float64
}
```
- It is also bit unclear what is required and what is not

## Vardiac function constructor

- Alternative to that is to make our constructor  a ``varidiac fn`` that accepts a arbitary no of mutating fns.

- Thus the implementation of the method will run all the fns ``one by one`` a instance the struct which will be returned by the constructor.

- This means, we also need to provide a set of functions that will mutate the fields.

```go
package people

type PersonOptionFunc func(*Person)

func WithName(name string) PersonOptionFunc {
  return func(p *Person) {
    p.name = name
  }
}

func WithAge(age int) PersonOptionFunc {
  return func(p *Person) {
    p.age = age
  }
}

func NewPerson(opts ...PersonOptionFunc) *Person {
  p := &Person{}
  for _, opt := range opts {
    opt(p)
  }
  return p
}

// usage:
p := people.NewPerson(people.WithName("Anton"), people.WithAge(25))

fmt.Println(p) // {name: "Anton", age: 25}
```

- The downside here is that the amount of ``WithXXX``functions are not obvious, and the consumers of the package would either have to search them in the package documentation or depend on their IDE autocomplete to show them the possible options.

- In my opinion, it does not give you any benefits over the ``Options``struct, but bring the downside that the options are not obvious.

## Middle ground

- A middle ground between constructor with required positional arguments and optional arguments would be to have the required fields for a given struct as positional arguments so that the consumer MUST pass them, and have everything else as optional parameters, which may be skipped.

```go
func NewPerson(name string, options *PersonOptions) *Person {
  p := &Person{name: name}
  if options == nil {
    return p
  }
  if options.Age != 0 /* OR options.Age != nil */ {
    p.age = options.Age /* OR p.age = *options.Age */
  }
  if options.Salary != 0 /* OR options.Salary != nil */  {
    p.salary = options.Salary /* OR p.salary = *options.Salary */
  }
  return p
}

// usage:
p := NewPerson("Anton", &people.PersonOptions{Age: 25})

fmt.Println(p) // {name: "Anton", age: 25}

```

## Further reading

[Ways to init go struct](https://asankov.dev/blog/2022/01/29/different-ways-to-initialize-go-structs/)


