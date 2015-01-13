Introduction
============

Searching for a good scientific computing framework? Not sure if torch is for you?
This page should help you get a quick overview of what torch has to offer.
Torch is a scientific computing framework with wide support for machine learning algorithms. 
It is easy to use and provides a very efficient implementation, 
thanks to an easy and fast scripting language, LuaJIT, and an underlying C and CUDA implementation.

Among other things, it provides:
  
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

Needless to say that torch is huge. This is because although Lua is a great programming language, 
when torch started to use it, it was mostly used for developing video games, or at least, it didn't 
have much support for scientific computing. Actually, if you have used Lua, you should know that 
it is a very light-weight language built to run on embedded systems like smart phones and microchips.

One of the reasons torch was written using Lua instead of the more popular alternatives like python was
that Lua makes it increadibly easy to interface high-level Lua scripts with low-level C/C++/Cuda code.
This allows users to easily optimize bottlenecks in their (and our) code by moving code from Lua 
to C or what not. Compare extending your favorite Python scientific library vs Torch 
with some custom C/CUDA code and you will see what we mean. 

Anyway, that isn't the only reason Lua was chosen, LuaJIT, a just-in-time compiler for Lua, 
makes even you high-level Lua code run fast. Unlike Python where a for-loop can become a bottleneck,
LuaJIT makes mince meat out of for-loops. No need to fear them like the plague anymore.

