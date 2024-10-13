 Arrays and Slices sequence of things laid down in memory one after the other


## Arrays
- Arrays are of fixed size and need to mention size

```go
var (
    a [3]int
)
```

Arrays are pass by value and elements are copied

```go
var (
    a = [3]int{0,0,0}
)

b := a // a gets copied to nb
```

## Slices

- Slices are of variable length backed by array
- Slices are passed by reference; no copying , updating OK

```go
    var []a int // nil,no storage
    var b = []int{1,2} // initialized
    a = append(a,3) // append to nil ok
    b = append(b,3)

    a = b // overwrites a
    e := a // same storage alias

    e[0] == b[0] //true
    

    var (
        d = [...]int{3,3,3} // length inferred as 3 , equal to no of elements present in the array
    )
```

- Slices are indexed like ``` [8:11]```
(Read as starting element and one past ending element 11-8 = 3 elements in our slice)


## Arrays vs Slice

| Slice | Array |
| -------- | ------- |
|  Variable length   | Length fixed at compile time
| Passed by reference | Passed by value     |
| Cannot be used as map key    | Can be used as map key|
| Has copy and append helpers | - |
| Useful as function parameters| Useful as pseudo constants
| Not comparable with == | Comparable with ==

- Slice cannot be assigned to array

- Since slices are pass by reference if at all need to modify the slice make a local copy

```go
    func foo(b []int){
        b[0] = 1 // modifies the orig passed value

        // To have a local copy
        c :=  make([]int ,len(b))
        copy(c , b) // dest ,source
    }
```

## Maps

- Maps are dictionaries indexed by keys, returning a value

- You can read from a nil map but inserting will panic

```go
    var m map[string]int // nil no storage
    p := make(map[string]int) // non-nil but empty

    a := p["the"] // returns 0
    b := m["the"] // returns 0

    m["the"]+= 1 // panic nil map

    m = p
    m["and"]++ // OK , same as p now
    c := p["and"] // returns 1
```
- Maps are passed by reference; no copying updating ok

- The type defined for the key must have **== and !=** defined 

- Maps cannot be compared to other maps

- Maps have special two result lookup function , The second variable says if key was there are not

```go
p := map[string]int{} // non-nil but empty

a := p["foo"] // returns 0
b,ok := p["foo"] // 0, false

p["foo"]++

c, ok := p["foo"] // 1, true


if w, ok := p["foo"]; ok {
    // we know w is not default value
}
```

## Builtins

| function | type | use |
| -------- | ------- |----|
|len(s)| string| string length|
| len(a),cap(a)| array | array length , capacity (constant)
| make(T,x) | slice | slice of type T with length x and capacity x
| make(T,x,y)| slice| slice of type T with length x and capacity y
| copy(d,c)| slice| copy from d c # min of two lengths
| c= append(c,d)| slice | append d to c and return a new slice result
len(s) , cap(s) | slice | slice length and capacity
|make(T) | map | make map of type T
| make(T,x)| map | make map of type T with hint for x elements
| delete(m, k)| map| delete key k if present in m
| len(m)|map| length of m

## nil

- nil is a type of zero , indicates absence of something.

Many builtins are nil safe len,cap,range

```go
    var s[]int
    var m map[string]int
    
    l := len(s) // length of slice s is zero

    i , ok := m["int"] // 0,false or missing key


    for _ , v := range s {
        // skip if is nil or empty
    }

```

```
    Make te zero value useful - Rob Pike
```

- nil is zero value of multiple types in go

| Type      | Value  |
|-----------|--------|
| bool      | false  |
| numbers   | 0      |
| string    | ""     |
| pointers  | nil    |
| slices    | nil    |
| maps      | nil    |
| channels  | nil    |
| functions | nil    |
| interfaces| nil |

```go
    type Person struct {
        Age int
        Name string
        Friend []Person
    }

    var p Person // Person{0,"", nil}
```
- The type of nil unless the predeclared identifier is nil go has no type.

## nil pointer ##
- points to nil a.k.a nothing
- zero value of pointers

## slice ##

- A slice has 3 properties pointer to empty array , len , cap

| Kinds of nil | Definition  |
|-----------|--------|
| pointers | point to nothing|
|slices| have no backing array|
|maps| are not initialized|
|channels| are not initialized|
|functions| are not initialized|
|interfaces| are not initialized|

# nil is useful

- pointers: methods can be called on nil receivers
- slices: perfectly valid zero values
- maps: perfect as ready-only values
- channels: essential for some concurrency patterns