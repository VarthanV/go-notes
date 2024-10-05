# Control statements, declarations and types

# if then else
 
- The next type of structure is a choice between alternatives

- All if-then  statements require braces

```go
    if a == b {
        fmt.Println("a equals b")
    } else {
        fmt.Println("a is not equal to b")
    }

```

They can start with short declaration or something

```go
    if err := doSomething(); err != nil {
        return err
    }
```

## For loops

- For loops provides automatic repetition

- There is only for (no do while) with options

- Explicit control with an index variable

```go
    for i:=0; i < 10; i++{
        fmt.Println(i*i)
    }
```

- Three parts all optional(initialize, check ,increment)

- The loop ends when check fails(i==10)

- Implicit control through range operator for arrays and slices

```go
// one var is is index
    for i := range myArr {
        fmt.Println(myArr[i])
    }

// two vars index,val
for i, v:= range myArr {
    fmt.Println(i,v)
}
```
The loop ends when range is exhausted

- Loop over maps

```go
    for k ,  v:= range  myMap{
        fmt.Println(k,v)
    }
```

- Maps in go dont have ordering they are just naive hash tables

- The loop ends when range is exhausted.

## An infinite loop with explicit break

```go
i , j := 0,3
```

```go
    for {
        i , j = i+50, j*j
        fmt.Println(i,j)
        if (i > j){
            break
        }
    }
```
- The use of ```continue``` to make iteration start over

## Labels and loops

- We need to break or continue the outer loop

```go
outer:
    for k := range itemsMap {
        for _ ,v := range returnedData{
            if k == v.id{
                continue outer
            }
        }
        t.Error("key not found ",k)
    }

```
- A label needed to refer outer loop

## Switch

- Switch is choice between alternatives

- Shortcut to replace series of if then statements

```go
    switch a := f.Get(); a {
        case 0, 1,2:
            fmt.Println("underflow")
        case 3,4,5,6,7,8:

        default:
            fmt.Println("warning overload")
    }
```
- Alternatives may be empty and don't fallthrough break is required

## Switch on true

- Arbitary comparisions may be made for switch without an argument

```go
    a := f.Get()

    switch {
        case a <=2:
            fmt.Println("underflow possible")
        default:
            fmt.Printn("default break")
    }

```

# Packages

- Everything lives in a package

- Every standalone program has a main package

```go
    package main
    
    func main(){
        fmt.Println("hello world")
    }
```

- Nothing is global either is in the current package or other

- It is either package scope or function scope

- Anything can be declared as package scope

```go
    package secrets

    const(
        DefualtID= "000--000"
    )
    
```
- Can't use the := operator

- Every name thats capitalized is expored

```go
    const CanUse = true

    type internal struct{

    }
    
    func cantUse(){

    }
```