# ABAP-OO

# :construction: Under construction :construction:

## Forewords
Hello everyone, welcome to ABAP-OO training. I hope you are all set for 2 days or more of fun learning about oriented object programming.

In this training, all the concepts introduced are related to the ABAP-OO programming. This means that some of the concepts introduced are only true in SAP context development.

However, as we enter the paradigm of oriented object programming, for those with some knowledge about it, you'll find a lot of similarities with what other languages are using.

Let's start, shall we ?
## Concepts

### Class
A class is the generic word that will be used to describe the skeletton of an object of the said class.

A class defines **ATTRIBUTES** and **BEHAVIORS**.

A set of **VALUATED ATTRIBUTES** defines a **STATE**.

The **BEHAVIOR** _alters/change_ this **STATE**.

A **BEHAVIOR** is equivalent to saying **METHOD**. Let's assume that both wording will be used now.

See the class as the ***highest level word to describe*** what will be introduced next

Now let's dig deeper into what's beyond that generic word.

An ABAP class is composed of 2 parts.
- Definition 
- Implementation

![Class definition implementation](img/Class_Definition_Implementation.PNG)

### Definition

-	Definition section will be the part where we’ll declare the collection of attributes and behaviors.
-	We will also specify the **VISIBILITY** of each attributes and behaviors.

But now, let's just start slowly and focus on this code.
```
CLASS lcl_flight DEFINITION.


ENDCLASS.
```

Pretty understandable syntax, right ?

Let's take a moment to discuss what it says.

This declares a local class (**L**CL_flight) named lcl_flight. [See naming conventions](NamingConventions.md).

All the code that will be declared between the two keywords **CLASS** and **ENDCLASS** will belongs to LCL_FLIGHT.

### Implementation

- Remember that in definition, we said that it defines the collection of behaviors.
- Defining here only resumes to listing the possible behaviors.
- To give a real body to the behavior, we need to implement it, code it.

**Implementation** is where all the coding is done.

Can you guess the syntax ? Pretty dull, huh ?

```
CLASS lcl_flight IMPLEMENTATION.


ENDCLASS.
```

Let's move on to [Exercice 1](ex1/ex1.md).
