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
   :description: Topic: Advanced indexing with numpy arrays, Difficulty: Hard, Category: Section
   :keywords: numpy array, integer array indexing, boolean array indexing, copy indexing, advanced
```

<!-- #region -->
# 进阶索引

我们将通过理解进阶索引来为我们对索引N维NumPy数组的讨论收尾。和允许我们访问数组单个成员和规则切片的基础索引不同，进阶索引相比灵活很多。比如说，我们可以使用整数数组来作为索引访问数组任意多甚至重复的成员：

```python
>>> import numpy as np

>>> x = np.array([[0, 1, 2],
...               [3, 4, 5],
...               [6, 7, 8]])

# 利用 `x` 的成员来创建以下2维数组：
#
#     [[x[0, 0], x[0, 1]],
#      [x[2, 2], x[2, 2]]]
>>> rows = np.array([[0, 0], 
...                  [2, 2]])

>>> cols = np.array([[0, 1], 
...                  [2, 2]])

>>> x[rows, cols]
array([[0, 1],
       [8, 8]])
```

同时，它允许我们使用*布尔值*数组来作为索引：

```python
# 使用布尔值数组来访问数组对角线的值

# 在我们想要访问的 `x` 的成员的位置提供 `True`
>>> bool_index = np.array([[ True, False, False],
...                        [False,  True, False],
...                        [False, False,  True]])

>>> x[bool_index]
array([0, 4, 8])
```

和基础索引不同，进阶索引永远都会返回内置数据的复制品。
```python
>>> np.shares_memory(x, x[rows, cols])
False

>>> np.shares_memory(x, x[bool_index])
False
```
进阶索引提供的灵活性使得它很难在不深入理解一些抽象极短的符号的情况下完整地讨论。如果读者想要一个追求完整性的讨论，请参阅[官方说明文档](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html#advanced-indexing)。在这里，我们将会尽可能平衡深入性和易读性地讨论进阶索引的核心功能。
<!-- #endregion -->

<div class="alert alert-info"> 

**进阶索引**：

设 $N$ 维数组 `x`，当 `index` 为以下之一时，`x[index]` 使用**进阶索引**（advanced indexing）：

- 整数或布尔类的 `numpy.ndarray`
- 一个至少有着一个*序列*类型对象（比如说列表，元组，或ndarray）成员的 `tuple`

通过进阶索引访问这些成员**永远都会返回数据的复制品*，而基础索引返回一个视阈。

</div>

<!-- #region -->
## 整数数组索引

### 索引一维数组
使用整数数组作为索引允许我们任意——无序甚至重复地——访问数组成员。设以下1维数组：

```python
y = np.array([ 0, -1, -2, -3, -4, -5])
```

我们可以无规律地访问任意多的数组成员；这在使用基础索引时是不允许的：

```python
# 使用整数数组进行进阶索引
>>> index = np.array([2, 4, 0, 4, 4, 4])
>>> y[index]
array([-2, -4,  0, -4, -4, -4])
```

<!-- #endregion -->

使用这种方法访问 `y` 成员的规则很容易理解。整数数组的每一个成员都被用来访问 `y` 的一个成员，如下：


\begin{equation}
\left(
\begin{array}{*{6}{X}}
  y[2] & y[4] & y[0] & y[4] & y[4] & y[4]
\end{array}
\right)
% 
\rightarrow
\left(
\begin{array}{*{6}{X}}
  -2 & -4 & 0 & -4 & -4 & -4
\end{array}
\right)
\end{equation}


和所有进阶索引的结果一样，这将返回数据的*复制品*。

<!-- #region -->
```python
# 进阶索引返回复制品
>>> np.shares_memory(y, y[index])
False
```
<!-- #endregion -->

<!-- #region -->
索引数组可以有着任意的形状；*返回的数组会有着相同的形状*。

```python
# 使用2维数组作为索引
>>> index_2d = np.array([[ 1,  2,  0],
...                      [ 5,  5,  5],
...                      [ 2,  3,  4]])

# 返回的数组形状于索引数组一样
>>> y[index_2d]
array([[-1, -2,  0],
       [-5, -5, -5],
       [-2, -3, -4]])
