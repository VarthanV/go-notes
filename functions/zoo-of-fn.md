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


- Declare a function type
```go
type Cruncher func(int) int
```

```go
func mul(n int) int {
  return n * 2
}
func add(n int) int {
  return n + 100
}
func sub(n int) int {
  return n - 1
}
```
- Crunch func processes a series of ints using a variadic Cruncher funcs:


```go
func crunch(nums []int, a ...Cruncher) (rnums []int) {
  // create an identical slice
  rnums = append(rnums, nums...)
  for _, f := range a {
    for i, n := range rnums {
      rnums[i] = f(n)
    }
  }
  return
}
```

## Anonymous fn

- A noname func is an anonymous func and it’s declared inline using a function literal.

- It becomes more useful when it’s used as a closure, higher-order func, deferred func, etc.


## Higher order fn

- A higher order func may take one or more funcs or it may return one or more funcs. Basically, it uses other funcs to do its work.

![Higher order fn](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*mFFmzfrmDn5v2dR8s_BRJA.png)

## Closures

- A closure can remember all the surrounding values where it’s defined. One of the benefits of a closure is that it can operate on the captured environment as long as you want — beware the leaks!

```go
type tokenizer func() (token string, ok bool)
```


- Split func below is an higher-order func which splits a string by a separator and returns a closure which enables to walk over the words of the split string. The returned closure can use the surrounding variables: “tokens” and “last”.

![Closures](https://miro.medium.com/v2/resize:fit:2000/format:webp/1*K8Zy2oENmAWWyT9Keh9NXw.png)


## Defer fn

- A deferred func is only executed after its parent func returns. Multiple defers can be used as well, they run as a stack, one by one.


## Credits

[Zoo fns](https://blog.learngoprogramming.com/go-functions-overview-anonymous-closures-higher-order-deferred-concurrent-6799008dde7b#35ce)