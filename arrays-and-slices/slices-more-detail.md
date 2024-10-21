## Slice and Array in detail

- The magic that behaves that makes a slice behave both as value 
and as pointer is that slice is actually a ``struct`` type

```go
package runtime

type slice struct {
        ptr   unsafe.Pointer
        len   int
        cap   int
}
```

![slice-internals](https://dave.cheney.net/wp-content/uploads/2018/07/slice.001.png)

-  This is important because unlike map and chan types slices are value types and are copied when assigned or passed as arguments to functions.

## Arrays

- Go’s arrays are values. An array variable denotes the entire array; it is not a pointer to the first array element (as would be the case in C).

![Arrays](https://go.dev/blog/slices-intro/slice-array.png)

- An array literal can be specified like so:

```go
b := [2]string{"Penn", "Teller"}

b := [...]string{"Penn", "Teller"} // Have the compiler count arrays for you

```

## Slices

- Arrays have their place but they're inflexible, So we don't see them often in code. They build
on top of arrays to provide great power and convenience.

- The type specification for a slice is ``[]T``, where ``T`` is the type of the elements of the slice. Unlike an array type, a slice type has no specified length.

```go
var s []byte
s = make([]byte, 5, 5) // type,capacity,length
// s == []byte{0, 0, 0, 0, 0}
```

- The zero value of slice is ``nil``

- A slice can also be formed by ``slicing`` an existing slice or array

```go
b := []byte{'g', 'o', 'l', 'a', 'n', 'g'}
// b[1:4] == []byte{'o', 'l', 'a'}, sharing the same storage as b
```

- Slice holds the ptr of first value of the underlying array , thats how slicing share same storage

![Slice-struct](https://go.dev/blog/slices-intro/slice-1.png)

- Declaration of slice and pointing to internal array

```go
s = s[2:4]
```

![Slicing-slice](https://go.dev/blog/slices-intro/slice-2.png)

- Slicing does not copy the slice’s data. It creates a new slice value that points to the original array. This makes slice operations as efficient as manipulating array indices.

- copy copies data from a source slice to a destination slice. It returns the number of elements copied.

```go
func copy(dst, src []T) int
```

- The append function appends the elements x to the end of the slice s, and grows the slice if a greater capacity is needed.

```go
func append(s []T, x ...T) []T
```

-  To append one slice to another, use ... to expand the second argument to a list of arguments.

```go
a := []string{"John", "Paul"}
b := []string{"George", "Ringo", "Pete"}
a = append(a, b...) // equivalent to "append(a, b[0], b[1], b[2])"
// a == []string{"John", "Paul", "George", "Ringo", "Pete"}
```

- Slice as a little data structure with two elements: a length and a pointer to an element of an array. You can think of it as being built like this behind the scenes:

```go
type sliceHeader struct {
    Length        int
    ZerothElement *byte
}

slice := sliceHeader{
    Length:        50,
    ZerothElement: &buffer[100],
}
```

- It’s important to understand that even though a slice contains a pointer, 
it is itself a value. Under the covers, it is a struct value holding a pointer and a length. It is not a pointer to a struct.

```go
func AddOneToEachElement(slice []byte) {
    for i := range slice {
        slice[i]++
    }
}
```

```go
func main() {
    slice := buffer[10:20]
    for i := 0; i < len(slice); i++ {
        slice[i] = byte(i)
    }
    fmt.Println("before", slice)
    AddOneToEachElement(slice)
    fmt.Println("after", slice)
}
```

- Even though the slice header is passed by value, the header includes a pointer to elements of an array, so both the original slice header and the copy of the header passed to the function describe the same array. Therefore, when the function returns, the modified elements can be seen through the original slice variable.

- Here we see that the contents of a slice argument can be modified by a function, but its header cannot. The length stored in the slice variable is not modified by the call to the function, since the function is passed a copy of the slice header, not the original. Thus if we want to write a function that modifies the header, we must return it as a result parameter, just as we have done here. The slice variable is unchanged but the returned value has the new length, which is then stored in newSlice.

## Representation of nil slice

```go
sliceHeader{
    Length:        0,
    Capacity:      0,
    ZerothElement: nil,
}

sliceHeader{}

```

- An empty slice can grow (assuming it has non-zero capacity), but a nil slice has no array to put values in and can never grow to hold even one element.



