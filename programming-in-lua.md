# Programming in Lua #

Lua is a pretty light-weight and simplistic programming language. If you like Python, you will 
probably appreciate Lua. In this section, we will provide a brief overview of Lua. And since 
most computer scientists seem to be using Python these days, we will attempt to use it as 
a reference point.

## Types ##
The language has 8 basic types of values: 
  
  * number ; 
  * string ; 
  * boolean ; 
  * table ; 
  * function ; 
  * nil ; 
  * userdata ; and
  * thread.
  
The number type represents a floating point (fractional) number. 
There is no separate integer (non-fractional) type. Lua allows simple arithmetic 
on numbers using the usual operators to add, subtract, multiply and divide.

```lua
th> ((2+2)-7)*5/9
-1.6666666666667
```

Notice that the numbers are not rounded into integers. 
They are floating point, or real numbers. 
We can assign values to variables using the = operator.
```lua
th> x = 7*6/3
th> print(x)
14	
```

Lua also uses strings (i.e. text) types. To create strings, wrap text in `"double quotes"` or `'single quotes'`:
```lua
th> print("hello")
hello
We can assign strings to variables just like we can numbers:
th> who = "Lua user"
th> print(who)
Lua user
```
We can concatenate (join together) strings together using the `..` operator, which acts
like Python's `+` operator (for strings):
```lua
th> print("hello " .. who) -- the variable "who" was assigned above
hello Lua user
th> print(who)
Lua user
```

Boolean values have either the value `true` or `false` (Python uses `True` or `False`). 
If a value is not `true`, it must be `false` and vice versa. 
The `not` operator can be placed before a boolean value to invert it.
```lua
th> x = true
th> print(x)
true
th> print(not x)
false
```

Boolean values are used to represent the results of logic tests. 
The equals `==`, and not equals `~=` operators will return boolean 
values depending on the values supplied to them.
```lua
th> print(1 == 0) -- test whether two numbers are equal
false
th> print(1 ~= 0) -- test whether two numbers are not equal
true
```
Like Python, for assignment you use a single equals sign (`=`), 
but for comparison you use a double equals sign (`==`). 

Lua has a general-purpose aggregate datatype called a table. 
Aggregate data types are used for storing collections 
(such as lists, sets, arrays, and associative arrays) containing other objects 
(including numbers, strings, or even other aggregates). 
Lua is a unique language in that tables (which are associative arrays) 
are used for representing all other aggregate types.

Tables are created using a pair of curly brackets `{}` . Let's create an empty table:
```lua
th> x = {}
th> print(x).
{}
th> print{a={b=2},1,2}
{
  1 : 1
  2 : 2
  a : 
    {
      b : 2
    }
}
```
