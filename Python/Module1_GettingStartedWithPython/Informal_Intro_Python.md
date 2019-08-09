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
   :description: Topic: Informal Introduction to Python, Difficulty: Easy, Category: Tutorial
   :keywords: python, installation, script, introduction, ipython, console, quick introduction
```

# 非正式的Python介绍
在你下载好了Anaconda版本的Python之后，让我们来写一些简单的Python代码吧！我们暂时先不编写完整的Python脚本，而只用一个方便的工具——IPython命令行——来快速地尝试一些简单的代码。Anaconda一并安装了IPython命令行；这将帮助你一步步增加代码片段，而不是要一次性执行整个脚本。

让我们打开IPython命令行。如果你是Mac或者Linux用户，打开你的命令行；如果你是Windows用户，打开 `cmd.exe`。输入 `ipython` 并敲击回车，你应该可以在屏幕上看到接下来的内容：

![IPython console example](attachments/ipython_0.PNG)

当我们输入简短的Python代码片段并输入回车时，CPython直译器会在IPython命令行中瞬间执行你的代码。

让我们熟悉一下Python的数字（number），字符串（string），和列表（list）。在这个过程中，我们会了解Python的标准库的一些模组，如math模组。IPython提供的自动完成和说明文档显示功能会帮助我们探索标准库和其中Python对象（object）的功能。尝试使用Python的字符串和列表会帮助我们对Python的通用界面产生印象，并演示这些基本数据类型的方便特征。

## 玩玩数字
是时候执行一些做简单算术的Python代码了。在IPython命令行中输入 `2 + 3` 并回车，你应该能看到接下来的输入和输出：

```python
2 + 3
```

这个命令行进程是持久性的，也就是说我们定义的变量可以在这个进程之后的代码重新调用。让我们定义变量 `x` 并向其赋值数值整数 `10`（请在命令行中跟随本教程）

```python
x = 10
```

我们可以在这个命令行里查看 `x` 的值。在下一行中输入 `x` 并回车：

```python
x
```

现在，让我们在二次方程 $x^{2} + 2x + 3$ 中调用 `x`，并计算结果。

```python
x**2 + 2*x + 3
```

Python的“标准库”——也就是和Python核心语言一起包装的各种工具和函数——包含了很多常见的数学函数。为了组织和结构，Python将这些数学函数存在一个叫做“math”的模组里。我们必须要在代码里导入（import）这个模组来使用这些函数。

```python
import math
```

在导入完之后，`math` 这个单词在我们代码里将代表math模组。IPython提供了一个查看math模组所有函数的方便方法。在命令行中输入 `math.`（注意在单词后面的点）并敲击 `<TAB>`。你应该看到接下来的这个列表:


![Displaying the contents of the math module](attachments/ipython_math.PNG)


一般来说，敲击 `<TAB>` 会让IPython去尝试自动完成你的代码。这个列表显示了所有可以在 `math.` 之后输入的内容。在“s”打头的函数中，我们可以看到 `sqrt()`。这就是一个求平方根的函数。输入 `math.sq` 然后在敲击 `<TAB>` ，你会看到IPython将自动完成你的代码成 `math.sqrt`。

让我们用这个函数来计算 $\sqrt{100}$：

```python
math.sqrt(100)
```

你可能会好奇为什么计算的结果显示的是 `10.0` 而非 `10`；在模组2中我们会了解这两个输出是两种不一样的数据*类型*。前者被称为浮点数，浮点指其小数点，而后者则是一个整数。函数 `math.sqrt` 的定义使得它永远都会返回浮点数。

当我们想要经常使用math模组里的某个函数时，我们可能会希望不需要每次都要打出math模组的前缀，`math.`。我们可以通过从math模组导入单独的函数来达成这个目的。我们将从math模组导入阶乘（factorial）函数。

```python
from math import factorial
```

我们现在可以在代码里直接调用 `factorial` 函数。5的阶乘是 $5! = 5\times 4\times 3\times 2\times 1 = 120$  

```python
factorial(5)
```

## 试试字符串
在写代码的时候，我们将书面文字理解为一连串的字符，也就是字符串（string）。因为Python提供了很多方便，高效的字符串处理函数，所以它很适合于文本处理工作。

让我们先通过在双引号之间敲击字符来创建第一个字符串吧：

```python
"the cat in the hat"
```

单引号也可以：

```python
'the dog in the sash'
```

译者注：记得切换到英文输入法的引号''和""。中文的‘’和“”是不会被识别的。

如果你是用单引号来创建一个字符串，那么那个字符串就可以包括双引号（反之亦然）：

```python
'He picked up the phone, "Hello? What do you want?" Bob was a rather impolite dude.'
```

当然，我们也有专门的特殊字符来帮助我们改变一个字符串的被打印出时的格式。比如说，如果 `\n` 出现在字符串中，它会被当为一个代表回车的字符。这仅仅会在你使用自带的 `print` 函数(一个告诉计算机去将文本显示在用户电脑屏幕上的函数）来打印这个字符串时才会显示，

让我们创建一个会被 `print` 时会显示为三行文字的字符串。

```python
print("I like to talk to dogs.\nI like to talk to cats.\nWhat's my deal?")
```

当然，字符串的作用远远不及仅仅存储文本！让我们探索一些操作字符串的方法。首先，我们将创建一个字符串并将其存储到一个名为 `sentence` 的变量:

```python
sentence = "Who would have thought that we were robots all along?"
```

让我们通过字符串的"length"（长度）来查看这个字符串有多少字符吧：

```python
len(sentence)
```

我们可以通过切片（slice）这个字符串来查看它的前四个字符，最后六个字符，或者中间的几个字符。

```python
sentence[:4]
```

```python
sentence[-6:]
```

```python
sentence[5:22]
```

我们也可以查看是否我们的这个字符串是否包含其它的某个的字符串。字符串 `"robot"` 是否在 `sentence` 里面呢（is `"robot"` **in** `sentence`）？

```python
"robot" in sentence
```

为了快速查看所有字符串可以调用的自带函数，我们可以再次使用IPython的自动完成功能。输入 `sentence.`（不要漏掉最后的点）并敲击 `<TAB>`。我们会看到一列表的函数：


![Built-in functions for a string](attachments/ipython_string.PNG)


让我们用名为count的函数来数一下 `sentence` 中有多少小写的w。

```python
sentence.count('w')
```

让我们看看replace函数是做什么用的。IPython提供一个很方便的看看函数说明文档的功能，只需在函数名后输入两个问号。例：

![Looking up documentation for a function](attachments/ipython_doc1.PNG)

利用我们刚得到的对字符串replace函数的理解，让我们用“computer”来代替“robot”:

```python
sentence.replace("robot", "computer")
```

## 玩玩列表
列表（list）是所有Python标准库的容器中的一种。它可以存储一序列的Python对象（object）。我们可以创建一列表的数字：

```python
[-1, 1/3, 10*2, 7-1]
```

列表可以存储任何类型的Python对象；它可以存储或混合数字，字符串，其它列表，和任何其他类型的对象。

```python
[1, 2, "a", 0.5, "apple and orange"]
```

你可以将两个列表粘连（concatenate）到一起。

```python
[1, 2, 3] + ["a", "b", "c"]
```

像字符串一样，列表本质上也是一个序列（sequence），所以我们可以通过项目在列表中的位置来访问它。这个位置叫做项目的索引（index）；一个序列首项的索引永远都是0.

```python
my_list = [10, 20, 30, 40, 50, 60]
```

```python
my_list[0]
```

```python
my_list[1]
```

你可以使用负整数来相对于列表结尾（右边）来索引。

```python
my_list[-1]
```

你也可以通过对列表中的项目赋值来修改列表的内容。

```python
-5 in my_list
```

```python
my_list[1] = -5
```

```python
my_list
```

```python
-5 in my_list
```

我们也同样可以通过切片（slice）这个列表来存取其中的多个项目，就像之前的字符串一样。

```python
my_list[:3]
```

这个切片出的结果也可以被用来更新列表的前三项内容。

```python
my_list[:3] = "abc"
```

```python
my_list
```

作为这个展示的结尾，让我们在这个列表的结尾附加（append）一个新项目。

```python
my_list.append("moo")
```

```python
my_list
```
