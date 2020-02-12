# Interfaces

In ABAP OO, interfaces are implemented independently and in addition to classes

An interface describe only the external parameters of a class, it does not contain
specific implementation.

You may already know or work with interfaces such as Badi.

## Syntax

```
INTERFACE lif_cockpit.
	METHODS : print, display.
ENDINTERFACE.
```

A class does not inherit from an interface, it **IMPLEMENTS** it.
```
CLASS lcl_flight DEFINITION.
	PUBLIC SECTION.
		INTERFACES: lif_cockpit.
		METHODS: display.
ENDCLASS.
```

```
CLASS lcl_flight IMPLEMENTATION.
	METHOD lif_cockpit~display.
	
	ENDMETHOD.
	METHOD lif_cockpit~display.
	
	ENDMETHOD.
	METHOD display.
	
	ENDMETHOD.
ENDCLASS.
```

## Characteristics

What would be the section in which you are declaring your interface ?

What is the visibility of an interface ?

How is a method of an interface has to be called ?

What extension of the class concept does an interface correspond to ?

## Use of an Interface

```
"REPORT REFERENTIAL

DATA : lo_flight TYPE REF TO lcl_flight.

CREATE OBJECT lo_flight TYPE lcl_airplane_passenger.

CALL METHOD lo_flight->lif_cockpit~display( ).

*or with new typo that you should use
lo_flight->lif_cockpit~print( ).
```

## Downcast

### Downcast with interface
```
DATA : 	i_cockpit TYPE REF TO lif_cockpit,
	lo_flight TYPE REF TO lcl_flight,
	lot_flight TYPE TABLE REF TO lcl_flight.
		
CREATE OBJECT lo_flight TYPE lcl_airplane_passenger.
APPEND lo_flight TO lot_flight.

CREATE OBJECT lo_flight TYPE lcl_postal_cargo.
APPEND lo_flight TO lot_flight.

LOOP AT lot_flight INTO lo_flight.
	i_cockpit = lo_flight.
	i_cockpit->display( ). "<=> lo_flight->lif_cockpit~display( ).
ENDLOOP.
```
