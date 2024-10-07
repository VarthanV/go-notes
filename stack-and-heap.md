# Stack and Heap and Escape Analysis

- In Go each go routine has its own stack memory and common pool of heap memory

Where var lives in stack or heap ?

 - Doesn't matter for the correctness of the program

 - Does affect performance of the program

 - Anything on the heap is managed by Garbage Collector.

 - The GC is good but causes latency
   - For the whole program, not just the part creating garbage

- If the program is not fast enough and large amount of heap allocation is detected in profiling or benchmarking then we need to worry

- Stacks are self-cleaning any data the space is reused depending on the context.

- When possible , the Go compilers will allocate variables that are local to a fn in the fn's stack frame.

- If the compiler cannot prove that the variable
is **not referenced after the function returns** , then the compiler must allocate the var on garbage-collected heap to avoid dangling pointer errors.

## Escape analysis

- In the current compilers, if a variable has its address taken that that var is a candidate for allocation on heap , However ever a basic escape analysis recognizes some cases wehn such vars will not live past the return from the fn and can reside on stack

Compiler flags

```bash
    go help build
    go tool compile -h 
    go build -gcflags "-m"
```

## Stays on stack

```go
package main

import "fmt"

func main() {
	n := 4
	inc(&n)
	fmt.Println(n)
}

func inc(x *int) {
	*x++
}

```

**Output**

```
# escape-analysis
./main.go:11:10: x does not escape
./main.go:8:13: ... argument does not escape
./main.go:8:14: n escapes to heap
```

**When are values constructed on heap ?**

- When a value could **possibly** be reference after the fn that constructed the value returns

- When the value is too large to fit on the stack

- When the compiler doesn't know size of value during compile time

## Commonly allocated values

- Values shared with **Pointers**

- Value stored in **interface** variables

- Func literal variables
    - Variables captured by a closure
- Backing data for **Maps , Channels , Slices and Strings**

## Conclusion ##

- Optimise for correction not performance

- Go puts only fn var on stack when it can prove a var is not used after function returns

- Sharing down typically stays on stack

- Sharing up typically escapes to heap

- Ask the compiler to find out ?

- Dont guess, use tooling (pprof,tracer)