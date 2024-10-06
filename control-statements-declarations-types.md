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

## Declaration

- Constant declaration with const

- Type declaration with type

- Variable declaration with var (must have type or initial value sometimes both)

- Shortly initialized variable declaration of type ``:=`` only inside a function

- Function declaration with func (methods may be only declared at package level)

- Formal parameters and named returns for a function


## var keyword

```go
    var a int // 0 by default
    var b int = 1
    var c = 1 // int
    var d  = 1.0 // float64

    var (
        x,y int
        z float64
        s string
    )
```
## Short declaration

The short declaration op has some rules

- It cant be used outside of a fun ction

- It must be used (instead of var in control statement) (if,etc)

- It must declare atleast one new var

```go
    err := do()
    err := bad() //WRONG
    x , err := val(); // OK
```

- It wont reuse an d existing declaration from an outer scope

## Shadowing short declarations

- Short declarations with := have some gotchaes

```go
    func main(){
        n, err := fmt.Println("hello")
        
        if _ err := fmt.Println(n); err != nil {
            fmt.Println(err)
        }
    }
```

- **Compile error** : the first errr is unused

This follows scoping rules because :=  is a declaration and second ``err`` is in the scope of the ``if`` statement


```go
    func bad ()error {
        var err error
        for {
            n, err  := foo() // shadows err above
        // .. it is scoped
            if err != nil {
                break
            }
            bar (buff)
        }
        return err // will always be nil
    }
```

## Structural typing

- It is the same type if it has same structure or behaviour

```go
    a := [...]int{1,2,3}
    b := [3]int{}

    a = b // ok

    c := [4]int{}

    a = c // NOT OK
```

It's the same type if it has same structure or behaviour

- Arrays of same size and base type

- Slices with same base type

- Maps of same key and value types

- Structs with same seq of fieldnames/types

- Functions with same parameters and return types

## Named typing

It is only the same type if it has same declared type name

```go
    type x int

    func main(){
        var a x // xis defined type

        b := 12 // b to int

        a = b// TYPE mismatch

        a = 12 // OK untyped literal

        a = x(b) // OK , type conversion
    }

```

- Go uses name typing for non-function user-declared types


## Numeric literals

- Go keeps arbitary precision for literal values (256 bits or more)

- Integer literals are untyped
    - Assign a literal to any size integer without conversion
    - Assign an integer literal to float

- Ditto float and complex

- Mathematical constants can be very precise

Pi = 3.1444949494949

- Constant arithmetic done at compile time doesnt lose precision

## Operators

**Arithemtic**: numbers only except + on string

```
    + - * / % ++ --
```

**Comparision**: only numbers / strings support order

```
    == != < > <= >=
```

**Boolean**: Only bools with shortcut eval

```
    ! && || 
```

**Bitwise**: Operate on integers

```
    & | ^ << >> &^
```
