
# Channels

- Channel is a one way communication pipe

- Things go in one end , come out the order

- In same order they went in

- Until the channel is closed

- Multiple readers and writers can share it safely

## Sequential process

```go
for {
    read()
    process()
    write()
}
```

This is perfectly nature if we think of reading & writing files or network sockets

# Communicating Sequential process

- Now put the parts together with channel to communicate

- Each part is independent

- All they share are channels between them

- The parts can run in parallel as hardware allows

- CSP provides a model for thinking about it that makes it **less hard**


## Goroutines

- A goroutine is a unit of **independent execution** (couroutine)

- It is easy to start goroutine; put **go** in front of a function call

- The trick is to know how the goroutine will stop:

  - You have well defined loop terminating condition or
  - You have signal completion through a **channel or context**,  or 
  - Let it run until the program stops

- Make sure doesn't get blocked by mistake

## Channel

- A channel is like a one-way socket or a Unix Pipe [Ref](https://www.geeksforgeeks.org/piping-in-unix-or-linux/)

- It is method of synchornization as well as communication

- We know that ``send(write)`` always **happens before** a ``receive(read)``

- It is also a vehicle for transferring ownership of data , So that only one 
goroutine at a time is writing the data (avoid race conditions).

- When we enter a critical section we need to acquire lock for other data structures
that is inherently handled by go runtime

> Don't communicate by sharing memory instead , share memory by communicating - Rob Pike


- We can restrict the use of channel as **read-only** and **write-only**

```go

// Write only
func writeOnly(ch chan<-string){

}

// Read only
func readOnly(ch <-chan string){}

```

- Channels block until input is available similar to console programs with user input
configured.

- We need to ``close`` channel promptly to prevent this blocking/leaking.

- You could only close a channel once.

- In buffered channel , A write can't read until somebody is ready to read
and reader can't reader until somebody is ready to write.


