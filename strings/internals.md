## String, Bytes , Runes and characters

##  What is a string?

- In Go a string is in effect a ``read-only-slice-of-bytes``.

- String hold ``arbitary`` bytes , It is not required to hold unicode text , UTF-8 text or any other predefined format.

- As the content of the string is concerned it is excatly equivalent to a ``slice of bytes``.

- Here is a string literal (more about those soon) that uses the \xNN notation to define a string constant holding some peculiar byte values. (Of course, bytes range from hexadecimal values 00 through FF, inclusive.)


```go
const sample = "\xbd\xb2\x3d\xbc\x20\xe2\x8c\x98"
```

## Printing strings

- Because some of the bytes in our sample string are not valid ASCII, not even valid UTF-8, printing the string directly will produce ugly output. The simple print statement

```go
fmt.Println(sample) // ��=� ⌘
```
- Examining individual pieces

```go
    for i := 0; i < len(sample); i++ {
        fmt.Printf("%x ", sample[i])
    }
// bd b2 3d bc 20 e2 8c 98
```
