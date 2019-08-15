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
   :description: Topic: Contorl flow with for-loops and while-loops, Difficulty: Easy, Category: Section
   :keywords: for-loop, while-loop, break, control flow, basic programming
```

# For循环和While循环
在本节中我们将会介绍“for循环”控制流以及“可迭代物”（iterable）的正式定义。这些功能极其强大强大。在未来，你会在你编写的Python代码中随处见到这些概念！


<div class="alert alert-warning">

**注**：

在本资源各处会有一些阅读理解练习。这些题目旨在帮助读者活学活用文中的知识点。练习题的答案可以在本页底部找到。
</div>

<!-- #region -->
## For循环
一个“for循环”（for-loop）允许我们迭代某个集合中的项目，并为每一次项目执行同一块代码。比如说，以下的代码会求元组中所有正数的和：
```python
total = 0
for num in (-22.0, 3.5, 8.1, -10, 0.5):
    if num > 0:
        total = total + num
```
“for循环”的语法为：

```
for <var> in <iterable>:
    一区块代码
```

`<var>` 指代任何合法的变量身份（variable-identifier）， `<iterable>` 则是任何**可迭代物**。我们会在下一节中更正式地讨论迭代物；现在你只需要知道任何序列都是可迭代物这一事实就行了。`for` 语句必须由一个冒号结尾，且for循环的主体必须由[空格限定](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Introduction.html#Python-Uses-Whitespace-to-Delimit-Scope)。

for循环的行为如下：

- 试图获取可迭代物的下一个成员。
- 如果可迭代物是空的，退出for循环且不运行其主体代码。
- 如果可迭代物提供了一个成员，为 `<var>` 赋值为该成员（如果 `<var>` 在之前并没有被定义，它将会在这里被定义）。
- 执行循环的主体代码。
- 返回第一步。

让我们来看一看一个例子吧：
```python
# 演示基本的for循环
total = 0
for item in [1, 3, 5]:
    total = total + item

print(total)  # `total` 的值为 1 + 3 + 5 = 9
# `item` 在这里依然被定义，且其值为5
```

此代码会执行以下的步骤：

1. 定义变量 `total`，并为其赋值 `0`
2. 迭代此列表，得出值 `1`，定义变量 `item` 并为其赋值 `1`
3. 为 `total` 赋值 `0 + 1`
4. 迭代此列表，得出值 `3` 并为 `item` 赋值该数字
5. 为 `total` 赋值 `1 + 3`
6. 迭代此列表，得出值 `5` 并为 `item` 赋值该数字
7. 为 `total` 赋值 `4 + 5`
8. 迭代此列表。因为现在已经在列表结尾了，列表将提供一个 `StopIteration` 信号，因此for循环会退出。
9. 打印 `total` 的值（9）

#### 可能的错误点
注意变量 `item` 在for循环退出之后依然存在。它会引用for循环迭代的最后一个值（在这个例子中 `item` 的值为5）。就算如此，*你不应该编写在for循环外面使用迭代变量的代码*。如果你循环一个*空*的可迭代物，迭代变量不会被定义：

```python
for x in []:         # 可迭代物是空的——迭代变量 `x` 将不会被定义
    print("Hello?")  # 此代码不会被执行
print(x)             # 程序报错，因为 `x` 没有被定义
```

因为我们在试图迭代一个空列表，`StopIteration` 会被立刻返回——甚至在变量 `x` 被定义之前。因此for循环中包含的代码从来没有被执行，之后的 `print(x)` 语句也会导致 `NameError`，因为 `x` 没有被定义！
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：简单for循环**

使用一个for循环和一个if语句，打印字符串 `"abcdefghij"` 中的每一个元音（vowel，也就是aeiou这几个字母）。

</div>

<!-- #region -->
## While循环
一个“while循环”（while-loop）允许你重复一区块代码，知道某个条件不再满足：

```
while <condition>:
    代码块
```

`<condition>` 是一个会返回 `True` 或 `False` 的表达式或任何可以输入进 `bool` 的对象。while循环的“主体”则是在while循环语句下方缩进的代码。

while循环的行为如下：

- 执行 `bool(<condition>)` 并在返回 `True` 时执行下方缩进的代码区块。不然，“退出”while循环并跳过缩进的代码。
- 如果缩进的代码被执行了，回到第一步。

让我们来看一看一个例子吧：
```python
# 演示基本的while循环
total = 0
while total < 2:
    total += 1  # 和 `total = total + 1` 等值

