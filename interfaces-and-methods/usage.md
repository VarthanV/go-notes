# Interface usage

## Dont do this

- The below is so ``Java-style`` interface usage.

- The steps are
    - Define an interface
    - Define one type that fulfils the interface
    - Define methods that satisfies the implementation of the interface.


```go
package animals 

type Animal interface {
	Speaks() string
}

// implementation of Animal
type Dog struct{}
func (a Dog) Speaks() string { return "woof" }
```

```go
package circus

import "animals"

func Perform(a animal.Animal) string { return a.Speaks() }

```

- The most obvious of which is that it has only one type that fulfils the interface with no obvious means of extension.

- Functions typically take concrete types instead of interface types.

## Do this instead

- Instead of writing types to fulfil interfaces, write interfaces to fulfil usage requirements.


- Instead of defining ``Animal`` in package animals, define it at point of use - in package circus.

```go
package animals

type Dog struct{}
func (a Dog) Speaks() string { return "woof" }
```

```go
package circus

type Speaker interface {
	Speaks() string
}

func Perform(a Speaker) string { return a.Speaks() }
```

- The more idiomatic way is
    - Define the types
    - Define the interface at point of use

- This way shows a reduced dependency on the components of package animals. Reduced dependencies is how you build robust software.

## Postel's Law

> Be conservative with what you do, be liberal with you accept

- Translate in Go as

> Accept interfaces, return structs

- By and large, this is a very good maxim on designing things to be robust. 

- The main unit of code in Go is a function. The pattern to follow when designing functions/methods is the following.

```go
func funcName(a INTERFACETYPE) CONCRETETYPE 
```

-  We accept anything that ``implements an interface`` - could be any interface, or a blank one, and return a value that is a ``concrete value``.

> “the empty interface says nothing” - Rob Pike

- So it’s preferable not to have functions take ``interface{}``.

## Use Case: Mocking

- An excellent demonstration of the usefulness of the Postel’s Law maxim is in the case of testing. If you have a function that looks like this

```go
func Takes(db Database) error 
```

- If ``Database`` is an interface then in testing code, you can just provide a mock implementation of ``Database`` without having to pass in a real database object.

## When Is It Acceptable To Define An Interface Upfront

- Defining an interface upfront is usually a code smell for overengineering. But there are clearly situations where you need define an interface upfront.
    - Sealed interfaces
    - Abstract data types
    - Recursive interfaces

## Sealed Interfaces

- Sealed interface is a method with unexported methods.

- This means users outside the package is unable to create types that fullfills the interface.

```go
type Fooer interface {
	Foo() 
	sealed()
}
```

- Only the package that defined ``Fooer`` can use and create any valid value of ``Fooer``. This allows for exhaustive type switches to be done.

- A sealed interface also allows for analysis tools to easily pick up any non-exhaustive pattern match

- [sum-types](https://github.com/BurntSushi/go-sumtype) package is used for the same

```go
package main

//go-sumtype:decl MySumType

type MySumType interface {
        sealed()
}

type VariantA struct{}

func (*VariantA) sealed() {}

type VariantB struct{}

func (*VariantB) sealed() {}

func main() {
        switch MySumType(nil).(type) {
        case *VariantA:
        }
}

```

## Abstract data types

- The other use of defining an interface upfront is to create a abstract data type. It may or may not be sealed.


- The sort package that comes in the standard library is a good example of this. It defines a sortable collection as

```go
type Interface interface {
    // Len is the number of elements in the collection.
    Len() int
    // Less reports whether the element with
    // index i should sort before the element with index j.
    Less(i, j int) bool
    // Swap swaps the elements with indexes i and j.
    Swap(i, j int)
}

```
- Now this has made a lot of people upset - because if you want to use the sort package you’d have to implement the methods for the interface, and people for the most part are upset about having to type three extra lines.

## Recursive interfaces

- This is probably another code smell, but there are times which are unavoidable, you perform something within a monad and end up with an interface that looks like this:

```go
type Fooer interface {
	Foo() Fooer
}
```
- The recursive interface pattern would require the interface be defined upfront, clearly. The guideline of defining an interface at the point of use is inapplicable here.

- This pattern is useful for creating contexts to operate in. Context-heavy code are usually self-contained within a package, with only the contexts exported (alá the tensor package), so I don’t actually see a lot of this. I’ve quite a bit more to say about contextual patterns, but I’ll leave that to another blog post.


## Further reading
[Golang interfaces](https://blog.chewxy.com/2018/03/18/golang-interfaces/)

