An interface specifies abstract behaviour in terms of **methods**

```go
type Stringer interface {
    String() string
}
```

Concrete types offer methods that satisfy the interface

```
    Interface <- (Method)  Object
```

## Method

- A method is a special type of function

- It has a **receiver** parameter before the function name param

```go
type IntSlice []int

func (is IntSlice) String() string{
    var (
        strs []string
    )

    // is is instance of IntSlice and has method String()
    for _ , v := range is {
        strs = append(strs,strconv.Itoa(v))
    }
    return ""
}
```

# Why interfaces?

- Without interfaces we need to write many functions for many concrete types , possibly coupled to them 

```go
func OutputToFile(f*File...){...}
func OutputToBuffer(b*Buffer...){...}
func OutputToSocket(s*Socket...){...}
```

- Better we define the fn in terms of abstract behavior

```go
type Writer interface {
    Write([]byte)(int,error)
}

func OutputTo(w io.Writer ,....){....}
```

- An interface specifies required behaviour as **method set**

- Any type that implements that method set satisfies the interface

```go
type String interface {
    String() string
}

func (is IntSlice) String() string{

}
```
- This is know as **duck typing**

- No type will declare itself to implement **ReadWriter** explicitly


## Not just structs

- A method may be defined on any user-declared type

- Methods can't be declared on primitive types like int but

```go
type MyInt int

func (i MyInt) String(){}
```

- The same method name may be bound to different types

## Receivers

- A method may take a pointer or value receiver but not both

```go

type Point struct{
    X ,Y float64
}

func (p Point) Offset(x,y float64) Point {
    return Point{p.x+x, p.y+y}
}

func (p*Point) Move(x,y float64) {
    p.X += x
    p.Y +=y 
}
```
- Taking a pointer allows the method to change the receiver (original object)

## Interfaces and substitution

- All methods must be present to satisfy the interface

```go
var w io.Writer
var rwc io.ReadWriteCloser

w = os.Stdout // OK , *os.File has write method
w = new(bytes.Buffer) // OK: *bytes.Buffer has Write method
w = time.Second // ERROR: no Write method

rwc = os.Stdout //OK: *os.File has all 3 methods
rwc = new(bytes.Buffer) // ERROR: no Close method

w = rwc // OK : io.ReadWriteCloser has write

rwc = w //ERROR: no close method

```
- We care more about the implementation rather than the object itself

## Interface satisfiability

- The receiver must be of the right type (pointer or value)

```go
type IntSet struct {}

func (*IntSet) String() string 

var _ = IntSet{}.String() // ERROR: String needs *IntSet (l-value)

var s IntSet 
var _ = s.String() // OK: s is a var &s used automatically

var _  fmt.Stringer = &s // OK
var _ fmt.Stringer = s // ERROR: no String method
```

## Interface Composition

- ``io.ReadWriter`` is actually defined by Go as two interfaces

```go
type Reader interface {
    Read(p []byte) (n int, err error)
}

type Writer interface {
    Write(p []byte) (n int, err error)
}

type ReadWriter interface {
    Reader
    Writer
}
```
- Small interfaces with composition where needed are more flexible

## Interface declarations

-  All methods for a given type must be declared in same package where the type is 
declared

-  This allows package imports to know all methods at compile time

- Can extend the type in a new package through embedding

```go
type Bigger struct {
    my.Big //get all Big methods via promotion
}

func (b Bigger) DoIt(){} // add one more method
```