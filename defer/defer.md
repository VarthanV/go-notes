## Defer

- It takes a func and executes it just before* the surrounding func returns whether there is a panic or not.

![Defer visualized](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*9jvoL3xXkwydC7C-EMcfkg.png)

- Go doesn’t need destructors because it has no built-in constructors. This is a good balance.

- Defer resembles to “finally” however the defer belongs to the surrounding “func” while the finally belongs to an exception “block”.

- Defer funcs are often used to release the acquired resources inside a func.

- Defer can recover from a panic to prevent the termination of a program if the panic emitted from the same goroutine.

![Recovereing from panic](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*H0luK0YxgVlSkXqFsyhSnw.png)


- Deferred func can be any type of func , so when used with anonymous functions it knows its surroundings better

```go
func example(){
    num := 42
    defer func(){
        // num is 13
    }()

    num = 13
}

```

- Go runtime will save any passed params to the deferred func at the time of registering the defer— not when it runs.

![Defer](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*GpuBW-PeJt4LwumKJwUwGQ.png)

- Defer stack is LIFO (Last in first out)

![LIFO](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*_s3_Lf92bz7W_U5EZPQSOA.png)

- The passed params to a deferred func are saved aside immediately without waiting for the deferred func to be run.

- So, when a method with a value-receiver is used with defer, the receiver will be copied (in this case Car) at the time of registering and the changes to it wouldn’t be visible (Car.model). Because the receiver is also an input param and evaluated immediately to “DeLorean DMC-12” when it’s registered with the defer.

### Without pointers

```go
type Car struct {
  model string
}
func (c Car) PrintModel() {
  fmt.Println(c.model)
}
func main() {
  c := Car{model: "DeLorean DMC-12"}
  defer c.PrintModel() //DeLorean DMC-12
  c.model = "Chevrolet Impala"
}
```

### With Pointers

```go
type Car struct {
  model string
}
func (c Car) PrintModel() {
  fmt.Println(c.model)
}
func main() {
  c := Car{model: "DeLorean DMC-12"}
  defer c.PrintModel() // "Chevrolet Impala"
  c.model = "Chevrolet Impala"
}
```

## Further reading

[Defer fn](https://blog.learngoprogramming.com/golang-defer-simplified-77d3b2b817ff)


