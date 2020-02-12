# Give it a state

## Attributes declaration

Using your local class, let's declare the following attributes :

```
company		| instance attribute  | public  | string
plane_number    | instance attribute  | public  | string
nb_seats 	| static attribute    | public  | number
nb_passengers	| instance attribute  | public	| number
manufacturer	| constant| public    | string  | 'Airbus'

serial_number    | instance attribute   | private | string
cockpit_firmware | static attribute   	| private | string
manufacturer_key | constant	      	| private | string  | 'AIB_FAL350'
nb_of_flight     | ?    		| ? 	  | ?
on_ground        | ?    		| ? 	  | ?
```

Instance attributes are declare using keyword **DATA**
Static attributes are declare using keyword **ClASS-DATA**
Constant attributes are declare using keyword **CONSTANTS**

```
CLASS lcl_flight DEFINITION.

  PUBLIC SECTION.

    DATA : mv_plane_number type string.
    CLASS-DATA: mv_nb_of_planes type i.
    CONSTANTS: mv_manufacturer type string value 'Airbus'.
	
  PRIVATE SECTION.

ENDCLASS
```

Repeat process to complete the class definition with the private attributes defined at the beginning of this exercise.

Now, your class should look like this:

```
CLASS lcl_flight DEFINITION.

  PUBLIC SECTION.

    DATA : mv_plane_number TYPE string.
    CLASS-DATA: mv_nb_of_planes TYPE i.
    CONSTANTS: c_manufacturer TYPE string VALUE 'Airbus'.
	
  PRIVATE SECTION.
  
    DATA : mv_serial_number TYPE string.
    CLASS-DATA: mv_cockpit_firmware TYPE string.
    CONSTANTS: c_manufacturer_key TYPE string VALUE 'AIB_FAL350'.

ENDCLASS
```

# Give it a behavior

Still using your local class, let's declare the following behaviors :

- constructor
```
CLASS lcl_flight DEFINITION.
	PUBLIC SECTION.
	[...]
	constructor IMPORTING iv_plane_number TYPE i
			      iv_company TYPE string.
ENDCLASS.
```

- get_free_seats  | instance method | public
```
CLASS lcl_flight DEFINITION.
	[...]
	METHODS: get_free_seats RETURNING VALUE(rv_nb_seats) TYPE i.
ENDCLASS.
CLASS lcl_flight IMPLEMENTATION.
	METHOD get_free_seats.
		rv_nb_seats = mv_nb_seats - nb_passengers.
	ENDMETHOD.
ENDCLASS.
```

- add_flight	| static method  | public
```
CLASS lcl_flight DEFINITION.
	[...]
	CLASS-METHODS: add_flight IMPORTING  	iv_plane_number TYPE i
 						iv_company TYPE string
				  RETURNING VALUE(ro_flight) TYPE REF TO lcl_flight.
ENDCLASS.
CLASS lcl_flight IMPLEMENTATION.
	METHOD add_flight.
		ro_flight = NEW lcl_flight( iv_plane_number = iv_plane_number
					    iv_company = iv_company ).
		ADD 1 to mv_nb_flight.
	ENDMETHOD.
ENDCLASS.
```

- add_passengers 	| instance method | public
```
CLASS lcl_flight DEFINITION.
	[...]
	METHODS: add_passengers IMPORTING iv_passengers TYPE i.
ENDCLASS.
CLASS lcl_flight IMPLEMENTATION.
	METHOD add_passengers.
		ADD  iv_passengers TO mv_nb_passengers.
	ENDMETHOD.
ENDCLASS.
```

- declare_take_off | instance method | private
```
CLASS lcl_flight DEFINITION.
	[...]
	METHODS: declare_take_off.
ENDCLASS.
CLASS lcl_flight IMPLEMENTATION.
	METHOD declare_take_off.
		WRITE: 'Flight' && mv_plane_number && 'taking off'.
	ENDMETHOD.
ENDCLASS.
```

- take_off	| instance method | public
```
CLASS lcl_flight DEFINITION.
	[...]
	METHODS: take_off.
ENDCLASS.
CLASS lcl_flight IMPLEMENTATION.
	METHOD take_off.
		mv_on_ground = abap_false.
		me->declare_take_off( ).
		"OR
		declare_take_off( ).
		"OR
		CALL METHOD declare_take_off( ).
	ENDMETHOD.
ENDCLASS.
```

- get_nb_flights | static method | public
```
CLASS lcl_flight DEFINITION.
	[...]
	CLASS-METHODS: get_nb_flights RETURNING VALUE(rv_nb_flights) TYPE i.
ENDCLASS.
CLASS lcl_flight IMPLEMENTATION.
	METHOD get_nb_flights.
		rv_nb_flights = mv_nb_flights.
	ENDMETHOD.
ENDCLASS.
```
