# Methods and Interfaces

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