# Go does OOP

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
