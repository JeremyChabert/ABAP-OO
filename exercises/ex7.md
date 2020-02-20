# Using static and dynamic type together

For the sake of the demonstration, I have created a ZTable as follow and I have populated it with values

![Ztable](../img/ZTable.PNG)

The data element ZDEFLIGHTTYPE is based on a domain with 2 values A for airplane passengers and C for airplane cargo.

Let's use it in our report. Again, you can comment the previous code.

Let's start by changing the type of iv_plane_number in our class definition.

It was ```i```, and we will change it for ```int2``` to avoid issue.

Change also ```iv_company``` and ```iv_airline``` type into ```any```

Now write this below the commented code.

```
DATA : lv_dynamic_type type string,
       lo_flight type ref to lcl_flight,
       lot_flights type table of ref to lcl_flight.

SELECT * FROM ZFLIGHT_ABAPOO INTO TABLE @DATA(lt_flights).

LOOP AT lt_flights INTO DATA(ls_flight).

  CASE ls_flight-flight_type.
  
    WHEN 'A'.
        CREATE OBJECT lo_flight TYPE lcl_airplane
        EXPORTING
        iv_plane_number =  ls_flight-flight_number
        iv_airline      =  ls_flight-company.
    WHEN 'C'.
        CREATE OBJECT lo_flight TYPE lcl_cargo
        EXPORTING
        iv_plane_number =  ls_flight-flight_number
        iv_company      =  ls_flight-company.
    WHEN OTHERS.
    
 ENDCASE.
 
 APPEND lo_flight TO lot_flights.
      
ENDLOOP.

LOOP AT lot_flights INTO lo_flight.
    WRITE:/ lo_flight->estimate_fuel_consumption( 15000 ).
ENDLOOP.
```
Did you notice the syntax we used ? ```CREATE OBJECT <instance> TYPE <dynamic instance type>```.

Well at this point, it's up to you to decide wether you'll instantiate your reference using the statement ```CREATE OBJECT``` or the following syntax:  ```NEW lcl_cargo( )```.

You'll have to declare 2 differents variables and append them into flights. The upcast will be then be executed with the APPEND statement.

```
  DATA : lv_dynamic_type TYPE string,
         lot_flights     TYPE TABLE OF REF TO lcl_flight.

  SELECT * FROM zflight_abapoo INTO TABLE @DATA(lt_flights).

  LOOP AT lt_flights INTO DATA(ls_flight).

    CASE ls_flight-flight_type.

      WHEN 'A'.
        DATA(lo_airplane) = NEW lcl_airplane(
        iv_plane_number =  ls_flight-flight_number
        iv_airline      =  ls_flight-company ).
        APPEND lo_airplane TO lot_flights.
      WHEN 'C'.
        DATA(lo_cargo) = NEW lcl_cargo(
        iv_plane_number =  ls_flight-flight_number
        iv_company      =  ls_flight-company ).
        APPEND lo_cargo TO lot_flights.
      WHEN OTHERS.

    ENDCASE.

  ENDLOOP.

  LOOP AT lot_flights INTO DATA(lo_flight).
    WRITE:/ lo_flight->estimate_fuel_consumption( 15000 ).
  ENDLOOP.
```
With this example, you see now that we can have a whole set of flights of different types in a single table of references.

And they will react differently to behavior calls if they implement it differently.

:warning: **Within the loop statement, as written currently, you can only call behaviors that are implemented by LCL_FLIGHT and that are or are not redifined by subclass(es)**

If you want to execute specific subclass behavior, you'll have to downcast(narrow) your reference and call the method.

If we push further the simplification of this code, we can consider the following.

