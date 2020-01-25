# Content

- Instantiation
- Re-assignment
- Memory address
- Garbage collector

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
