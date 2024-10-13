- First class objects
- Define them - even inside other functions
- Create anonymous function literals
- Pass them function parameters / returns values
- Store them in variables
- Store them in slices and maps (but not as keys)
- Store them as fields of a structure type
- Send and receive them in channels
- Write methods against a function type
- Compare a function var against nil 

## Function scope

Almost anything can be defined inside a function

```go
    func Do()error {
        const a =  21
        type b struct{}

        var c int

        func doIt(){

        }
    }
```
- Methods cannot be defined in a function (only at package scope)

## Function signatures

- The signuature of a fn is order & type of its parameters and return values

- It does not depend on maps of those params or returns

```go
    var try func(string,int)string

    func Do(a string, b int)string{

    } // This function is same structural type as above try
```

## Paramater terms

- A function declaration lists formal params

```go
    func do(a,b int){}
```
- A function call has actual params

```go
    result := do(1,2)
```
- A parameter is passed **by value** if the fn gets a copy; the caller can't see changes
of the copy.

- A paramater is passed **by reference** if the function can modify 
the  actual param such that the caller sees the changes.

**By value**

- numbers
- bool
- arrays
- structs

**By reference**

- things passed by pointer (&x)
- strings (but they're immutable)
- slices
- maps
- channels

## Parameter passing: the ultimate truth

- Params may be passed by value

- Actually all params are passed by copying something (i.e by value)

- If the thing copied is pointer or descriptor then shared backing store (array,hashtable)
etc can be changed through it.

Thus we think of it as reference

## Important
- When we pass a slice to a function the slice descriptor is copied but the underlying
store (array) is not copied thus changing the value in the callee function
reflects in the caller function.

## Return values

Function can have multiple return values

```go
func do(a int) int {
        return 1
}

func bar(a string)(int,error){
        return 1,nil
}
```
- Every return statement must have all values specified

## Deferred execution

How to make sure something gets done?

- close a file when opened
- close a socket/HTTP request made
- unlock a mutex we locked

The defer statement captures a function call to run later

```go
func main(){
    f , err := os.Open("a.txt")
    if err != nil {

    }

    defer f.Close()
}
```

The call to close is guaranteed to run at function exit (the first return call made)

- Defers get stacked up (LIFO)
- If we have ``defer A()`` go first and ``defer B()`` go second order of execution

```
    defer B()
    ---------------------
    defer A()
```
- Defer is function scoped

- Not good practice to use in loops since defer is function scoped

- The value gets copied at defer statement (not a reference)

```go
func main(){
    a := 10
    defer fmt.Println(a) // 10
    a = 11
    fmt.Printnl(a) //11
}

```

- Defer statement runs before return is "done"


```go
    func doIt() (a int){
        defer func(){
            a = 2
        }()
        a = 1 
        return
    }    
    // returns 2
```
- We have named return value and naked return

(not recommended)