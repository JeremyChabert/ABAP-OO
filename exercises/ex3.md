# Moving closer to a real scenario

Let's comment the code previously typed !
```
↑↑↑↑
"class definition & implementation above

END-OF-SELECTION.

*DATA : lo_flight TYPE REF TO lcl_flight.
*
*CREATE OBJECT lo_flight 
*	EXPORTING iv_plane_number = '354'
*		  iv_company	  = 'SIA'.
*		  
*lo_flight->add_passengers( 10 ).
*lo_flight->take_off( ).
*
*DATA(lo_flight2) = lcl_flight=>add_flight( 
*			iv_plane_number = '9443'
*			iv_company	  = 'AFR').
*			
*lo_flight->add_passengers( 56 ).
*lcl_flight=>get_nb_flights( ).
*WRITE:/ lcl_flight=>get_nb_flights( ).

```

Now type this below it:

```
↑↑↑↑
"class definition & implementation above

END-OF-SELECTION.

DATA : lot_flight TYPE TABLE OF REF TO lcl_flight.

DATA(lo_flight) = lcl_flight=>add_flight( 
			iv_plane_number = '1234'
			iv_company	  = 'SIA').

APPEND lo_flight TO lot_flight.

lo_flight = lcl_flight=>add_flight( 
			iv_plane_number = '9443'
			iv_company	  = 'AFR').

APPEND lo_flight TO lot_flight.

lo_flight = lcl_flight=>add_flight( 
			iv_plane_number = '5434'
			iv_company	  = 'LFH').

APPEND lo_flight TO lot_flight.

LOOP AT lot_flight INTO lo_flight.
	DATA(lv_passengers) = sy-tabix * 35.
	WRITE:/ lo_flight->get_free_seats( ).
	lo_flight->add_passengers( lv_passengers ).
    WRITE:/ lo_flight->get_free_seats( ).
	lo_flight->take_off( ).
ENDLOOP.
```
:bulb: Compile (```CTRL + F3```)

Run the report (```F8```)

What is the number of passengers in flight 1 ? flight 2 ? flight 3 ?

What mecanism did you use to handle several instances ?

Why didn't you lose any instance with the garbage collector ?
