# Our class is becoming a mother of two

Let's add some new behaviors to our root class to start

```
CLASS LCL_FLIGHT DEFINITION.

	PUBLIC SECTION.
	[...]
	
	CLASS-METHODS: class_constructor.
	
	PROTECTED.
	
	METHODS: estimate_fuel_consumption IMPORTING iv_distance TYPE i RETURNING VALUE(re_fuel) type p.
	DATA : mv_weight type i.
	CONSTANTS: c_factor type f value 0.000012.
	
ENDCLASS.
```

```
CLASS LCL_FLIGHT IMPLEMENTATION.
	METHOD class_constructor.
		mv_weight = 115000. "weight of A350Neo w/o kerosene (in kg)
	ENDMETHOD.
	METHOD estimate_fuel_consumption.
		DATA lv_total_weight type i.
		LOOP AT mot_tanks INTO DATA(lo_tank).
			ADD lo_tank->get_weight( ) TO lv_total_weight .
		ENDLOOP.
		ADD mv_weight TO lv_total_weight.
		re_fuel = ceil( lv_total_weight * iv_distance * c_factor ).
	ENDMETHOD.
ENDCLASS.
```
## First daughter : LCL_CARGO

```
CLASS LCL_CARGO DEFINITION INHERITING FROM LCL_FLIGHT.
		PUBLIC SECTION.
		DATA : mv_packages type i,
			   mv_total_packages_weight type i,
			   mv_company type string.
ENDCLASS.
```

## Second daughter : LCL_AIRPLANE

```
CLASS LCL_AIRPLANE DEFINITION INHERITING FROM LCL_FLIGHT.
		PUBLIC SECTION.
		DATA : mv_passengers type i,
			   mv_luggages   type i,
			   mv_airline	 type string.
		CONSTANTS : c_avg_weight type i value 75,
    c_avg_lugg_weight type i value 22.
ENDCLASS.
```

## Growing their own characters

```
CLASS LCL_CARGO DEFINITION INHERITING FROM LCL_FLIGHT.
	PUBLIC SECTION.
		METHODS : constructor IMPORTING iv_company TYPE string,
				  add_package IMPORTING iv_volume TYPE i
          iv_weight TYPE i,
				  estimate_fuel_consumption REDEFINITION.
ENDCLASS.

CLASS LCL_CARGO IMPLEMENTATION.

	METHOD constructor.
		super->constuctor( ).
		mv_company = iv_company.
		DATA(lo_extra_tank) = NEW lcl_tank.
		APPEND lo_extra_tank TO mot_tanks.
	ENDMETHODS.
	
	METHOD estimate_fuel_consumption.
		DATA lv_total_weight type i.
		LOOP AT mot_tanks INTO DATA(lo_tank).
			ADD lo_tank->get_weight( ) TO lv_total_weight .
		ENDLOOP.
		
		ADD mv_weight TO lv_total_weight.
		re_fuel = ceil( lv_total_weight * iv_distance * c_factor ).
	ENDMETHOD.
	
ENDCLASS.

```

:question: Did you notice the super-> namespace used in the constructor of the subclass?
What is its utility ?
		
```
CLASS LCL_AIRPLANE DEFINITION INHERITING FROM LCL_FLIGHT.
	PUBLIC SECTION.
		METHODS : constructor IMPORTING iv_airline TYPE string,
    add_passengers IMPORTING iv_passengers TYPE i 
    iv_luggages 	TYPE i,
    estimate_fuel_consumption REDEFINITION.
ENDCLASS.

CLASS LCL_AIRPLANE IMPLEMENTATION.

	METHOD constructor.		
		mv_airline = iv_airline.
		super->constuctor( ).

	ENDMETHODS.
	
	METHOD estimate_fuel_consumption.
		DATA lv_total_weight type i.
		LOOP AT mot_tanks INTO DATA(lo_tank).
			ADD lo_tank->get_weight TO lv_total_weight .
		ENDLOOP.
		ADD mv_weight TO lv_total_weight.
		DATA(lv_passengers_n_luggages_weight) = mv_passengers * c_avg_weight + mv_luggages * c_avg_lugg_weight.
		ADD lv_passengers_n_luggages_weight TO lv_total_weight.
		re_fuel = ceil( lv_total_weight * iv_distance * c_factor ).
	ENDMETHOD.
	
ENDCLASS.
```

:question: Does it compile now ? Why ?

Right, let's invert super->constructor and extra declaration.
```
	METHOD constructor.	
		super->constuctor( ).	  "↑
		mv_airline = iv_airline.  "↓
	ENDMETHODS.
```
Now you know that inside a constructor, the super->constructor shall **ALWAYS** be called first.
