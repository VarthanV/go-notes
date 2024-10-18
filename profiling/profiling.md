
## Profiling

- The Go profiler is a tool built into the Go programming language that helps developers analyze the performance of their code by providing insights into CPU usage, memory allocation, and other performance metrics. It helps identify bottlenecks, memory leaks, and inefficient parts of the code by tracking how much time or memory each function consumes. Go's built-in `pprof` package is commonly used for profiling, and developers can generate various types of profiles (CPU, memory, goroutines, etc.) to optimize their programs.

- The number one way to leak memory is to leak a goroutine , The most common way is to leave a socket hung up.


## How to profile

- We must build the program as a binary: ``go build``

-  Assume we are using normal http package to host our server , Different libraries have different instructions. Normally it gets binded to the below
route

``http://localhost:8080/debug/pprof/profile``

- After 30 seconds it downloads a profile file (This should be configured)

- Then we can run the tool:

```shell 
go tool pprof <binary> <profile-file>
```

- Interactive with a pro,pt

- just get the top entries with -top

- open a browser with ```shell -http=":6060"```

##  More reading

[Performance](https://github.com/enocom/gopher-reading-list?tab=readme-ov-file#performance)

[Pprof](https://jvns.ca/blog/2017/09/24/profiling-go-with-pprof/)