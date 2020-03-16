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
   :description: Topic: variable naming and assignment, Difficulty: Medium, Category: Section
   :keywords: variable naming, valid names, mutable, immutable, reference, pointer
<!-- #endraw -->

# 变量和赋值

<div class="alert alert-warning">

**注**：

本文各处的阅读理解练习旨在帮助你练习使用文章中的知识点。练习题的答案可以在本页结尾找到。
</div>

<!-- #region -->
变量的意义在于帮助我们编写灵活易重用的代码。假设我们希望编写代码来记录学生的考试成绩，这过程背后的逻辑不管是记录小明的92分还是小丽的94分都应该是一样的。为此我们可以使用变量，如 `name` 和 `grade`，来作为这些信息的占位符。在本小节中，我们将会展示如何在Python中定义变量。

在Python中，符号 `=` 代表着“赋值”（assignment）操作符。变量在 `=` 左边，而用来赋值的对象在符号的右边：
```python
name = "Brian"  # 为变量 `name` 赋值字符串 "Brian"
grade = 92      # 为变量 `grade` 赋值整数 92
```
将左右弄混（如 `92 = name`）会导致语法错误。当变量被某个对象（如一个数字或字符串）赋值时，我们经常会说这变量是该对象的*引用*（reference）。比如说，变量 `name` 是字符串 `"Brian"` 的引用。这意味着，当一个变量被对象赋值后，你可以在代码的其它地方将其作为该对象的代名词（或占位符）使用：
```python
# 演示使用变量
name = "Brian"
grade = 92
failing = False

if grade < 60:
    failing = True

# 在文本文件“student_grades.txt”最后写: 
# 名字 | 成绩 | 不及格
with open("student_grades.txt", mode="a") as opened_file:
    opened_file.write("{} | {} | {}".format(name, grade, failing))
```

## 变量的合法名字
合法的变量名字可以包括任何字母或数字（`a-z`，`A-Z`，`0-9`）以及下划线（`_`）；合法的名字不可以由数字打头。

- `var`：合法
- `_var2`：合法
- `ApplePie_Yum_Yum`：合法
- `2cool`：**不合法**（数字打头）
- `I.am.the.best`: **不合法**（包括了 `.`）

变量名也不能和其它Python保留的特殊名字冲突。因此，下列名字也不能作为变量名：

- `for`，`while`，`break`，`pass`，`continue`
- `in`，`is`，`not`
- `if`，`else`，`elif`
- `def`，`class`，`return`，`yield`，`raises`
- `import`，`from`，`as`，`with`
- `try`，`except`，`finally`

还有一些unicode符号可以作为Python变量名中的字符，但本文在此不进行讨论。
<!-- #endregion -->

<!-- #region -->
## 可变和不可变对象
对象的**可变性**指它的内态（state）是否能被改变。**可变对象**的内态可以被修改，而**不可变对象**则不能。比如说，列表就是可变对象的一种。在创建之后，我们可以更新列表的内容——代替，增加，或删除它的成员。

```python
# 演示列表的可变性
>>> x = [1, 2, 3]
>>> x[0] = -4  # 将 `x` 索引为0的成员修改成 -4
>>> x
[-4, 2, 3]
```

让我们来看看这里具体发生了什么，我们：

1. 创建（初始化）了一个列表，其内态为 `[1, 2, 3]`。
2. 为变量 `x` 赋值了此列表；`x` 现在是该列表的一个引用了。
3. 通过我们的引用变量 `x` 更新了列表索引为0的成员以储存整数 `-4`。

这并没有创建一个新的列表对象，而仅仅*修改*了原本的列表。这也是为什么在命令行中打印 `x` 会显示 `[-4, 2, 3]` 而不是 `[1, 2, 3]`。

元组则是不可变对象的一种。在创建之后，我们没有任何办法可以改变元组的内态；任何看起来像是在更新元组的代码起始都是在创建一个全新的元组。

```python
# 演示元组的不可变性
>>> x = (1, 2, 3)
>>> x[0] = -4  # 试图将 `x` 索引为0的成员修改为 -4
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-11-a858573fdc63> in <module>()
      1 x = (1, 2, 3)
----> 2 x[0] = -4  # attempt to replace element-0 of `x` with -4

TypeError: 'tuple' object does not support item assignment
```

