# Go from OOPS point of view

## Primer

**Virtual method** : Methods can be overriden/re-implemented by subclass

**Non Virtual Method**: Methods cannot be overridden by sublcass

```c#
using System;

class Animal
{
    // Virtual method can be overridden in derived classes
    public virtual void Speak()
    {
        Console.WriteLine("Animal speaks.");
    }

    // Non-virtual method cannot be overridden
    public void Eat()
    {
        Console.WriteLine("Animal eats.");
    }
}

class Dog : Animal
{
    // Overriding the virtual method
    public override void Speak()
    {
        Console.WriteLine("Dog barks.");
    }

    // Attempting to override non-virtual method won't change its behavior
    public new void Eat()  // This hides the base class method, but doesn't override it
    {
        Console.WriteLine("Dog eats.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Animal myAnimal = new Dog();

        // Polymorphism: Calls Dog's version because Speak is virtual
        myAnimal.Speak();  // Output: Dog barks.

        // Calls Animal's version because Eat is non-virtual
        myAnimal.Eat();    // Output: Animal eats.
    }
}

```

## Cheatsheet

| Golang |Struct|
|---|---|
| struct  | class with fields, only non-virtual methods  |
| interface | class without fields only virutal methods|
| embedding| multiple ineritance and composition| 
| receiver| implicit this parameter|

**Virtual Method:** A virtual method in object-oriented programming is a method in a base class that can be overridden in derived classes

## Golang struct is a class (non-virtual)

- A ***golang-struct*** is a class with ***fields*** where all methods are non virtual 

```go
type Rectangle struct {
    Name string
    Width, Height float64
}

func (r Rectangle) Area() float64 {
    return r.Width * r.Height
}
```

Pseudocode wrt class
```
class Rectangle
    field Name: string
    field Width: float64
    field Height: float64
    method Area() 
        return this.Width * this.Height
```

## Constructor

- There is a ***zero*** value defined for each type , so if not provided at instantiation , all fields will have the zero value

Example

```go
package main
import ."fmt"

type Rectangle struct {
    Name string
    Width, Height float64
}

func main() {
	var a Rectangle
	var b = Rectangle{"I'm b.", 10, 20}
	var c = Rectangle{Height: 12, Width: 14}

	Println(a)
	Println(b)
	Println(c)
}

```

**Output**

```
{ 0 0}
{I'm b. 10 20}
{ 14 12}
```

## Golang "embedding" is akin to multiple inheritance with non-virtual methods

- By embedding a struct into another we have mechanism similar to multiple inheritance with non-virtual members.

- Let's call "base" the struct embedded and "derived" the struct doing the embedding.

- After embedding the base fields and methods are directly available in dervved struct. Internally a hidden field is created named as **base-struct-name**.

- Base **fields and methods** are directly available as if they were declared in the struct , but the fields and methods can be **shadowed**

- **Shadowing** means defining another field or method with the same name (and signature) of a base field or the method

- Once shadowed, the only way to access the base member is to use to hidden field named as base-struct-name

```go
type base struct {
	a string
	b int
}

type derived struct {
	base // embedding
	d    int
	a    float32 //-SHADOWS
}

func main() {
	var x derived

	fmt.Printf("%T\n", x.a) //=> x.a, float32 (derived.a shadows base.a)

	fmt.Printf("%T\n", x.base.a) //=> x.base.a, string (accessing shadowed member)
}

```
- All base members can be accessed via the hidden field named as the **base-struct-name**

- All inherited methods are called on the **hidden field struct** named as **base-struct-name**.

- A base method cannot see or know about derived methods or fields. Everything is non-virtual.

- When working with structs and embedding, everything is STATICALLY LINKED. All references are resolved at compile time.

## Multiple Embedding

```go

type NamedObj struct {
    Name string
}

type Shape struct {
    NamedObj // embedding 
    color int32
    isRegular bool
}

type Point struct {
    x,y float64
}

type Rectangle struct {
    NamedObj
    Shape 
    center Point // standard composition
    Width , Height float64
}

func main(){
    var aRect = Rectangle{
		NamedObj{"name1"},
		Shape{NamedObj{"name2"}, 0, true},
		Point{0, 0},
		20, 2.5
	}

	fmt.Println(aRect.Name)
	fmt.Println(aRect.Shape)
	fmt.Println(aRect.Shape.Name)
}

```
In var aRect Rectangle:

- ```aRect.Name``` and ```aRect.NamedObj.Name``` refer to the same field

- ```aRect.color``` and ```aRect.Shape.color``` refer to the same field

- ```aRect.name``` and ```aRect.NamedObj.name``` refer to the same field, but ```aRect.NamedObj.name``` and ```aRect.Shape.NamedObj.name``` are different fields

