# Abstract and final

If you look at your code now, you'll see that estimate_fuel_consumption is redefined in both ```LCL_CARGO``` and ```LCL_AIRPLANE```.

If we take a moment to consider our code, it's irrelevant to have this method with a possible implemenation at ```LCL_FLIGHT``` level.

Let's fix this and declare this method as abstract.
```
METHODS: estimate_fuel_consumption ABSTRACT IMPORTING iv_distance TYPE i RETURNING VALUE(re_fuel) TYPE f .
```
and remove the method implementation at ```LCL_FLIGHT``` level

Oh and ```get_nb_flights``` looks pretty final to me, right ?

How could the total of flights could be different with the calling classes ?

So let's proceed to make it final.

 ```
 get_nb_flights FINAL RETURNING VALUE(rv_nb_flights) TYPE i .
 ``` 
 
:bulb: compile. Does it ? Why ? Indeed, it's already a class method. :smiley:

But we can do it on get_free_seats:

 ```
get_free_seats FINAL RETURNING VALUE(rv_nb_seats) TYPE i
 ```
 
 Last but not least, we are going to prevent ```LCL_CARGO``` from further inheritance so we will declare it as final on class level.
 
  ```
  CLASS LCL_CARGO DEFINITION FINAL.
  ```
 
# Using inheritance to its next level

You can comment the code specific in the report starting below ```END-OF-SELECTION```

Write this instead 

```
"#PART 6#
  DATA: lo_flight    TYPE REF TO lcl_flight,
        lot_flights  TYPE TABLE OF REF TO lcl_flight,
        lo_cargo     TYPE REF TO lcl_cargo,
        lo_cargo1    TYPE REF TO lcl_cargo,
        lo_airplane  TYPE REF TO lcl_airplane,
        lo_airplane1 TYPE REF TO lcl_airplane.

  CREATE OBJECT lo_cargo
    EXPORTING
      iv_plane_number = 1234
      iv_company      = 'DHL'.

  lo_cargo->add_package(
    EXPORTING
      iv_volume = 10 "m3
      iv_weight = 2522 "kg
  ).

  lo_cargo->add_package(
    EXPORTING
      iv_volume = 78 "m3
      iv_weight = 7857 "kg
  ).
  lo_flight = lo_cargo.

  APPEND lo_flight TO lot_flights.

  CREATE OBJECT lo_cargo1
    EXPORTING
      iv_plane_number = 8421
      iv_company      = 'FedEx'.

  lo_cargo1->add_package(
    EXPORTING
      iv_volume = 30 "m3
      iv_weight = 2500 "kg
  ).

  APPEND lo_cargo1 TO lot_flights.

  CREATE OBJECT lo_airplane
    EXPORTING
      iv_plane_number = 831
      iv_airline      = 'AFR'.

  lo_airplane->add_passengers(
    EXPORTING
      iv_passengers = 200
      iv_luggage    = 150
  ).
  APPEND lo_airplane TO lot_flights.

  CREATE OBJECT lo_airplane1
    EXPORTING
      iv_plane_number = 7867
      iv_airline      = 'SIA'.

  lo_airplane->add_passengers(
    EXPORTING
      iv_passengers = 200
      iv_luggage    = 150
  ).

  APPEND lo_airplane1 TO lot_flights.

  LOOP AT lot_flights INTO lo_flight.
    WRITE:/ lo_flight->estimate_fuel_consumption( iv_distance = 1000 ).
  ENDLOOP.
  
```

:question: 
- What is the output of the WRITE statement ? 
- Why are the values different ?
- How is this principle call ?

Coming from the recent inheritance on our class ```LCL_FLIGHT```, this class is not used anymore to create instance.

So let's declare it as abstract.
```
CLASS LCL_FLIGHT DEFINITION ABSTRACT.
```

:bulb: compile 

:question: at this point, you should have an issue. Why ?

Comment the erroneous code

```
METHOD add_flight.
    "ro_flight = NEW lcl_flight( iv_plane_number = iv_plane_number
              "iv_company = iv_company ).
    "ADD 1 TO mv_nb_flights.
ENDMETHOD.
```

And move this line in the constructor of ```LCL_FLIGHT```
```
	ADD 1 TO mv_nb_flights
```

:bulb: compile 
