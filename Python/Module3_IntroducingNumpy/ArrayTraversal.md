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
   :description: Topic: Numpy array traversal ordering, Difficulty: Medium, Category: Section
   :keywords: row-major order, c order, column-major order, f order, traversal, array iteration
<!-- #endraw -->

# 迭代数组和数组遍历顺序
在本节中，你将会学到：

- NumPy迭代数组的函数。
- 有不止一个合理的方法用NumPy在迭代数组，区别仅仅在于NumPy遍历多维数组的方法。
- NumPy默认使用的行优先的遍历方法。

<!-- #region -->
NumPy提供了无论数组形状都可以完整迭代数组每一个成员的重要工具。比如说，请回忆，Python内置的 `enumerate` 函数允许我们一个一个获取可迭代物中的每一个成员，以及它在迭代中的索引：

```python
# 枚举可迭代物中的成员
>>> [i for i in enumerate("abcdef")]
[(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd'), (4, 'e'), (5, 'f')]
```

相似的，NumPy提供了[ndenumerate](https://docs.scipy.org/doc/numpy/reference/generated/numpy.ndenumerate.html#numpy.ndenumerate)函数。它枚举N维数组中的每一个成员并同时提供它在数组中的N维索引。
```python
>>> import numpy as np

# 演示 `np.ndenumerate`
# 形状为 (2, 2, 3) 的数组
>>> x = np.array([[[ 25,   6, -35],
...                [  9, -40, -29]],
...        
...               [[ -8,   2, -37],
...                [ 17,  10, -45]]])

>>> [i for i in np.ndenumerate(x)]
[((0, 0, 0), 25),
 ((0, 0, 1), 6),
 ((0, 0, 2), -35),
 ((0, 1, 0), 9),
 ((0, 1, 1), -40),
 ((0, 1, 2), -29),
 ((1, 0, 0), -8),
 ((1, 0, 1), 2),
 ((1, 0, 2), -37),
 ((1, 1, 0), 17),
 ((1, 1, 1), 10),
 ((1, 1, 2), -45)]
```

请注意，有三个成员的整数数组描述了对应数组成员的索引：

```python
>>> x[0, 0, 0]
25

>>> x[0, 0, 1]
6

>>> x[0, 0, 2]
-35

>>> x[0, 1, 0]
9
```

请查看[官方NumPy说明文档](https://docs.scipy.org/doc/numpy/reference/routines.indexing.html#iterating-over-arrays)来阅读完整的辅助迭代数组的函数列表。官方说明文档同时提供了[一个细致的迭代数组的教程](https://docs.scipy.org/doc/numpy/reference/arrays.nditer.html#iterating-over-arrays)，相比本文完整细致多很多。接下来，我们需要讨论NumPy在遍历N维数组时使用的默认顺序。


<!-- #endregion -->

<!-- #region -->
## 如何遍历数组：行优先（C）vs 列优先（F）遍历顺序
请注意 `np.ndenumerate` 迭代 `x` 的顺序。它首先遍历了 `x` 的页0行0的列，然后它遍历了页0行1的列，如此反复。这个遍历顺序有什么特殊的呢？比如说，为什么它不首先遍历每一列中的行呢？我们也可以发现 `reshape` 函数没有必要遵循什么独特的顺序。不如说，以下的重塑操作完全可以合理地返回两个结果中的任何一个
```
  array([0, 1, 2, 3, 4, 5]).reshape(2, 3) -->  array([[0, 1, 2],  or   array([[0, 2, 4],
                                                      [3, 4, 5]])             [1, 3, 5]])
```

这两个返回的数组都有着合理的形状且，根据你遍历它们，都保存了原本数字序列的顺序。左边的数组当你首先遍历每一行中的列，然后再遍历下一行的时候保存了顺序。因此，这个叫做**行优先**顺序。右边的数组当你首先遍历每一列中的行，然后再遍历下一列的时候保存了顺序。因此，这个叫做**列优先**排序。没有说一个排序就一定比另外一个好。虽然如此，*NumPy的函数需要遍历数组时永远都会默认使用行优先顺序*。

<div class="alert alert-warning"> 

**记住这点**：

NumPy在任何数组遍历操作时默认使用行优先顺序。

</div>

在处理2维数组时这两个顺序不是很烧脑，但是在处理更高维数组时它们该如何表现呢？我们不仅仅需要担心行和列，而可能会需要处理“几打成员为行列的页”（这会是一个4维数组）这种拗口的情况。基本规则如下：

- **行优先顺序（C顺序）{NumPy默认}**：遍历时首先增加*最后轴*的索引，直到遇到该轴的结尾，然后再增加倒数第二个索引，如此反复。
- **列优先顺序（F顺序）**：遍历时首先增加*第一个轴*的索引，直到该轴的结尾，然后再增加第二个轴的索引，如此反复。

<div class="alert alert-info">

**注**：

“行优先”排序也称为“C顺序”的原因是因为这也是C语言使用的遍历方法。反之，“列优先”排序也称为“F排序”的原因是这也是Fortran语言使用的遍历方法。像 `reshape` 的NumPy函数允许你选择 `order="C"`（默认）或 `order="F"` 来控制数组遍历的顺序；这些选择对应行优先和列优先顺序。

</div>

为了使以上讨论更加实际，让我们考虑NumPy是如何将形状为 (24,) 的数组重塑为形状为 (2,3,4) 的数组：
```python
# 将形状为 (24,) 的数组重塑为形状为 (2,3,4) 的数组
>>> np.arange(2*3*4).reshape(2,3,4)
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7],
        [ 8,  9, 10, 11]],

       [[12, 13, 14, 15],
        [16, 17, 18, 19],
        [20, 21, 22, 23]]])
```

根据NumPy的默认行优先顺序，我们可以根据以下步骤进行重塑：

  1. 创建一个形状为期望形状的空数组：(2, 3, 4)。
  2. 首先将输入数组的第0个成员插入到输出数组的 (0, 0, 0) 位置。
  3. *通过首先增加最后轴的索引*来进入到下一个位置，并将输入数组的下一个成员插入。
  4. 如果你遇到了某轴的结尾（比如说，轴2只有4个位置），重置该轴的索引为0，并增加前面一个轴的索引。回到第3步。

这个遍历过程在一步步看到之后会更好理解：

***
**使用NumPy默认的“行优先”顺序，将形状为 (24,) 的数组重塑为形状为 (2,3,4) 的数组**
```

                                  输入数组          输出数组
                                -----------    ---------------
                                  成员: 0    -> 成员: (0, 0, 0)
                                  成员: 1    -> 成员: (0, 0, 1)
                                  成员: 2    -> 成员: (0, 0, 2)
                                  成员: 3    -> 成员: (0, 0, 3) *页0行0填满。进入到下一行*
                                  成员: 4    -> 成员: (0, 1, 0)
                                  成员: 5    -> 成员: (0, 1, 1)
                                  成员: 6    -> 成员: (0, 1, 2)
                                  成员: 7    -> 成员: (0, 1, 3) *页1行0填满。进入到下一行*
                                  成员: 8    -> 成员: (0, 2, 0)
                                  成员: 9    -> 成员: (0, 2, 1)
                                  成员:10    -> 成员: (0, 2, 2)
                                  成员:11    -> 成员: (0, 2, 3) *页0行2填满。进入到下一页！*

                                  成员:12    -> 成员: (1, 0, 0)
                                  成员:13    -> 成员: (1, 0, 1)
                                  成员:14    -> 成员: (1, 0, 2)
                                  成员:15    -> 成员: (1, 0, 3) *页1行0填满。进入到下一行*
                                  成员:16    -> 成员: (1, 1, 0)
                                  成员:17    -> 成员: (1, 1, 1)
                                  成员:18    -> 成员: (1, 1, 2)
                                  成员:19    -> 成员: (1, 1, 3) *页1行1填满。进入到下一行*
                                  成员:20    -> 成员: (1, 2, 0)
                                  成员:21    -> 成员: (1, 2, 1)
                                  成员:22    -> 成员: (1, 2, 2)
                                  成员:23    -> 成员: (1, 2, 3) *页1行2填满。完成！*
```

***

同样的过程可以扩展到将单个多维数组重塑成另外一个形状不同的多维数组的情况下。我们只需要使用“行优先”规则来分别遍历输入和输出数组就行了。假设我们想要将一个形状为 (2,3,4) 的数组重塑为形状为 (6,4) 的数组，过程如下：

***
**使用NumPy默认的“行优先”顺序，将形状为 (2,3,4) 的数组重塑为形状为 (6,4) 的数组**
```

                                      输入数组             输出数组
                                  ----------------    ---------------
                                   成员: (0, 0, 0)  ->  成员: (0, 0)
                                   成员: (0, 0, 1)  ->  成员: (0, 1)
                                   成员: (0, 0, 2)  ->  成员: (0, 2)
                                   成员: (0, 0, 3)  ->  成员: (0, 3)
                                   成员: (0, 1, 0)  ->  成员: (1, 0)
                                   ...
                                   ...
                                   成员: (1, 2, 3)  ->  成员: (5, 3)
```
<!-- #endregion -->

虽然这样的记录可能第一眼看起来很麻烦，但是你很可能将会发现自己对行优先排序有了足够的直观理解来永远不需要像上面这样写整个表格！重塑数组形状来修改你访问数组数据的能力在数据科学应用中非常常用。同时，理解NumPy如何处理数组遍历是理解类似于数组广播（array broadcasting）和进阶索引（advanced indexing）必不可缺的奠基石。


<div class="alert alert-info">

**reshape是它自己的逆操作**：

根据以上的讨论，`reshape` 完全可以“逆反”它自己的作用：`np.arange(10).reshape(5,2).reshape(10)` 将会返回 `array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])`。花些时间来理解为什么这一点永远成立，不论中间有着多少步重塑。

</div>


## 官方说明文档链接

- [行优先顺序的定义](https://docs.scipy.org/doc/numpy/glossary.html#term-row-major)
- [列优先顺序的定义](https://docs.scipy.org/doc/numpy/glossary.html#term-column-major)
- [迭代数组常规](https://docs.scipy.org/doc/numpy/reference/routines.indexing.html#iterating-over-arrays)
- [数组迭代的细致描述](https://docs.scipy.org/doc/numpy/reference/arrays.nditer.html#iterating-over-arrays)
