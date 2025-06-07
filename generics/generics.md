# Generics

- Generics in Golang help us to write flexible and reusable code without sacrificing type safety

- Generics let you write one function that works for any type.

```go
func PrintSlice[T any](s []T) {
    for _, v := range s {
            fmt.Println(v)
    }
}
```

- ``T`` is a type parameter.

- ``[T any]`` means "T can be any type".

- ``[]T`` means a slice of type ``T``.

## New features of Go 1.18

- Type parameters for functions and types

- Type sets defined by interfaces.

- Type inference.


## Type Parameters List

- Type Parameters List are like ordinary parameter list with square brackets. Conventionally start with uppercase to emphasize they are types.


```go
    [P,Q constraint , R constraint]
```

## Generic min func

```go
    func min(x,y float64) float64{
        if x < y {
            return x
        }
        return y
    }

    func min[T constraints.Ordered](x,y T) T {
        if x < y {
            return x
        }
        return y
    } 

// Generic function
    func min[T constraints.Ordered](a, b T) T {
	if a < b {
		return a
	}
	return b
}
```

## constraints.Orderer

-  It is a constraint that restricts type parameters to types that support comparison operators like ``< , <= , > , >=``


```go
    int, int8, int16, int32, int64,
    uint, uint8, uint16, uint32, uint64, uintptr,
    float32, float64,
    string
```

- Ordered defines set of all integers , floating point and string types .The < operator is supported by every type in this set.

# Instantiation of Generics

- Substitute type args for type paramaters.

- Check that type args implements their constraints


```go
   fmin := min[float64]
   m := fmin(1.0,2.0) 
```

## Generic binary tree

```go
    type Tree [T interface{}]{
        left, right *Tree[T],
        data T
    }

    func (t*Tree[T]) Lookup(x T) * Tree[T]

```

![alt text](https://go.dev/blog/intro-generics/method-sets.png)

![alt text](https://go.dev/blog/intro-generics/type-sets.png)

```go
    type Ordered interface {
        Interger | Float | ~string
    }
```

- The ``~`` is used to represent that we would like to match all the implementations of string instead of string excatly, For example

```go
    type Status string
// The status also satisfies the constraints of the Ordered Interface
```

- ``any`` is an alias for the empty interface.


# Automatic type inference for generic function call

```go
    func min[T constraints.Ordered](x,y T) T

    var a, b , m float64

    m = min[float64](a,b)
    m = min(a,b) // Equivalent to m = min[float64](a,b)
```

# When to use Generics

- Write code , don't design types.

- We need to write what we want then we can build the types for it.


In **Go generics**, **parameters** refer to the **type parameters**—they are like placeholders for types, just like function parameters are placeholders for values.

---

### 🧠 What are Type Parameters?

They are variables for types, declared in square brackets `[]`, and are used to make functions, methods, or types **generic** (work with different data types without duplicating code).

---

### ✅ Example

```go
func PrintSlice[T any](s []T) {
	for _, v := range s {
		fmt.Println(v)
	}
}
```

* `T` is the **type parameter**.
* `any` is the **constraint** (here, it means T can be any type).
* `PrintSlice` can now work with slices of `int`, `string`, `float64`, etc.

---

### 🧱 Type Parameter vs Type Argument

| Term           | What it means                           | Example                                    |
| -------------- | --------------------------------------- | ------------------------------------------ |
| Type Parameter | The placeholder name for the type       | `T` in `PrintSlice[T any]`                 |
| Type Argument  | The actual type you use when calling it | `int` in `PrintSlice[int]([]int{1, 2, 3})` |

---

### 💡 Analogy

Think of type parameters like function parameters:

```go
func Add(a int, b int) int {
    return a + b
}
```

Here, `a` and `b` are parameters for values.

In generics:

```go
func Add[T constraints.Ordered](a T, b T) T {
    return a + b
}
```

Now, `T` is a parameter for **types**.

---

Let me know if you'd like examples using structs or interfaces too!


## When are type parameters useful

- Functions that work on slices , maps and channels of any element type.

- General purpose DS (Linkedlist , binary trees)

- When operation of type parameters,  prefer to use function to methods.

- When a method look same for all the types.

## When not use generics

- When just calling  a method on the type argument.
- When the implementation of a common method is different for each type

```go
// good
func ReadFour(r io.Reader) ([]byte,error)

// bad
func ReadFour[T io.Reader](r T)([]byte,error)

```