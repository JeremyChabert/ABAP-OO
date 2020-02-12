# Create your first class

## Definition

:computer: Using SE38, create a new report called ZABAPOO_TRAIN_***XXX*** where XXX is your trigramme

- Title : My first ABAP OO report
- Type : Executable report
- Status: Test report

:floppy_disk: ***save***

Write down following code 

```
CLASS lcl_flight DEFINITION.


ENDCLASS.
```

:bulb: ***compile***

:question: Why is it compiling ?

## Implementation

:computer: Now add the implementation section. (no tips this time)

:bulb: ***compile***

And this is all it takes to start a class. Well an empty one, but still, you've created your first class.

How do we write it using in the definition of our class ?

## Visibility

:computer: start to type ***PUBLIC*** and use ```CTRL + SPACE``` to get suggestions from auto completion.

Do it again for ***PRIVATE***

:floppy_disk: save then :bulb: compile (or shorter ```CTRL + F3```)

Now, your code should look like this

```
[...]
CLASS LCL_FLIGHT DEFINITION.
	PUBLIC SECTION.
	
	PRIVATE SECTION.
ENDCLASS.

CLASS LCL_FLIGHT IMPLEMENTATION.

ENDCLASS.
[...]
```

# Questions

- What are the keywords to wrap the definition part of a class ?
- Where will you declare attributes that you want to hide from external referential ?
- Where will you declare a behavior ? Where will you define it ?
- What's the word that define the process of giving different visiblity settings to attributes and behaviors ?