```
<!-- #endregion -->

\begin{equation}
\left(
\begin{array}{*{3}{X}}
  y[1] & y[2] & y[0] \\
  y[5] & y[5] & y[5] \\
  y[2] & y[3] & y[4]
\end{array}
\right)
% 
\rightarrow
\left(
\begin{array}{*{3}{X}}
  -1 & -2 & 0 \\
  -5 & -5 & -5 \\
  -2 & -3 & -4
\end{array}
\right)
\end{equation}

<!-- #region -->
<div class="alert alert-info"> 

**阅读理解：整数数组索引（1维）**

设以下数组：

```python
y = np.array([ 0, -1, -2, -3, -4, -5])
```

通过进阶索引，使用一个整数数组索引来产生以下数组：

```python
# 1
array([-1])

#2
array([-1, -2, -1, -2])

#3
array([[ 0, -5],
       [-1, -4]])

#4
array([[-2],
       [-3],
       [-2]])
```

</div>
<!-- #endregion -->

<!-- #region -->
### 索引N维数组

在之前处理1维数组的例子中，我们提供了一个索引数组来通过数组唯一的维度访问其成员。你可能也猜到了，为了用相似的方法访问 $N$ 维数组，我们必须提供 $N$ 个索引数组；每个对应一个维度。

所有 $N$ 个索引数组必须有着一样的形状，而且它们共享的形状决定了返回数组的形状。所有 $N$ 个索引数组的对应成员将用来作为索引来访问数组的一个具体的成员。比如说，设以下3维数组：

```python
# 使用整数索引数组索引一个3维数组
>>> z = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7],
...                [ 8,  9, 10, 11]],
...
...               [[12, 13, 14, 15],
...                [16, 17, 18, 19],
...                [20, 21, 22, 23]]])
```

我们输入三个索引数组，分别对应着在轴0，轴1，和轴2的该访问的索引。如果我们项返回数组：`array([ 3, 23,  4])`，这些成员的位置如下：

- `3`：页0，行0，列3
- `23`：页1，行2，列3
- `4`：页0，行1，列0

每个索引数组必须有着形状 (3,) 来返回正确形状的数组。为轴0输入的数组必须是 `np.array([0, 1, 0])` 来为 `3` 选择页0，为 `23` 选择页1，和为 `4` 选择页0。另外两个索引数组也是相似的。

```python
# 描述每个成员的页
>>> ind0 = np.array([0, 1, 0])

# 描述每个成员的行
>>> ind1 = np.array([0, 2, 1])

# 描述每个成员的列
>>> ind2 = np.array([3, 3, 0])

>>> z[ind0, ind1, ind2]
array([ 3, 23,  4])
```
<!-- #endregion -->

正式来讲，索引数组将通过[行优先顺序](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/ArrayTraversal.html)被遍历，而每一个整数索引的组合将被用来索引 `z` 并填入返回数组的对应位置。


\begin{equation}
   z\big[ind_0[0], ind_1[0], ind_2[0]\big]
% 
\rightarrow
out[0] \\
   z\big[ind_0[1], ind_1[1], ind_2[1]\big]
% 
\rightarrow
out[1] \\
   z\big[ind_0[2], ind_1[2], ind_2[2]\big] 
% 
\rightarrow
out[2] \\
\end{equation}

\begin{equation}
\left(
\begin{array}{*{3}{X}}
   z[0, 0, 3] & z[1, 2, 3] & z[0, 1, 0] 
\end{array}
\right)
% 
\rightarrow
\left(
\begin{array}{*{3}{X}}
  3 & 23 & 4
\end{array}
\right)
\end{equation}

<!-- #region -->
```python
# 使用整数索引数组来返回一个形状为 (2, 2) 的数组
>>> ind0 = np.array([1, 1, 0, 1]).reshape(2, 2)
>>> ind1 = np.array([1, 2, 0, 0]).reshape(2, 2)
>>> ind2 = np.array([1, 3, 1, 3]).reshape(2, 2)
>>> z[ind0, ind1, ind2]
array([[17, 23],
       [ 1, 15]])
```

\begin{equation}
\left(
\begin{array}{*{3}{X}}
  z[1, 1, 1] & z[1, 2, 3] \\
  z[0, 0, 1] & z[1, 0, 3]
\end{array}
\right)
% 
\rightarrow
\left(
\begin{array}{*{3}{X}}
  17 & 23 \\
  1 & 15 
\end{array}
\right)
\end{equation}
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info"> 

**阅读理解：整数数组索引（N维）**

设以下数组：

```python
>>> z = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7],
...                [ 8,  9, 10, 11]],
...
...               [[12, 13, 14, 15],
...                [16, 17, 18, 19],
...                [20, 21, 22, 23]]])
```

通过进阶索引，使用整数数组索引来返回以下数组：

```python
# 1
array([[ 0,  5, 10],
       [12, 17, 22]])

#2
array([[ 0, 23],
       [23,  0]])

