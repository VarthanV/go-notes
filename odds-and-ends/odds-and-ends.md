
## Enumerated Types

- There are almost no real enumerated types in Go

- We can almost make using named type and constant

```go
type shoe int

const (
    tennis shoe = iota
    dress
    sandal
    clog
)
```
- iota strats at 0 in each block and increments with 1,2,3...

## Traditional Flags are easy

```go
type Flag uint

const (
    FlagUp Flag  = 1 << iota //is up
    FlagBroadCast
    FlagLoopBack
    FlagMultiCast
)
```

- These flags take values in a power of two sequence
- That makes easy to combine


## Variable argument list


```go
fmt.Pritnf("$#v\n",myMap)

func sum(nums ... int) int {
    var total int

    for _ , num := range nums { // []int
        total +=num
    }
    return total
}

```

- Only last param in a param list is variable type

- We cannot pass slice only we can spread and send it

```go
s:= []int{1,2,3,4}
sum(s...)
```

```go
s = append(s, s...)
```

# Size integers

- Sometimes we need to handle low-level protocols
- So we need to work with integers thave have particular size and / or unsifned


In Go, you can use various integer types that differ in their sizes and whether they are signed or unsigned. Here's a summary of the integer types with their respective sizes:

## Signed Integers
- int8 : 8-bit integer (from -128 to 127)
- int16 : 16-bit integer (from -32,768 to 32,767)
- int32 : 32-bit integer (from -2,147,483,648 to 2,147,483,647)
- int64 : 64-bit integer (from -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807)
- int : Platform dependent; either 32 or 64 bits (based on architecture)
Unsigned Integers

## Unsigned Integers
- uint8 : 8-bit unsigned integer (from 0 to 255)
- uint16 : 16-bit unsigned integer (from 0 to 65,535)
- uint32 : 32-bit unsigned integer (from 0 to 4,294,967,295)
- uint64 : 64-bit unsigned integer (from 0 to 18,446,744,073,709,551,615)
- uint : Platform dependent; either 32 or 64 bits (based on architecture)




