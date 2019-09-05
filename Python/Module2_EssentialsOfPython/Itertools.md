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
   :description: Topic: simple use cases of python itertools, Difficulty: Easy, Category: Tutorial
   :keywords: itertools, examples, zip, range, enumerate, chain, combinations
```

<!-- #region -->
# Python的“Itertools”
Python的[itertools模组](https://docs.python.org/3/library/itertools.html)提供了一套核心的快速，高效使用使用内存的创建迭代器的工具。我会将在本节快速的展示一些itertool的工具。大部分这些函数会创建[生成器](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html)，因此我们将会需要迭代它们才能展示它们的使用方法。此模组极其有用——本文强烈建议你花一些时间去了解该模组的内容。

Python有三个属于itertools的内置函数 `range`，`enumerate`，`zip`，但因为它们极其有用，所以Python会自动让你可以使用这些函数，而不需要导入它们。熟练使用 `range`，`enumerate`，和 `zip` 是很重要的技能。

**range**

生成包含描述的“界限”（range）中的整数的列表：
```python
# 将会生成 0.. 1.. 2.. ... 8.. 9
>>> range(10)
range(0, 10)

>>> list(range(10))
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# 将会生成 0.. 3.. 6.. 9
>>> range(0, 10, 3)
range(0, 10, 3)

>>> list(range(0, 10, 3))
[0, 3, 6, 9]
```

**enumerate**

枚举可迭代物中的对象：每轮迭代提供一元组，其内容为迭代轮数（从0开始）以及对应的可迭代物成员。
```python
# 将会生成 (0, 'apple').. (1, 'banana').. (2, 'cat').. (3, 'dog')]
>>> enumerate(["apple", "banana", "cat", "dog"])
<enumerate at 0x23e3557b3f0>

>>> list(enumerate(["apple", "banana", "cat", "dog"]))
[(0, 'apple'), (1, 'banana'), (2, 'cat'), (3, 'dog')]
```

**zip**

将多个可迭代物的对应成员拉链（zip）到元组中。这为凑对多个可迭代物中对象很有用。
```python
>>> names = ["Angie", "Brian", "Cassie", "David"]
>>> exam_1_scores = [90, 82, 79, 87]
>>> exam_2_scores = [95, 84, 72, 91]

# 将会生成 ('Angie', 90, 95).. ('Brian', 82, 84).. ('Cassie', 79, 72).. ('David', 87, 91)]
>>> zip(names, exam_1_scores, exam_2_scores)
<zip at 0x20de1082608>

>>> list(zip(names, exam_1_scores, exam_2_scores))
[('Angie', 90, 95), ('Brian', 82, 84), ('Cassie', 79, 72), ('David', 87, 91)]
```
***
以下是一些 `itertools` 模组中有用的函数：

**itertools.chain**

将多个可迭代物背靠背连接在一起，产生一个可迭代物：
```python
>>> from itertools import chain
>>> gen_1 = range(0, 5, 2)               # 0.. 2.. 4
>>> gen_2 = (i**2 for i in range(3, 6))  # 9.. 16.. 25 
>>> iter_3 = ["moo", "cow"]
>>> iter_4 = "him"

# 将会生成：0.. 2.. 4.. 9.. 16.. 25.. 'moo'.. 'cow'.. 'h'.. 'i'.. 'm'
>>> chain(gen_1, gen_2, iter_3, iter_4)
<itertools.chain at 0x20de109ec18>
```

**itertools.combinations**
生成所有长度为n的可迭代物中成员的“组合”：
```python
>>> from itertools import combinations

# 将会生成：(0, 1, 2).. (0, 1, 3).. (0, 2, 3).. (1, 2, 3)
>>> combinations([0, 1, 2, 3], 3)  # 从 [0, 1, 2, 3] 的数字生成所有长度为3的组合
<itertools.combinations at 0x20de10a7728>
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：Itertools第一部分**

使用 `itertools.combinations` 函数来找出从列表 `["apples", "bananas", "pears", "pears", "oranges"]` 随机选择两个成员会提供包含"apples"和"pears"的组合的几率。

</div>


<div class="alert alert-info">

**阅读理解：Itertools第二部分**

设列表 `x_vals = [0.1, 0.3, 0.6, 0.9]`，创建一个生成器 `y_gen` 来生成 $y = x^2$ 在每一个 $x$ 对应的的y值。然后，使用 `zip` 来创建一个包含 $(x, y)$ 元组的列表。

</div>


## 官方说明文档链接

- [range](https://docs.python.org/3/library/stdtypes.html#typesseq-range)
- [enumerate](https://docs.python.org/3/library/functions.html#enumerate)
- [zip](https://docs.python.org/3/library/functions.html#zip)
- [itertools](https://docs.python.org/3/library/itertools.html)

<!-- #region -->
## 阅读理解答案

**Itertools第一部分：解**

```python
>>> from itertools import combinations
>>> ls = ["apples", "bananas", "pears", "pears", "oranges"]
>>> comb_ls = list(combinations(ls, 2))
>>> comb_ls.count(("apples", "pears")) / len(comb_ls)
0.2
```

<!-- #endregion -->

<!-- #region -->
**Itertools第二部分：解**

```python
>>> x_vals = [0.1, 0.3, 0.6, 0.9]
>>> y_gen = (x**2 for x in x_vals)
>>> list(zip(x_vals, y_gen))
[(0.1, 0.01), (0.3, 0.09), (0.6, 0.36), (0.9, 0.81)]
```

在这个情况下，使用 `zip` 其实有点没必要的复杂化了。我们完全可以跳过创建 `y_gen` 并使用列表理解：
```python
>>> x_vals = [0.1, 0.3, 0.6, 0.9]
>>> [(x, x**2) for x in x_vals]
[(0.1, 0.01), (0.3, 0.09), (0.6, 0.36), (0.9, 0.81)]
```
<!-- #endregion -->
