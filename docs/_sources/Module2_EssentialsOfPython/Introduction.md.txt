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
   :description: Topic: introduction to control flow, Difficulty: Easy, Category: Section
   :keywords: overview, summary, if, else, function, for-loop, if, else, control flow
```

<!-- #region -->
# 控制流简介
简单来讲，“控制”你代码的“流程”（control flow）就是去影响你程序中代码执行的顺序。到现在为止，本文中的所有代码都是线性执行的；比如说：

```python
# 没有任何“控制流”的简单代码，也就是说，
# 代码没有任何逻辑分枝，循环，或代码包装
x = 6
y = 23
print("x + y = ", x + y)
print("x - y = ", x - y)
```

但假如你想要让你的代码在 `x` 是偶数时有着不同的行为呢？或者假设你想要为 `x` 和 `y` 之中的所有数字做一些计算呢？在本模组结尾，你应该会理解如何编写代码来提供这些和其它更多的逻辑分枝。

控制流工具将会极大地提升你编写实用代码的能力。它们是现代编程语言经典的成分，因此它们在Python，C，C++，Java，和其它很多编程语言中的功能都差不多（语法区别除外）。

作为一个预览，让我们编写一个返回 m 和 n 之间有多少个可以被3整除的数字的函数：

```python
def cnt_div_by_3(m, n):
    """ 返回在 [m, n] 中有多少被3整除的数字。"""
    count = 0
    for num in range(m, n + 1):
        if num % 3 == 0:  # 请回忆：x % y 会提供 x / y 的余数
            count += 1
        else:
            pass # 这个 `else-pass` 语句其实并没有必要
                 # 我们仅仅为了此简介的清晰解释在此包含
    return count
```
（注：我们有其它有效率高很多的方法来计算这个结果——你可以想到任何方法吗？）

此代码包含着几个很重要的“控制流”特征：

- 语句 `def cnt_div_by_3(m, n):` 定义一个函数：可以在程序其它地方调用的模组化的代码块。
- `for num in range(m, n + 1):` 这行则示意一个for循环，让代码迭代一序列的数字并每轮执行同一组代码。
- `if num % 3 == 0:` 和 `else` 指示程序有条件执行代码片段——也就是在特定条件满足时才执行。

在下文的节中，你将会正式地学习if-elif-else组，for循环和可迭代对象，以及函数来帮助你在代码中执行有效的控制流。

在深入讨论这些内容之前，让我们花一些时间来学习Python限定不同控制流结构体区域的语法。

## Python使用空格来限定区域

虽然这些函数定义，循环，和有条件执行的语句在大部分现代编程语言通用，这些语言的语法区别经常在于它们限定控制流结构体*主体*的方法。比如说，C++使用弯括号来限定逻辑区块，如：

```cpp
// 演示C++使用弯括号来限定区域
int x = 1;

if (x > 10)
    {
    // 我们在的if区块被弯括号限定包围（限定）
    x = x + 1;
    }
// 我们现在在if区块外面
x = x + 3;
```

Python**使用空格（也就是缩进）来限定区域（scope）**：

```python
# 演示Python使用空格来限定区域
x = 1

if x > 10:
    # 我们在一个if区块中；此行由4个空格打头
    x = x + 1
# 我们现在在if区块外面；这里没有任何空格打头
x = x + 3
```

看一下本节开始的范例并注意其函数定义，for循环，和里面的if-else区块都由逐渐增加的缩进来区分。

Python的语法对空格限定符的定义比较宽松。它的规则为：

- 一个或更多的空符（空格和tab）足以作为缩进。
- 一个有缩进的区块的每一行必须使用同样的缩进级别。比如说，如果有缩进的区块的第一行有两个打头的空格，那么此区块之后的每一行都要由两个空格打头。

虽然Python的语法比较宽松，但我并不如此：Python缩进的[标准风格](https://www.python.org/dev/peps/pep-0008/#indentation) 是每一层**使用四个空格**。我强烈建议你遵循这个标准。大部分的IDE和命令行（包括Jupyter记事本）会自动在你进入某个以上的构造体时为你增加四个空格的缩进。

让我们通过几个例子（包括错误的例子）来复习一下在Python中限定区域的规则。

```python
# 可以，但很恶心：使用一个空格使得缩进很难看清。用四个空格
if True:
 x = 1  # 一个空格
 y = 2  # 一个空格
```
<!-- #endregion -->

<!-- #region -->
```python
# 不合法：在一个区块中使用多个级别的缩进会导致 IndentationError
def my_func(x):
    x = x + 1  # 四个空格
       y = 3   # 八个空格
    z = x + y  # 四个空格
    return z   # 四个空格
```

```python
# 可以，但很恶心：if区块使用四个空格来限定。而else区块使用两个空格
# 来限定。这虽然合法（因为在每个区块中的缩进相同），但是你应该永远
# 使用四个空格来缩进
if True:
    x = 3  # 四个空格
    y = 2  # 四个空格
else:
  x = 2  # 两个空格
  y = 1  # 两个空格
```

```python
# 棒棒的！for循环的主体由一层的四空格的缩进定义，而其中的if-else
# 区块有着自己的额外的四空格缩进。
for i in [1, 2, 3, 4]:
    if i == 2 or i == 4:  # 四个空格
        x = "even"        # 八个空格
    else:                 # 四个空格
        x = "odd"         # 八个空格
```
<!-- #endregion -->
