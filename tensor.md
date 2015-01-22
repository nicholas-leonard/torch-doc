# Tensor #

The `Tensor` class is probably the most important class in Torch. 
Almost every package depends on or uses this class. It is *__the__*
class for handling numeric data. As with pretty much anything in the
[Torch](https://github.com/torch/torch7/blob/master/README.md) distribution, 
tensors are [serializable](https://github.com/torch/torch7/blob/master/doc/file.md#torch.File.serialization).

## Multi-dimensional matrix ##

A `Tensor` is potentially a multi-dimensional matrix. The number of
dimensions is unlimited that can be created using
[LongStorage](https://github.com/torch/torch7/blob/master/doc/storage.md) with more dimensions.

Example:
```lua
 --- creation of a 4D-tensor 4x5x6x2
 z = torch.Tensor(4,5,6,2)
 --- for more dimensions, (here a 6D tensor) one can do:
 s = torch.LongStorage(6)
 s[1] = 4; s[2] = 5; s[3] = 6; s[4] = 2; s[5] = 7; s[6] = 3;
 x = torch.Tensor(s)
```

The number of dimensions of a `Tensor` can be queried by
[dim()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.dim). 
Size of the `i-th` dimension is
returned by [size(i)](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.size). A [LongStorage](https://github.com/torch/torch7/blob/master/doc/storage.md)
containing all the dimensions can be returned by
[size()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.size).

```lua
> print(x:nDimension())
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
It can be accessed using [storage()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.storage). 
While the memory of a Tensor has to be contained in this unique `Storage`, it might
not be contiguous: the first position used in the `Storage` is given
by [storageOffset()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.storageOffset) 
(starting at `1`). And the _jump_ needed to go from one element to another
element in the `i-th` dimension is given by
[`stride(i)`](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.stride). 
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
= x:storage()[x:storageOffset()
           +(3-1)*x:stride(1)+(4-1)*x:stride(2)+(5-1)*x:stride(3)]
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


## Efficient memory management ##

_All_ tensor operations in this class do _not_ make any memory copy. All
these methods transform the existing tensor, or return a new tensor
referencing _the same storage_. This magical behavior is internally
obtained by good usage of the [stride()](https://github.com/torch/torch7/blob/master/doc/tensor.md#torch.Tensor.stride) and
[storageOffset()](tensor.md#torch.storageOffset). Example:
```lua
> x = torch.Tensor(5):zero()
> print(x)
0
0
0
0
0
[torch.DoubleTensor of dimension 5]
> x:narrow(1, 2, 3):fill(1) -- narrow() returns a Tensor
                            -- referencing the same Storage as x
> print(x)
 0
 1
 1
 1
 0
[torch.Tensor of dimension 5]
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

## Package Reference ##

The torch package reference can be found [here](https://github.com/torch/torch7/blob/master/README.md).

## BLAS ##

## CUDA Tensors ##
