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
   :description: Topic: the basics of iterables in python, Difficulty: Medium, Category: Section
   :keywords: iterables, max, min, sum, all, any, itertools, enumerate, unpack
<!-- #endraw -->

# 可迭代物
我们之前在讨论for循环的时候介绍了*可迭代物*（iterable）——也就是任何你可以“迭代”（如for循环）的对象。

<div class="alert alert-info">

**定义**：

**可迭代物**泛指任何支持一次返回一个成员的Python对象。这使得它可以用for循环迭代。
</div>

常见的可迭代物包括列表，元组，和字符串——任何你可以用for循环迭代的序列（sequence）。我们也将遇到一些并非序列的集合，如词典（dictionary）和集（set）；这些也是可迭代物。Python也提供在每轮迭代时“生成”（generate）一个成员的可迭代物——也就是说它甚至不需要同时在内存中存储所有的成员。我们会在后文花一整节来讨论生成器（generator）——一种特殊的迭代器——因为它们会帮助我们编写高效率的代码。

本节将会讨论如何在你的代码中使用可迭代物。

<div class="alert alert-warning">

**注**：

在“幕后”，可迭代物是任何提供 `__iter__()` 方法（method）或实现了 `Sequence` 语义的 `__getitem__()` 方法的Python对象。在你阅读面向对象模组时，这些细节将会更加显著。
</div>

