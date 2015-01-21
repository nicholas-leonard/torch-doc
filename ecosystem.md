# Torch Ecosystem #

As new users, it is easy to get lost in the torch ecosystem.
We therefore provide this section to try and give a brief 
overview of the different packages, reference documentation, tutorials, 
forums and such.

## Packages ##
The torch distribution used to consist for the most part of a singular monolithic 
torch package. In 2013, it was split into smaller more 
manageable packages, each located in its own GitHub repository. The 
main packages are :
  
  * [torch](https://github.com/torch/torch7) : tensors, class factory, serialization, BLAS ;
  * [nn](https://github.com/torch/nn) : neural network Modules and Criterions;
  * [optim](https://github.com/torch/optim) : SGD, LBFGS and other optimization functions ;
  * [gnuplot](https://github.com/torch/gnuplot) : ploting and data visualization ;
  * [paths](https://github.com/torch/paths) : make directories, concatenate file paths, and other filesystem utilities ;
  * [image](https://github.com/torch/image) : save, load, crop, scale, warp, translate images and such ;
  * [trepl](https://github.com/torch/trepl) : the torch LuaJIT interpreter ;
  * [cwrap](https://github.com/torch/cwrap) : used for wrapping C/CUDA functions in Lua ;

Each package repository includes its own documentation via a `README.md` 
which may contain links to a `docs/` directory. 

### CUDA Packages ###
In addition to the core packages, the distribution also includes 
packages that allows the more compute-intensive routines to be run 
seamlessly on NVIDIA Graphical Processing Units (GPUs) using the free 
NVIDIA CUDA programming language. These lightning-fast packages include :
 
  * [cutorch](https://github.com/torch/cutorch) : tensors and BLAS ;
  * [cunn](https://github.com/torch/cunn) : Modules and Criterions ;

For the most part, converting Tensors, Modules or Criterions to CUDA 
is as easy as :
```lua
require 'cutorch'

a = torch.randn(3,4) -- a torch.DoubleTensor()
b = a:cuda() -- a torch.CudaTensor()
c = torch.CudaTensor(3,4):zero()
d = torch.add(b,c) -- d = b + c (note that this allocates memory for d)
d:add(b,c) -- like the previous line, but reuses existing result memory d
a:copy(d) --copy torch.CudaTensor to torch.FloatTensor
```

It is even easier for Modules and Criterions :
```lua
require 'cunn'

input = torch.randn(3,4):cuda()
layer = nn.Linear(4,5)

layer:cuda()
output = layer:forward(input)
```
However, not all Modules, Criterion and Tensor BLAS operations have 
equivalent CUDA implementations. Yet, most do.

### Third-Party Packages ###
Dozens of third-party packages have grown around the core torch packages.
These are listed in the torch [Cheatsheet](https://github.com/torch/torch7/wiki/Cheatsheet).

## Tutorials ##
Torch comes with its own set of tutorials. The most popular and well 
organized is the [madbits tutorial](http://code.cogbits.com/wiki/doku.php?id=tutorial_basics).
The code for these tutorials is located in [this repository](https://github.com/torch/tutorials).
Various demo application code can be found [here](https://github.com/torch/demos).
While the previous tutorials and demos all rely on the 
[optim](https://github.com/torch/optim/blob/master/README.md) packages, 
more can be found within the [dp](https://github.com/nicholas-leonard/dp/blob/master/README.md)
documentation which rely on a higher-level interface to torch. Implementations 
of large-scale neural networks using torch can be found among the 
[fbcunn examples](https://github.com/facebook/fbcunn/tree/master/examples/imagenet/models).

## Forums ##

For support, users can search the Torch 
[Google Groups](https://groups.google.com/forum/embed/?place=forum%2Ftorch7#!forum/torch7) and ask questions.
The community is usually very quick to reply.
