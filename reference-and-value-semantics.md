# Reference and Value semantics

|Pointers| Values  |
|-----------|--------|
| Shared not copied | Copied,not shared|

- Value semantics leads to higher integrity, particularly with concurrency (don't share)

- Pointer semantics may be more efficient

- Best approach with concurrency is don't share anythings

## Common use of pointers

- Some objects cant be copied safely (mutex)

- Some objs are too large to copy efficiently (size > 64 bytes pointers)

- Some methods need to change (mutate) the receiver later.

- When deocding protocl data into an object (JSON , etc)

```go
    var r Response
    err := json.Unmarshal(j, &r)
```
- When using pointer to signal a null object

## Must not copy 

Any struct with a mutex must be passed by ref

```go
type Employee struct {
        mu sync.Mutex
        Name string
}

func do(e*Employee){
    e.mu.Lock()
    defer e.mu.Unlock()
}
```

# May copy

Any small struct under 64 bytes should be copied

```go
type Widget struct {
    ID int
    Count int
}

func Expend(w Widget)Widget {
    w.Count-- 
    return w
}

// Go routinely copies string and slice descriptors
```
## Semantic consistency

If a thing is to be shared then always pass a pointer

```
f1(emp *Employee)
    |
    V
f2(emp *Employee)
    |
    V
f3(emp Employee) // passes a copy
    |
    V

f4(emp *Employee) // Changes are lost
    |
    V
```

## Stack allocation

- Stack allocation is more efficient

- Accessing a var directly is more efficient than following a pointer

- Accessing a dense sequence of data is more efficient than sparse data (an array is faster than a linkedlist etc)

## Heap allocation

Go would prefer to allocate on stack, but sometime can't

- A function returns pointer to a local object

- A local object is captured in fn closure

- A pointer to local obj is sent via channel

- Any obj is assigned to interface

- Any obj whose size is variable at runtime(slices)

The use of **new** has nothing to do with it

## For loops

The value returned by range is always a copy

```go
    for i, thing := range things {
        // thing is a copy
    }
```
Use idx if need to mutate element

```go
for i := range things {
    things[i].which = foo
}

```
## Slice safety

Anytime a function mutates a slice that's passed in , we must return a copy

```go
func update(things []thing) []thing{
    things = append(things.x)
    return things
}
```
That's because slice backing array may be reallocated to grow

Keeping a pointer to elem of slice is risky

```go
type user struct {name string; count int}

func addTo(u*User) {u.count++}

func main(){
    users := []user{{"alice",0},{"bob",0}}

    alice := &users[0] // risk
    amy := user{"amy",1}
    users = append(users,amy) // slice regrown so the pointers ref lost

    addTo(alice) // alice is likely slace pointer
    fmt.Println(users)// alice count 0
}

```
Taking the address of mutating loop var wrong

```go
func (r OfferResolver) Changes() []ChangeResolver{
    var result []ChangeResolver

    for _ , change := r.d.Status.Changes {
        result = append(result, ChangeResolver{&change})//wromg
    }
    return result
}
```

Wrong: all the returned resolver point to last change in list

Fix:

```go
func (r OfferResolver) Changes() []ChangeResolver{
    var result []ChangeResolver

    for _ , change := r.d.Status.Changes {
        c := change
        result = append(result, ChangeResolver{&c})//wromg
    }
    return result
}
```

## Inheritance

Inheritance has conflicting meanings

- substitution(subtype) polymorphism

- structural sharing of impl details

Subclass inherits from superclass/ baseclass

## Why inheritance is bad? 

- It injects a dependence on superclass into subclass

- what if the superclass changes behavior?

- what if the abstract concept is leaky?

Not having inheritance means better encapsulation and isolation

Inheritance will force you in term of communication with objects.

## OO in Go

- Go offers four main support for OO programming

- Encapsulation using the package for visblity control

- abstraction and polymorphism using interface types

- Enhanced composition to provide structure sharing

Go doesn't offer substitutability based on types

Substitutability is based only on interfaces ,purely a function of abstract behavior