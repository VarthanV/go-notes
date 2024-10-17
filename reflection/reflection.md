
## Type assertion

- ``interface{}`` says nothing since it has no methods

- It is a **generic** thing but sometimes we need is real type

- We can extract a specific type with a type assertion (a/k/a "downcasting")

- This has the form value . (T) for some type T

```go
var w  io.Write = os.Stdout

f := w . (*os.File) // success: f == os.Stdout

c: =w. (*bytes.Buffer) // panic: interface holds *os.File not *bytes.Buffer

```
- If we use the two-result version we can avoid panic

```go
var w  io.Write = os.Stdout

f,ok := w . (*os.File) // success: f == os.Stdout

c,ok :=w. (*bytes.Buffer) //  failure: !ok , b== nil

```

## Deep Equality

- We can use the reflect package in UTs to check equality

```go
want := struct {
    a: "a string",
    b: []int{1,2,3}, // not comparable with == 
}

got := goGetIt(..)

if ! reflect.DeepEqual(got,want){

}
```

**Primitive types**: For simple types like integers, strings, and booleans, it behaves similarly to ==.

**Slices and arrays**: DeepEqual checks if they have the same length and if all elements are deeply equal. Two nil slices are considered equal.

**Maps**: It compares the keys and values in both maps, treating nil and empty maps as different.

**Structs**: The function checks each field, comparing them deeply.

**Pointers**: If both values point to the same memory address or both are nil, they are considered equal; otherwise, the values they point to are compared.

**Interfaces**: The types and underlying values must both be deeply equal.


## Switch on type

We can also use type assertion in a switch statement

