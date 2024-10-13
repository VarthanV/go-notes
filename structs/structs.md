# Structs

- Struct is aggregate of different types of fields

- Each struct has a field

```go

type Employee struct{
    Name string
    Number int
    Boss *Employee
    Hired time.Time
}

func main(){
    var e Employee
    fmt.Printf("%T %+[1]v\n ",e) // Gives output with field names

}
```
- A type can refer to itself as long as it is done through pointer

```go

type Employee struct{
    Name string
    Number int
    Boss Employee // WRONG
    Hired time.Time
}

```

- When initializing a map with a key and struct always intialize it as key and to struct pointer

- The reason is the map regarranges itself when key is deleted , it might not hold the value of actual struct but refers somewhere in memory when looked up the value is loaded from the address

```go
func main(){
    c := map[string]Employee{}

    c["L"] = Employee{"L",2, nil, time.Now()}
    c["L"].Number++ // throws an error
    fmt.Println(&c["L"]) // Address is not fixed

    // Best approach
    c := map[string]*Employee{}
}
```
- When declaring slice of structs best practices

```go
    var (
        a = []Employee{} // Better practice
        b = []*Employee{} // Do when absolute necessary
        c = *[]Employee{} // namakidhu venda da
    )
```

# Anonymous Structs

- Anonymous structs can be declared but not conveninent

```go
func main(){
    var album = struct{
        title string
        artist string
    }{
        "Why this Kolaveri",
        "Anirudh"
    }
}

var pAlbum = &album // Fine as well as we initialize it 
fmt.Println(album,pAlbum)

```

```go

type album1 struct {
    title string
}

type album2 struct{
    title string
}

func main (){
    var a1 = album1{
        "Gangster",
    }
    
    var a2 = album2 {
        "Foo",
    }

    a1 = a2 // Will not work , They are not same named types 
}
```
- The types are not equal since they are different names , but they are convertible

```go
    a1 = album1(a2)
```

## Struct compatibility

Two struct types are compatible if

- The fields have same types and names
- In same order
- and the same tags (*)

- A struct may be copied or passed as a parameter in its entirety

- A struct is comparable if all its fields are comparable

- The zero value for struct is zero for each field in turn (Zero value of struct == zero value of all of the fields)

- Byte slices are not comparible

Anonymous structs are compatible if they follow the rules

```go
func main(){
    v1 := struct {
        X int `json:"foo"`
    }{1}

    v2 := struct {
        X int `json:"foo"`
    }{2}

    v1 = v2
    fmt.Println(v1) // prints {2}
}

```

- Named struct types are convertible if they are compatible

```go
type T1 struct{
    X int `json:"foo"`
}

type T2 struct{
    X int `json:"bar"`
}

func main(){
    v1 := T1{1}
    v2 := T2{2}

    v1 = T1(v2) // type conversion
    fmt.Println(v1) // prints 2
}

```
 ## Structs are passed by value unless a pointer is used

 ```go
var white album

func soldAnother(a album){
    // Changed only locally
    a.copiess ++
}

func soldAnother(a* album){
    a.copies++
}

 ```

 - The dot notation works on pointers too , equivalent to ``(*a).copies``

 ## Make the zero value useful

 For example in ```bytes.Buffer``` the initial value of struct is ready to use empty buffer

 ```go
type Buffer struct {
    buff []byte
    off int
    lastRead readOP
}
 ```
 
 It has nil slice we can append to and off starts as o the zero value for readOp is opInvalid

 ## Empty structs

- A struct with no fields is useful it takes no space

```go
// a set type instead of bool
var isPresent map[int]struct{}

// a very cheap channel type
done := make(chan struct{})
```

## Struct tags have many uses

- Tags can also be used in conjuction with sql queries

```go
import "github.com/jmoiron/sqlx"

type item struct {
    Name string `db:"name"`
    When string `db:"created"`
}

func PutStats(db *sqlx.DB, item*item)error {
    stmt:= `
        INSERT into items(name,created) VALUES(:name,:created);
    `

    _ , err := db.NameExec(stmt,item)
    return err
}
```