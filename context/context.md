# Context

- In Go servers , each incoming request is handled in its own ``go-routine``

- Request handlers often start additional goroutines to access backends such as ``databases`` and ``RPC services``

- [context](https://pkg.go.dev/context) package that makes it easy to pass request-scoped values, cancellation signals, and deadlines across API boundaries to all the goroutines involved in handling a request.

- The core of the ``context`` package is the ``Context`` type

```go
// A Context carries a deadline, cancellation signal, and request-scoped values
// across API boundaries. Its methods are safe for simultaneous use by multiple
// goroutines.
type Context interface {
    // Done returns a channel that is closed when this Context is canceled
    // or times out.
    Done() <-chan struct{}

    // Err indicates why this context was canceled, after the Done channel
    // is closed.
    Err() error

    // Deadline returns the time when this Context will be canceled, if any.
    Deadline() (deadline time.Time, ok bool)

    // Value returns the value associated with key or nil if none.
    Value(key interface{}) interface{}
}
```

## Done method

- The ``Done`` method returns a channel that acts as a cancellation signal to functions running on behalf of the ``Context``

- When the channel is closed, the functions should abandon their work and return.

- The `Err` method returns an error indicating why the `Context` was canceled

- A `Context` does not have a `Cancel` method for the same reason the Done channel is receive-only.

- A `Context` is safe for simultaneous use by multiple goroutines. Code can pass a single `Context` to any number of goroutines and cancel that `Context` to signal all of them.

- The `Deadline` method allows functions to determine whether they should start work at all; if too little time is left, it may not be worthwhile. Code may also use a deadline to set timeouts for`` I/O operations``.

- ``Value`` allows a Context to carry request-scoped data. That data must be safe for simultaneous use by multiple goroutines.

## Derived Contexts

- The `context` package provides functions to derive new `Context` values from existing ones. 

-  These values form a tree: when a `Context` is canceled, all ``Contexts`` derived from it are also canceled.

- ``Background`` is the root of any ``Context`` tree; it is never canceled:

```go
// Background returns an empty Context. It is never canceled, has no deadline,
// and has no values. Background is typically used in main, init, and tests,
// and as the top-level Context for incoming requests.
func Background() Context
```

- ``WithCancel`` and ``WithTimeout`` return derived Context values that can be canceled sooner than the parent Context. The Context associated with an incoming request is typically canceled when the request handler returns

- ``WithCancel`` is also useful for canceling redundant requests when using multiple replicas.

- ``WithTimeout`` is useful for setting a deadline on requests to backend servers.

```go
// WithCancel returns a copy of parent whose Done channel is closed as soon as
// parent.Done is closed or cancel is called.
func WithCancel(parent Context) (ctx Context, cancel CancelFunc)

// A CancelFunc cancels a Context.
type CancelFunc func()

// WithTimeout returns a copy of parent whose Done channel is closed as soon as
// parent.Done is closed, cancel is called, or timeout elapses. The new
// Context's Deadline is the sooner of now+timeout and the parent's deadline, if
// any. If the timer is still running, the cancel function releases its
// resources.
func WithTimeout(parent Context, timeout time.Duration) (Context, CancelFunc)
```

- ``WithValue`` provides a way to associate request-scoped values with a Context
```go
// WithValue returns a copy of parent whose Value method returns val for key.
func WithValue(parent Context, key interface{}, val interface{}) Context
```


> When designing an API with context, remember the advice: pass context.Context in as an argument; don’t store it in structs.

## Further reading

[Context and Structs](https://go.dev/blog/context-and-structs)

[Context](https://pkg.go.dev/context)
