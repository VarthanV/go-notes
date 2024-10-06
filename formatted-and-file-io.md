# Formatted and file IO

## Standard I/O

- Unix has notion of three standard I/O streams

- They are open by default in every program

- Most programming languages have followed this convention
    - Standard Input
    - Standard output
    - Standard error (output)

- These are normally mapped to console/terminal but can be redirect


## Formatted I/O

- Acheived using fmt package to do IO

- By default we print to standard output

```go

package main

import(
    "fmt"
    "os"
)

func main(){
    fmt.Println("to std out") // to std out
    fmt.Fprintln(os.Stderr , "erorr out") // print to stderr
}

```

## Family of functions

```go

// Always os.stdout

fmt.Println()
fmt.Printf()

// Print to anything thas Write() method

fmt.Fprintln(io.Writer, ...inteface{})
fmt.Fprintf(io.Writer)

// return a string

fmt.Sprintln(...interface{})string
fmt.Sprintf()

```

## Format codes


|Code| Usage  |
|-----------|--------|
| %s | uninterpreted bytes of string or slice|
|%q| a double quote string safely escaped with go|
|%c|the character represented by corresponding Unicode code point|
|%d|base 10|
|%x|base 16 with lowe case letters for a-f|
|%f|decimal point with no exponent , eg 123.456|
|%t| the word true or false|
|%v| the value in default format when printing structs the plus flag (+%v) add fields name
|%#v| Go representation of value
|%T| Go syntax rep of a type value|
| %%| Literal percent sign ; consumes no value|

`

# File I/O

- Package os has functions to open or create files list dirs etc and hosts the File type

- Package io has utils to read and write , bufio provides buffered io/scanners.

- package ``strconv`` has utils convert to/from string representations.

