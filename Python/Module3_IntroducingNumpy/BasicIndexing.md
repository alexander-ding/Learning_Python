---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.1'
      jupytext_version: 1.1.0-rc0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```raw_mimetype="text/restructuredtext"
.. meta::
   :description: Topic: Numpy array basic indexing, Difficulty: Medium, Category: Section
   :keywords: basic index, slice, no copy index, multidimensional array, nd array, view, reverse, axis
```

<!-- #region -->
# 基础和进阶索引简介

到现在为止，我们看到我们可以通过为数组每一维度提供整数或切片对象来访问NumPy数组的成员。索引和切片数组的维度叫做基础索引（basic indexing）。NumPy也提供了一套成熟的“进阶索引”（advanced indexing）系统来为我们提供强大的不仅限于为轴提供整数和切片的访问数组成员的方法。比如说，我们可以使用进阶索引来访问所有 `x` 的负数成员。

```python
# 演示基本和进阶索引
>>> import numpy as np
>>> x = np.array([[ -5,   2,  0, -7],
...               [ -1,   9,  3,  8],
...               [ -3,  -3,  4,  6]])

# 访问行0和行2的列1.
# 这是一个基础索引的范例。这将会创建一个 `x` 内在数据的
# “视阈”（view）；没有任何数据会被复制。
>>> x[::2, 1]
array([ 2, -3])

# 进阶索引的范例。
# 访问所有 `x` 的负数成员。这将复制被访问数据。
>>> x[x < 0]
array([-5, -7, -1, -3, -3])
```

我们将会看到虽然基础索引为我们提供了数组数据的*视阈*（view）而不复制其成员，进阶索引会需要复制数组被访问的数据。在这里，我们将定义基础索引并理解操作数组视阈的一些细节。在下一节，我们会具体学习进阶索引。
<!-- #endregion -->

## 基础索引
我们在本小节之初将定义基础索引。然后，我们将讨论这个定义的每个部分。最后，我们将会仔细研究基础索引允许我们在不复制的情况下引用其内置数据的重要性。

<div class="alert alert-info"> 

**定义：基础索引**：

设 $N$ 维数组 `x`，`x[index]` 使用**基础索引**的时候是当 `index` 是一个含有以下任何对象类型组合的*元组*：

