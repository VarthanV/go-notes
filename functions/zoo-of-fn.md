# Zoo of functions

## Name fn

- A name fn has a name and declared at package level , outside of the body of another fn

```go
func Len(s string) int {
    return ...
}

```

**Anatomy**

- ``Len``: Function name
- ``s``: param name
- ``int``: result type
- `` Len(s string) int``: Signature

## Variadic funcs

- Variadic funcs can accept an optional number of input parameters.

```go
func f(names ...string)
```

- The function can accept optional number of string as params

- Either they can be as the last param or only param to a function

## Methods

- When we attach a func to type , The fn becomes a method of the type , so it can be called
through the type

- Go will pass the type (``receiver``) to the method when it is called

```go
type Count int

func (c Count) Incr() int {
  c = c + 1
  return int(c)
}

```

- The behavior of the method is similar to this

```go
func Incr(c Count) int
```

![Anatomy of method](https://miro.medium.com/v2/resize:fit:1400/format:webp/1*pTk0c8mVLT-sHBASXKDkXw.png)

## Value receiver

- The value of the Count instance is copied and passed to the method when it’s called.

```go
var c Count; c.Incr(); c.Incr()
// output: 1 1
```

- It doesn’t increase because ``c`` is a value-receiver.

## Pointer receiver

- To attach the counter's value we need to attach the ``Incr`` function to the ``Count`` pointer type

```go
func (c *Count) Incr() int {
  *c = *c + 1
  return int(*c)
}

var c Count
c.Incr(); c.Incr()
// output: 1 2

```

## Interface methods

- Let's recreate the program using interface methods

![Interface anatomy](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*El6Ok6ugLnHPNCsFAF5nQg.png)

## First class functions

- First-class means that funcs are value objects just like any other values which can be stored and passed around.

![First class functions](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*UYozg9jW6Gan8fymRHaTGw.png)

