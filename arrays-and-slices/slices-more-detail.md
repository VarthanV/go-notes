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
