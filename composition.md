# Composition

The fields of embedded struct are promoted to level of embedding
structure

```go
type Pair struct {
    Path string
    Hash string
}

type PairWithLength struct{
    Pair 
    Length int
}

p1 := PairWithLength{Pair{"u","fpp",121}}
fmt.Println(p1.Path, p1.Length) // not p1.x.Path
```

- Promoted means the fields of ``Pair`` appear in same level as fields
of ``PairWithLength``

- There is advantage to promotion we not only promote ``fields`` 
but also ``methods``

- Can embed a type in struct which is not struct

```go
type IntSlice []int

type Foo struct {
    IntSlice // Possible
}
```

## Method promotion

```go
type Pair struct {
    Path string 
    Hash string
}

func (p Pair) String() string {
    return "I am stringer"
}

type PairWithLength struct {
    Pair
    Length int
}

func main(){
    p := Pair{"foo", "bar"}
    pl := PairWithLength{Pair{"foo","bar"},133}
    fmt.Println(p)
    fmt.Println(p1) // String method of p is taken into account
}
/*
Output: I am stringer
*/

```

```go
type Pair struct {
    Path string 
    Hash string
}

func (p Pair) String() string {
    return "I am pair"
}

func (p PairWithLength) String() string {
    return "I am pair with length"
}
type PairWithLength struct {
    Pair
    Length int
}

func main(){
    p := Pair{"foo", "bar"}
    pl := PairWithLength{Pair{"foo","bar"},133}
    fmt.Println(p)
    fmt.Println(p1) // String method of pairwithlength is taken into account
}
/*
Output: I am pair with length
*/

```

- ```PairWithLength``` != ```Pair``` , PairWithLength is not subtype of pair 
it is just composed with another type and not inherited

## Composition with pointer types

A struct can embed a pointer to another type; promotion of its fields and methods
work the same way

```go
type Fizgig struct {
    *PairWithLength 
    Broken bool
}

fg := Fizgig{
    &PairWithLength{Pair{"foo","bar"},133},
    false,
}
```

# Sortable interface

- Real world use case of extensiblity of ``sort.Interface``

```go
type Interface interface {
    Len ()int
    Less(i, j int) bool
    Swap(i,j int)
}

func Sort(data Interface)

```

```go
entries := []string{"foo","bar"}
stringSlice := sort.StringSlice(entries) // convert to interface
sort.Sort(stringSlice)

type Organ struct {
    Name string
    Weight int
}

type Organs []Organ
func (s Organs) Len() int {return len(s)}

// Extend interface , you get it

```

## Sorting in reverse

Use ``sort.Reverse`` which defined as

```go
type reverse struct {
    Interface
}

func (r reverse) Less(i,j int) bool {
    return r.Interface.Less(j,i)
}

func Reverse(data Interface) Interface {
    return &reverse{data}
}

entires := []string{"foo","bar"}

sort.Sort(sort.Reverse(sort.StringSlice(entries)))
```

## Make the nil value useful

```go
type StringStack struct {
    data []string // zero value ready to use
}

func (s*StringStack) Push(x string){
    s.data = append(s.data,x)
}

func (s *StringStack) Pop()string {
    ///

    panic("pop empty stack")
}

```

## Nothing in Go prevents calling a method with nil reciever

```go
type IntList struct {
    Value int
    Tail *IntList
}

func (l*IntList) Sum() int {
    if  list == nil {
        return 0
    }
    return list.Value + list.Tail.Sum()
}

```