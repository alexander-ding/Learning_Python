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

因为操作对象/数据序列的能力极其重要，我们第一课（相对）深入的Python探讨将会学习序列类型。前面的阅读向读者介绍的Python的列表和字符串。虽然两者能存储的内容不同，*列表和字符串都是一种序列*——它们能够存储一组有限的有顺序的对象（比如说 `"cat"` 和 `"tac"`，虽然成员字符相同，是两个不一样的字符串）。因此，列表，字符串，和其它Python的序列类型共享一个常用的接口来帮助用户查阅，调用，和总结它们的内容。

在本节中，我们将：

- 介绍元组——我们将见到的最后一种内置序列类型。
- 展示用来查阅和总结序列内容的通用接口。
- 细致讨论Python的检索系统。我们可以用它来查阅序列具体的项目或子序列。
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

**检查多种类型**: 

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

**检查某对象是否为序列成员：**`obj in seq`
```python
# 利用 'in' 和 'not in' 在检查是否为成员
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
# 通过 '+' 来连接两个序列
>>> [1, 2] + [3, 4]  # 这将创建一个新序列
[1, 2, 3, 4]

>>> "c" + "at"
"cat"
```

**重复连接某序列：**`n*seq1` 或 `seq1*n`
```python
# 和 `cat + cat + cat` 等值
>>> "cat" * 3   # 创建一个新字符串
'catcatcat'

>>> 4 * (1, 5)  # 创建一个新元组
(1, 5, 1, 5, 1, 5, 1, 5)
```

**检查序列成员数：**`len(seq)`
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

**返回序列中** `x` **出现的次数**: `seq.count(x)`
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

2\. 到现在，我们讨论了多少个序列类型？其中哪些创建不可变对象？哪些创建可变对象？在可变类型中选择一个并编写修改其对象的范例代码。

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

我们也可以切片（“slice”）序列，也就是提供起始索引和结尾索引，来返回其中包含的子序列：
```python
# 通过“切片”来提供序列内容的子序列
>>> x[0:3] # 这子序列包含索引为0，1，2（注意不包含3）的项目
"abc"
```
熟练访问序列成员和子序列是极其重要的技能。这索引功能也会在我们操作NumPy数组（array）时出现。接下来，让我们来具体描述Python的索引和切片功能。

### 索引
Python允许你通过序列中单个成员的*索引*（index），一个识别成员在序列中未知的独特整数，来获取该成员。*Python使用从0开始的索引模式*，并允许你是用负数来从序列结尾往开头数。拿字符串 `"Python"` 来说，接下来的图表将显示该序列的索引：
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
>>> x[0]     # 索引从0开始
1

>>> x[-4]    # 每个成员都有着对应的正索引和负索引
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

去对一个序列“索引”指的是通过提供序列中单个成员的索引，一个表达成员在序列中位置的整数，来获取它：`seq[index]`。Python使用一个从0开始的索引系统，也就是说序列中第一个成员是索引0。负索引允许你通过对象相对序列结尾的位置来获取它。
</div>
<!-- #endregion -->

<!-- #region -->
### 切片
Slicing a sequence allows us to retrieve a subsequence of items, based on the indexing scheme that we reviewed in the preceeding subsection. Specifying a slice consists of:

- A start-index: the sequence-position where the slice begins (this item is *included* in the slice).
- A stop-index: the sequence-position where the slice ends (this item is *excluded* from the slice).
- A step-size, which permits us to take every item within the start & stop bounds, or every *other* item, and so on. It is important to note that a negative step-size permits us to traverse a sequence *in reveresed order*. 

The basic syntax for slicing is: `seq[start:stop:step]`, using colons to separate the start, stop, and step values.

```python
# demonstrating the basics of slicing a sequence
>>> seq = "abcdefg"

# start:0, stop:4, step:1
>>> seq[0:4:1]
'abcd'

# start:1, stop:4, step:1
>>> seq[1:4:1]
'bcd'

# start:1, stop:5, step:2
>>> seq[0:5:2] # get every other entry within [start, stop)
'ace'

# starting and stopping at the same index produces an empty sequence
# start:0, stop:0, step:1
>>> seq[0:0:1]
''
```

Slicing provides sensible default start, stop, and step values. Their default values are:

- start: 0
- stop: `len(seq)`
- step: 1

You can omit any of these values or specify `None` in that entry to use the default value. You can omit the second colon entirely, and the slice will use a step-size of 1.  
```python
# using default start, stop, and step values
>>> seq = "abcdefg"

# start: 0, stop: 7, step: 1 
>>> seq[:]  # equivalent: `seq[None:None]`
'abcdefg'

# start: 0, stop: 7, step: 2
>>> seq[::2]
'aceg'
```

Negative values can also be used in a slice. Specifying a negative step-value instructs the slice to traverse the sequence *in reverse order*. In this case, the default start and stop values will change so that `seq[::-1]` produces the sequence in reverse.

```python
# using a negative step size reverses the order of the sequence
>>> seq[::-1]
'gfedcba'
```

As we saw with using negative indices, specifying negative start/stop values in a slice permits us to indicate indices relative to the end of the list. 
```python
# a slice returning the last two values of the sequence
>>> seq[-2:]
'fg'

# a slice returning all but the last two values of the sequence
>>> seq[:-2]
'abcde'
```

