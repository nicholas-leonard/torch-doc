# Tensor #

The `Tensor` class is probably the most important class in Torch. 
Almost every package depends on or uses this class. It is *__the__*
class for handling numeric data. As with pretty much anything in the
[Torch](https://github.com/torch/torch7/blob/master/README.md) distribution, 
tensors are [serializable](https://github.com/torch/torch7/blob/master/doc/file.md#torch.File.serialization).

## Multi-dimensional matrix ##

A `Tensor` is potentially a multi-dimensional matrix. The number of
dimensions is unlimited. The `size` attributed (in Python/Numpy they call this a `shape`) can be set using
[LongStorage](https://github.com/torch/torch7/blob/master/doc/storage.md) with as many dimensions as 
required.

Example:
```lua
 --- creation of a 4D-tensor 4x5x6x2
 z = torch.Tensor(4,5,6,2)
 --- for more dimensions, (here a 6D tensor) one can do:
 s = torch.LongStorage{4,5,6,2,7,3}
 x = torch.Tensor(s)
```

The number of dimensions of a `Tensor` can be queried by
[dim()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.dim). 
The size of the `i-th` dimension is
returned by [size(i)](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.size). A [LongStorage](https://github.com/torch/torch7/blob/master/doc/storage.md)
containing all the dimensions can be returned by
[size()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.size).

```lua
> print(x:dim())
6
> print(x:size())
 4
 5
 6
 2
 7
 3
[torch.LongStorage of size 6]
```

## Internal data representation ##

The actual data of a Tensor is contained by a
[Storage](https://github.com/torch/torch7/blob/master/doc/storage.md). 
It can be accessed using the 
[storage()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.storage) method. 
While the memory of a Tensor has to be contained in this unique `Storage`, it might
not be contiguous: the first position used in the `Storage` is given
by [storageOffset()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.storageOffset) 
(starting at `1`). And the _jump_ needed to go from one element to another
element in the `i-th` dimension is given by
[stride(i)](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.stride). 
In other words, given a 3D tensor :

```lua
x = torch.Tensor(7,7,7)
```
accessing the element `(3,4,5)` can be done by
```lua
= x[3][4][5]
```
or equivalently (but slowly!)
```lua
= x:storage()[x:storageOffset()+(x:size(1)-1)*x:stride(1)+(x:size(2)-1)*x:stride(2)+(x:size(3)-1)*x:stride(3)]
```
One could say that a Tensor is a particular way of _viewing_ a
`Storage`: a `Storage` only represents a chunk of memory, while the
Tensor interprets this chunk of memory as having dimensions:
```lua
> x = torch.Tensor(4,5)
> s = x:storage()
> for i=1,s:size() do -- fill up the Storage
>> s[i] = i
>> end
> print(x) -- s is interpreted by x as a 2D matrix
  1   2   3   4   5
  6   7   8   9  10
 11  12  13  14  15
 16  17  18  19  20
[torch.DoubleTensor of dimension 4x5]
```

Note also that in torch ___elements in the same row___ [elements along the __last__ dimension]
are contiguous in memory for a matrix (a 2D tensor). In other words, torch Tensors use the 
[row-major order](http://en.wikipedia.org/wiki/Row-major_order):
```lua
> x = torch.Tensor(4,5)
> i = 0
>
> x:apply(function()
>> i = i + 1
>> return i
>> end)
>
> print(x)
  1   2   3   4   5
  6   7   8   9  10
 11  12  13  14  15
 16  17  18  19  20
[torch.DoubleTensor of dimension 4x5]

> return  x:stride()
 5
 1  -- element in the last dimension are contiguous!
[torch.LongStorage of size 2]
```
This is exactly like in C (and not `Fortran`, which is column-major).

## Tensors of different types ##

Actually, several types of Tensors exists:
```lua
ByteTensor -- contains unsigned chars
CharTensor -- contains signed chars
ShortTensor -- contains shorts
IntTensor -- contains ints
FloatTensor -- contains floats
DoubleTensor -- contains doubles
```

Most numeric operations are implemented _only_ for `FloatTensor` and `DoubleTensor`. 
Other Tensor types are useful for saving memory space or encapsulating index values.

## Default Tensor type ##

For convenience, _an alias_ `torch.Tensor` is provided, which allows the user to write
type-independent scripts, which can then be run after choosing the desired Tensor type with
a call like
```lua
torch.setdefaulttensortype('torch.FloatTensor')
```
See [torch.setdefaulttensortype](https://github.com/torch/torch7/blob/master/doc/utility.md#torch.setdefaulttensortype) for more details.
By default, the alias "points" on `torch.DoubleTensor`.


## Extracting Sub-Tensors ##

Many tensor operations in do _not_ make any memory copy, which is good because
memory copies can be expensive. For example, consider a 2D Tensor from which we want to extract 
a 2x3 sub-tensor in the top left corner:
```lua
> a = torch.Tensor{{1,2,3},{4,5,6},{7,8,9}}
> b = a:narrow(1,1,2):narrow(2,1,3)
> print(b)
 1  2  3
 4  5  6
[torch.DoubleTensor of dimension 2x3]
```
We used the [narrow()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.narrow) 
method to do so. If we wanted to extract the second row vector, we could have used
[select()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.select):
```lua
> c = a:select(1,2)
> print(c)
 4
 5
 6
[torch.DoubleTensor of dimension 3]
```
These methods are good examples of how multiple Tensors can reference the same storage. 
To illustrate this, let us [fill()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.fill) Tensor `c` with `-1`:
```
th> c:fill(-1)
-1
-1
-1
[torch.DoubleTensor of dimension 3]
th> print(a)
 1  2  3
-1 -1 -1
 7  8  9
[torch.DoubleTensor of dimension 3x3]
th> print(b)
 1  2  3
-1 -1 -1
[torch.DoubleTensor of dimension 2x3]
```

If you really need to copy a `Tensor`, you can use the 
[copy()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.copy) method:
```lua
> y = torch.Tensor(x:size()):copy(x)
```
Or the convenience method
```lua
> y = x:clone()
```

### Efficient Memory Management ###

Often times Tensor operations will be repeatly executed in loops. 
For example consider the following function which we need for some reason:
```lua
function beuhp(a)
   local res = a:clone():zero() -- the result Tensor
   for i=1,10 do
      local b = a:clone()
      local c = torch.add(b,i)
      local d = torch.pow(c,i)
      res = res + c + d
   end
   return res
end
```
This is a good example of really bad code as for every iteration it re-allocates memory for different Tensors.
It would be much better to reusing existing memory as much as possible. 
Let us make it a little better by taking the memory allocations out of the for loop and by 
using in-place operations instead of operations that allocate new memory by creating new storages:
```lua
function beuhp(a)
   local res = a:clone():zero() -- the result Tensor
   local b = a.new(a:size()) -- an empty tensor of same type and size as a
   for i=1,10 do
      b:copy(a)
      b:add(i)
      res:add(b)
      b:pow(i)
      res:add(b)
   end
   return res
end
```
We could further optimize this function by using an upvalue (in this case `beuhpBuffer`) 
to reuse the internal memory buffer between function calls, thereby making the 
function a [closure](http://en.wikipedia.org/wiki/Closure_%28computer_programming%29):
```lua
local beuhpBuffer = {}
function beuhp(a)
   local res = a:clone():zero() -- the result Tensor
   local aType = torch.type(a)
   local b = beuhpBuffer[aType] or a.new() 
   beuhpBuffer[aType] = b
   b:resizeAs(a)
   for i=1,10 do
      b:copy(a)
      b:add(i)
      res:add(b)
      b:pow(i)
      res:add(b)
   end
   return res
end
```
Finally, we could allow the function to accept a result Tensor argument in which the results could be 
stored. This would allow the function itself to be called multiple times without allocating any memory:
```lua
local beuhpBuffer = {}
function beuhp(res, a)
   if not a then
      a = res
      res a:clone():zero() -- the result Tensor
   end
   local aType = torch.type(a)
   local b = beuhpBuffer[aType] or a.new() 
   beuhpBuffer[aType] = b
   b:resizeAs(a)
   for i=1,10 do
      b:copy(a)
      b:add(i)
      res:add(b)
      b:pow(i)
      res:add(b)
   end
   return res
end
```

## Package Reference ##

The torch package reference can be found [here](https://github.com/torch/torch7/blob/master/README.md).

## BLAS ##

## CUDA Tensors ##

Not mentionned above is a special Tensor type : `CudaTensor`. 
Like a `FloatTensor` it stores 4 byte floating point numbers (floats). However, instead of 
being stored on the host for CPU processing, they are stored on a device, 
specifically an NVIDIA GPU device that support CUDA. 
Like the`FloatTensor` and `DoubleTensor`, the `CudaTensor` supports
most numeric operations which are made available when 
the [cutorch](https://github.com/torch/cutorch) package is imported.
