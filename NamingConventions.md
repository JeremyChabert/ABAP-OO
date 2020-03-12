# Conventions

## Classes
Classes declared in a report will be aliases as ***LCL***_<name>, this means their usage will only be local to this report

Classes declared as global will be aliases as ***Z|YCL***_<name>, this means their usage isn't restrict to a specific report. 
The global classes declared in SE24 are reusable.

## Parameters
Signature of methods are to be named conventionally as follow:
- **I*X*_\<name>** for **IMPORTING** parameters
- **E*X*_\<name>** for **EXPORTING** parameters
- **C*X*_\<name>** for **CHANGING** parameters
- **R*X*_\<name>** for **RETURNING** parameters

**X** is for the type of parameters
- **V** for variable
- **S** for structure/work-area
- **T** for table
- **O** for object

# Clean code

https://github.com/SAP/styleguides/blob/master/clean-abap/CleanABAP.md
