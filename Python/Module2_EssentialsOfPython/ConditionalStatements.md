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
   :description: Topic: Conditional Statements, Difficulty: Easy, Category: Section
   :keywords: if, else, elif, inline if, switch statement, comparison operator, bool, truth, is operator
```


# 条件语

<div class="alert alert-warning">

**注**：

在本资源各处会有一些阅读理解练习。这些题目旨在帮助读者活学活用文中的知识点。练习题的答案可以在本页底部找到。
</div>

<!-- #region -->
在本节中，我们将会介绍 `if`，`else`，和 `elif` 语句。它们允许你编写仅仅在某些条件满足时才会执行的代码片段，或者在条件不满足时执行某些其它代码。比如说，以下代码在 `x` 是负数的时候返回 `x` 的平方，在 `x` 是正数时返回 `x` 的立方：
```python
# 简单的if-else区块
if x < 0:
    x = x ** 2
else:
    x = x ** 3
```

请通过“基本对象类型“一节来回忆布尔类型（代表真或否值）的一些基础特征。我们将会延续该节的讨论来介绍比较运算和成员检查，并讨论内置 `bool` 类的更多有用之处。
<!-- #endregion -->

<!-- #region -->
## 比较运算
比较语句（comparison statement）最后一定会返回某个布尔对象：`True` 或 `False`。在Python中总共有八种比较运算：

| 运算符   | 含义                    |
| --------- | ----------------------- |
| `<`       | 小于（不等于）            |
| `<=`      | 小于或等于      |
| `>`       | 大于（不等于）   |
| `>=`      | 大于或等于   |
| `==`      | 等于                   |
| `!=`      | 不等于               |
| `is`      | 相同对象         |
| `is not`  | 不相同对象 |

其中前六个运算符和数学中常见的相对应：

```python
>>> 2 < 3
True
```

注意 `=` 和 `==` 有着两个很不一样的含义。前者是赋值运算符，而后者是等值运算符：

```python
>>> x = 3   # 对变量 `x` 赋值 3
>>> x == 3  # 检查 `x` 和 3 是否有着同样的值
True
```

Python 允许你连接多个对比运算符来创建”复式“（compound）对比：

```python
>>> 2 < 3 < 1  # 等值于 (2 < 3) and (3 < 1)
False
```

`==` 运算符检查两个对象是否有着同样的值，而 `is` 运算符检查两个对象是否是*相同的*对象。比如说，创建两个有着一样内容的列表会得到两个*不同*的列表，但它们的“值”一样：

```python
# 演示 `==` 对比 `is`
>>> x = [1, 2, 3]
>>> y = [1, 2, 3]

>>> x == y
True

# `x` 和 `y` 引用两个等值但不同的列表
>>> x is y
False
```

因此 `is` 运算符最常用来检查一个变量是否引用 `None` 对象，或两个布尔对象中的一个：
```python
>>> x = None
>>> x is None
True

# (2 < 0) 返回对象 `False`
# 因此这会成为：`False is False`
>>> (2 < 0) is False
True
```

使用 `is not` 来检查两个对象是否不是同一个对象：
```python
>>> 1 is not None
True
```
<!-- #endregion -->

<!-- #region -->
## `bool` 和非布尔类对象的真值
请回忆两个布尔对象 `True` 和 `False` 严格来讲即属于 `bool` 也属于 `int` 类型，并分别有着值 `1` 和 `0` 这一事实：

```python
>>> isinstance(True, int)
True

>>> int(True)
1

>>> isinstance(False, int)
True

>>> int(False)
0

>>> 3*True - False
3

>>> True / False
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-4-f8487d9d0863> in <module>()
----> 1 True / False

ZeroDivisionError: division by zero
```

类似的，Python也会对非布尔对象提供相应的布尔值。比如说，数字0对应着 `False`，而任何非0的数字则对应 `True`。内置对象的布尔值可以通过Python内置的命令 `bool` 来得出：

```python
# 使用 `bool` 来查看非布尔类对象
# 的布尔值为 `True` 或 `False`
>>> bool(0)
False
```

任何非0的Python整数对应 `True`：

```python
# 非0值最后评判为 `True`
>>> bool(2)
True
```
以下的内置Python对象通过 `bool` 对应 `False`：

- `False`
- `None`
- 任何数字类的0:`0`, `0.0`, `0j`
- 任何空序列，如空字符串或列表：`''`，`tuple()`，`[]`，`numpy.array([])`
- 空词典（dictionary）或集（set）

因此任何非0的数字或非空的序列或集合都会通过 `bool` 对应 `True`。

<div class="alert alert-info">

**经验**：

`bool` 函数允许你得到非布尔类对象对应的布尔值。比如说，`bool([])` 返回 `False`，而 `bool([1, 2])` 返回 `True`。
</div>
<!-- #endregion -->

<!-- #region -->
## `if`，`else`，和 `elif`
我们现在将介绍简单而强大的 `if`，`else`，和 `elif` 条件语句（conditional statement）。这将允许我们在代码中创建简单的分枝。比如说，假设你在为某电子游戏编写代码，并想要根据你角色的血量（整数）来更新ta的状态，下面的代码就是一个例子：

```python
if num_health > 80:
    status = "good"
