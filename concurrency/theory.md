
## Some definitions of conucrrency

- Execution happens in non-deterministic order

- Undefined out of order execution

- Non sequential execution

- Parts of program execute out of order or in partial order

## Non deterministic

Different behaviour on different runs event with same input

Run 1: {1,2a,2b,3a,3b,4}

Run2: {1,2a,3a,2b,3b,4}

We dont mean different results but different trace of execution

## Formal definition

## Concurrency

- Parts of the program may execute independently in some non-deterministic(partial) order.

- Don't have complete control on whether is executed parallely or in overlapping time 

# Parallelism

- Parts of a program execute independently at same time

- You can have concurrency with a single core processor (think interrupt handling in OS)

- Parallelism can happen only on  a multi-core processor

- Concurrency doesn't make program faster , parallelism does

- Parallelism can be acheived by concurrency if the scheduler executes two different parts in different core , Parallelism can be acheived by concurrency and concurrency is not parallelism.

- Concurrency is about dealing with things happening out-of-order

- Parallelism is about things actually happening at same time

- A single program **won't have parallelism without concurrency**

- We need concurrency to allow parts of program to execute independently.

## Race condition

- Possiblity of the non-deterministic executing programs to get the outcome wrong.

- A race condition is a bug.

- Race conditions involve independent parts of program changing things that are shared

- Solutions making sure operation produce a consistent state to any shared data

   - Dont share anything
   - Make shared things read-only
   - allow only one writer to shared things
   - make the ready-modify-write operations atomic

- In the last case , we are adding more sequential order to our operations.