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
   :description: Topic: understanding python sequences, Difficulty: Easy, Category: Section
   :keywords: list, tuple, string, slice, index, negative index, get item, pop, append, examples
```

<!-- #region -->
# 序列类型

<div class="alert alert-warning">

**注**：

本文各处的阅读理解练习旨在帮助你练习使用文章中的知识点。练习题的答案可以在本页结尾找到。
</div>

下列对象都是序列（sequence）的种类：
```python
# 序列的例子

# 列表（list）
[0, None, -2, 1]

# 字符串（string）
"hello out there"

# 元组（tuple）
("a", False, 0, 1)

# NumPy数组
numpy.ndarray([0.2, 0.4, 0.6, 0.8])
```

因为操作对象/数据序列的能力极其重要，我们第一课（相对）深入的Python探讨将会学习序列类型。前面的阅读向读者介绍了Python的列表和字符串。虽然两者能存储的内容不同，*列表和字符串都是一种序列*——它们能够存储一组有限多的有顺序的对象（比如说 `"cat"` 和 `"tac"` 虽然成员字符相同，但是其实是两个不一样的字符串）。因此，列表，字符串，和其它Python的序列类型共享一个常用的接口来帮助用户查阅，调用，和总结它们的内容。

在本节中，我们将：

- 介绍元组——我们将见到的最后一种内置序列类型。
- 展示用来查阅和总结序列内容的通用接口。
- 细致讨论Python的检索系统。我们可以用它来查阅序列具体的成员或子序列。
<!-- #endregion -->

<!-- #region -->
## 元组
最后一个我们还未讨论的内置序列类型叫做 `tuple`（元组）类型。元组和列表相似，都可以存储任何对象（数字，字符串，列表，其它元组，或多种对象，等等）。和列表的方括号不同的是元组由圆括号创建：

```python
# 创建元组
>>> x = (1, "a", 2)  # 有三个物件的元组

# (3) 并不能创建只有一个物件的元组
# 在这个情况下，你必须在最后添加一个额外的逗号
>>> y = (3,)         # 有一个对象的元组

>>> type(x)
tuple

>>> isinstance(y, tuple)
True
```

<div class="alert alert-warning">

**检查多种类型**：

通过输入一个包含多类型的元组，你可以用 `isinstance` 来一次性检查多种类型。如下：
```python
isinstance(x, (tuple, list, str))
```

这将检查 `x` 是否是元组*或*列表*或*字符串。
</div>
<!-- #endregion -->

<!-- #region -->
和列表不同的是，*当你创建元组之后，你无法修改它*。也就是说，元组是*不可变*（immutable）的，而列表是*可变*（mutable）的。元组一般相比列表会使用更少的内存，因为我们提前知道元组的大小不会改变。另外一个元组的好用点在于它保证存储的数据不会被之后的代码给改变。

```python
# 列表的内容可以被修改；列表是“可变”的
>>> x = [1, "moo", None] 
>>> x[0] = 2
>>> x
[2, 'moo', None]

# 元组的内容不可以被修改；元组是“不可变”的
>>> y = (1, "moo", None)  # (a, b, ...) 创建一个元组
>>> y[0] = 2
TypeError: 'tuple' object does not support item assignment
```

你可以用 `tuple` 来将其它序列（或广义来讲，其它任何可迭代物）转化成元组。这和 `str` 和 `list` 的行为类似。
```python
# `tuple` 可以利用其它序列来创建元组
>>> x = [2, 4, 8]
>>> y = tuple(x)

>>> x 
[2, 4, 8]

>>> y
(2, 4, 8)
```
<!-- #endregion -->

<!-- #region -->
## 操作序列
下文将总结不同Python序列类型（包括列表，元组，和字符串）的通用界面。此界面将允许你查看，总结，合并，和获取序列的成员。

**检查某对象是否为序列成员**：`obj in seq`
```python
# 利用 'in' 和 'not in' 检查对象是否为序列成员
>>> x = (1, 3, 5)

>>> 3 in x
True

>>> 0 in x
False

>>> 0 not in x
True

# 你可以检查字符串是否为其它字符串的子序列
>>> "cat" in "the cat in the hat"
True

# 而其它序列类型不支持子序列检查
>>> [1, 2] in [1, 2, 3, 4]
False

