---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.3.0rc2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #raw raw_mimetype="text/restructuredtext" -->
.. meta::
   :description: Topic: Introduction to numpy arrays, Difficulty: Easy, Category: Section
   :keywords: numpy array, ndarray, introduction, overview
<!-- #endraw -->

<!-- #region -->
# N维数组简介
是时候让我们了解NumPy——在Python中做数字工作的旗舰模组——了！以使用此模组，我们需要在代码中“导入”（import）NumPy模组：

```python
import numpy as np
```

你也可以运行 `import numpy` 来代替以上语句，但是以上语句允许我们在代码中使用简写 'np' 而不需要写出全称 'numpy'。这是一个常用的简写。

N维数组（ND-array）是NumPy模组的明星。它能够存储一序列的数字。像Python列表一样，你可以通过“索引”（indexing）来访问该数组的成员，也可以通过“切片”（slicing）来访问数组的子序列。那么NumPy的N维数组和Python列表有什么区别，且为什么有一整个围绕着这个数组的数字模组呢？N维数组特殊于以下两点。它可以：

 1. 提供在多个维度访问它内在数据的接口。
 2. 通过编译的C代码（而不是Python代码）高效地对其成员或有规律的子序列进行数学操作；这个过程叫做矢量化（vectorization）。

让我们扫一眼这个模组提供的内容吧。以下代码创建了一个内容为数字0-8的N维数组：

```python
>>> import numpy as np
>>> x = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8])
```

此对象属于NumPy定义的类 `numpy.ndarray`。

```python
# N维数组属于类 `numpy.ndarray`
>>> type(x)
numpy.ndarray

>>> isinstance(x, np.ndarray)
True
```
<!-- #endregion -->

<!-- #region -->
我们可以重塑（reshape）此数组的形状，使其可以在2个维度中被访问：
```python
>>> x = x.reshape(3,3)
>>> x
array([[0, 1, 2],
       [3, 4, 5],
       [6, 7, 8]])
```

我们将使用NumPy的一个“矢量化”过的函数来为数组中每一个数都求平方（而不需要编写for循环）：
```python
>>> np.power(x, 2)  # 也可以使用简写来计算：x**2
array([[ 0,  1,  4],
       [ 9, 16, 25],
       [36, 49, 64]], dtype=int32)
```

让我们求数据每一列的平均值：
```python
>>> np.mean(x, axis=1)
array([ 1.,  4.,  7.])
```

我们可以使用广播来为 `x` 的每一列求多个不同的幂：
```python
>>> x ** np.array([0., 1., 2.])
array([[  1.,   1.,   4.],
       [  1.,   4.,  25.],
       [  1.,   7.,  64.]])
```

基本索引（basic indexing）允许我们获得 `x` 的多维度切片：
```python
>>> x[:2, :3]
array([[0, 1, 2],
       [3, 4, 5]])
```

进阶索引可以用来获取 `x` 中所有的偶数；让我们更新 `x` 来让它所有的偶数都乘以-1吧：

```python
>>> x[x % 2 == 0] *= -1
>>> x
array([[ 0,  1, -2],
       [ 3, -4,  5],
       [-6,  7, -8]])
```

在本模组结尾，这些代码片段都将会成为你理解范围内，且NumPy的强大功能也将得到充分的展示。
<!-- #endregion -->

## 官方说明文档链接

- [N维数组](https://docs.scipy.org/doc/numpy/reference/arrays.ndarray.html)
- [NumPy基础](https://docs.scipy.org/doc/numpy/user/basics.html#numpy-basics)
- [NumPy参考](https://docs.scipy.org/doc/numpy/reference/index.html)