<!-- #region -->
## 操作可迭代物的函数
以下是一些有用的操作可迭代物的函数：

 - `list`, `tuple`, `dict`, `set`：利用可迭代物的成员来分别创建列表，元组，[词典](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html)，或[集](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_III_Sets_and_More.html#集数据结构)。
 - `sum`：求可迭代物内容的和。
 - `sorted`：返回成员为可迭代物成员顺序排列版本的列表。
 - `any`：迭代可迭代物。如果可迭代物中*任何*（any）一个成员满足 `bool(item)` 为 `True`，立刻返回 `True` 并终止迭代。
 - `all`：只有在可迭代物中*所有*（all）成员都满足 `bool(item)` 为 `True` 时返回 `True`。
 - `max`：返回可迭代物中的最大值。
 - `min`：返回可迭代物中的最小值。

```python
# 操作可迭代物内置函数的例子
>>> list("I am a cow")
['I', ' ', 'a', 'm', ' ', 'a', ' ', 'c', 'o', 'w']

>>> sum([1, 2, 3])
6

>>> sorted("gheliabciou")
['a', 'b', 'c', 'e', 'g', 'h', 'i', 'i', 'l', 'o', 'u']

# 以下所有物件都导致 `bool(item)` 为 `False`
>>> any((0, None, [], 0))
False

# 以下所有物件都导致 `bool(item)` 为  `True`
>>> all([1, (0, 1), True, "hi"])
True

>>> max((5, 8, 9, 0))
9

>>> min("hello")
'e'
```
<!-- #endregion -->

<!-- #region -->
## 操作可迭代物的技巧
Python提供了一些用来操作可迭代物的一些语法上”技巧“：”解包“（unpack）和“枚举”（enumerate）。虽然这些功能一开始看起来像奇技淫巧，但是它们会帮助我们编写干净易读的代码和正确易懂的算法，因此我们会花一些时间去讨论它们。同时，这些技巧也会帮助我们使用其它棒棒的Python功能，如理解语句（comprehension statement）。我们会在后文进行讨论。

### “解包”可迭代物
假设你的列表中存储了三个值，且你想要将每个值赋值给一个单独的变量。利用前文的知识点，你很可能会编写以下的代码：

```python
# 将列表内容赋值给变量的简单脚本
>>> my_list = [7, 9, 11]

>>> x = my_list[0]
>>> y = my_list[1]
>>> z = my_list[2]
```

Python提供一个极其有用的功能，叫做**可迭代物解包**（iterable unpacking）。它允许我们编写以下优雅，简单的代码：

```python
# 使用可迭代物解包来赋值多个变量
>>> my_list = [7, 9, 11]

>>> x, y, z = my_list
>>> print(x, y, z)
7 9 11
```

也就是说，Python直译器会“注意”赋值符左边的多个变量并以此“解包”右边的可迭代物（在此例中为一个列表）。虽然在本例中这个功能看起来没那么关键，但实际上它是一个*极其*有用的提升代码可读性的功能。

在for循环成员为可迭代物的可迭代物时，可迭代物解包功能极其有用。比如说，假设我们有一列表的元组，每个元组包含一对名字和成绩：

```python
>>> grades = [("Ashley", 93), ("Brad", 95), ("Cassie", 84)]
```

根据上一节所述，如果我们循环此列表，那么每一轮迭代变量都会被赋值为其中的元组：

```python
for entry in grades:
    print(entry)
```
会打印：
```
('Ashley', 93)
('Brad', 95)
('Cassie', 84)
```

我们很有可能会想要单独使用学生的名字和成绩（比如说，使用学生名字来访问其成绩记录并添加对应的成绩）；所以说，我们需要索引 `entry` 两次来获得两个不同的变量。但是，因为for循环每一轮的赋值步骤真的其实就是 `entry = ("Ashley", 93)`，我们可以使用可迭代物解包！也就是说，我们可以用 `name, grade` 来代替 `entry`。Python会为其中每个变量进行解包。

```python
# 本for循环的第一次迭代会将赋值解包：name, grade = ("Ashley", 93)
# 然后第二次迭代：name, grade = ("Brad", 95)
# 如下反复
for name, grade in grades: 
    print(name)
    print(grade)
    print("\n")
```
打印：
```
Ashley
93

Brad 
95

Cassie 
84
```
这个for循环代码简洁并非常可读。本文强烈推荐读者在这种情况下使用可迭代物解包。

可迭代物解包的行为有时并不如此易懂。假如你试图将一个有10个对象的可迭代物解包到4个变量时会发生什么？虽然我们之前讨论的内容概括了最基础的解包方式，知道[Python提供更加全面复杂的可迭代物解包语法](https://www.python.org/dev/peps/pep-3132/#specification)这一事实也是很有帮助的。我们会在之后看到解包在创建和使用函数时的用处。

<div class="alert alert-info">

**经验**：

Python为“解包”（unpack）可迭代物内容——将每个成员赋值给它对应的变量——提供了轻便的语法。这允许我们在for循环可迭代物集合时编写易懂易读的代码。
</div>
<!-- #endregion -->

<!-- #region -->
### 枚举可迭代物
内置的[enumerate](https://docs.python.org/3/library/functions.html#enumerate)（枚举）函数允许我们在迭代时获取可迭代物成员在迭代过程中的位置：

```python
# 简单使用 `enumerate`
>>> for entry in enumerate("abcd"):
...    print(entry)
(0, 'a')
(1, 'b')
(2, 'c')
(3, 'd')
```

`enumerate` 函数接受一个可迭代物并返回一个新的可迭代物，其成员为包含迭代位置和对应成员的元组。如此，我们在枚举这个可迭代物的成员。为了演示此功能的用处，假设我们想要记录列表中所有成员值为 `None` 的索引。我们可以通过在for循环迭代列表时记录迭代位置来达到这个目的。

```python
# 记录可迭代物中值为 `None` 的成员的位置
none_indices = []
iter_cnt = 0  # 手动记录迭代位置

for item in [2, None, -10, None, 4, 8]:
    if item is None:
        none_indices.append(iter_cnt)
    iter_cnt = iter_cnt + 1

# `none_indices` 现在存储 [1, 3]
```

利用 `enumerate` 和元组解包，我们可以简化此代码并避免使用 `iter_cnt` 这个变量。

```python
# 使用 `enumerate` 函数来记录迭代位置
none_indices = []

# 请注意这里的可迭代物解包！
for iter_cnt, item in enumerate([2, None, -10, None, 4, 8]):  
    if item is None:
        none_indices.append(iter_cnt)
        
# `none_indices` 现在存储 [1, 3]
```

<div class="alert alert-info">

**经验**：

内置的[enumerate](https://docs.python.org/3/library/functions.html#enumerate)函数应在任何需要记录for循环迭代位置的情况下（和可迭代物解包一起）使用。和元组解包一起使用会更加简化代码。
</div>
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：enumerate**

在for循环中使用可迭代物 `"abcd"`，`enumerate` 函数，以及元组解包来创建以下列表：`[(0, 'a'), (1, 'b'), (2, 'c'), (3, 'd')]`

</div>


<div class="alert alert-info">

**阅读理解：是否顺序排列？**

使用控制流和循环工具来检查某个成员为数字的可迭代物是否顺序排列。

将变量 `unsorted_index` 初始化为 `None`。如果可迭代物*不*顺序，那 `unsorted_index` 应该存储序列*第一个*不符合顺序的成员的索引。如果可迭代物顺序排列，那 `unsorted_index` 应该在最后依然为 `None`，且你的代码应该打印 "sorted!"。

比如说：

 - 设可迭代物 `my_list = [0, 1, -10, 2]`，`unsorted_index` 应该为 `2`。
 - 设可迭代物 `my_list = [-1, 0, 3, 6]`，`unsorted_index` 应该为 `None`，且你的代码应该打印 "sorted!"。

</div>


## 官方说明文档链接

- [可迭代物定义](https://docs.python.org/3/glossary.html#term-iterable)
- [操作可迭代物的函数](https://docs.python.org/3/howto/functional.html#built-in-functions)
- [枚举](https://docs.python.org/3/library/functions.html#enumerate)

<!-- #region -->
## 阅读理解答案
**enumerate：解**

```python
out = []
for num, letter in enumerate("abcd"):
    out.append((num, letter))
```

**是否顺序排列？：解**
```python
my_list = [0, 1, -10, 2]
unsorted_index = None

for index, current_num in enumerate(my_list):
    if index == 0:
        prev_num = current_num
    elif prev_num > current_num:
        unsorted_index = index
        break
    prev_num = current_num
else:
    print("sorted!")
```
<!-- #endregion -->