# 只有当 [1, 2] 为列表中的项目时才会被视作列表成员
>>> [1, 2] in [None, [1, 2], None]
True
```

**连接序列：**`seq1 + seq2`
```python
# 通过 '+' 来粘连两个序列
>>> [1, 2] + [3, 4]  # 这将创建一个新序列
[1, 2, 3, 4]

>>> "c" + "at"
"cat"
```

**重复连接某序列**：`n*seq1` 或 `seq1*n`
```python
# 和 `cat + cat + cat` 等值
>>> "cat" * 3   # 创建一个新字符串
'catcatcat'

>>> 4 * (1, 5)  # 创建一个新元组
(1, 5, 1, 5, 1, 5, 1, 5)
```

**检查序列成员数**：`len(seq)`
```python
# 查看序列的长度
>>> len("dog")
3

>>> len(["dog", "dog"])
2

>>> len([])
0
```

**查看序列中** `x` **第一次出现的位置的索引**：`seq.index(x)`
```python
>>> "cat cat cat".index("t")  # 't' 第一次出现在索引（index）2
2

# `index` 并不会检查序列中包含的成员序列
# 例，它会检查 1，2，和 "moo"，而不是1，2，“m”，“o”，“o”
>>> [1, 2, "moo"].index("m")
ValueError: 'm' is not in list
```

**返回序列中** `x` **出现的次数**：`seq.count(x)`
```python
>>> "the cat in the hat".count("h")
3

# `count` 不会检查序列中包含的成员序列
# 因此，它不会“看到” `[1, 2]` 中的 1。
>>> [1, [1, 2], "111", 1].count(1)  
2
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：序列基础**

1\. 将列表 `[True, None, 22]` 转化成元组。

2\. 到现在，我们讨论了多少种序列类型？其中哪些创建不可变对象？哪些创建可变对象？在可变类型中选择一个并编写修改其成员对象的范例代码。

</div>

<!-- #region -->
### 索引和切片简介
我们可以通过提供序列中单个成员的*索引*（index）来访问此物件。Python序列的第一个成员的索引是0，第二个是1，如此下去。
```python
# 通过索引来访问序列的单个物件
>>> x = "abcdefg"
>>> x[0]
"a"

>>> x[2]
"c"
```

我们也可以切片（“slice”）序列，也就是通过提供起始索引和结尾索引来返回其中包含的子序列：
```python
# 通过“切片”来提供序列内容的子序列
>>> x[0:3] # 这子序列包含索引为0，1，2（注意不包含3）的成员
"abc"
```
熟练访问序列成员和子序列是极其重要的技能。索引功能也会在我们操作NumPy数组（array）时出现。接下来，让我们来具体描述Python的索引和切片功能。

### 索引
Python允许你通过序列中单个成员的*索引*（index），一个识别成员在序列中位置的独特整数，来获取该成员。*Python使用从0开始的索引模式*，并允许你使用负数来从序列结尾往开头数。拿字符串 `"Python"` 来说，接下来的图表将显示该序列的索引：
```
 +---+---+---+---+---+---+
 | P | y | t | h | o | n |
 +---+---+---+---+---+---+
   0   1   2   3   4   5  
  -6  -5  -4  -3  -2  -1
```

第一行的数字提供了字符串0到5的索引；第二行提供了对应的负索引。

**正索引** 

- 0 $\rightarrow$ P
- 1 $\rightarrow$ y
- 2 $\rightarrow$ t
- 3 $\rightarrow$ h
- 4 $\rightarrow$ o
- 5 $\rightarrow$ n

**负索引** 

- -6 $\rightarrow$ P
- -5 $\rightarrow$ y
- -4 $\rightarrow$ t
- -3 $\rightarrow$ h
- -2 $\rightarrow$ o
- -1 $\rightarrow$ n

通过此索引模式，Python保留变量名或对象后的方括号语法来允许用户“获取成员”：`seq[index]`。

```python
# 演示序列索引
>>> x = [1, 2, 3, 4]

# 这叫做“获取成员”语法。
>>> x[0]      # 索引从0开始
1

>>> x[-4]     # 每个成员都有着对应的正索引和负索引
1

>>> x[-1]     # 负索引是相对序列结尾而言的
4

>>> "cat"[2]  # 你可以直接检索序列对象
't'

>>> (True, False)[-1]
False
```

<div class="alert alert-info">

**经验**：

