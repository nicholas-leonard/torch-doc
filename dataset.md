# Dataset #

The first thing we need to do before we begin implementing some awe-inspiring models is 
to prepare the data. More specifically, we would like to make it accessible in Lua.
Your data could be stored on disk in any of a dozen formats. We cannot cover them all here.

In most cases, where you data is small enough to fit in memory all at once, you will want to 
convert it such that it is contained by no more than a few Tensors. Usually, for supervised 
learning problems, this means storing all inputs and targets into their own contiguous Tensors.
Tensors are a really efficient means of encapsulating your data, not to mention all the querying
facilities they provide. 

Using [torch.save](https://github.com/torch/torch7/blob/master/doc/serialization.md#torch.save), 
you can then save your data on disk as Tensors for efficient storage and loading via 
[torch.load](https://github.com/torch/torch7/blob/master/doc/serialization.md#torch.load). 

## Image Classification Example ##

Suppose your data is stored as a bunch of large images in `/path/to/images/`. 
Each image filename has the following format : `image[id]_[class].jpg`, where `id` and `class` uniquely  
identify the input image and target class, respectively. We have approximately 60,000 such images and 
they are of size (`width x height`) `3200 x 2800`, but we only expect our model to use 
images of size `64 x 56`. 

We want to load these images, downscale them, and store them into an `inputs` Tensor. 
We also want to store all the target classes into a `targets` Tensor.
First, we require the necessary packages and set some global variables:
```lua
require "torch"
require "paths"
require "image"
require "lfs" --luafilesystem

dataPath = "/path/to/images"
```

Since we want to store all this into Tensors, we should first count the number of 
images to find out the size of our Tensors. While we are at it, lets create a
map associating unique class names to indexes:
```lua
nFile = 0 
nClass = 0
classes = {}
for dir in lfs.dir(dataPath) do
  local class = dir:match('image[0-9]+_([0-9]+)%.jpg')
  if class then
    nFile = nFile + 1
    if not classes[class] then
      nClass = nClass + 1
      classes[class] = nClass
    end
  end
end
```
Lets allocate those Tensors. We will use `torch.FloatTensor` 
for the `inputs` as it requires half the memory footprint of a 
`torch.DoubleTensor`. The same can be saif of `torch.IntTensor` vs. `torch.LongTensor`.
```lua
inputs = torch.FloatTensor(nFile, unpack(inputSize))
targets = torch.IntTensor(nFile)
```
