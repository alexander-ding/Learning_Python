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
   :description: Topic: Overview of formatting in Python Like You Mean It, Difficulty: Easy, Category: Instructions
   :keywords: overview, formatting, background, code block, console style
```

<!-- #region -->
# 代码格式的简单指南
本节将快速介绍本文将采用的代码格式和风格。你暂时不需要理解这些代码的具体细节。这个指南仅仅是为了让你了解接下来的内容的样貌。

任何嵌入在正文中的代码都会遵循如此格式：“变量 `x` 被更新...”。当这种格式不可用时，这类小代码片段会被两个反引号围绕。如下：

```python
# 变量 `x` 将被更新
```

长篇的Python代码则会存在于专门的彩色代码块中。这些代码块一般会由一行总结代码块作用的注解打头：

```python
# 演示基本的for循环
cnt = 0
for i in range(10):
    cnt += 1

# `cnt` 现在是10
```

符号 `>>>` 出现于“命令行风格”的代码中，用于区分用户输入的代码和命令行的输出。这类代码块的可以帮助我们轻松地显示计算结果，而不需要老是调用 `print` 函数。比如说，接下来的代码将设定变量 `x` 为整数 `1`，然后显示 `x + 2` 的结果：

```python
# 通过 >>> 演示输入和输出的不同格式

>>> x = 1
>>> x + 2
3
```

译者注：命令行在这里泛指各大平台上的类似Windows命令行的程序——你可能会叫它们Git Bash，Terminal，命令行，Shell，Kernel，等等。全文将用“命令台”一词来指代这种程序。

不管是“纯粹”的代码块还是“命令行风格”的代码块，请在章节内认为它们的效应是持久化的。比如说，在某一节开始定义的函数完全在这一节后面被调用：
```python
# 定义一个范例函数
def my_func(x):
    return x**2
```

我们可以花些时间讨论 `my_func` 并调用它：
```python
# 演示 `my_func`
>>> my_func(10.)
100.
```

最后，iPython命令台和Jupyter记事本的输入和输出都像如下显示：
<!-- #endregion -->

```python
2 + 3
```
