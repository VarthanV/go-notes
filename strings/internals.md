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

## UTF-8 and String literals

-  Indexing a string yields its bytes, not its characters

- String is just a bunch of bytes

- When we store a character value in a string we store it as ``byte-at-a-time`` representation

-  Go strings are always UTF-8, but they are not: only string literals are UTF-8.

- Strings can contain arbitrary bytes, but when constructed from string literals, those bytes are (almost always) UTF-8.


```go
func main() {
    const placeOfInterest = `⌘`

    fmt.Printf("plain string: ")
    fmt.Printf("%s", placeOfInterest)
    fmt.Printf("\n")

    fmt.Printf("quoted string: ")
    fmt.Printf("%+q", placeOfInterest)
    fmt.Printf("\n")

    fmt.Printf("hex bytes: ")
    for i := 0; i < len(placeOfInterest); i++ {
        fmt.Printf("%x ", placeOfInterest[i])
    }
    fmt.Printf("\n")
}
```

```
plain string: ⌘
quoted string: "\u2318"
hex bytes: e2 8c 98 
```

## Code points, characters, and runes

- The Unicode standard uses the term ``code point`` to refer to the item represented by a single value.

- The code point``U+2318``, with hexadecimal value ``2318``, represents the symbol ``⌘``

- The Go language defines the word ``rune`` as an alias for the type ``int32``

``⌘`` is rune with integer value ``0x2318``

## Salient points

- Go source code is always UTF-8.

- A string holds arbitrary bytes.

- A string literal, absent byte-level escapes, always holds valid UTF-8 sequences.

- Those sequences represent Unicode code points, called runes.

- No guarantee is made in Go that characters in strings are normalized.


## Range loops

- Go treats UTF-8 specially, and that is when using a for range loop on a string.

```go

    const nihongo = "日本語"
    for index, runeValue := range nihongo {
        fmt.Printf("%#U starts at byte position %d\n", runeValue, index)
    }

```

- Output
```
U+65E5 '日' starts at byte position 0
U+672C '本' starts at byte position 3
U+8A9E '語' starts at byte position 6
```

## Libraries

```go
const nihongo = "日本語"
    for i, w := 0, 0; i < len(nihongo); i += w {
        runeValue, width := utf8.DecodeRuneInString(nihongo[i:])
        fmt.Printf("%#U starts at byte position %d\n", runeValue, i)
        w = width
    }
```
- [Library](https://go.dev/pkg/unicode/utf8/)

## Further reading

[Go strings](https://go.dev/blog/strings)