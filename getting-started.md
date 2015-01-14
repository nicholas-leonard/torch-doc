# Getting Started #

Installing Torch can be accomplished by running these two commands:

```bash
$ curl -sk https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash
$ curl -sk https://raw.githubusercontent.com/torch/ezinstall/master/install-luajit+torch | bash
```
The [first script](https://raw.githubusercontent.com/torch/ezinstall/master/install-deps) 
installs the basic package dependencies that LuaJIT and Torch require. 
The [second script](https://raw.githubusercontent.com/torch/ezinstall/master/install-luajit+torch) 
installs [LuaJIT](http://luajit.org/luajit.html), [LuaRocks](http://luarocks.org/), 
and then uses LuaRocks (the lua package manager) to install core packages like
[torch](https://github.com/torch/torch7/blob/master/README.md), 
[nn](https://github.com/torch/nn/blob/master/README.md) and 
[paths](https://github.com/torch/paths/blob/master/README.md), as well as a few other packages. 
These scripts work on Ubuntu >= 12.04 and OSX >= 10.8.

Torch and all its related packages, are now distributed as simple LuaRocks packages. 
It only requires LuaJIT (>= 2.0) and Luarocks (>= 2.0.12). To make sure you
have the right versions you can open a terminal and run:

```bash
$ luarocks --version
Luarocks 2.0.12
```
and
```bash
$ luajit -v
LuaJIT 2.0.2 -- Copyright (C) 2005-2013 Mike Pall. http://luajit.org/
```

If you install Luajit and LuaRocks yourself, you'll have to specify the URL to the Torch rock server, 
to find Torch. It is recommended to use the above scripts to automate that process. 

```bash
$ luarocks --server=https://raw.githubusercontent.com/torch/rocks/master install torch
```
Other packages can be installed the same way, using Luarocks:

```bash
$ luarocks --server=https://raw.githubusercontent.com/torch/rocks/master install image
$ luarocks --server=https://raw.githubusercontent.com/torch/rocks/master list
```
Once installed you can run torch with the command "th" from you prompt!

The easiest way to learn and experiment with Torch is by starting an
interactive session (also known as a torch read-eval-print loop or [TREPL]):

```bash
$ th
 
  ______             __   |  Torch7                                   
 /_  __/__  ________/ /   |  Scientific computing for Lua.         
  / / / _ \/ __/ __/ _ \  |                                           
 /_/  \___/_/  \__/_//_/  |  https://github.com/torch   
                          |  http://torch.ch            
	
th> torch.Tensor{1,2,3}
 1
 2
 3
[torch.DoubleTensor of dimension 3]

th>
```

To exit the interactive session, type `^c` twice — the control key
together with the `c` key, twice, or type `os.exit()`.
Once the user has entered a complete expression, such as ``1 + 2``, and
hits enter, the interactive session evaluates the expression and shows
its value. 

To evaluate expressions written in a source file `file.lua`, write
`th> dofile "file.lua"`.

To run code in a file non-interactively, you can give it as the first
argument to the th command::

```bash
$ torch file.lua
```

There are various ways to run Lua code and provide options, similar to
those available for the ``perl`` and ``ruby`` programs:

```bash
 $ th -h
Usage: th [options] [script.lua [arguments]]

Options:
  -l name            load library name
  -e statement       execute statement
  -h,--help          print this help
  -a,--async         preload async (libuv) and start async repl (BETA)
  -g,--globals       monitor global variables (print a warning on creation/access)
  -gg,--gglobals     monitor global variables (throw an error on creation/access)
  -x,--gfx           start gfx server and load gfx env
  -i,--interactive   enter the REPL after executing a script
```
 
## Resources ##

In addition to this manual, there are various other resources that may
help new users get started with torch:
  
  * torch.ch
  * cheatsheet
