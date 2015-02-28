# Object-Oriented Programming #

The Lua programming language provides the necessary 
functionaly to allow for the creation of classes and objects. 
This is made possible using Lua [metatables](http://nova-fusion.com/2011/06/30/lua-metatables-tutorial/).
However, object-oriented programming (OOP) using metatables doesn't result in the prettiest of code.

Torch simplifies OOP by abstracting away the nitty-gritty details into 
[simple functions](https://github.com/torch/torch7/blob/master/doc/utility.md).
The most important of which is [torch.class](https://github.com/torch/torch7/blob/master/doc/utility.md#torch.class),
which allows the user to easily create new classes.

## [metatable] torch.class(name, [parentName]) ##

This function creates a new `Torch` class called `name`. 
If `parentName` is provided, the class will inherit
`parentName` methods. A class is a table which has a particular metatable.

If `name` is of the form `package.className` then the class `className` will be added to the specified `package`.
In that case, `package` has to be a valid (and already loaded) package, i.e. a table.

For example, let us create class `fo.Foo`:
```lua
-- package fo
fo = {}
--- creates a class "fo.Foo"
local Foo = torch.class('fo.Foo')

--- the constructor
function Foo:__init(contents)
  self.contents = contents or "this is some text"
end

--- a method
function Foo:print()
  print(self.contents)
end

--- another one
function Foo:bip()
  print('bip')
end
```

Now we can use class `fo.Foo` to instantiate objects:
```lua
foo = fo.Foo("Hello World")
foo:print()
Hello World
```

We can further use `torch.class` to create a class `torch.Bar` which inherits from `fo.Foo`:
```lua
local Bar, parent = torch.class('torch.Bar', 'fo.Foo')

function Bar:__init(stuff, contents)
  --- call the parent initializer on self
  parent.__init(self, contents)

  --- do some stuff
  self.stuff = stuff
end

-- a new method
function Bar:boing()
  print('boing!')
end

--- override parent's method
function Bar:print()
  print(self.contents)
  print(self.stuff)
end
```
Class `torch.Bar` can also be used to instantiate objects:
```
bar = torch.Bar("ha ha!")
bar:print() -- overrided method
bar:boing() -- child method
bar:bip()   -- parent's method
```

<a name="torch.type"/>
## [string] torch.type(object) ##

Checks if `object` has a metatable. If it does, and if it corresponds to a
class created via `torch.class`, then this returns a string containing the name of the
class. Otherwise, it returns the Lua `type(object)` of the object.

```lua
> torch.type(torch.Tensor())
torch.DoubleTensor
> torch.type({})
table
> torch.type(7)
number
```

## Serialization ##

Torch provides 4 high-level methods to serialize or deserialize arbitrary Lua and Torch objects.

The first two functions are useful to serialize or deserialize data to or from files:

  - [torch.save(filename, object, [format])](https://github.com/torch/torch7/blob/master/doc/serialization.md#torch.save)
  - [[object] torch.load(filename, [format])](https://github.com/torch/torch7/blob/master/doc/serialization.md#torch.load)

The next two functions are useful to serialize or deserialize data to or from strings:

  - [[str] torch.serialize(object, [format])](https://github.com/torch/torch7/blob/master/doc/serialization.md#torch.serialize)
  - [[object] torch.deserialize(str, [format])](https://github.com/torch/torch7/blob/master/doc/serialization.md#torch.deserialize)

Serializing to files is useful to save arbitrary data structures, or share them with other people.
Serializing to strings is useful to store arbitrary data structures in databases, or 3rd party
software, or transfering objects between different processes or Lua states.

These functions can be used to serialize any objects instantiated with a `torch.class` metatable, as well 
as any of the Lua types (excluding threads and coroutines, for obvious reasons).