- 整数
- [切片](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Slicing)对象
- [省略号](https://docs.python.org/3/library/constants.html#Ellipsis)（ellipsis）对象
- [numpy.newaxis](http://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/Broadcasting.html#Inserting-Size-1-Dimensions-into-An-Array)对象

通过基础索引来访问数组对象*并不会复制数组的成员*。反而，这会创建一个内置数据的“视阈”。
</div>


<!-- #region -->
### 用整数和切片对象索引
我们在[访问数组多维度的内容](http://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/AccessingDataAlongMultipleDimensions.html)一节已经提供了完整的使用整数和切片来访问数组成员的教程。根据之前的定义，*这些都是基础索引的范例*。

作为那一节讨论的内容的复习，请回忆，你可以通过提供 $N$ 个整数或切片对象或两种混合来选中 $N$ 维数组的单个成员或“子部分”。我们也了解了当你提供少于 $N$ 个索引时NumPy会自动将剩余位置用末尾切片“填满”。请记住，索引从0开始，所以 `x` 的第4列对应着列3。

```python 
# 访问 `x` 行1，最后一列的成员
>>> x[1, -1]
8

# 访问 `x` 在前两行和前三列中的子数组
>>> x[:2, :3]
array([[-5,  2,  0],
       [-1,  9,  3]])

# 当我们提供索引数少于维度时，NumPy会自动
# 用“末尾”切片填满空余的位置
>>> x[0]  # 等值于 x[0, :]
array([-5,  2,  0, -7])
```

请回忆，我们熟悉的[切片](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Slicing)语法其实在“幕后”会创建 `slice` 对象。

```python
# 复习 `slice` 对象

# 等值于：x[:2, :3]
>>> x[slice(None, 2), slice(None, 3)]
array([[-5,  2,  0],
       [-1,  9,  3]])
```
<!-- #endregion -->

### 将元组作为N维索引使用
根据基础索引的定义，我们必须将我们的数组索引以元组的形式来提供才能使用基础索引。事实上，我们一直以来都在使用索引元组！也就是说，每当我们使用语法 `x[i, j, k]` 来索引一个数组时，我们其实是在创建一个包含这些索引的[元组](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Tuples)。也就是说，`x[i, j, k]` 等值于 `x[(i, j, k)]`。

`x[i, j, k]` 将创建元组 `(i, j, k)` 并将其输入到数组的“获取成员”功能中。因此， `x[0, 3]` 等值于 `x[(0, 3)]`。

<!-- #region -->
```python
# 使用元组的N维索引
# `x[i, j, k]` 等值于 `x[(i, j, k)]`

# 等值于：x[1, -1]
>>> x[(1, -1)]  
8

# 等值于：x[:2, :3]
>>> x[(slice(None, 2), slice(None, 3))]  
array([[-5,  2,  0],
       [-1,  9,  3]])

# 等值于：x[0]
>>> x[(0,)]
array([-5,  2,  0, -7])
```

所有在这个“获取成员”语法中使用的对象都会被打包成一个元组。比如说，`x[0, (0, 1)]` 等值于 `x[(0, (0, 1))]`。你可能会惊讶于这是合法索引这一事实。但是，请注意，*它并不使用基础索引*；在这里使用的索引是包含一个整数*和另一个数组*的元组，所以这不符合基础索引的规则。

最后，请注意，基础索引的规则只允许索引*元组*。提供索引列表会导致你使用进阶索引，而不是基础索引！

```python
# 基础索引只接受元组
>>> x[(1, -1)]  
8

# 使用列表来索引会导致你使用进阶索引
>>> x[[1, -1]]
array([[-1,  9,  3,  8],
       [-3, -3,  4,  6]])
```
<!-- #endregion -->

<!-- #region -->
### 省略号和newaxis对象
根据从之前对广播的讨论，`numpy.newaxis` 对象可以作为数组的索引使用来[向数组插入大小为1的维度](http://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/Broadcasting.html#Inserting-Size-1-Dimensions-into-An-Array)。

```python
# 通过 `np.newaxis` 插入大小为1的维度
>>> x.shape
(3, 4)

>>> x[np.newaxis, :, :, np.newaxis].shape
(1, 3, 4, 1)

# 使用显式的元组来当索引
>>> x[(np.newaxis, slice(None), slice(None), np.newaxis)].shape
(1, 3, 4, 1)
```

我们也可以使用内置的 `Ellipsis`（省略号）对象来在索引中插入切片来强制让索引填满和数组维度一样多的索引。就像 `:` 可以用来代表 `slice` 对象一般，`...` 也可以用来代表 `Ellipsis` 对象。

```python
>>> y = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7]],
...        
...               [[ 8,  9, 10, 11],
...                [12, 13, 14, 15]],
...        
...               [[16, 17, 18, 19],
...                [20, 21, 22, 23]]])

# 等值于：`y[:, :, 0]`
>>> y[..., 0]
array([[ 0,  4],
       [ 8, 12],
       [16, 20]])

# 使用显式的元组索引
>>> y[(Ellipsis, 0)]
array([[ 0,  4],
       [ 8, 12],
       [16, 20]])

# 等值于：`y[0, :, 1]`
>>> y[0, ..., 1]
array([1, 5])
```

一个索引不能拥有超过一个 `Ellipsis` 在其中。这在你处理不同维度的数组时极其有用。来访问某数组 `z` 的所有维度的列0，如果是2维数组会是 `z[:, 0]`，如果是3维数组是 `z[:, :, 0]`，以此重复。`z[..., 0]` 则能够简短地包装这里所有的迭代。
<!-- #endregion -->

<div class="alert alert-info"> 

**经验**：

基础索引将在你向NumPy数组输入的索引是一个元组，其成员为：整数，`slice`，`numpy.newaxis`，或 `Ellipsis` 对象。通过基础索引返回的数组是原本数组内置数据的一个*视阈*；基础索引并不会导致任何数据被复制。

</div>


<div class="alert alert-info"> 


**阅读理解：省略号**

设 $N$ 维数组 `x`，索引 `x` 来获取轴0的第一个成员，轴 $N-1$ 的最后一个成员，并切片中间所有维度。$N$ 至少为 $2$。

</div>


<div class="alert alert-info"> 

<!-- #region -->
**阅读理解：基础索引**

设形状为 (4, 3) 的数组

```python
>>> arr = np.array([[ 0,  1,  2,  3],
...                 [ 4,  5,  6,  7],
...                 [ 8,  9, 10, 11]])
```

以下哪些索引方法使用基础索引？也就是说，以下哪些索引满足了基础索引的规则？

 - `arr[0]`
 - `arr[:-1, 0]`
 - `arr[(2, 3)]`
 - `arr[[2, 0]]`
 - `arr[np.array([2, 0])]`
 - `arr[(0, 1), (2, 3)]`
 - `arr[slice(None), ...]`
 - `arr[(np.newaxis, 0, slice(1, 2), np.newaxis)]`

</div>
<!-- #endregion -->

<!-- #region -->
## 创建数组视阈
如上所述，使用基础索引不会返回访问数据的复制品；反而，它会返回内置数据的一个*视阈*（view）。NumPy提供了 `numpy.shares_memory` 函数来检查两个数组是否引用了相同的内置数据。

```python
>>> z = np.array([[ 3.31,  4.71,  0.4 ],
...               [ 0.21,  2.85,  3.21],
...               [-3.77,  4.53, -1.15]])

# `subarray` 是通过基础索引获得的 `z` 的列0
>>> subarray = z[:, 0]
>>> subarray
array([ 3.31,  0.21, -3.77])

# `subarray` 是 `z` 的数组数据的一个视阈
>>> np.shares_memory(subarray, z)
True
```

通过基础索引返回的单个数字*不*和父数组共享内存。
```python
>>> z[0, 0]
3.31

>>> np.shares_memory(z[0, 0], z)
False
```
函数 `numpy.copy` 可以用来复制数组，使得返回的数组并不和输入的数组共享内存。

```python
# 创建一个数组的复制品
>>> new_subarray = np.copy(subarray)
>>> new_subarray
array([ 3.31,  0.21, -3.77])

>>> np.shares_memory(new_subarray, z)
False
```

通过算数操作符（`+, -, *, /, //, **`）使用的数组会返回完全不同，和原本数组不共享内存的数组。

```python
# 像 `subarray + 2` 这样的数学表达式将创建不同的数组，
# 而不是视阈
>>> np.shares_memory(subarray + 2, subarray)
False
```

因此，通过 `subarray = subarray + 2` 更新 `subarray` *不*会重现 `subarray` 引用的原本数据。反而，`subarray + 2` 会将新创建的数组赋值给变量 `subarray`。NumPy也提供了直接更新原本数组内置数据而不创建新数组的数学操作。我们将在下一小节讨论这些功能。
<!-- #endregion -->

<div class="alert alert-info"> 

<!-- #region -->
**阅读理解：视阈**

设

```python
x = np.array([[ 0,  1,  2,  3],
              [ 4,  5,  6,  7],
              [ 8,  9, 10, 11]])
```

以下哪些表达式创建了 `x` 的视阈？也就是说，在哪些情况下 `x` 和被创建的变量引用着相同的内置数组数据？使用 `np.shares_memory` 来检查你的答案。

- `a1 = x`
- `a2 = x[0, 0]`
- `a3 = x[:, 0]`
- `a4 = x[:, 0] + np.array([-1, -2, -3])`
- `a5 = np.copy(x[:, 0])`
- `a6 = x[np.newaxis]`
- `a7 = x.reshape(2, 3, 2)`
- `a8 = 2 + x`

</div>
<!-- #endregion -->

## 增强数组的内置数据
因为基础索引返回数组内置数据的*视阈*，我们需要花些时间来理解，相比于会创建有着不同数据的新数组的操作，我们可以通过哪些方式来*增强*（augment）输入的内置数据。我们将在其看到：

- 原地赋值
- 增强赋值
- 提供 `out` 参数的NumPy函数

可以用来原地（in-place）增强数组的数据。

<!-- #region -->
### 原地赋值

赋值操作符 `=` 可以被用来原地更新数组的数据。设数组 `a` 和它的视阈 `b`。
```python
>>> a = np.array([0, 1, 2, 3, 4])
>>> b = a[:]
>>> np.shares_memory(a, b)
True
```

为 `a` 赋值一个新数组仅仅改变了 `a` 引用的数据，并使得 `a` 和 `b` 没有关联，而并不会修改 `b` 。
```python
# `a` 在赋值后引用另外一个数组
>>> a = np.array([0, -1, -2, -3, -4])

# `b` 依然引用着原本的数据
>>> b
array([0, 1, 2, 3, 4])

>>> np.shares_memory(a, b)
False
```

为 `a` 的视阈，也就是 `a[:]`，进行操作会告诉NumPy去通过赋值原地代替 `a` 的数据。

```python
# 重新初始化`a` 和 `b`。
# `b` 任然是 `a` 的视阈
>>> a = np.array([0, 1, 2, 3, 4])
>>> b = a[:]

# 向 `a` 的*视阈*赋值一个新数组会导致NumPy原地
# 更新其数据
>>> a[:] = np.array([0, -1, -2, -3, -4])
>>> a
array([ 0, -1, -2, -3, -4])

# `b` 任然是相同数据的视阈，因此它受到这个原地赋值
# 的影响
>>> b
array([ 0, -1, -2, -3, -4])

>>> np.shares_memory(a, b)
True
```

这个视阈赋值功能可以用来原地更新数组的一部分。

```python
>>> p = np.array([[ 0,  1,  2,  3],
...               [ 4,  5,  6,  7],
...               [ 8,  9, 10, 11]])
>>> q = p[0, :]

# 设行0，列0为值-40
# 设行0，列2为值-50
>>> p[0, ::2] = (-40, -50)

# 为 `p` 的一部分广播赋值-1
>>> p[1:, 2:] = -1
>>> p
array([[-40,   1, -50,   3],
       [  4,   5,  -1,  -1],
       [  8,   9,  -1,  -1]])
```
同样的，这将会更新内置的数据，所以这数据的所有视阈都会反映这个变化。
```python
# `q` 依然是 `p` 第0行的视阈
>>> q
array([-40,   1, -50,   3])
```
<!-- #endregion -->

<!-- #region -->
### 增强赋值
我们对Python基础数学表达式的讨论提到了[增强赋值语句](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Augmented-Assignment-Statements)（augmented assignment statement）。它为我们提供了一个方便的更新变量值的简写语法。比如说，赋值表达式 `x = x + 5` 可以通过增强赋值来重写为 `x += 5`。

虽然 `x += 5` 在处理Python基础对象（整数，浮点数，等等）时真的只是一个简写，*但是NumPy数组的增强赋值和它们更长的版本有着根本性不同的行为*。具体来讲，它们会直接更新数组引用的内置数据，而不会创建一个新的不同的数组。因此这会影响所有该数组的视阈。我们在以下演示。

```python
# 演示对NumPy数组的增强赋值会更新该数组引用的内置数据。
>>> a = np.array([[ 0,  1,  2,  3],
...               [ 4,  5,  6,  7],
...               [ 8,  9, 10, 11]])

# `b` 和 `c` 都是通过基础索引创建的 `a` 第0行的视阈
>>> b = a[0]
>>> c = a[0]
>>> np.shares_memory(a, b) and np.shares_memory(a, c)
True

# 使用数学表达式更新 `b` 会创建一个新数组并消除 `a` 和 `c` 的关系
>>> b = b * -1
>>> b
array([ 0, -1, -2, -3])

>>> np.shares_memory(a, b)
False

# 使用增强赋值更新 `c` 会更新 `c` 引用的内置数据
>>> c *= -2
>>> c
array([ 0, -2, -4, -6])

>>> np.shares_memory(a, c)
True

# 请注意，这个更新也会在 `a` 中反映，因为它依然和 `c` 共享内存
>>> a
array([[ 0, -2, -4, -6],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
```
<!-- #endregion -->

<!-- #region -->
### 提供 `out` 来原地进行NumPy操作
我们没有任何理由只能在算数操作时可以原地更新数据。[NumPy的各种数学函数](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/VectorizedOperations.html#NumPy%E2%80%99s-Mathematical-Functions)提供了一个可选的关键词参数 `out` 来提供“储存”数学运算结果的地方。默认的情况下，这个操作会在内存中创建一个不同的新数组并不修改输入数据。

```python
# 在 `numpy.exp` 中提供 'out' 参数来原地更新数组数据

# `b` 是 `a` 的视阈
>>> a = np.array([0., 0.2, 0.4, 0.6, 0.8, 1.])
>>> b = a[:]
>>> np.shares_memory(a, b)
True

# 提供 'out=a' 将会告诉NumPy去覆盖 `a` 引用的数据
>>> np.exp(a, out=a)
array([ 1., 1.22140276, 1.4918247, 1.8221188, 2.22554093, 2.71828183])

# `b` 依然是现在更新过的数据的视阈
>>> b
array([ 1., 1.22140276, 1.4918247, 1.8221188, 2.22554093, 2.71828183])
```

<!-- #endregion -->

<!-- #region -->
### 原地增强数据的好处和危险
理解数组和它们引用的内置数据的关系是极其重要的。*原地增强数据的操作比它们必须为创建新数组来分配内存的版本更加高效*。也就是说，像 `array += 3` 的表达式比 `array = array + 3` 更加高效。

虽然如此，*在不知情的情况下*一不小心原地更新数组数据并影响数组所有视阈则会是一个巨大的错误；这将导致NumPy新手用户代码中出现难以查清的bug。以下函数 `add_3` 将会修改输入数组的数据。

```python
# 在函数内原地更新数组
def add_3(x):
    x += 3 
    return x

>>> x = np.array([0, 1, 2])
>>> y = add_3(x)
>>> y
array([3, 4, 5])

# `x` 在 `add_3(x)` 被调用时都会被更新
>>> x
array([3, 4, 5])
```

除非你本意就是让 `add_3` 修改输入数组，这样做的问题很大。为了修补这个问题，你可以在函数开头复制输入数组；在这之后，你可以随意地使用增强语句原地更新复制过的数据。
```python
def add_3(x):
    x = np.copy(x)
    x += 3 
    return x
```
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info"> 

**阅读理解：原地增强数组数据**

设

```python
x = np.array([[ 0.,  1.,  2.,  3.],
              [ 4.,  5.,  6.,  7.],
              [ 8.,  9., 10., 11.]])

y = x[0, :]
```

以下哪些表达式会更新 `x` 原本引用的数据？

```python 
# 1.
>>> x += 3
```

```python 
# 2.
>>> y *= 2.4
```

```python 
# 3.
>>> x = x + 3
```

```python 
# 4.
>>> y = np.copy(y)
>>> y += 3
```

```python 
# 5.
>>> np.log(x[1:3], out=x[1:3])
>>> y += 3
```

```python 
# 6.
>>> y[:] = y + 2
```

```python 
# 7.
>>> y = y + 2
```

```python 
# 8.
>>> x[:] = 0
```

```python 
# 9.
>>> def f(z): z /= 3
>>> f(y)
```

</div>
<!-- #endregion -->

<div class="alert alert-info"> 

**经验**：

赋值数组视阈，增强赋值语句，和提供 `out` 参数的NumPy函数都是原地增强数组数据的方法。这将会影响任何相同数据的视阈。同时，这些原地更新的操作比它们需要分配内存来创建新数组的版本更加高效。虽然如此，请不要胡乱使用原地数据增强，因为这早晚在你代码中会导致恶性的bug。

</div>


## 官方说明文档链接

- [基础索引](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html#indexing)
- [“视阈”的定义](https://docs.scipy.org/doc/numpy/glossary.html#term-view)


## 阅读理解答案：


**省略号：解**

设 $N$ 维数组 `x`，索引 `x` 来获取轴0的第一个成员，轴 $N-1$ 的最后一个成员，并切片中间所有维度。$N$ 至少为 $2$。

在这个索引中使用 `Ellipsis` 对象将允许我们告诉NumPy在 `x` 中间的 $N - 2$ 个轴中插入切片：

`x[0, ..., -1]` 或 `x[0, Ellipsis, -1]`


**基础索引：解**

以下哪些的索引满足了基础索引的规则？

 - `arr[0]` ✔
 - `arr[:-1, 0]`  ✔
 - `arr[(2, 3)]`  ✔
 - `arr[[2, 0]]`  ✘（索引是一个 `list`，而不是 `tuple`）
 - `arr[np.array([2, 0])]` ✘（索引是一个 `numpy.ndarray`，而不是 `tuple`）
 - `arr[:, (2, 3)]`  ✘（索引包含了一个元组；只有`int`，`slice`，`np.newaxis`，`Ellipsis` 是被允许的）
 - `arr[slice(None), ...]`  ✔
 - `arr[(np.newaxis, 0, slice(1, 2), np.newaxis)]`  ✔

<!-- #region -->
**视阈：解**

设

```python
x = np.array([[ 0,  1,  2,  3],
              [ 4,  5,  6,  7],
              [ 8,  9, 10, 11]])
```

以下哪些表达式创建了 `x` 的视阈？也就是说，在哪些情况下 `x` 和被创建的变量引用着相同的内置数组数据？使用 `np.shares_memory` 来检查你的答案。

- `a1 = x` ✔
- `a2 = x[0, 0]` ✘；当基础索引返回单个数字时，这个数字不和父数组共享内存。
- `a3 = x[:, 0]` ✔
- `a4 = x[:, 0] + np.array([-1, -2, -3])` ✘；对NumPy数组的算数操作默认创建不同的数组。
- `a5 = np.copy(x[:, 0])` ✘；`numpy.copy` 指示NumPy去创建数组的复制品。
- `a6 = x[np.newaxis]` ✔
- `a7 = x.reshape(2, 3, 2)` ✔
- `a8 = 2 + x` ✘；对NumPy数组的算数操作默认创建不同的数组。
<!-- #endregion -->

<!-- #region -->
**原地增强数组数据：解**

设

```python
x = np.array([[ 0.,  1.,  2.,  3.],
              [ 4.,  5.,  6.,  7.],
              [ 8.,  9., 10., 11.]])

y = x[0, :]
```

以下哪些表达式会更新 `x` 原本引用的数据？

```python 
# 1.
>>> x += 3 ✔
```

```python 
# 2.
>>> y *= 2.4 ✔
```

```python 
# 3.
>>> x = x + 3 ✘
```

```python 
# 4.
>>> y = np.copy(y)
>>> y += 3 ✘
```

```python 
# 5.
>>> np.log(x[1:3], out=x[1:3]) ✔
```

```python 
# 6.
>>> y[:] = y + 2 ✔
```

```python 
# 7.
>>> x = np.square(x) ✘
```

```python 
# 8.
>>> x[:] = 0 ✔
```

```python 
# 9.
>>> def f(z): z /= 3
>>> f(y) ✔
```

```python 
# 10.
>>> np.square(y, out=y) ✔
```
<!-- #endregion -->
