
- select allows any ``ready`` alternative to proceed among
    - a channel we can read from
    - a channel we can write to
    - a default action that's always ready

- Most often select runs in a loop so we keep trying 

- We can out a timeout or done channel into select

- We can compose channels as synchoronization primitives.
- Traditional primitives (mutex,condition variable) can't be composed.

- Select is usaully put in for loop to read multiple times.

- In a select block the default case is always ready and will be choosen if no other case is

```go
func sendOrDrop(data []byte){
    select {
        case ch <- data:
        // sent ok: do nothing
        default:
            log.Printf("drop %d bytes ",len(data))
    }
}
```
- Don't use default inside a loop , the select will busy wait and waste CPU