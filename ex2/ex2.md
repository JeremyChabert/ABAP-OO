#Attributes (state)

The attributes describe the list of variables that can be valuated in the instance of a class

There are **3** types of attributes
- **INSTANCE** attributes
- **STATIC** attributes
- **CONSTANT** attributes

Instance attributes are defined using **DATA** keyword

Static attributes are defined using **CLASS-DATA** keyword

Constant attributes are defined using **CONSTANTS** keyword

```
CLASS lcl_flight DEFINITION.

PUBLIC SECTION.

DATA : mv_instance_attr type string value 'Instance attribute'.
CLASS-DATA: mv_static_attr type string value 'Static attribute'.
CONSTANTS: mv_constant_attr type string value 'Constant attribute'.

PRIVATE SECTION.

DATA : mv_instance_attr2 type string value 'Instance attribute 2'.
CLASS-DATA: mv_static_attr2 type string value 'Static attribute 2'.
CONSTANTS: mv_constant_attr2 type string value 'Constant attribute 2'.

ENDCLASS
```

## Properties

- Constant
  -	It exists **ONLY ONCE** for a class
  -	It **shares its value** to every instance of the same class
  -	**Cannot be modified**
 
 ![constant_attr](../img/constant_attr.PNG)
 
- Static
  -	It exists **ONLY ONCE** for a class
  -	It **shares its value** to every instance of the same class
  -	**Can be modified**
  
 ![static_attr](../img/static_attr.PNG)
 
- Instance
  -	It exists for every instance of an object of our class **BUT** it doesn't share it's value with other instances
  -	**Can be modified**

![instance_attr](../img/instance_attr.PNG)
   
   - It can be specified that attribute is in **READ-ONLY** mode

```  
DATA : mv_instance_readonly_attr type string value 'Instance attribute read-only' READ-ONLY.
```

## Access


```
REPORT ZMYREPORT


CLASS lcl_flight DEFINITION.

PUBLIC SECTION.

DATA : mv_instance_attr type string value 'Instance attribute'.
CLASS-DATA: mv_static_attr type string value 'Static attribute'.
CONSTANTS: mv_constant_attr type string value 'Constant attribute'.

PRIVATE SECTION.

DATA : mv_instance_attr2 type string value 'Instance attribute 2'.
CLASS-DATA: mv_static_attr2 type string value 'Static attribute 2'.
CONSTANTS: mv_constant_attr2 type string value 'Constant attribute 2'.

ENDCLASS

START-OF-SELECTION.

```

#Methods (behavior)

