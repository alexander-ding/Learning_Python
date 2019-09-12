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
   :description: Topic: Numpy array attributes, Difficulty: Easy, Category: Section
   :keywords: ndim, shape, size, itemsize, dtype, examples
```

<!-- #region -->
# 基本的数组属性
有了我们对NumPy多维数组的理解后，我们现在来了解一些使用代码来查看数组属性（例如维度）的方法吧。理解数组的“形状”（shape）是什么极其重要。

我们将使用以下的数组来进行讨论：
 
```python
>>> import numpy as np
>>> example_array = np.array([[[ 0,  1,  2,  3],
...                            [ 4,  5,  6,  7]],
...
...                           [[ 8,  9, 10, 11],
...                            [12, 13, 14, 15]],
...
...                           [[16, 17, 18, 19],
...                            [20, 21, 22, 23]]])
```
根据之前的讨论，这是一个有着一下结构的3维数组：

 - 轴0描述从**3页**中选中哪一个。
 - 轴1描述从任何一页中的**2行**中选中哪一个。
 - 轴2描述从任何一页和行中的**4列**中选中哪一个。

**ndarray.ndim**: 

数组的轴（维度）数。

```python
# 数组的维度
>>> example_array.ndim
3
```
<!-- #endregion -->

<!-- #region -->
**ndarray.shape**:

成员为代表数组每个维度的项目数量的整数元组。如果一个2维数组有着 $N$ 行和 $M$ 列，那么形状将是 $(N, M)$。所以这个形状元组的长度等于数组的维度。

```python
# 数组的形状
>>> example_array.shape
(3, 2, 4)
```

**ndarray.size**:

这个数组总共的成员数。这等于数组形状的每个数的乘积。
```python
# 数组的大小：数组中存了多少个成员
>>> example_array.size
24
```

**ndarray.dtype**:

描述数组成员的数据类型的对象。请回忆，NumPy的N维数组都是*同质*（homogenous）的：它们只能拥有同样数据类型的数字。

```python
# `example_array` 存储整数。每个整数使用32比特（bit）的内存
>>> example_array.dtype
dtype('int32') 
```

**ndarray.itemsize**:

数组每个成员的大小，单位是字节（byte）（1字节等于8比特）。比如说，类型为 `float64` 的数组的itemsize就是8 $(= \frac{64}{8})$，而类型为 `complex32` 的数组的itemsize就是4 $(= \frac{32}{8})$。
```python
# `example_array` 的每个整数占用4字节（32比特）的内存
>>> example_array.itemsize
4
```
<!-- #endregion -->

## 官方说明文档链接

- [数组属性](https://docs.scipy.org/doc/numpy/reference/arrays.ndarray.html#array-attributes)
