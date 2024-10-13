# Scope vs Lifetime

- Scope is static , based on code at compile time

- Lifetime depends on program execution (runtime)

```go

    func doIt() *int {
        var b int

        return &b
    }
```
- Variable b can only be seen inside ``doIt`` but its value will live on

- The value(object) will live so long as part of the program keeps to a pointer to it

## Closure

A closure is when a fn inside another fn "closes over" on or more local variables of outer function

```go
    func fib() func() int {
        a ,b := 0,1
        return func()int {
            a , b = b,a+b
            return b
        }
    }
```
- The inner fn gets reference to outer function vars

- Those vars may end up with much longer lifetime than expected as there's a reference to inner func

## Usage of closures

- **Data encapsulation**: Closures allow creation of private variables that are not accessible from outside function which helps encapsulate and protect data.

- **Maintain State**: Maintain state across multiple fn calls without relying on global vars , Each time closure is invoked it can refer to data stored in outer fn's scope

- **Callbacks and event handlers**: Preserve variables from outer fn when callback is executed later

- **Functional programming**: Currying or partial application where functions are transform


```go
    sort.Slice(ss, func(i, j int )bool{
        return ss[i] > s[j]
    })

    // The variable ss gets closed over to the function signature without being passed explicitly as parameter
```
