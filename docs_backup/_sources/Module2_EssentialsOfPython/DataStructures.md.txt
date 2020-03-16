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
   :description: Topic: Data Structures, Difficulty: Medium, Category: Section
   :keywords: Big-O, complexity, efficiency, algorithm, interview preparation, list, tuple, sequence
<!-- #endraw -->

<!-- #region -->
# 数据结构（第一部分）：简介
我们在此介绍Python内置的数据结构（data structure）。你应该已经了解了列表和元组，两个帮助处理序列数据的数据结构。我们将会介绍另外两个重要的内置数据结构：

- 词典（dictionary）：一对一对应“键”（key）和“值”（value）
- 集（set）：用来进行集运算（如并集和交集）的无序集合

这些数据结构不仅仅是一些有着好用核心功能的方便构造题；它们同时也提供了用C语言（或你使用的Python直译器编译用的语言）编译的高效核心功能的接口。比如说，让我们来编写一个检查某对象是否为可迭代物成员的函数：

```python
def is_in(seq, target):
    """ 如果 `target` 在 `seq` 中则返回真"""
    for item in seq:
        if item == target:
            return True
    return False
```

以上函数和Python在“幕后”使用的C语言成员检测算法（当然，这是假设你在使用CPython直译器，但极大可能是这个情况）是一样的。因为内置的成员检测函数是在“更底层”实现的并因此不需要被直译，所以它比我们在Python中定义的函数更快：
```python
>>> x = [1, "moo", 3, True, 5, None, 7, 8]

>>> is_in(x, -1)  # 在我的机子上花了 980 纳秒
False

>>> -1 in x       # 在我的机子上花了 320 纳秒
False
```
在此，Python的内置成员检测函数比我们的版本快了三倍。同时，我们应该了解各个数据结构的优势。比如说，集（set）的成员检测比列表还要快：

```python
# 在列表中检测是否为成员
>>> -1 in [1, "moo", 3, True, 5, None, 7, 8]  # 在我的机子上花了 295 纳秒
False

# 在集中检测是否为成员
>>> -1 in {1, "moo", 3, True, 5, None, 7, 8}  # 在我的机子上花了 65 纳秒
False
```
通过使用集而不是列表，我们得到了4.5倍的速度优化。这是因为集允许Python使用一个完全不同的成员检测算法。在Python程序员的角度上，我们仅仅将方括号替换成了花括号！我希望以上范例为你提供了足够学习Python数据结构和它们“幕后”使用的算法的理由。

<div class="alert alert-info">

**经验**：

Python的数据结构有着自己内置的功能。同时，理解每个数据结构适合的工作对编写高效的Python代码极其重要。你不需要死记这些信息，但是你应该知道这些信息存在并经常查阅这些信息。
</div>
<!-- #endregion -->

<!-- #region -->
## 描述算法复杂度
为了有意义地比较算法的效率，总结算法在问题大小变化时如何表现是很有用的信息。两个排序算法可能在排列几十个项目时速度相当，但它们完全可能在排列几千个项目时有着很不一样的效率。

“大O”符号（big-O notation）允许我们表达算法在问题大小变化时运算时间如何改变。具体来讲，它代表着算法在“最坏情况下”的表现。

比如说，拿在本节开始写的 `is_in` 函数来讲，我们迭代了一个集合并检查它是否有着某个项目。这个算法的最坏情况就是当项目并不是集合的成员——我们需要迭代整个集合才能确定它并不拥有我们要找的项目。所以如果我们将该集合的大小扩大 $n$ 倍，那它会花 $n$ 倍多的时间来迭代整个集合并决定一个项目不是其成员（在此同样假设最坏情况）。因为 `is_in` 的最坏情况的运行时间和集合的大小 $n$ 呈线性关系，所以我们用大O符号来将该算法的运行时间复杂度写为 $\mathcal{O}(n)$。

现在，假设我们写的成员检查函数很糟糕，其进行了嵌套迭代：

```python
def is_in_slow(seq, target):
    """ 如果 `target` 为 `seq` 成员，返回真。"""
    for item in seq:
        # 我们为seq中的项目重新迭代整个seq！
        for item2 in seq:
            if item == target:
                return True
    return False
```

