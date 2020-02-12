# Give it a state

## Attributes declaration

Using your local class, let's declare the following attributes :

```
plane_number    | instance attribute  | public  | string
nb_places 	| static attribute    | public  | number
manufacturer	| constant| public    | string  | 'Airbus' 

serial_number    | instance attribute  | private | string
cockpit_firmware | static attribute    | private | string
manufacturer_key | constant	       | private | string  | 'AIB_FAL350'
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

Now your class should look like this
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