1. First I add the 2 following statements before my main class.
```
CLASS lcl_cargo DEFINITION DEFERRED.

CLASS lcl_airplane DEFINITION DEFERRED.
```
2. Then I define my method add_flights as follow and I add a statis instance of collection of flights.
```
CLASS lcl_flight DEFINITION ABSTRACT.

  PUBLIC SECTION.
    [...]
    CLASS-DATA: [...]
                mot_flights     TYPE TABLE OF REF TO lcl_flight.
       [...]
    CLASS-METHODS : 
       [...]
      add_flight IMPORTING iv_plane_number  TYPE int2
                           iv_company       TYPE any
                           iv_type          TYPE zdeflighttype
                 RETURNING VALUE(ro_flight) TYPE REF TO lcl_flight,
      [...]

ENDCLASS.
```
3. Then I implement the add_flight method as follow
```
  METHOD add_flight.

    CASE iv_type.

      WHEN 'A'.
        CREATE OBJECT ro_flight TYPE lcl_airplane
          EXPORTING
            iv_plane_number = iv_plane_number
            iv_airline      = iv_company.


      WHEN 'C'.
        CREATE OBJECT ro_flight TYPE lcl_cargo
          EXPORTING
            iv_plane_number = iv_plane_number
            iv_company      = iv_company.
            
    ENDCASE.
    
    APPEND ro_flight TO mot_flights.
    
  ENDMETHOD.
```
4. In my report, i'll replace my current code with this.
```
LOOP AT lt_flights INTO DATA(ls_flight).

    DATA(lo_flight) = lcl_flight=>add_flight(
      EXPORTING
        iv_plane_number = ls_flight-flight_number
        iv_company      = ls_flight-company
        iv_type         = ls_flight-flight_type
    ).

ENDLOOP.

LOOP AT lcl_flight=mot_flights INTO lo_flight.
    WRITE:/ lo_flight->estimate_fuel_consumption( 15000 ).
ENDLOOP. 
```

:warning: if you have an issue at compilation, 

```L'accès aux membres de la classe introduite par "CLASS LCL_AIRPLANE
DEFINITION DEFERRED" est possible uniquement après la définition (CLASS
LCL_AIRPLANE DEFINITION).
```
Consider then taking the implementation of ```LCL_FLIGHT``` after the definition of ```LCL_CARGO``` and ```LCL_AIRPLANE```.

Like this ↓
```
CLASS LCL_FLIGHT DEFINITION.
ENDCLASS.
CLASS LCL_CARGO DEFINITION.
ENDCLASS.
CLASS LCL_AIRPLANE DEFINITION.
ENDCLASS.

CLASS LCL_FLIGHT IMPLEMENTATION.
ENDCLASS.
CLASS LCL_CARGO IMPLEMENTATION.
ENDCLASS.
CLASS LCL_AIRPLANE IMPLEMENTATION.
ENDCLASS.
```
Now, we will take a minute or two to appreciate the simplicity of the code.

- I add flights to my main class
- I hide the differenciation of instance creation inside the add_flight method.
- As I store the created instance inside a table of references (pointers) and also return the created instance, I can manipulate the returned instance in my main report **AND** it will be automatically reflected in my table as I'm working on the object through the reference.

## Opening words on development

Usually, while programming, the requirement is pretty short :
- confirm orders
- convert orders
- change delivery status
- calculate next steps of production order
- ...

Using OOP makes it easier to read the code and see if you're inline with the requirement.

It's also a bridge between the developer and the functionnal. A fonctionnal won't understand the content of the method (unless he has a technical background) BUT the sequence of the calls will be sufficient for him to acknowledge the process.

For example, let's suppose we are dealing with 3 types of orders that shall be confirmed and moved to the next status.

```
LOOP AT lot_orders INTO DATA(lo_order).
    CHECK lo_order->is_open( ).
    CHECK lo_order->has_items( ).
        lo_order->lock( ).
        lo_order->confirm_next_step( ).
        lo_order->set_next_status( ).
        lo_order->unlock( ).
ENDLOOP.
```

With 8 lines in the main report, we can structure the main requirement such as confirming an order. We could have pushed further encapsulation the other calls inside a more generic method called ```confirm_order```

Nevertheless, what it does is pretty understandable from everyone and can be cross-checked against the specifications.

And if we enter into the technical aspects now, we are able to have a set of different types of orders with different:
- lock mechanism
- confirmation of next step mechanism.
- setting of next status

**WITHOUT** changing a single line in the report. Using polymorphism, we are taking care of this specifities related to the order types.
