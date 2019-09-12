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
   :description: Topic: Creating numpy arrays, Difficulty: Easy, Category: Section
   :keywords: create array, ndarray, ones, random, zeros, empty, examples, arange, linspace, reshape, hstack, vstack
```

<!-- #region -->
# 创建NumPy数组的函数
本节将会讨论创建不同形状和内容的NumPy数组的方法。NumPy提供了非常多的创建数组的函数：

```python
>>> import numpy as np

# 使用Python序列创建数组
>>> np.array([i**2 for i in range(5)])
array([ 0,  1,  4,  9, 16])

# 创建成员都是1的数组
>>> np.ones((2, 4))
array([[ 1.,  1.,  1.,  1.],
       [ 1.,  1.,  1.,  1.]])

# 创建成员为均匀分布数字的数组
>>> np.linspace(0, 10, 5)
array([  0. ,   2.5,   5. ,   7.5,  10. ])

# 通过随机从平均值为1，标准差为5的正态分布中
# 抽取10个数字来创建一个数组
>>> np.random.normal(1, 5, 10)
array([ 2.549537  ,  2.75144951,  0.60031823,  3.75185732,  4.65543858,
        0.55779525,  1.15574987, -1.98461337,  5.39771083, -7.81395192])

# 创建某个特定数据类型的数组
>>> np.array([1.5, 3.20, 5.78], dtype=int)
array([1, 3, 5])
```

## 使用Python序列创建数组
你可以利用NumPy的 `array` 函数利用Python的 `list` 或 `tuple` 来创建数组。NumPy将分析它接受的数据结构来决定数组的维度和形状。比如说，一列表的数字将会被用来创建一个1维数组：

```python
# 一列表的数字会成为1维数组
>>> np.array([1., 2., 3.])  # 形状：(3,)
array([ 1.,  2.,  3.])
```

嵌套的列表/元组将会被用来创建多维数组。比如说，“成员为等长的数字列表的列表”将会成为2维数组；列表中的每一个子列表都会成为数组中的一行。因此，一个长度为2，成员为长度为3的列表将会导致一个形状为 (2,3) 的数组被创建：

```python
# 一列表的数字列表将会创建2维数组
>>> np.array([[1., 2., 3.], [4., 5., 6.]])  # 形状：(2, 3)
array([[ 1.,  2.,  3.],
       [ 4.,  5.,  6.]])
```

“成员为成员为等长的数字列表的等长数字列表的列表”将会成为3维数组，如此反复。请回忆，通过使用重复粘贴，`[0]*3` 将会创建 `[0, 0, 0]`。让我们使用这个来创建两个成员为三个每个包含四个0的列表的列表吧；因此将这个列表输入到 `np.array` 中会创建一个2x3x4的内容为0的数组：
```python
# 一列表的数字0列表的列表会成为一个3维数组
>>> np.array([[[0]*4]*3]*2)
array([[[0, 0, 0, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0]],

       [[0, 0, 0, 0],
        [0, 0, 0, 0],
        [0, 0, 0, 0]]])
```

你很少会需要使用列表来像这样创建高维数组。与其如此，我们将在下面介绍其它更加方便生成高维度数据的数组创建函数。比如说，我们将会看到 `np.zeros` 函数为我们提供了一个更文明的创建高维0数组的方法。
<!-- #endregion -->

<div class="alert alert-warning"> 

**警告**！

你其实*可以*通过成员为*不等长*列表的列表来创建数组。这样做产生的数组并*不是*一个N维数组，因为它没有定义明确的维度。这样做会产生一个*对象数组*（object array），而它并不能享受大部分的NumPy功能和好处。这是NumPy模组比较少见的一个特征。如果你不真的很清楚自己在做什么的话就避免使用它！

</div>

<!-- #region -->
## 创建常量数组：`zeros` 和 `ones`
NumPy提供了函数 `zeros` 和 `ones`，其创建用户提供形状的成员数字分别为0和1的数组。

```python
# 创建一个 3x4 的0数组
>>> np.zeros((3, 4))
array([[ 0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.],
       [ 0.,  0.,  0.,  0.]])

# 创建一个形状为 (4,) 的1数组
>>> np.ones((4,))
array([ 1.,  1.,  1.,  1.])
```

NumPy提供了额外的创建常数值数组的函数。请查看[官方说明文档](https://docs.scipy.org/doc/numpy/reference/routines.array-creation.html#ones-and-zeros)来阅读完整的列表。
<!-- #endregion -->

<!-- #region -->
## 创建序列数组：`arange` 和 `linspace`
[arange](https://docs.scipy.org/doc/numpy/reference/generated/numpy.arange.html#numpy.arange)函数允许你根据一个起始点（start）（包含），结尾点（stop）（不包含），和步距（step）来初始化一序列的整数。这和 `range` 函数很相似；但是，`arange` 将立刻将这个序列创建成数组，而 `range` 返回一个生成器。
```python
>>> np.arange(0, 10, 1)  # start（包含）：0，stop（不包含）：10，step：1
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

# 仅仅为 `arange` 提供一个数字相当于为其提供结尾点
# 默认使用start=0 和 step=1
>>> np.arange(10)  # 等值于：start：0，stop：10，step：1
array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