Although the colon-syntax for slicing, `seq[start:stop:step]`, appears nearly ubiquitously in Python code, it is important to know that there is a built-in `slice` object that Python uses to form slices. It accepts the same start, stop, and step values, and produces the same sort of slicing behavior:
```python
# using the `slice` object explicitly
>>> seq = "abcdefg"
>>> seq[slice(0, 3, 1)]
'abc'
```

This gives you the ability to work with slices in more creative ways in your code, since it allows you to assign a variable to a slice.
```python
# using the `slice` object to slice several sequences
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

**Takeaway**:

To "slice" a sequence is to retrieve a subsequence by specifying a start-index (included), a stop-index (excluded), and a step value. Negative values can be supplied for these, and default values are available as well. The common slicing syntax `seq[start:stop:step]` is actually just a nice shorthand for using a `slice` object: `seq[slice(start, stop, step)]`.  
</div>
<!-- #endregion -->

<!-- #region -->
#### Handling out-of-bounds indices
Attempting to get a member from a sequence using an out-of-bounds index will raise an `IndexError`:
```python
>>> x = [0, 1, 2, 3, 4, 5] # x only contains 6 items
>>> x[6]  # try to access the 7th item in `x`
IndexError: list index out of range

>>> x[-7]
IndexError: list index out of range
```

However, specifying an out-of-bounds start or stop value for a slice does not raise an error. Instead, the nearest valid start/stop value is used instead:
```python
# no bounds checking is used for slicing
>>> x[:10000]
[0, 1, 2, 3, 4, 5]
```

<div class="alert alert-warning">

**Warning!**

The lack of bounds-checking for slices can be a major source of errors when starting out with Python. Just because your code isn't raising an error does not mean that you have computed the correct start/stop values for your slice!
</div>
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**Reading Comprehension: Indexing and Slicing Sequences**

In Python, a **sequence** is any ordered collection of objects whose contents can be accessed via "indexing". A sub-sequence can be accessed by "slicing" the sequence. You saw, in the required reading, that Python's lists and strings are both examples of sequences. The following questions will help you explore the power of sequence indexing and slicing.

Given the tuple: 
```python
x = (0, 2, 4, 6, 8)
```
Slice or index into `x` to produce the following:

1. `0`
2. `8` (using a negative index)
3. `(2, 4, 6)` (using a slice-object)
4. `(4,)`
5. `4` 
6. `4` (using a negative index)
7. `(6, 8)` (using a negative index for the start of the slice)
8. `(2, 6)`
9. `(8, 6, 4, 2)`

</div>
<!-- #endregion -->

<div class="alert alert-info">

**Reading Comprehension: Checking Your General Understanding**

Write a piece of code for each of the following tasks. If the task is impossible/ill-posed explain why.

1) Using the string "blogosphere", slicing, and repeat-concatenation, create the string: 'boopeeboopeeboopeeboopeeboopee'. (hint: how would you slice "blogosphere" to produce "boopee", think step-size)

2) Assume that a tuple, `x`, contains the item `5` in it at least once. Find where that first entry is, and change it to `-5`. For example `(1, 2, 5, 0, 5)` $\rightarrow$ `(1, 2, -5, 0, 5)`.

3) Given a sequence, `x`, and a valid negative index for `x`, `neg_index`, find the corresponding positive-value for that index. That is, if `x = "cat"`, and `neg_index = -3`, which is the negative index that would return `"c"`, then you would want to return the index `0`. 

</div>


## Links to Official Documentation

- [Sequences](https://docs.python.org/3/library/stdtypes.html#typesseq)
- [Tuples](https://docs.python.org/3/library/stdtypes.html#tuple)
- [Immutable Sequence Types](https://docs.python.org/3/library/stdtypes.html#immutable-sequence-types)
- [Mutable Sequence Types](https://docs.python.org/3/library/stdtypes.html#mutable-sequence-types)

<!-- #region -->
## Reading Comprehension Exercise Solutions:

**Basics of sequences**

```python
# 1. Change the list [True, None, 22] into a tuple.
>>> tuple([True, None, 22])
(True, None, 22)
```

2\. We have been introduced to three Python types that are sequential in nature: strings, lists, and tuples. Among these, lists are the only mutable objects. We can demonstrate this by simply appending a new element to the end of a list that has already been constructed.
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
**Indexing and Slicing Sequences: Solutions**

1. `x[0]`
2. `x[-1]`
3. `x[slice(1, 4, 1)]`
4. `x[2:3]`
5. `x[2]`
6. `x[-3]`
7. `x[-2:]`
8. `x[1:4:2]`
9. `x[:0:-1]`

**Checking Your General Understanding: Solutions**

1) "boopee" is every-other letter in "blogosphere", thus slicing this sequence with a step-size of 2, `"blogosphere"[::2]`, returns "boopee". We can then use `seq*n` to repeat this sequence five times. Thus the solution is
```python
>>> "blogosphere"[::2]*5
'boopeeboopeeboopeeboopeeboopee'
```

2) Tuples are immutable objects. This means that their content cannot be changed once it is created. Thus this question is ill-posed! How clever am I for writing that question? I feel so clever. Wow. I'm great.

If that question was posed in terms of a *list*, then the solution would be:
```python
>>> x = [1, 2, 5, 0, 5]
>>> x[x.index(5)] = -5
>>> x
[1, 2, -5, 0, 5]
```

3) Refer to the "index" diagram to see that this is the simple relationship between positive and negative indices for a given sequence: 
```python
pos_index = neg_index + len(x)
```
<!-- #endregion -->