### 可变和不可变对象类型
以下是Python中常见的一些可变和不可变对象。在我们开始讨论词典（dictionary）和集（set）时这些知识会很有用。
<!-- #endregion -->

**一些不可变对象**

 - [数字](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#数字类型)（整数，浮点数，复数）
 - [字符串](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#字符串)
 - [元组](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#元组) 
 - [布尔值](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#布尔类)
 - ["冻结"集](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython%2FDataStructures_III_Sets_and_More.html#集数据结构)（frozen-set）

**一些可变对象**

 - [列表](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#列表)
 - [词典](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html)
 - [集](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_III_Sets_and_More.html#集数据结构)
 - [NumPy数组](https://cn.pythonlikeyoumeanit.com/module_3.html)

<!-- #region -->
## 多个变量引用一个可变对象
你可以向变量赋值给其它已经存在的变量。这将会导致这些变量都引用同一个对象：
```python
# 演示引用同一个对象的变量行为
>>> list1 = [1, 2, 3]  #  `list1` 引用 [1, 2, 3]
>>> list2 = list1      #  `list2` 和 `list1` 现在都引用 [1, 2, 3]

>>> print(list1)
[1, 2, 3]

>>> print(list2)
[1, 2, 3]
```

具体来讲，这两个变量都引用这列表的*同一个实例*（instance）。这意味着如果列表有变化，所有引用它的变量都会反映这个变化：

```python
>>> list1.append(4)  # 在 [1, 2, 3] 结尾附加4
>>> print(list1)
[1, 2, 3, 4]
```

我们可以看到 `list2` 还在引用*相同的，更新过的*列表，所以它和 `list1` 同步：
```python
>>> print(list2)
[1, 2, 3, 4]
```
一般来讲，为变量 `b` 赋值变量 `a` 会导致它们引用系统内存中*相同*的对象，而为变量 `c` 赋值变量 `a` 或 `b` 会导致有第三个变量引用同样的对象。然后，该对象的任何变化（也就是*mutation*）会在所有引用它的变量中反映（`a`，`b`，和`c`）。

当然，为两个变量赋值内容一样但*不同*的列表意味着一个列表的变化不会影响另外那个：

```python
>>> list1 = [1, 2, 3]  #  `list1` 引用 [1, 2, 3]
>>> list2 = [1, 2, 3]  #  `list2` 引用*另外*一个列表，其值为 [1, 2, 3]

>>> list1.append(4)  # 在 [1, 2, 3] 结尾附加4
>>> print(list1)
[1, 2, 3, 4]

>>> print(list2)     # `list2` 依然引用它自己的列表
[1, 2, 3]
``` 
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：切片列表是否会创建一个列表的引用？**

设变量 `x`，赋值为一个列表，和变量 `y`，赋值为 `x` 的一个“切片”。`x` 和 `y` 是否引用同样的列表？也就是说，当你更新 `x` 和 `y` 的子序列时，变化是否会在两个变量中都显示？请编写一些简单的代码来查明此问题。

</div>

<!-- #region -->
<div class="alert alert-info">

**阅读理解：理解引用**

根据我们对可变和不可变对象的讨论，预测下列情况中 `y` 的最终值：

```python
>>> x = 3
>>> y = x

# 和 `x = x * 3` 等值
>>> x *= 3
>>> x
9

>>> y
???
```

</div>
<!-- #endregion -->

<!-- #region -->
## 阅读理解答案

**切片列表是否会创建一个列表的引用？：解**

根据以下的行为，我们可以得出切片列表*并不*创建一个列表的新引用。反而，切片列表会创建对应列表子序列的新列表：
```python
>>> x = [0, 1, 2, 3]

>>> y = x[:2] 
>>> y      # `y` 是否和 `x` 引用同一个列表？
[0, 1]

>>> x[0] = -1  # 更新 `x` 引用的列表中的某个成员
>>> x
[-1, 1, 2, 3]

>>> y      # `y` 引用的列表并没有反映这个更新
[0, 1]
```
<!-- #endregion -->

**理解引用：解**

整数不可变，因此 `x` 肯定是引用一个全新的对象（`9`），而 `y` 还在引用 `3`。