```

</div>
<!-- #endregion -->

<div class="alert alert-info"> 

**经验**：

$N$ 维数组的成员可以通过输入 $N$ 个整数的索引数组来访问，每个索引对应着数据的一个轴。索引数组之间形状必须相同，而它们共享的形状决定了返回数组的形状。这是进阶索引的一种，因此这将创建一份父数组的复制品。

</div>

<!-- #region -->
## 布尔数组索引
NumPy也支持使用布尔值数组来作为索引的进阶索引数组的方法。在最简单的情况下，这是一个非常易懂优雅的根据逻辑条件选中数组成员的方法。

```python
# 使用布尔数组进行进阶索引
>>> x = np.array([[[-0.26,  0.49,  0.18],
...                [ 0.43,  0.3 ,  0.29]],
...        
...               [[-0.44,  0.3 ,  0.28],
...                [ 0.27, -0.09, -0.13]]])

# 当 `x` 为正数时是 `True`
>>> bool_ind = x > 0
>>> bool_ind
array([[[False,  True,  True],
        [ True,  True,  True]],

       [[False,  True,  True],
        [ True, False, False]]], dtype=bool)

>>> x[bool_ind]
array([ 0.49,  0.18,  0.43,  0.3 ,  0.29,  0.3 ,  0.28,  0.27])

>>> np.shares_memory(x, x[bool_ind])
False
```
在最简单的情况下，布尔索引的行为如下：设 `x` 为 $N$ 维数组，`ind` 为一个和 `x` *一样形状*的布尔数组。那么 `x[ind]` 将返回一个1维数组，根据使用[行优先顺序](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/ArrayTraversal.html)遍历 `x` 和 `ind` 产生。每当 `ind` 的一个成员为 `True` 时，就往返回数组添加 `x` 的对应成员。请检查以上范例来确认这个行为是否如这里所描述一样。
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info"> 

**R阅读理解：布尔索引**

设以下数组：

```python
>>> h = np.array([[ 0.01,  0.03,  0.1 ,  0.25],
...               [ 0.38,  0.22,  0.15,  0.34],
...               [-0.29,  0.13, -0.26,  0.33]])
```

使用布尔数组索引和NumPy的[逻辑函数](https://docs.scipy.org/doc/numpy/reference/routines.logic.html)来选中 `h` 满足一下条件的成员。因为你在处理浮点数，[你不应该检查两值完全相等](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Understanding-Numerical-Precision)；反而，使用函数[numpy.isclose](https://docs.scipy.org/doc/numpy-dev/reference/generated/numpy.isclose.html)。

1. 所有 `h` 的负成员
2. 所有 `h` “等于” `0.01` 或 `0.33` 的成员
3. 所有 `h` 在域 `(0.1, 0.3)` 之间的成员。

</div>
<!-- #endregion -->

<!-- #region -->
#### 将布尔索引数组转化为整数索引数组：numpy.where
函数[numpy.where](https://docs.scipy.org/doc/numpy/reference/generated/numpy.where.html)可以用来接受一个布尔值数组并返回一*元组*的通过整数数组索引（在本节开头讨论过）访问数组中为 `True` 的成员的索引数组。

```python
# 演示 `np.where`
>>> bool_ind
array([[[False,  True,  True],
        [ True,  True,  True]],

       [[False,  True,  True],
        [ True, False, False]]], dtype=bool)

>>> np.where(bool_ind)
(array([0, 0, 0, 0, 0, 1, 1, 1], dtype=int64),
 array([0, 0, 1, 1, 1, 0, 0, 1], dtype=int64),
 array([1, 2, 0, 1, 2, 1, 2, 0], dtype=int64))
```

本例返回了三个整数索引数组的元组，每个数组对应 `bool_ind` 的一个维度。这些索引数组描述了 `bool_ind` 中为 `True` 的位置。同时，请回忆，`bool_ind` 是为了访问 `x` 中正数而创建的数组；这个函数返回的索引数组元组也可以用来达成相同的目的。

```python
>>> ind0, ind1, ind2 = np.where(bool_ind)
>>> x[ind0, ind1, ind2]
array([ 0.49,  0.18,  0.43,  0.3 ,  0.29,  0.3 ,  0.28,  0.27])

# 没有必要解包这些数组。你可以之间将元组作为索引使用
>>> x[np.where(bool_ind)]
array([ 0.49,  0.18,  0.43,  0.3 ,  0.29,  0.3 ,  0.28,  0.27])
```

`np.where` 在我们想要了解数组的*哪里*（where）满足了某个特殊条件时很有用。比如说，假设我们想要知道 `x` 中的哪些页包含了大于0.4的值：

```python
# `x` 的哪些页包含大于0.4的值？

