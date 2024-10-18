
## Go benchmarks

- Go has standard tools and convention for running benchmarks

- Benchmarks live in test files ending with ``_test.go``

- You run benchmarks with ``go test -bench``

- Go runs only ``BenchmarkXXX`` functions

## A few things to consider when benchmarking

- Is the data/code in cache?

- did we hit a GC collection ?

- did virtual memory has to page in/ out

- did the compiler remove code via optimization

- Are we running in parallel? How many cores? 

- Are those physical or virtual

- Is core shared with something else

- What other process are running in the machine