
## Conventional synchronization

Package ``sync`` foe example

- Mutex
- Once
- Pool
- RWMutex
- WaitGroup

- Package ``sync/atomic`` for atomic scalar reads and writes

## Mutual Exclusion

- What if multiple goroutines must read and write some data ?

- We must make sure only **one** of them can do at any instant (critical section)

- We can accomplish this with type of clock
    - Acquire the lock before accesing the data
    - Any other goroutine will block waiting to get the lock
    - Release the lock when done

## Mutexes in action

```go
type SafeMap struct {
    sync.Mutex // not safe to copy
    m map[string]int
}

func (s*SafeMap) Incr(key string){
    s.Lock()
    defer s.Unlock()

    s.m[key]++
}

```
Using ``defer`` is good habit to avoid mistakes.