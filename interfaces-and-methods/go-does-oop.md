## OOPS

The essential element of OOPS is

- Abstraction
- Encapsulation
- Polymorphism
- Inheritance

## Abstraction

Decoupling behaviour from implementation details

The Unix filesystem is greate example of Abstraction

Roughly five basic functions hide all messy inner details

- open
- close
- read
- write
- ioctl

Many different OS things can be treated like files (Sockets etc)


## Encapsulation

Hiding implementation details from misuse

- It's hard to maintatin abstraction for details exposed

- The internals may be manipulated in the ways contrary to concepts behind abstraction. 

- User's of abstraction may come to depend on internal details but those might change.

Eg: Controlling visiblity of names using private variables

## Polymorphism

Polymorphism means literally many shapes (multiple types) behind single interface

Three main types

- **ad hoc**: Found in function/operator overloading.

- **parametric**: commonly known as generic programming (Generics)

- **subtpye**: subclasses substituting for superclasess.

Protocol oriented programming uses explicit interface types 

Behavior is completely seperate from implementation good for abstraction

Go resists this
- No inheritance

- No subtype

It insted emphazises interfaces.

Go interfaces are small

```go
type Stringer interface {
    String()string
}
```
- A ``Stringer`` can pretty print itself , Anything that implements ``String`` is a ``Stringer``

## Wrapping interfaces

```go
type LogReader struct {
    io.Reader
}

func (r LogReader) Read(b []byte) (int,error){
    //
}

// Wrapping bytereader with LogReader

r := LogReader{ByteReader('a')}
b := make([]byte, 10)
r.Read(b)
```

By wrapping we compose interface values

## Further Reading

- [Inheritance is evil](https://codeburst.io/inheritance-is-evil-stop-using-it-6c4f1caf5117)

- [Inheritance Tax](https://media.pragprog.com/titles/tpp20/inheritance-tax.pdf)