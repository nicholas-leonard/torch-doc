Introduction
============

Searching for a good scientific computing framework? Not sure if torch is for you?
This page should help you get a quick overview of what torch has to offer.
Torch is a scientific computing framework with wide support for machine learning algorithms. 
It is easy to use and provides a very efficient implementation, 
thanks to an easy and fast scripting language, LuaJIT, and an underlying C and CUDA implementation.

Although Lua is a great programming language, when torch started, 
Lua was mostly used for developing video games, or at least, it didn't 
have much support for scientific computing. Actually, if you have used Lua, you should know that 
it is a very light-weight language built to run on embedded systems like smart phones and microchips.
It doesn't come with huge standard libraries like Python.

One of the reasons torch was written using Lua instead of the more popular alternatives like Python was
that Lua makes it increadibly easy to interface high-level Lua scripts with low-level C/C++/Cuda code.
This allows users to easily optimize bottlenecks in their (and our) code by moving code from Lua 
to C or what not. Compare extending your favorite Python scientific library vs Torch 
with some custom C/CUDA code and you will see what we mean. 

Anyway, that isn't the only reason Lua was chosen, LuaJIT, a just-in-time compiler for Lua, 
makes even you high-level Lua code run fast. Unlike Python where a for-loop can become a bottleneck,
LuaJIT makes mince meat out of for-loops. No need to fear them like the plague anymore. 

However, one of the strong points of Python vs Lua are its out-of-the-box support 
for object-oriented programming (OOP), serialization (pickling), and so much more. 
While OOP is possible using tables and meta-tables in Lua, torch makes it so much easier
on the programming by providing a simple interface for creating polymorphic classes and 
serialization thereof. 

Okay, so we can't introduce you to everything torch is as it is so much. So in the interests of 
brevity, this is what Torch provides at a glance:
  
  * a powerful N-dimensional array ;
  * lots of routines for indexing, slicing, transposing, and so forth and so on ;
  * amazing interface to C, via LuaJIT ;
  * linear algebra routines ;
  * neural network, and energy-based models ;
  * numeric optimization routines ;
  * object-oriented support for Lua ;
  * multi-threading using task queues ;
  * datasets for training and testing models ;
  * interactive top-level language shell (REPL) ; 
  * image processing routines for cropping, scaling, etc. ;
  * A plotting package to visualize Tensor objects ;
