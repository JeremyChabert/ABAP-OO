# Advanced inheritance concepts

## Static type and dynamic type

The static type of a reference variable is determined by defining the variable using **TYPE REF TO**. 
It cannot and should not be modified. It establishes the attributes and methods that can be called.

The dynamic type of a reference variable is the type of the instance it is currently referring to. It is, therefore, determined by an assignment and may change during program execution. 
It establishes the source code to be executed for redefined methods.

**Remember this**
- **Static**: Types checked before run-time
- **Dynamic**: Types checked on the fly, during execution
Rule of :thumbsup: : **The static type is always more general than or the same as the dynamic type.**

```
DATA : lo_flight TYPE REF TO lcl_flight,
       "lo_flight static type => lcl_flight.
       lo_cargo    TYPE REF TO lcl_cargo.
       "lo_cargo static type => lcl_cargo.
CREATE OBJECT lo_cargo.

lo_flight = lo_cargo.
"lo_flight dynamic type => lcl_cargo.

```

Static and dynamic type allow us to introduce :exclamation: **3 IMPORTANT** :exclamation: keywords:
1. Upcast
2. Downcast
3. Polymorphism :cat2: :leopard: :tiger2:

## Upcast
**In object-oriented programming, upcasting is the act of casting a reference of a sub-class to one of its super class(es).**
```
DATA : lo_flight TYPE REF TO lcl_flight,
       "lo_flight static type => lcl_flight.
       lo_cargo    TYPE REF TO lcl_cargo.
       "lo_cargo static type => lcl_cargo.
CREATE OBJECT lo_cargo.

lo_flight = lo_cargo. "child (cargo) to parent (flight) type refinement.
```
## Downcast
**In object-oriented programming, downcasting is the act of casting a reference of a base class to one of its derived class(es).**
```
DATA : lo_flight TYPE REF TO lcl_flight,
       "lo_flight static type => lcl_flight.
       lo_cargo    TYPE REF TO lcl_cargo.
       "lo_cargo static type => lcl_cargo.
CREATE OBJECT lo_cargo.

lo_flight = lo_cargo.
lo_cargo ?= lo_flight  "parent (flight) to child (cargo) type refinement.
" Note that this operation has to be explicited using '?' which specifies that we know this operation can be perform
```

Let's suppose this now 
```
DATA : lo_flight TYPE REF TO lcl_flight,
       "lo_flight static type => lcl_flight.
       lo_cargo    TYPE REF TO lcl_cargo.
       "lo_cargo static type => lcl_cargo.
CREATE OBJECT lo_cargo.

lo_cargo ?= lo_flight  "parent (flight) to child (cargo) type refinement.
```
This will trigger a system error *MOVE_CAST_ERROR*

:question: Why :question:

## Polymorphism

When objects of several classes **react differently to the same method call**, we speak of **POLYMORPHISM**. 

This happens when classes implement the same method differently using inheritance then redefining a method from the superclass in subclasses.

Suppose the super-class LCL_FELINAE and the sub-classes LCL_ACINONYX, LCL_FELIS

:cat2: lo_felis->speak( ) => MIAOW 

:leopard: lcl_acinonyx->speak( ) => GROAARR 

When an instance receives an instruction to execute a specific method, this method is executed if it has been implemented by the class to which the instance belongs.

If this class has only inherited it without redefining it, then a search :arrow_up: is launched within the inheritance hierarchy to find the implementation of this method.

From a technical point of view, **the dynamic type of the reference variable is used to search for the implementation of a method**, unlike the static type,

## Downcast, upcast, polymorphism getting real

```
DATA : lo_cargo TYPE REF TO lcl_cargo_airplane,
       lo_jet   TYPE REF TO lcl_airplane_jet,
       lo_airplane_passenger TYPE REF TO lcl_airplane_passenger,
       lt_plane_list TYPE TABLE OF REF TO lcl_flight.
       
CREATE OBJECT lo_cargo. => address RAER3432x3454
CREATE OBJECT lo_jet.   => address ZARVDVDEEx4361
CREATE OBJECT lo_airplane_passenger. => address BC53AVBT434x0874

APPEND: lo_cargo TO lt_plane_list,     "=> equivalent to an upcast
        lo_airplane_passenger TO lt_plane_list,
        lo_jet TO lt_plane_list.
        

LOOP AT lt_plane_list TO DATA(lo_flight).   "=> equivalent to downcast ?=
  lo_flight->estimate_fuel_consumption( ).    "=> polymorphism
  lo_flight->take_off_procedure( ).
ENDLOOP.
```
 
## Abstraction
 
### Abstraction of a class
 
```
CLASS lcl_flight DEFINITION ABSTRACT.
```
 
_AN ABSTRACT CLASS **CANNOT** HAVE INSTANCE_. 
 
References to such classes are used to refer to instances of subclasses of the abstract class at run time. 
 
The CREATE OBJECT statement has an additional option. You can specify the class of the instance explicitly.
 
```
 CREATE OBJECT <refClassAbstraite> TYPE <SubClassNonAbstraite>
 
 DATA : lcl_flight TYPE REF TO lcl_flight.
 CREATE OBJECT lo_flight. "==> Would be wrong has AN ABSTRACT CLASS **CANNOT** HAVE INSTANCE_. 
 
 CREATE OBJECT lo_flight TYPE lcl_airplane_passenger. "==> Correct !
```
 
:question: Why does abstract class even exists :question:
 
Abstract classes are generally used as an incomplete structure for concrete subclasses to define a uniform interface. 
 
In our example, we don’t want any instance on lcl_flight as we will only define our flights as either “Cargo” or “Passenger”. 
 
It’s then irrelevant to define an instance of the super-class.
 
### Abstraction of a method
  
Let's take LCL_FELINAE, the Felinae definition is too vague to implement the method SPEAK. 
 
A Felinae shall be of its sub-class type such as Cats or Leopards to be have a precise way of speaking (MIAOW or GROARRR)
 
- **Classes with at least one abstract method are necessarily abstract.**
- **Static methods and constructors cannot be abstracted (they cannot be redefined).**

## Finalization
 
### Finalization of a class
 
 
### Finalization of a method