elif num_health > 50:
    status = "okay"
elif num_health > 0:
    status = "danger"
else:
    status = "dead"
```

每个 `if`，`elif`，和 `else` 语句必须由冒号结尾，且其中所有语句都必须由[空格限定](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Introduction.html#Python-Uses-Whitespace-to-Delimit-Scope)。

以下的伪代码演示条件语句的通用模版：

```
if <expression_1>:
    本缩进区域的代码只有在以下条件满足时才会被执行...
    - bool(<expression_1>) 为 True
elif <expression_2>:
    本缩进区域的代码只有在以下条件满足时才会被执行...
     - bool(<expression_1>) 之前为 False
     - bool(<expression_2>) 为 True
...
...
elif <expression_n>:
    本缩进区域的代码只有在以下条件满足时才会被执行...
      - bool(<expression_1>) 之前为 False
      - bool(<expression_2>) 之前为 False
      ...
      ...
      - bool(<expression_n-1>) 之前为 False
      - bool(<expression_n>) 为 True
else:
    本缩进区域的代码只有在
    之前所有表达式为 False 时
    才会被执行...

```

实际运用时它可以看起来是这样：

```python
x = [1, 2]

if 3 < len(x):
    # bool(3 < 2) 返回 False，所以此区块的代码将被跳过
    print("`x` has more than three items in it")
elif len(x) == 2
    # bool(len(x) == 2) 返回 True，此区块的代码被执行
    print("`x` has two items in it")
elif len(x) == 1
    # 此语句根本没有被检查
    print("`x` has one items in it")
else:
    # 此语句根本没有被检查
    print("`x` is an empty list")

"`x` has two items in it"
```

在最简洁的格式下，一个条件语句只需要一个 `if` 句段。`else` 和 `elif` 句段只能在一个 `if` 句段之后。

```python
# 一个只有“if”句段的条件语句
x = -1

if x < 0:
    x = x ** 2
# `x` 现在为 1
```

类似的，条件语句可以有一个 `if` 和一个 `else` 而没有任何 `elif`：

```python
# 一个有一个“if”句段和一个“else”句段
# 的条件语句
x = 4

if x > 2:
    x = -2
else:
    x = x + 1
# `x` 现在为 -2
```

条件语句也可以有一个 `if` 和一个 `elif` 而没有 `else`：

```python
# 一个有一个“if”句段和一个“elif”句段
# 的条件语句
x = 'abc'

if len(x) < 9:
    x = x * 3
elif len(x) > 40:
    x = 'cba'
# `x` 现在为 'abcabcabc'
```

请注意在一个if-elif-else语句中只有其中一段可以被执行；要么“if”区块的代码被执行，要么一个“elif”区块的代码被执行，要么“else”区块的代码被执行。但是，背靠背的if语句是完全独立于对方的。因此它们的代码区块可以被在一次运行中都执行（假设它们对应的条件语句都最后为 `True`）。

```python
# 连续的if语句是互相独立的
x = 5
y = 0

if x < 10:
    y += 1 

if x < 20:
    y += 1
    
# y 现在为 2
```
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**阅读理解：条件语句**

1\. 设 `my_list` 为一个列表。根据以下代码：
```python
first_item = None

if my_list:
    first_item = my_list[0]
```

如果 `my_list` 为 `[]`，会发生什么？这是否会产生一个 `IndexError`？`first_item` 会是什么？

2\. 设变量 `my_file` 为一个存储着文件名的字符串，其中英文句号 `.` 代表着文件名的结尾和文件类型的开始。编写代码来获取 `my_file` 的文件名。

`my_file` 中最多只会有一个句号。妥善处理 `my_file` *没有*文件类型的情况。

也就是说：

- `"code.py"` $\rightarrow$ `"code"`
- `"doc2.pdf"` $\rightarrow$ `"doc2"`
- `"hello_world"` $\rightarrow$ `"hello_world"` 

</div>
<!-- #endregion -->

<!-- #region -->
### 单行的if-else语句
Python支持一个有限制版本的一行长的if-else语句。以下代码：

```python
if num >= 0:
    sign = "positive"
else:
    sign = "negative"
```

可以被简写成：

```python
sign = "positive" if num >=0 else "negative"
```

这代表了单行（inline）if-else语句的内在语法：

<div class="alert alert-info">

**单行if-else语句**：

表达式 `A if <condition> else B` 当 `bool(<condition>)` 为 `True` 时返回 `A`，不然该表达式会返回 `B`。
</div>

和完整的“if-elif-else”表达式对比，这个语法是极其受限制的——它不允许“elif”语句，也不允许if/else之间多行的代码块。

单行if-else语句可以在任何地方使用，不仅仅是在赋值语句的右侧。这有时很方便：
```python
# 在不同情况下使用单行if-else语句

