# Why should I really consider OOP (oriented object programming)

Let's take a moment to consider why OOP is a good approach of coding in the actual world of programming concept.

## Change the actor

If you consider a standard report as it's coded today, the report instructs what to do with data using tables and work-areas.

For example, this code below ↓↓↓

```
LOOP AT lt_vbap INTO DATA(ls_vbap).
	PERFORM confirm_saleorder USING ls_vbap.
ENDLOOP.
```

Switching to the oriented object programming, it's now the object that does something.
↓↓↓
```
LOOP AT sales_order_list INTO DATA(lo_saleorder).
	lo_saleorder->confirm( ).
ENDLOOP.
```
It does not seems much but as a matter of fact, it means a lot. 

Now when reading quickly the code, you're able to know who is doing the action (here, an order) and what action it's performing ( given the name is self explanatory)

## Get rid of global/local variables nightmare

Today, a report contains a lot of variables. Some of them are local to a form routine and others are global to the report (or included in a global top dependency) and others are global to a function module used in the report.

In report, today, you'll see a lot of ```CLEAR``` statement to be sure to do not bring with a new iteration, the data from the previous iteration.

But still, at the last iteration of a ```LOOP```, the workarea will retain the last iteration's data, if not cleared.

Here comes the great principle of encapuslation of data in an reference variable.

With the encapsulation principle in object progamming, the data related to an object are encapsulated within it and may or maybe not accessible from external context (given the visiblity granted).

This ensure that you can guarantee the state of your object, any time.

You want a variable to be shared by all instances ? You have that possibility by declaring it static (we'll see those words later). 

You even have the possibility to prevent its further valuation or to hide it from external context ( read-only, private).

## Inter-language

What you'll learn is transposable to any language. The more you use it in your SAP development, the easier it will be to apply it on a new language.

## Readibility

One of the benefits of using OOP is that you have a whole set of concept that allows you to expose or not the data, to make it dependant or not of the object you're manipulating.

Object does the action. 

A little bit as modularisation that you should practice today in your coding, in oriented object programming, you'll split the code into methods dedicated to perfom one specific behavior.

Doing so, our ```confirm``` method will in fact embedded other methods, an it'll be hidden and forbidden to be used outside its dedicated context.

Ex:

confirm -> public
- check_status -> private
- lock_order -> private
- move_to_next_step -> private
  - confirm_step -> private
  - write stuff in ZTable -> private
  - trigger something -> private
- confirm_status -> private
...

## Reusability

Your class if designed correctly can be reused and enhanced without touching the main functionnalities.

Consider a multinational company having an ERP for every country where she has an assembly line.

Some of the development have to be share among all the ERP to provide same level of service but with minor features which are country dependent.

You could code the main core of the service inside a report and spread it to the other ERP and then adjust it everywhere it needs to be.

But what if you make a mistake in the core functionnalities of the report and want to change that ? 

You'll have to take the code of the report, apply changes being aware of the possible impacts on any country specific development.

What if you are providing a new feature that the national company might or not implement ?

How would you handle those problematics ?

Classes !

You can develop a main class. 

Then you spread this main class to every national company. And on these  specific ERP, you can inherit from the main class, redefining only what needs to be.

Now you can be sure that your specific development will remain untouched even if you reimport the whole main class code.