我们为 `seq` 中的每个项目重新迭代 `seq` 一次，因此在最坏情况下我们需要迭代 $n$ 个成员 $n$ 次——总共在算法中走了 $n^{2}$ “步”。因此，我们说 `is_in_slow` 是一个 $\mathcal{O}(n^{2})$ 的算法：当我们将 `seq` 的大小翻倍时，我们的 $\mathcal{O}(n)$ 算法会花两倍多的时间（线性增加），而我们的 $\mathcal{O}(n^{2})$ 算法会花四倍多的时间（平方增加）。

以下是大O符号更加正式的定义：
<div class="alert alert-block alert-info"> 
**“大O”符号**（big-O notation）：假设 $n$ 代表着算法输入的“大小”，且数学表达式 $f(n)$ 描述该算法在*最坏情况*下需要花多少计算步骤处理大小为 $n$ 的输入，那么算法的运行时间复杂度用**“大O”符号**表达为 $\mathcal{O}(f(n))$。
</div>

几个重要的点：

- 我们仅仅在乎 $f(n)$ 最高次数的项。也就是说，$\mathcal{O}(n + n^{2})$ 应被写为 $\mathcal{O}(n^{2})$。
- 我们永远不在乎关系中的常数。也就是说，就算一个算法迭代某序列两次，它的大O复杂度应该被写为 $\mathcal{O}(n)$，而不是 $\mathcal{O}(2n)$。
- 一个运行时间*不根据输入大小而改变*的算法是一个 $\mathcal{O}(1)$ 算法。
  - 例：一个返回列表中第二个成员的函数。
- 我们有其它更加微妙，不仅仅考虑最坏情况的分析算法复杂度的方法，因为最坏情况分析可能太过悲观。“大O”符号“也可以用来代表平均表现，[摊销](https://en.wikipedia.org/wiki/Amortized_analysis)（amortized）表现，和其它分析类型。在此，我们仅仅使用最坏情况分析。
<!-- #endregion -->

<div class="alert alert-info">

**经验**：

我们将会使用“大O”符号 $\mathcal{O}(f(n))$ 来总结Python数据结构使用的算法的表现。
</div>


## 序列性数据结构：列表和元组
“序列类型”小节已经介绍了列表和元组。请回忆这两个数据结构都提供了相同的交互界面来访问和总结其不同类型对象的成员序列。但是，列表是可以被改变的——你可以对其更新，删除，并添加项目——而元组不可变。因此列表*可变*，而元组*不可变*。在本节你将会找到很多操作序列数据结构的内置函数的算法复杂度。

你可以在[这里](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)找到所有的Python列表函数。

#### 列表/元组复杂度
设 `seq` 为长度为 $n$ 的**列表或元组**；$i$ 和 $j$ 为从区间 $[0, n-1]$ 中随意抽取的整数；$k$ 为任何在此操作中使用的子序列的长度。以下为各种常见的列表和元组操作（根据CPython的实现）的复杂度：

|操作| 复杂度 | 解释 |
|---|---|---|
|`len(seq)`| O(1)| 返回序列中成员的数量 |
|`seq[i]`| O(1) | 从序列中获取单个成员 |
|`seq[i:j]`| O(k) | 从序列中获取长度k的切片 |
|`for item in seq..`| O(n) | 迭代序列 |
|`obj in seq`| O(n) | 检查 `obj` 是否为 `seq` 的成员 |
|`seq.count(obj)`| O(n) | 返回 `obj` 在 `seq` 出现的次数 |
|`seq.index(obj)`| O(n)| 返回 `obj` 在 `seq` 中的索引 |


#### 列表复杂度
我们在此考虑一些列表可用而元组不可用的改变序列的操作，如 `append`。请注意列表的实现导致：

- 在列表*结尾*添加或删除成员的操作为 $\mathcal{O}(1)$
- 在列表*开头*添加或删除成员的操作为 $\mathcal{O}(n)$

设 `my_list` 为长度为 $n$ 的列表，而 `i` 为从区间 $[0, n-1]$ 随意抽取的整数。以下为列表各种操作（在CPython实现中）的复杂度：

|操作| 复杂度 | 解释 |
|---|---|---|
|`my_list[i] = obj`| O(1) | 将列表第i位的项目设为新对象。|
|`my_list.append(obj)`| O(1) | 在列表结尾添加新对象。|
|`my_list.pop()`| O(1) | 删除在列表*结尾*的对象。|
|`my_list.pop(0)`| O(n) | 删除在列表*开头*的对象。|
|`my_list.sort()`| O(nlog(n)) | 返回同列表的顺序版本。|