对一个序列“索引”指的是通过提供序列中单个成员的索引——也就是表达成员在序列中位置的整数——来获取它：`seq[index]`。Python使用一个从0开始的索引系统，也就是说序列中第一个成员是索引0。负索引允许你通过对象相对序列结尾的位置来获取它。
</div>
<!-- #endregion -->

<!-- #region -->
### 切片
我们可以利用类似前文索引的切片（slicing）来获取列表的子序列。一个切片（slice）包含了以下的部分：

- 起始索引（start-index）：切片第一个物件的索引（索引指向的成员将*会被包含*在该切片中）。
- 结尾索引（stop-index）：切片结尾的索引（索引指向的成员*不会被包含*在切片中）。
- 步距（step-size）：这将允许我们获取起始和结尾中的每个成员，或每*两个*成员，或每*n个*成员。负步距将允许从结尾往开始*反向*获取每n个成员。

切片的基本语法为：`seq[start:stop:step]`。注意使用冒号“:”来分别起始，结尾，和步距。

```python
# 演示序列切片
>>> seq = "abcdefg"

# start：0, stop：4，step：1
>>> seq[0:4:1]
'abcd'

# start：1，stop：4，step：1
>>> seq[1:4:1]
'bcd'

# start：1，stop：5，step：2
>>> seq[0:5:2] # 获取 [start, stop) 中的每两个成员
'ace'

# start和stop同一个索引时会得到空序列
# start：0，stop：0，step：1
>>> seq[0:0:1]
''
```

切片默认的start，stop，和step值都很实用。具体如下：

- start：0
- stop：`len(seq)`
- step：1

对其中任何一个值，你可以通过不提供值或提供 `None` 来使用默认值。你也可以不提供第二个冒号和后面的步距来使用默认的步距1。
```python
# 使用默认的start，stop，和step值
>>> seq = "abcdefg"

# start：0，stop：7，step：1
>>> seq[:]  # 和 `seq[None:None]` 等值
'abcdefg'

# start：0，stop：7，step：2
>>> seq[::2]
'aceg'
```

你也可以在切片中使用负数。负步距将会*反向*迭代选中的序列。在这个情况下，默认的起始和结尾索引将会变成使得 `seq[::-1]` 提供该序列的反向版本的值。

```python
# 使用负步距来提供序列的反向版本
>>> seq[::-1]
'gfedcba'
```

和负索引一般，在切片中使用负起始或结尾值允许我们表达相对于序列结尾的位置。
```python
# 该切片将返回序列的最后两个值
>>> seq[-2:]
'fg'

# 该切片将返回除了最后两个值之外的列表序列
>>> seq[:-2]
'abcde'
```

虽然切片的冒号语法 `seq[start:stop:step]` 在Python代码中随处可见，你应知道Python使用一个内置的 `slice` 对象来创建切片。它接受一样的start，stop，和step值，并提供一样的切片行为：
```python
# 显性使用 `slice` 对象
>>> seq = "abcdefg"
>>> seq[slice(0, 3, 1)]
'abc'
```

这将允许你在代码中做一些更加有意思的事情，因为你可以将变量赋值为切片对象。
```python
# 使用 `slice` 对象来切片多个序列
>>> seq1 = "apple"
>>> seq2 = (1, 2, 3, 4, 5)
>>> seq3 = [True, False, None]

>>> reverse = slice(None, None, -1)

>>> seq1[reverse]
'elppa'

>>> seq2[reverse]
(5, 4, 3, 2, 1)

>>> seq3[reverse]
[None, False, True]
```

<div class="alert alert-info">

**经验**：

“切片”序列指通过指定起始索引（被包含），结尾索引（不被包含），和步距来获取子序列。你可以提供负值，也可以使用默认值。常见的 `seq[start:stop:step]` 语法其实就是使用 `slice` 对象的简写：`seq[slice(start, stop, step)]`。
</div>
<!-- #endregion -->

<!-- #region -->
#### 处理超界索引
试图使用某个超界的索引来获取序列成员会导致 `IndexError`：
```python
>>> x = [0, 1, 2, 3, 4, 5] # `x` 只有6个成员
>>> x[6]  # 试图获取 `x` 的第7个成员
IndexError: list index out of range

>>> x[-7]
IndexError: list index out of range
```

但注意，在切片中提供超界的起始或结尾索引并不会导致错误。Python会使用最接近的合法起始或结尾值：