>>> np.arange(-5, 6, 2) # start（包含）：-5，stop（不包含）：6，step：2
array([-5, -3, -1,  1,  3,  5])
```

[linspace](https://docs.scipy.org/doc/numpy/reference/generated/numpy.linspace.html)函数提供了在用户提供的范围 $[i, j]$（$i$ 和 $j$ 都在这个范围内包括）内生成 $N$ 个*等距*的点的功能。这函数经常用来生成计算数学函数时使用的输入域值（例，在 $-\pi$ to $\pi$ 之间等距计算sine函数的值）。

```python
# 在 [-1, 1] 中生成5个等距的点
>>> np.linspace(-1, 1, 5)
array([-1. , -0.5,  0. ,  0.5,  1. ])

# 在 [3, 4] 中生成2个等距的点
>>> np.linspace(3, 4, 2)
array([ 3.,  4.])

# 在 [-pi, pi] 中生成100个等距的点
>>> np.linspace(-np.pi, np.pi, 100)
array([-3.14159265, ..., 3.14159265])
```

NumPy提供了其它用来创建序列数组的函数，如在对数刻度上创建等距数组。请查看[官方说明文档](https://docs.scipy.org/doc/numpy/reference/routines.array-creation.html#numerical-ranges)来阅读完整列表。
<!-- #endregion -->

<!-- #region -->
## 随机取值来生成数组
在 `np.random` 中有多个函数可以根据用户的形状创建数组，其值由某用户选择的统计分布中随机抽取得到：
```python
# 通过在 [0, 1) 中均匀分布来创建形状为 (3,3) 的数组
>>> np.random.rand(3,3)
array([[ 0.09542611,  0.13183498,  0.39836068],
       [ 0.7358235 ,  0.77640024,  0.74913595],
       [ 0.37702688,  0.86617624,  0.39846429]])

# 通过从平均值为0，方差为1的正态分布（也就是高斯分布）中抽取值
# 来创建形状为 (5,) 的数组
>>> np.random.randn(5)
array([-1.11262121, -0.35392007,  0.4245215 , -0.81995588,  0.65412323])
```
当然，NumPy有着[非常多其它的函数](https://docs.scipy.org/doc/numpy/reference/routines.random.html#distributions)来允许你从很多种类的统计分布中抽取值。本文介绍的内容仅仅NumPy中随机生成数字功能的皮毛而已。

<!-- #endregion -->

<!-- #region -->
## 创建某数据类型的数组
在之前的所有函数中你都可以向其输入一个关键词参数 `dtype` 来指示NumPy在创建函数内容时使用指定的数据类型。

```python
# 向数组中填满32比特的浮点数
>>> np.array([1, 2, 3], dtype="float32") 
array([ 1.,  2.,  3.], dtype=float32)

# `arange` 使用的默认数据类型为32比特整数
>>> np.arange(0, 4).dtype  
dtype('int32')

# 通过设置 `dtype` 你可以调整 `arange` 产生的数据类型
>>> np.arange(0, 4, dtype="float16")
array([ 0.,  1.,  2.,  3.], dtype=float16)

# 生成形状为 (4,4) 的数组，其成员为64比特的复数0
>>> np.zeros((4, 4), dtype="complex64")
array([[ 0.+0.j,  0.+0.j,  0.+0.j,  0.+0.j],
       [ 0.+0.j,  0.+0.j,  0.+0.j,  0.+0.j],
       [ 0.+0.j,  0.+0.j,  0.+0.j,  0.+0.j],
       [ 0.+0.j,  0.+0.j,  0.+0.j,  0.+0.j]], dtype=complex64)
```

请查看[NumPy官方说明文档](https://docs.scipy.org/doc/numpy/user/basics.types.html#array-types-and-conversions-between-types)来阅读完整的可用数组数据类型列表。
<!-- #endregion -->

<!-- #region -->
## 合并数组
和Python的列表和元组类似，1你可以粘连多个NumPy数组。但是，因为NumPy的数组可以是多维度的，我们可以选择粘连数组的维度。
```python
# 演示合并数组的方法
>>> x = np.array([1, 2, 3])
>>> y = np.array([-1, -2, -3])

# 将 `x` 和 `y` “竖向”（vertically）堆积（stack）
>>> np.vstack([x, y])
array([[ 1,  2,  3],
       [-1, -2, -3]])

# 将 `x` 和 `y` “横向”（horizontally）堆积
>>> np.hstack([x, y])
array([ 1,  2,  3, -1, -2, -3])
```

合并数组的完整函数列表[可以在NumPy官方说明文档](https://docs.scipy.org/doc/numpy/reference/routines.array-manipulation.html#joining-arrays)找到。NumPy也提供对应的函数来将单个数组切分成多个独立数组。
<!-- #endregion -->

## 官方说明文档链接

- [常数数组](https://docs.scipy.org/doc/numpy/reference/routines.array-creation.html#ones-and-zeros)
- [numpy.array](https://docs.scipy.org/doc/numpy/reference/generated/numpy.array.html#numpy-array)
- [序列数组](https://docs.scipy.org/doc/numpy/reference/routines.array-creation.html#numerical-ranges)
- [随机分布](https://docs.scipy.org/doc/numpy/reference/routines.random.html#distributions)
- [数组类型](https://docs.scipy.org/doc/numpy/user/basics.types.html#array-types-and-conversions-between-types)
- [合并数组](https://docs.scipy.org/doc/numpy/reference/routines.array-manipulation.html#joining-arrays)
