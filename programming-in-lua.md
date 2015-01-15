# Programming in Lua #

Lua is a pretty light-weight and simplistic programming language. If you like Python, you will 
probably appreciate Lua. In this section, we will provide a brief overview of Lua. And since 
most computer scientists seem to be using Python these days, we will attempt to use it as 
a reference point.

## Types ##
The language has 8 basic types of values: 
number, string, boolean, table, function, nil, userdata, and thread.
  
### Number ###
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

### String ###

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

### Boolean ###

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

### Table ###

Lua has a general-purpose aggregate datatype called a table. 
This aggregate data type is used for storing collections 
(such as lists, sets, arrays, and associative arrays) containing other objects 
(including numbers, strings, or even other aggregates). 
Lua is a unique language in that tables (which are associative arrays) 
are used for representing all other aggregate types.
Similarly to Python's dictionary data type, the table 
is also used to represent objects and classes (here, metatables).

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
Note that the last call to the `print()` function did not require parenthesis. This is 
because a function call using nothing but a table expression `{}` 
argument can do away with the parenthesis.
The same goes for string expressions such that you can write :

```lua
th> print"Hello World"
Hello World
```

### Functions ###

In Lua, functions are assigned to variables, just like numbers and strings. Functions are created using the `function` keyword. Here we create a simple function which will print a friendly message.

```lua
th> foo = function () print("hello") end -- declare the function
th> foo() -- call the function
hello
th> print(foo) -- get the value of the variable "foo"
function: 0035D6E8
```
Notice that we can assign functions to variables, just like the other values.
The ability to do this is because Lua has first class values. 
This means that all values are treated the same way. 
This is a very powerful and useful feature of Lua.
A function can be part of a table:
```lua
th> a = "aeiou" -- a string
th> b = 13      -- a number
th> c = function()  -- a function
th>  print ("\n\n\tAin't it grand")
th> end
th> d = { a, b ,c} -- put them in a table
th> function printit(tata)  -- print their types.
th> for key, value in ipairs(tata) do print(key, type(value)) end
th> end
th> printit(d)
1       string
2       number
3       function
```
Notice the `ipairs` iterator, which is a very common function used to iterate lists (tables)
like `d`. This is different from Python that has implicit iterators for lists. 
However, if instead of `printit(d)`, we call :
```lua
th> printit{a=1,b=2,c=3}
```
then nothing would get printed as `{a=1,b=2,c=3}` is not a list, i.e. it doesn't
have at least an element at index 1. To iterate through all `key,value` pairs of a table,
we could use `pairs`. Similarly, calling `#d` would yield the number of list element in `d`,
but calling `#{a=1,b=2,c=3}` would yield zero.