```python
# 切片并不要求索引在界限内
>>> x[:10000]
[0, 1, 2, 3, 4, 5]
```

<div class="alert alert-warning">

**警告！**

入门Python时，切片不检查索引超界这一特征可能会成为一大错误源。代码不报错不代表你代码计算的起始和结尾索引就是正确的！
</div>
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**阅读理解：索引和切片序列**

在Python中，**序列**（sequence）指任何包含对象的有序集合。你可以通过“索引”（indexing）来获取其中的成员，你也可以通过“切片”（slicing）来获取其中的子序列。你也在阅读本文时了解了Python的列表和字符串也都是序列这一事实。接下来的阅读理解题目将会帮助你探索序列索引和切片的强大性。

设元组：
```python
x = (0, 2, 4, 6, 8)
```
对 `x` 切片或索引来获取以下：

1. `0`
2. `8`（使用负索引）
3. `(2, 4, 6)`（使用切片）
4. `(4,)`
5. `4`
6. `4`（使用负索引）
7. `(6, 8)`（切片并使用负起始索引）
8. `(2, 6)`
9. `(8, 6, 4, 2)`

</div>
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：检查你的理解**

为下列每个任务编写一组代码来完成它。如果你认为任务不可能或要求不合理，请解释为什么。

1) 设字符串 "blogosphere"，通过切片和多次粘连，创建字符串：'boopeeboopeeboopeeboopeeboopee'。（提示：你该如何切片 "blogosphere" 来获得 "boopee" 呢？步距。）

2) 设包含对象 `5` 至少一次的元组 `x`。找出该对象第一次出现的位置，并将其修改成 `-5`。比如说，`(1, 2, 5, 0, 5)` $\rightarrow$ `(1, 2, -5, 0, 5)`。

3) 设序列 `x` 和 `x` 的一个合法的负索引 `neg_index`，找出该索引对应的正值。比如说，如果 `x = "cat"`，`neg_index = -3`（也就是 `"c"` 的负索引），那你应该返回索引 `0`。

</div>


## 官方说明文档链接

- [序列](https://docs.python.org/3/library/stdtypes.html#typesseq)
- [元组](https://docs.python.org/3/library/stdtypes.html#tuple)
- [不可变序列类](https://docs.python.org/3/library/stdtypes.html#immutable-sequence-types)
- [可变序列类](https://docs.python.org/3/library/stdtypes.html#mutable-sequence-types)

<!-- #region -->
## 阅读理解答案

**序列基础**

```python
# 1. 将列表 `[True, None, 22]` 转化成元组
>>> tuple([True, None, 22])
(True, None, 22)
```

2\. 我们介绍了三种序列性的Python类型：字符串，列表，和元组。在这其中，列表是唯一的可变对象。我们可以通过在一个已经创建的列表结尾附加新成员来演示它的可变性。
```python
>>> x = [1, 2, 3]
>>> x
[1, 2, 3]

>>> x.append("I'm different now")
>>> x
[1, 2, 3, "I'm different now"]
```
<!-- #endregion -->

<!-- #region -->
**索引和切片序列：解**

1. `x[0]`
2. `x[-1]`
3. `x[slice(1, 4, 1)]`
4. `x[2:3]`
5. `x[2]`
6. `x[-3]`
7. `x[-2:]`
8. `x[1:4:2]`
9. `x[:0:-1]`

**检查你的理解：解**

1) “boopee”也就是“blogosphere”的每两个字母，因此使用步距2来切片该序列，`"blogosphere"[::2]`，会返回“boopee”。我们可以使用 `seq*n` 来重复该序列5此。因此解是
```python
>>> "blogosphere"[::2]*5
'boopeeboopeeboopeeboopeeboopee'
```

2) 元组是不可变的。这意味着在创建后它们的内容是不可以被修改的，因此该任务不可能完成！写这个问题的我得有多聪明呢？我感觉自己贼聪明。哇，我真棒。

如果以上问题要求你修改一个*列表*，那解为：
```python
>>> x = [1, 2, 5, 0, 5]
>>> x[x.index(5)] = -5
>>> x
[1, 2, -5, 0, 5]
```

3) 查阅之前的索引图表来理解以下正索引和负索引的简单关系：
```python
pos_index = neg_index + len(x)
```
<!-- #endregion -->
