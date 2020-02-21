Let's change the ```take_off``` method and declare it as an event.

```
CLASS lcl_flight DEFINITION.

    EVENTS : take_off EXPORTING VALUE(ev_plane_number) TYPE string.
  
```

Now we will create a new class to listen to the event and react to this event: ```lcl_controller```    

```  
CLASS lcl_controller DEFINITION.
  PUBLIC SECTION.
    METHODS : on_takeoff FOR EVENT take_off OF lcl_flight IMPORTING ev_plane_number sender.
ENDCLASS.
```

```
CLASS lcl_controller IMPLEMENTATION.
  METHOD on_takeoff.
    WRITE:/ 'Good flight to you' && ' ' && ev_plane_number.
 
  ENDMETHOD.
ENDCLASS.
```

In the report part, we will declare an instance on controller and we will make it listen to the flight's event.
```
DATA(lo_controller) = NEW lcl_controller( ).
```

Now let's see what happens if we run the following code.

```
LOOP AT lot_flights INTO DATA(lo_flight).
	SET HANDLER lo_controller->on_takeoff( ) FOR lo_flight ACTIVATION abap_true.
	lo_flight->leave_aiport( ).
ENDLOOP.
```

Now we will call the leave_airport method a two times in the same loop.

```
LOOP AT lot_flights INTO DATA(lo_flight).
	lo_flight->leave_aiport( ).
	lo_flight->leave_aiport( ).
ENDLOOP.
```

Are the flights leaving the airport a second time ? Why ?

Now, we will deactivate the listening after the event processing.
```
  METHOD on_takeoff.
    WRITE:/ 'Good flight to you' && ev_plane_number.
    SET HANDLER on_takeoff FOR sender ACTIVATION space.
  ENDMETHOD.
```

Let's run again our code.

Better ! Right ?