print(total)  # `total` 现在的值为 2
```

此代码会执行以下步骤：

1. 定义变量 `total`，并为其赋值 `0`
2. 运算 `0 < 2`，其返回 `True`：进入循环主体的代码块
3. 执行代码块：为 `total` 赋值 `0 + 1`
4. 运算 `1 < 2`，其返回 `True`：进入循环主体的代码块
5. 执行代码块：为 `total` 赋值 `1 + 1`
6. 运算 `2 < 2`，其返回 `False`：*跳过*循环主体的代码块
7. 打印 `total` 的值（2）

注意，如果我们一开始定义 `total = 3`，那条件表达式 `3 < 2` 会直接返回 `False`，且缩进的代码块将一次都不会执行。

<div class="alert alert-warning">

**警告！** 

条件语句永远为真的while循环是有可能的。这会让你的代码无限运行下去！如果这在Jupyter记事本中发生，打断或重启你的内核（kernel）。
</div>
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：简单while循环**

设一列表 `x`，其成员皆为的非零正数。在列表的结尾附加列表所有值的和。重复进行这一步，直到 `x` 的最后一个成员的值至少是100。使用一个while循环。

如果你定义 `x = [1]`，那在你的while循环结束后 `x` 应该是 `[1, 1, 2, 4, 8, 16, 32, 64, 128]`。

</div>

<!-- #region -->
## 循环中的 `break`，`continue`，和 `else` 句段
你可以在for循环和while循环的主体中使用 `continue` 和 `break` 语句。它们分别提供一些额外的“短路”和提前退出循环的方式。

在循环中遇到 `break` 会导致程序立刻退出循环：

```ipython
# 提前退出循环
>>> for item in [1, 2, 3, 4, 5]:
...     if item == 3:
...         print(item, " ...break!")
...         break
...     print(item, " ...next iteration")
```
```
1  ...next iteration
2  ...next iteration
3  ...break!
```

你可以在任何循环结尾添加一个 `else` 句段。此else语句的主体*只有在循环在自然退出（没有被 `break` 语句提前退出）后才会执行*。

```ipython
# 在循环结尾添加一个else句段
>>> for item in [2, 4, 6]:
...     if item == 3:
...         print(item, " ...break!")
...         break
...     print(item, " ...next iteration")
... else:
...     print("if you are reading this, then the loop completed without a 'break'")
```
```
2  ...next iteration
4  ...next iteration
6  ...next iteration
if you are reading this, then the loop completed without a 'break'
```

在循环中遇到 `continue` 语句会导致程序立刻回到检查条件语句这一步。
```python
# 演示循环中的 `continue` 语句
>>> x = 1
>>> while x < 4:
...     print("x = ", x, ">> enter loop-body <<")
...     if x == 2:
...         print("x = ", x, " continue...back to the top of the loop!")
...         x += 1
...         continue
...     x += 1
...     print("--reached end of loop-body--")
```
```
x =  1 >> enter loop-body <<
--reached end of loop-body--
x =  2 >> enter loop-body <<
x =  2  continue...back to the top of the loop!
x =  3 >> enter loop-body <<
--reached end of loop-body--
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：控制循环中的流**

重复循环一个列表的整数，求其中偶数的和并将此和加到一个总值中。重复此过程，直到总值超过100，或当你已经循环这个列表超过50次时。只有整数大于等于100时打印总值。

</div>


## 官方说明文档链接

- ['for' 语句](https://docs.python.org/3/reference/compound_stmts.html#the-for-statement)
- ['while' 语句](https://docs.python.org/3/reference/compound_stmts.html#the-while-statement)
- ['break'，'continue'，和 'else' 句段](https://docs.python.org/3/tutorial/controlflow.html#break-and-continue-statements-and-else-clauses-on-loops)
- ['pass' 语句](https://docs.python.org/3/tutorial/controlflow.html#pass-statements)


## 阅读理解答案：

<!-- #region -->
**简单for循环：解**
```python
for letter in "abcdefghij":
    if letter in "aeiou":
        print(letter)
```

<!-- #endregion -->

<!-- #region -->
**简单while循环：解**
```python
while x[-1] < 100:
    x.append(sum(x))
```
<!-- #endregion -->

<!-- #region -->
**控制循环中的流：解**

```python
x = [3, 4, 1, 2, 8, 10, -3, 0]
num_loop = 0
total = 0

while total < 100:
    for item in x:
        # 当 `item` 是奇数时立刻返回
        # for循环
        if item % 2 == 1:
            continue
        else:
            total += item
    num_loop += 1
    
    # 只有在循环了50次之后才
    # 从while循环中break
    if 50 < num_loop:
        break
else:
    print(total)
```

<!-- #endregion -->
