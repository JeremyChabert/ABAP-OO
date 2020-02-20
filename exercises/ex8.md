So far, we have seen methods, attributes, inheritance, downcast, upcast, abstraction and finalization.

Next is **interface**.

Still using our training report, we will declare our first interface.

Start with removing ```declare_take_off``` from ```LCL_FLIGHT```

```
INTERFACE lif_cockpit.

    METHODS: declare_take_off,
             declare_landing,
             initiate_take_off_seq.
  
ENDINTERFACE.
```

Now let's declare that ```LCL_CARGO``` is implementing our new interface.

```
CLASS lcl_cargo DEFINITION INHERITING FROM lcl_flight.
  PUBLIC SECTION.
    [...]
    INTERFACES : lif_cockpit.
    [...]
ENDCLASS.
```
:bulb: compile. You should get an error. Why ?

Correct! Now that you said that ```LCL_CARGO``` implements lif_cockpit, you have to implement the method also. 

Remember ? An interface is an abstract class with all its methods abstracts. It means that it needs to be redefined in the inheriting/implementing classes.

```
CLASS lcl_cargo IMPLEMENTATION.
[...]
  METHOD lif_cockpit~declare_landing.
    WRITE:/ 'Cargo flight' && mv_plane_number && 'has landed'.
  ENDMETHOD.

  METHOD lif_cockpit~declare_take_off.
    WRITE:/ 'Cargo flight' && mv_plane_number && 'taking off'.
  ENDMETHOD.

  METHOD lif_cockpit~initiate_take_off_seq.
    lif_cockpit~declare_take_off( ).
    take_off( ).
  ENDMETHOD.
[...]
ENDCLASS.
```

Now proceed the same with LCL_AIRPLANE.

```
CLASS lcl_airplane IMPLEMENTATION.
[...]
  METHOD lif_cockpit~declare_landing.
    WRITE:/ 'Passengers flight' && mv_plane_number && 'has landed'.
  ENDMETHOD.

  METHOD lif_cockpit~declare_take_off.
    WRITE:/ 'Passengers flight' && mv_plane_number && 'taking off'.
  ENDMETHOD.

  METHOD lif_cockpit~initiate_take_off_seq.
    lif_cockpit~declare_take_off( ).
	"give_safety_speech( ).
	"reassure_passengers( ).
    take_off( ).
  ENDMETHOD.
[...]
ENDCLASS.
```

Another possibility would have been to do like this .
```
CLASS lcl_flight DEFINITION ABSTRACT.
    [...]
	INTERFACES : lif_cockpit ALL METHODS ABSTRACT .
	[...]
ENDCLASS.
```

```
CLASS lcl_cargo DEFINITION FINAL INHERITING FROM lcl_flight.
    [...]
	METHODS : 
	 [...]
      lif_cockpit~declare_landing REDEFINITION,
      lif_cockpit~declare_take_off REDEFINITION,
      lif_cockpit~initiate_take_off_seq REDEFINITION.
	[...]
ENDCLASS.
```
```
CLASS lcl_airplane DEFINITION FINAL INHERITING FROM lcl_flight.
    [...]
	METHODS : 
	 [...]
      lif_cockpit~declare_landing REDEFINITION,
      lif_cockpit~declare_take_off REDEFINITION,
      lif_cockpit~initiate_take_off_seq REDEFINITION.
	[...]
ENDCLASS.
```

We will declare a new method common to both ```LCL_CARGO``` and ```LCL_AIRPLANE```.

Where should we declare it ? 

So do it.

```
CLASS LCL_FLIGHT ABSTRACT DEFINITION.

	PUBLIC SECTION.
	[...]
	METHODS : leave_airport ABSTRACT.
	[...]
```
```
CLASS lcl_cargo DEFINITION [...]. 
[...]
  METHODS leave_airport REDEFINITION.
[...]
```	
```
CLASS lcl_cargo IMPLEMENTATION.
[...]
  METHOD leave_airport.
    lif_cockpit~initiate_take_off_seq( ).
  ENDMETHOD.
[...]
```	
