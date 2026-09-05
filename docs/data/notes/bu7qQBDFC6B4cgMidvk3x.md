
A struct is a composite type 
- Stands for “user defined data structure”

In C++, it is similar to a class (though by default the access level is public)
- A struct is similar to an interface, in that we define a shape, which effectively becomes our new composite type. We can then initialize new variables that take this shape
- The primary use of struct is for the construction of complex datatypes
- Consider that a composite type such as this is analogous to a record in a database table. In a table, we implicitly create a type that has the same shape of the table  (in this sense the composite type is called a record)
- All variables contained in a struct are physically located together. The consequence of this is that we can access the struct with a single pointer
