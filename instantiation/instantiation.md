# Content

- Instantiation
- Memory address
- Garbage collector
- Re-assignment
- Namespace
- Delegation

# Instantiation
A class defines a generic set of attributes and behavior for any variable referencing this class.

An object can only be created and addressed using a **reference variable**. They correspond to pointers.
```
DATA : lo_flight TYPE REF TO lcl_flight
```
**Objects** are truly created when using **CREATE OBJECT** keyword in the calling system. 

After this statement (CREATE OBJECT), the calling system is dynamically asking for a memory address and assign the object to it.

This is **INSTANTIATION**.

```
DATA : lo_flight1 TYPE REF TO lcl_flight,
       lo_flight2 TYPE REF TO lcl_flight.
       
lo_flight1 === lo_flight2 ? => TRUE

CREATE OBJECT lo_flight1.
CREATE OBJECT lo_flight2.

lo_flight1 === lo_flight2 ? => FALSE
```

:question: Why ?

## Memory address/pointers

A create object statement allocates a unique **MEMORY ADDRESS**.

lo_flight1 and lo_flight2 have then 2 differents **POINTERS** to these **ADDRESSES**.

Even if they share the same state, they do not share the same address.

Let's call this **IDENTITY**

## Wording

So far we have seen **CLASS** 

We have also seen variable declared as ```TYPE REF TO```. It's a **REFERENCE VARIABLE**

A **reference variable** is exactly what it tells: a variable that is a reference to something, here a **CLASS**

Finally, with the **instantiation**, we provide an **IDENTITY** (through unique pointer) and a **STATE** to our **REFERENCE VARIABLE**

## Garbage collector

The Garbage collector **deletes from memory any object as soon as there are no longer any reference on it.** 

It’s a system routine that’s delete automatically these objects whenever it’s not possible to address from the memory. 

It frees memory space that hold these references.

## Practical use of reassignement

We just said that Garbage collector deletes any object that doesn't have any reference.

:question: How do we do manipulate several instances and keep the code concise without losing the memory space of an instance ?

=> **INTERNAL TABLE OF REFERENCE VARIABLE**

```
DATA : lto_flights TYPE TABLE REF TO lcl_flight.
```

```
DATA : lo_flight TYPE REF TO lcl_flight,
       lo_flight1 TYPE REF TO lcl_flight,
       lto_flight TYPE TABLE REF TO lcl_flight.

CREATE OBJECT lo_flight
               EXPORTING iv_name = 'SIA-001'.
APPEND lo_flight1 TO lto_flight.

CREATE OBJECT lo_flight1
               EXPORTING iv_name = 'AFR-001'.
APPEND lo_flight1 TO lto_flight.

LOOP AT lto_flight TO lo_flight.
       ...
       WRITE:\ lo_flight->mv_name.
       "first iter => 'SIA-001'.
       "second iter => 'AFR-001'.
ENDLOOP.
```
**REMEMBER THIS** Internal table of variable reference allows to manipulate several instances without losing any into the garbage collection

## Namespace

In a class, names of attributes, methods, events, constants, types, and aliases all share the same namespace.

Methods include a local namespace. Definitions of variables can cover the components of a class.

You can address the object itself from its own methods using the reference variable implicitly available: **ME**.

```
CLASS lcl_flight DEFINITION.
  PUBLIC SECTION.
    METHODS : constructor IMPORTING iv_name TYPE STRING.
  PRIVATE SECTION.
    DATA : name type STRING.
ENDCLASS.

CLASS lcl_flight IMPLEMENTATION.
   METHOD constructor.
       DATA name TYPE string value '-airplane'.
       CONCATENATE im_name name INTO name. 
   ENDMETHOD.
ENDCLASS.
```
```
[...]
 METHOD constructor.
    DATA name TYPE string value '-airplane'.
    CONCATENATE im_name name INTO ME->name. 
 ENDMETHOD.
[...]
```

## Delegation

The delegation supposed to have at least 2 objects involved in a treatment of a functionality.

![Delegation](../img/delegation.png)

The recipient of this functionality delegates the execution of it to a delegate.

The main benefit of delegation (as a reuse mechanism) is the option to change the behavior of the recipient by substituting delegation (at run-time). 

For example, the delegation allows the aircraft to be equipped with a new tank without altering the call for the flight class or each wing can receive different instruction that will be handle in the wing class

**Successful encapsulation often requires you to use delegation.**