- ```aRect.NamedObj``` and ```aRect.Shape.NamedObj``` are the same type, but refer to different objects

## Method shadowing

- Since all **golang-struct** methods are **non-virtual** , we cannot override methods (interfaces are needed for that).

- If we have a method ```show()``` for example in struct ```NamedObj``` and also define a method ```show()``` in struct ```Rectangle```, ```Rectangle/sho()``` will SHADOW the parent's struct ```NamedObj/Show()```

```go

type base struct {
	a string
	b int
}

//method xyz
func (this base) xyz() {
	fmt.Println("xyz, a is:", this.a)
}

//method display
func (this base) display() {
	fmt.Println("base, a is:", this.a)
}

type derived struct {
	base // embedding
	d    int
	a    float32 //-SHADOWED
}

//method display -SHADOWED
func (this derived) display() {
	fmt.Println("derived a is:", this.a)
}

func main() {
	var a derived = derived{base{"base-a", 10}, 20, 2.5}

	a.display() // calls Derived/display(a)
	// => "derived, a is: 2.5"

	a.base.display() // calls Base/display(a.base), the base implementation
	// => "base, a is: base-a"

	a.xyz() // "xyz" was not shadowed, calls Base/xyz(a.base)
	// => "xyz, a is: base-a"
}
```

## Multiple inheritance

- Since **inheritance (embedded fields)** include inherited field names in **inheriting class (struct)** all embedded fieldnames should not collide, The fields must be renamed if there is a collision. This rules avoids the diamond problem by not allowing it

- Golang allows you create a **Diamond** inheritance and will complaint only when try to access a parent's class field amibgously.

## Golang methods and "receivers" (this)

- It is defined outside of the struct body.

- Since it is outside the class , it has an extra setion before the method name to define the receiver (this).

- The extra section defines **this** as an **explicit param**.

- Since there is special section to define **this(receiver)** , we can also name for the this , in idiomatic golang we use short name of the struct

```go
//class NamedObj
type NamedObj struct {
	Name string
}

//method show
func (n NamedObj) show() {
	Println(n.Name) // "n" is "this"
}

//class Rectangle
type Rectangle struct {
	NamedObj      //inheritance
	Width, Height float64
}

//override method show
func (r Rectangle) show() {
	Println("Rectangle ", r.Name) // "r" is "this"
}

```

## Structs vs interfaces

- A golang-interface is a class with no fields and only VIRTUAL methods

- The inteface in Golang is designed to complement structs. This is very important relationship 
in golang **Interface fits perfectly with structs**

```
Structs: classes, with fields, All NON-VIRTUAL methods

Intefaces: classes , with NO fields, ALL VIRTUAL methods
```

## Interfaces

- A golang interface is a class with Nofields and all virtual
methods

Given this definition use an interface to

- Declare a var or param of type interface

- Implement an interface , by declaring all interface
virtual methods in a struct

- Embed golang interface into other interface

-  a pointer type can access the methods of its associated value type

- create abstractions by considering the functionality that is common between datatypes, instead of the fields that are common between datatypes
an interface{} value is not of any type; it is of interface{} type

- interfaces are two words wide; schematically they look like (type, value)
it is better to accept an interface{} value than it is to return an interface{} value

- a pointer type may call the methods of its associated value type, but not vice versa
- everything is pass by value, even the receiver of a method

- an interface value isn't strictly a pointer or not a pointer, it's just an interface

- if you need to completely overwrite a value inside of a method, use the * operator to manually dereference a pointer

```go
package main

import (
	"fmt"
)

/*
class Animal
   virtual abstract Speak() string
*/
type Animal interface {
	Speak() string
}

/*
class Dog
  method Speak() string //non-virtual
     return "Woof!"
*/
type Dog struct {
}

func (d Dog) Speak() string {
	return "Woof!"
}

/*
class Cat
  method Speak() string //non-virtual
     return "Meow!"
*/
type Cat struct {
}

func (c Cat) Speak() string {
	return "Meow!"
}

/*
class Llama
  method Speak() string //non-virtual
     return "LaLLamaQueLLama!"
*/
type Llama struct {
}

func (l Llama) Speak() string {
	return "LaLLamaQueLLama!"
}

/*
func main
  var animals = [ Dog{}, Cat{}, Llama{} ]
  for each animal in animals
     print animal.Speak() // method dispatch via jmp-table
*/

func main() {
	animals := []Animal{Dog{}, Cat{}, Llama{}}
	for _, animal := range animals {
		fmt.Println(animal.Speak()) // method dispatch via jmp-table
	}
}

```

## Empty interface

- B