>>> ind0, ind1, ind2 = np.where(x > 0.4)

# 删除重复的答案
>>> np.unique(ind0)
array([0], dtype=int64)

# 只有页0有这样的值
```
<!-- #endregion -->

有着对NumPy[逻辑函数](https://docs.scipy.org/doc/numpy/reference/routines.logic.html)的理解，布尔数组索引为访问数组成员提供了一个方便，不在乎数组形状和内容格式的操作界面。这种索引方法在对数组进行进行增强更新时极其有用。我们将在下一小节讨论它。

<!-- #region -->
<div class="alert alert-info"> 

**阅读理解：numpy.where**

设数组：

```python
>>> b = np.array([[False, False,  True],
...               [False,  True, False],
...               [ True,  True, False]], dtype=bool)
```

*预测* `np.where(b)` 的输出是什么。
</div>
<!-- #endregion -->

<!-- #region -->
## 通过进阶索引进行原地和增强赋值

虽然进阶索引不提供[内置数据的*视阈*](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/BasicIndexing.html#Producing-a-View-of-an-Array)，它依然可以用来帮助我们对数组其中的区域进行原地和增强赋值。比如说，假设你想要对数组进行阀值操作，使得其所有负成员都被0代替。通过原地复制，布尔索引将会使得这个工作极其轻松：

```python
# 通过进阶索引赋值
>>> x = np.array([[ 0.38, -0.16,  0.38, -0.41, -0.04],
...               [-0.47, -0.01, -0.18, -0.5 , -0.49],
...               [ 0.02,  0.4 ,  0.33,  0.33, -0.13]])

# 将所有 `x` 的负成员设为0（这里自动使用了广播）
>>> x[x < 0] = 0
>>> x
array([[ 0.38,  0.  ,  0.38,  0.  ,  0.  ],
       [ 0.  ,  0.  ,  0.  ,  0.  ,  0.  ],
       [ 0.02,  0.4 ,  0.33,  0.33,  0.  ]])
```

我们也将演示使用整数索引数组来进行增强赋值的方法。
```python
# 通过整数数组进行增强赋值
>>> ind0 = np.array([0, -1])
>>> ind1 = np.array([0, 1])
>>> x[ind0, ind1]
array([ 0.38, 0.4])

# 等值于：x[ 0, 0] *= 100
#        x[-1, 1] *= 100
>>> x[ind0, ind1] *= 100

>>> x
array([[  38.,   0. , 0.38 ,   0.,  0.  ],
       [   0.,   0. ,    0.,   0.,  0.  ],
       [ 0.02,  40. , 0.33,  0.33,  0.  ]])
```
<!-- #endregion -->

<!-- #region -->
请回忆，你可以通过整数数组索引来访问数组中重复的成员。增强赋值*只会为重复成员进行一次*。

```python
>>> y = np.array([4, 6, 8])

# y[0]被访问了三次，而y[2]被访问了一次
>>> y[np.array([0, 0, 0, 2])]
array([4, 4, 4, 8])

# 增强更新仅仅对y[0]进行一次
>>> y[np.array([0, 0, 0, 2])] += 1
>>> y
array([5, 6, 9])
```
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info"> 

**阅读理解：使用进阶索引赋值**

设数组：

```python
>>> x = np.array([[ 0.58,  0.05,  0.84,  0.21],
...               [ 0.88,  0.98,  0.45,  0.13],
...               [ 0.1 ,  0.52,  0.58,  0.38],
...               [ 0.84,  0.76,  0.25,  0.07]])
```

将 `x` 的对角线成员代替为 `(-1, -2, -3, -4)`，并对 `x` 中所有大于 `0.8` 的成员加 `1`。
</div>
<!-- #endregion -->

<!-- #region -->
## 混合使用基础和进阶索引
整数和布尔值数组可以和 `slice`，`numpy.newaxis`，以及 `int` 对象混合使用来创建同时使用基础和进阶索引模式的索引。

```python
# 混合使用进阶和基础索引技巧
>>> z = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7],
...                [ 8,  9, 10, 11]],
...
...               [[12, 13, 14, 15],
...                [16, 17, 18, 19],
...                [20, 21, 22, 23]]])

>>> ind0 = np.array([True, False])

