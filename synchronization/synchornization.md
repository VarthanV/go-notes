
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


## RW mutexes

- Sometimes we need to prefer readers to (infrequent) writers

```go
type InfoClient struct {
    mu sync.RWMutex
    token string 
    tokenTime time.Time
    TTL time.Duration
}

func (i*InfoClient) CheckToken() (string,time.Duration){
    i.mu.RLock()
    defer i.mu.RUnlock() 

    return i.token  , i.TTL-time.Since(i.tokenTime)
}
```

- **Read lock (RLock)**: When a read lock is held, other goroutines can also acquire read locks, but no goroutine can acquire the write lock until all read locks are released.

- **Write lock (Lock)**: When the write lock is held, no other goroutine can acquire either a read or write lock. This ensures exclusive access to the resource for writing.

## Summary:
- When a read lock is held, a write lock is blocked.
- When a write lock is held, both read and write locks are blocked.

## Atomic Primitives

```go
func do() int {
    var n int64
    var w sync.WaitGroup

    for i := 0 ; i < 1000; i++{
        w.Add(1)

        go func(){
            atomic.AddInt64(&n,1) // fixed , Couples FETCH,DECODE , EXECUTE as single instruction
            w.Done() 
        }
    }
    w.Wait()
    return int(n)
}
```

## Only-once execution

A sync.Once obkect allows us to ensure a fn runs only only (only first call 
to Do will call the fn passed in )

```go
var once sync.Once

var x *signleton

func initialize(){
    x = NewSingleton()
}

func handle (){
    once.Do(initialize)
}
```

Checking ```x== nil``` in handler is unsafe


## Pool 

A ``Pool`` provides for efficient and safe reuse of objects , but it is a
container of ``interface{}``

```go
var bufPool = sync.Pool{
    New: func() interface{}{
        return new(bytes.Buffer)
    }
}

func Log(w io.writer , key,val string){
    b := bufPool.Get(). (*bytes.Buffer) // more reflection
    b.Reset() 
    W.Write(b.Bytes())
    bufPool.Put(b) // use bytes and put back to pool
}

```