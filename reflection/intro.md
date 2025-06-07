# Reflection

- Reflection is the ability of the program to inspect and modify its own type in the runtime.

- It is done using the ``reflect`` package of Golang.


## Key Types in reflect package

- ``reflect.Type ` -  Information about the type.

- ``reflect.Value `` -  Information about the value.


```go
import (
	"fmt"
	"reflect"
)

func main() {
	x := 42
	t := reflect.TypeOf(x)   // reflect.Type
	v := reflect.ValueOf(x)  // reflect.Value

	fmt.Println("Type:", t)
	fmt.Println("Value:", v)
}
```

## reflect.Type

- ``TypeOf()`` returns the type of a value.

```go
 t := reflect.TypeOf("hello")
fmt.Println(t.Kind()) // string``
```


## TypeOf vs Kind

```go
func typeAndKindDifference(){
      p := person{Name:"John"}
      typeOfP := reflect.TypeOf(p) 
      kindOfP := typeOfP.Kind()
      fmt.Println("Type ",typeOfP)  
      fmt.Println("Kind ",kindOfP)
  
}
/*
    Type  main.person
    Kind  struct
*/
```

In Go, `reflect.TypeOf` and `.Kind()` are both used in reflection to inspect the type information of a value, but they serve **different purposes**:

---
- Type represents the actual Type

- Kind is of like underlying type (string, int or float64)

### 🔹 `reflect.TypeOf()`

* **Purpose**: Returns the **concrete type** of the value.
* **Returns**: `reflect.Type`
* Think of it like the **name** of the type (e.g., `int`, `[]string`, `MyStruct`, `*MyStruct`)

```go
t := reflect.TypeOf(42)
fmt.Println(t)         // int
fmt.Println(t.Name())  // int
```

---

### 🔹 `Kind()`

* **Purpose**: Returns the **underlying kind/category** of the type.
* **Returns**: `reflect.Kind` (an enum)
* Think of it like the **class** of the type (e.g., `slice`, `struct`, `ptr`, `int`, etc.)

```go
t := reflect.TypeOf([]string{"hello"})
fmt.Println(t.Kind()) // slice
```

---

### 🔸 Example: Difference in Action

```go
type MyInt int

func main() {
    var x MyInt = 5
    t := reflect.TypeOf(x)

    fmt.Println("Type:", t)        // MyInt
    fmt.Println("Name:", t.Name())// MyInt
    fmt.Println("Kind:", t.Kind())// int
}
```

* `Type`: `MyInt` (the custom type name)
* `Kind`: `int` (the base type category)

---

### 🔸 Summary Table

| Aspect         | `TypeOf()`                          | `.Kind()`                                  |
| -------------- | ----------------------------------- | ------------------------------------------ |
| Purpose        | Gets the exact, declared type       | Gets the underlying category               |
| Return type    | `reflect.Type`                      | `reflect.Kind`                             |
| Example Output | `MyStruct`, `[]string`, `*int`      | `struct`, `slice`, `ptr`, `int`            |
| Use case       | Type name comparison, method checks | Switch-case over kind, type generalization |

---

Let me know if you want a visual or to explore how to use this in generic logic or JSON unmarshalling!


## reflect.Value

- ``ValueOf()`` of returns the value and we can do the following
    - Access ``.Int(), .String(), .Float()`` 
    - Can modify (if addressable) ``.Set()``
```go
    v := reflect.ValueOf(42)
    fmt.Println(v.Int()) // 42
```

## Accessing Struct Fields

```go
type Person struct {
	Name string
	Age  int
}

p := Person{"John", 30}
v := reflect.ValueOf(p)
t := reflect.TypeOf(p)

for i := 0; i < t.NumField(); i++ {
	fmt.Println(t.Field(i).Name, v.Field(i))
}

```

## Modifying values (Need pointer)

```go
x := 10
    v := reflect.ValueOf(&x).Elem()
    v.SetInt(20)
    fmt.Println(x) // 20
```

## CanSet and CanAddr
- ``CanSet()`` checks if the value is settable.

- ``CanAddr()`` checks if the value can be addressed (like a variable).


## Gotchas

- Reflection can panic if misused (e.g., calling .Int() on a string).

- Avoid excessive reflection — it's best used for:
    - Marshalling (e.g., JSON)
    - Writing generic libraries
    - Framework development