# 选中页0，所有行，和最后一列
>>> z[ind0, :, -1]
array([[ 3,  7, 11]])
```
<!-- #endregion -->

处理混合基础和进阶索引的规则不能被简短地在此讨论。请查看[官方NumPy说明文档](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html#combining-advanced-and-basic-indexing)来仔细阅读这些规则。实际操作中，基础和进阶索引一般都会单独使用。


## 官方说明文档链接

- [进阶索引](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html#advanced-indexing)
- [逻辑函数](https://docs.scipy.org/doc/numpy/reference/routines.logic.html)
- [numpy.where](https://docs.scipy.org/doc/numpy/reference/generated/numpy.where.html)
- [混合基础和进阶索引](https://docs.scipy.org/doc/numpy/reference/arrays.indexing.html#combining-advanced-and-basic-indexing)


## 阅读理解答案：

<!-- #region -->
**整数数组索引（1维）：解**

```python
y = np.array([ 0, -1, -2, -3, -4, -5])
```

```python
# 1
>>> ind1 = np.array([1])
>>> y[ind1]
array([-1])

#2
>>> ind2 = np.array([1, 2, 1, 2])
>>> y[ind2]
array([-1, -2, -1, -2])

#3
>>> ind3 = np.array([[0, 5],
...                  [1, 4]])
>>> y[ind3]
array([[ 0, -5],
       [-1, -4]])

#4
>>> ind4 = np.array([[2],
...                  [3],
...                  [2]])
>>> y[ind4]
array([[-2],
       [-3],
       [-2]])
```
<!-- #endregion -->

<!-- #region -->
**整数数组索引（N维）：解**

```python
>>> z = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7],
...                [ 8,  9, 10, 11]],
...
...               [[12, 13, 14, 15],
...                [16, 17, 18, 19],
...                [20, 21, 22, 23]]])
```

```python
# 1
>>> ind0 = np.array([0, 0, 0, 1, 1, 1]).reshape(2, 3)
>>> ind1 = np.array([0, 1, 2, 0, 1, 2]).reshape(2, 3)
>>> ind2 = np.array([0, 1, 2, 0, 1, 2]).reshape(2, 3)
>>> z[ind0, ind1, ind2]
array([[ 0,  5, 10],
       [12, 17, 22]])

# 2
>>> ind0 = np.array([0, 1, 1, 0]).reshape(2, 2)
>>> ind1 = np.array([0, 2, 2, 0]).reshape(2, 2)
>>> ind2 = np.array([0, 3, 3, 0]).reshape(2, 2)
>>> z[ind0, ind1, ind2]
array([[ 0, 23],
       [23,  0]])
```
<!-- #endregion -->

<!-- #region -->
**Boolean Indexing: Solution**

```python
>>> h = np.array([[ 0.01,  0.03,  0.1 ,  0.25],
...               [ 0.38,  0.22,  0.15,  0.34],
...               [-0.29,  0.13, -0.26,  0.33]])

# 1
>>> h[h < 0]
array([-0.29, -0.26])

# 2
>>> h[np.logical_or(np.isclose(h, 0.01), np.isclose(h, 0.33))]
array([ 0.01,  0.33])

>>> h[np.logical_and(0.1 < h, h < 0.3)]
array([ 0.25,  0.22,  0.15,  0.13])
```
<!-- #endregion -->

<!-- #region -->
**numpy.where：解**

设数组：

```python
>>> b = np.array([[False, False,  True],
...               [False,  True, False],
...               [ True,  True, False]], dtype=bool)
```

*预测* `np.where(b)` 的输出是什么。

这将会返回一个有着两个整数索引数组的元组。它们分别包含着 `b` 的成员值为 `True` 的位置对应轴0和轴1的索引。这些索引的顺序是根据行优先顺序遍历 `b` 产生的。这将返回：

```python
(array([0, 1, 2, 2], array([2, 1, 0, 1])
```
<!-- #endregion -->

<!-- #region -->
**使用进阶索引赋值：解**

```python
>>> x = np.array([[ 0.58,  0.05,  0.84,  0.21],
...               [ 0.88,  0.98,  0.45,  0.13],
...               [ 0.1 ,  0.52,  0.58,  0.38],
...               [ 0.84,  0.76,  0.25,  0.07]])

>>> x[np.arange(4), np.arange(4)] = range(4)
>>> x[0.8 < x] += 1
>>> x
array([[ 0.  ,  0.05,  1.84,  0.21],
       [ 1.88,  2.  ,  0.45,  0.13],
       [ 0.1 ,  0.52,  3.  ,  0.38],
       [ 1.84,  0.76,  0.25,  4.  ]])
```
<!-- #endregion -->