>>> x = 2

# 如 `x` 并不是负数，这将储存 1
# 如 `x` 为负数，这将储存 0
>>> my_list = [1 if x >= 0 else 0]
>>> my_list
[1]

>>> "a" if x == 1 else "b"
'b'
```
我们将在讨论列表理解语句（list comprehension statement）时注意到这个语法很有用的地方。但请注意谨慎使用这一语法。比如说，不要在数学表达式中使用单行if-else语句，因为最后的结果令人抓狂：

```python
# 永远不要这么做，永远！
2 - 3 if x < 1 else 1 + 6*2 if x >= 0 else 9
```
<!-- #endregion -->

<!-- #region -->
## 短路逻辑表达式
在对条件语句有了更深刻的理解之后，我们将扩展我们对Python逻辑表达式的研究来讨论短路（short-circuiting）。在Python中，一个逻辑表达式会从左到右运算，并在表达式结果确定的那一步就返回，并*不会运算表达式剩下的部分*。也就是说，表达式可以*短路*。

比如说，`and` 运算仅仅会在它两边的表达式都为 `True` 时才会返回 `True`。因此，表达式 `False and <anything>` 一定会返回 `False`；同时，这个表达式会返回 `False` 而*不运算* `bool(<anything>)`。

为了演示这一行为，请见下面的例子：

```python
# 演示短路逻辑表达式
>>> False and 1/0  # 运算 `1/0` 会导致错误
False
```
根据上文的讨论，`False and` 会短路这个表达式，让它不运算 `bool(1/0)` 就返回。换转参数顺序会明显地揭示这一点。

```python
# 表达式是从左到右运算的
>>> 1/0 and False
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-1-3471672109ee> in <module>()
----> 1 1/0 and False

ZeroDivisionError: division by zero
```

在实际操作中，你可以通过短路来浓缩你的代码。假设我们代码的一节在处理变量 `x`，其可能是一个[数字](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Number-Types)或[字符串](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Strings)。假设我们当 `x` 是全大写的字符串时想要有特殊的处理方式。以下代码

```python
# 这在 `x` 不是字符串时会报错
if x.isupper():
    # 对此大写字符串进行操作
```
是有问题的，因为 `isupper` 只应该在我们确定 `x` 是字符串之后才调用；如果 `x` 是一个数字，我们的代码会报错。我们可以重写代码

```python
# 一个可用但难看的过滤非字符串对象的方法
if isinstance(x, str):
    if x.isupper():
        # 对此大写字符串进行操作
```

但一个更加优雅和简短的处理多层次检测的方法是短路逻辑表达式。

```python
# 利用短路来简写必要的检查
if isinstance(x, str) and x.isupper():
    # 对此大写字符串进行操作
```

请注意如果 `x` 不是字符串，那 `isinstance(x, str)` 会返回 `False`；因此 `isinstance(x, str) and x.isupper()` 会短路并直接返回 `False`，从来不运算 `bool(x.isupper())`。这是这种检查任务的推荐方法。这代码对比等值的多层if语句更简短和可读。
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**阅读理解：短路表达式**

回忆前文检查 `x` 是大写字符串的范例。以下代码的“bug“是什么？为什么它并没有正确地利用短路？

```python
# 我做错了什么？
if x.isupper() and isinstance(x, str):
    # 对此大写字符串进行操作
```

</div>
<!-- #endregion -->

## 官方说明文档连接

- [布尔值](https://docs.python.org/3/library/functions.html#bool)
- [检查真值](https://docs.python.org/3/library/stdtypes.html#truth-value-testing)
- [布尔操作符](https://docs.python.org/3/library/stdtypes.html#boolean-operations-and-or-not)
- [对比](https://docs.python.org/3/library/stdtypes.html#comparisons)
- ['if'语句](https://docs.python.org/3/reference/compound_stmts.html#the-if-statement)


## 阅读理解答案：

<!-- #region -->
**条件语句：解**

1\. 如果 `my_list` 为 `[]`，那 `bool(my_list)` 会返回 `False`，而对应的代码块则会被跳过。因此 `first_item` 会是 `None`。

2\. 首先，检查 `my_file` 是否包含 `.`。如果是，找到它的索引并对字符串进行切片（切到该索引）。如果不是，`my_file` 就已经是文件名了。
```python

my_file = "code.pdf"

if "." in my_file:
    dot_index = my_file.index(".")
    filename = my_file[:dot_index]
else:
    filename = my_file
```
<!-- #endregion -->

<!-- #region -->
**短路表达式：解**

此代码
```python
# 我做错了什么？
if x.isupper() and isinstance(x, str):
    # 对此大写字符串进行操作
```

并没有考虑表达式从左到右运算的顺序。也就是说，在这个例子中 `bool(x.isupper())` 永远都会首先被运算，并且会在 `x` 不是字符串时报错。因此，之后的 `isinstance(x, str)` 语句完全没用。
<!-- #endregion -->
