

## Channel state

- Channel block unless ready to read write

- A channel is ready to write if 
    - it has buffer space or
    - atleast one reader is ready to read

- A channel is ready to read if
  - it has unread data in its buffer
  - at least one writer is ready to write
  - it is closed 

- Channels are unidrectional , but have two ends which can be passed
seperately as parameters

- An end for **writing and closing**

```go
func get(url string ch chan<-result) { // write-only end

}
```

- An end for reading
```go
func collect(ch <- chan result) map[string]int{ // read-only end

}
```

## Closed Channels

- Closing a channel causes it to return zero value

- We can recieve a second value , if channel closed ?

```go
func main(){
    ch := make(chan int, 1)
    ch <- 1

    b, ok := <-ch
    fmt.Println(b,ok) // 1 true

    close(ch)

    c, ok:= <- chan 
    fmt.Println(c,ok) // o,false
}
```

- A channel can only be closed once (else it will panic)

- One of the main issues with goroutines is ending them

- An unbuffered channel requires a reader and writer 
(a writer blocked on a channel with no reader will leak)

- Closing a channel is often a signal that work is done

- Only one **go** routine can close a channel

- We may need some way to coordinate closing a channel or stopping goroutines 
(beyond the channel itself)