# Goroutine internals

- Goroutines are not OS level threads and they are not excatly green threads(Threads that are managed by languages runtime).

- They are high level abstraction called ``coroutines``.

## Coroutines

- Coroutines are simply concurrent subroutines (functions,closures or methods in Go) that are ``nonpreemptive`` (cannot be interrupted).

- They have multiple points which allows for suspension or reentry points.

- Goroutines doesnt define their suspension or reentry points.

- Go runtime observes the runtime behavior of goroutines and automically suspend when thet are blocked and resumes when they become unblocked.

- In a way that makes them preemptable only when they are blocked.

- Go's mechanism for hosting goroutines is an implementation of ``M:N`` scheduler. Which means ``M`` green threads maps to ``N`` OS threads.

- Goroutines are then scheduled on to green threads , when we have more goroutines than green threads the scheduler handles the distribution of goroutines across available threads and ensures when these goroutines become blocked other goroutines can be run..

- Go follows a model of concurrency called ``fork-join`` model. The word ``fork`` refers to the fact that any point in program it can split of as a child branch of execution to run concurrently alongside the parent.

- The word  ``join`` refers to the fact that at some point in time these concurrenct branches will join together.

- Where the child rejoins with its parent is called ``join-point``