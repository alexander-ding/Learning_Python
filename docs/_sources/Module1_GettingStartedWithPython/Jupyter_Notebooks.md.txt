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
   :description: Topic: Jupyter notebooks, Difficulty: Easy, Category: Tutorial
   :keywords: jupyter, jupyter lab, notebook, kernel, basics, server, console, plot, beginner, data science
<!-- #endraw -->

# Jupyter记事本
近年来，Jupyter Notebook（记事本）成为了一个极其流行的帮助用户使用Python和其它语言做实验的工具。它的流行代表着数据科学实验形式的转变。

Jupyter记事本类似于IPython命令行，但，相比起IPython每次编写运行一行代码的形式，你可以轻松地编辑和重新执行*任何*在记事本中的代码。同时，你可以保存一个记事本，然后在未来去运行和修改它。Jupyter记事本提供很多特别实用的功能。比如说，你可以在记事本中嵌入可视化的多媒体（如表格和图画）并利用[Markdown语法](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)编写方便美观的文本来演示和解释记事本的内容。

通过以上这些方式，Jupyter记事本成为了一个出众的实际应用工具。比如说，你可以在一个记事本上跟随本文的流程，边读边运行本文的代码片段和回答阅读理解问题，并同时使用Markdown标题来视觉上区分记事本的不同区块。当我做研究时，我经常创建Jupyter记事本以编写代码来分析数据，绘制各种实验结果的图表，并撰写详细的Markdown文本来记录我的工作。最后产生的成果是一个我可以分享给我同事的记事本。我可以在几个月后重新打开这个记事本而不会不记得我之前都做了些什么。

## JupyterLab

[JupyterLab](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)是由Project Jupyter团队开发的一个全新的网页端交互界面。通过它，你可以在你的浏览器中编辑并运行Jupyter记事本，命令行，和文本编辑器。除了各种功能和相比Jupyter记事本更加成熟的用户界面之外，JupyterLab还提供了可以用来显示数据，图片，和代码输出的在Jupyter记事本之外的可移动窗口。这将允许你高效地进行数据科学工作。

本文在这里推荐你阅读[JupyterLab说明文档](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)来了解它提供的额外功能。

下文将针对Jupyter记事本来进行讨论，但是运行JupyterLab服务器和操作其中的记事本的方法和Jupyter记事本基本完全一样。Jupyter记事本和JupyterLab都可以[用Anaconda安装](https://cn.pythonlikeyoumeanit.com/Module1_GettingStartedWithPython/Installing_Python.html)。

## 运行记事本服务器并创建记事本
废话不多说，让我们来使用Jupyter记事本吧！

在你的命令行中，导航到随便一个你愿意创建文件的文件夹。如果你不知道怎么做，上网搜一下！

当你的命令行在你想要的文件夹内后，运行（键入以下内容然后回车）：`jupyter notebook`

你应该会看到以下的内容出现在你的命令行中：

![Starting a jupyter notebook server on your machine](attachments/jupyter_login.PNG)

这是一个在你机器上运行的“记事本服务器”——它将会处理所有在你浏览器和机器之间的交互。一个看起来像文件夹浏览器的窗口应该会在你的浏览器中打开。

![File explorer that opens in your browser](attachments/jp_files.PNG)


你可以利用这个界面进入子文件夹并打开任何已经存储的Jupyter记事本。*不管是新是旧，每次你想要打开一个Jupyter记事本时你都需要使用这个文件夹浏览窗口。*在窗口右上角，你可以点击展开菜单“New”并选择选项 `Python 3`。

![](attachments/jp_dropdown.PNG)

你的浏览器会打开一个新的窗口并显示一个名为 `Untitled.ipynb` 的运行Python 3壳的Jupyter文件夹。点击 `File > Rename` （文件 > 重命名）会允许你修改记事本的名字。`.ipynb` 则是Jupyter记事本的文件类型后缀（`ipynb` 代表着 "IPython-notebook"，也就是这些Jupyter记事本在2014年前的名字）。在这个记事本中运行的命令会被Python像IPython命令输入行一样直译并执行。

<div class="alert alert-warning">

**Jupyter记事本并不使用互联网**：

虽然Jupyter记事本在你的浏览器中打开，*所有的东西都是在本地机器上发生的*。你不需要连接互联网来使用Jupyter记事本！在命令行中运行的记事本服务器仅仅管理浏览器和本地机器的交互。比如说，它会将代码发送到你的CPython直译器去运行。
</div>




<!-- #region -->
## 记事本小节
在Python命令行中，你每次只能编写一行代码；但一个记事本让你可以以小节（cell）为单位来编写和运行代码。在你的记事本的第一小节中，输入以下代码：

```python
x = 3
y = 4
```
然后敲击 `<SHIFT>+<ENTER>`。这会运行这个小节里的所有代码（在这个例子中，这会分别给 `x` 和 `y` 赋值 3 和 4）并在下面创建一个新的小姐。在新的小节中输入以下代码：

```python
x + y
```
敲击 `<SHIFT>+<ENTER>` 以运行此代码。小节下面会显示数字 7——也就是运行 `3 + 4` 的结果：

![jupyter notebook example](attachments/jupyter_early.png)

请注意，记事本“知道”在不同小节中的变量。这不仅仅在从上到下时适用——你可以在第三小节定义 `z = 2`，然后在第一小节运行调用 `z` 的代码。唯一需要注意的是运行这些小节的*顺序*。注意 `In[1]` 代表这个小节是第一个在这个记事本中被执行过的小节，而 `In[2]` 代表着第二个被执行过的小节。

正式来讲，在一个记事本中的所有小节共享同一个“命名空间”（namespace）：在任何一个小节中定义的变量可以在任何其它相同记事本的小节中调用或重新定义，但但不同的记事本的命名空间互相完全独立。也就是说你可以同时编写多个记事本，而它们永远也不会“知道”互相的存在。

使用记事本的一大好处是你可以快速地修改这些小节（比如将 `x = 3` 改成  `x = 10`）并重新执行它们来尝试你正在编写的新代码。这是一个简单却极其强大的编写新代码的环境。
<!-- #endregion -->

## 样例记事本
为了演示一个更加有趣的记事本使用情景，让我们创建记事本来绘制一些数据图。我们将使用matplotlib，一个用于绘制数据的Python模组，和NumPy，旗舰的数字工作Python模组。我们将导入这些模组。然后，我们将定义一些数学函数。最后，我们将绘制这些函数在大量数值上的解。

```python
import numpy as np
import matplotlib.pyplot as plt

# 这会告诉Jupyter将matplotlib的图表嵌入在记事本中
%matplotlib notebook 
```

```python
def sinc(x):
    return np.sin(x) / x

def d_sinc(x):
    "sinc函数的导数"
    return np.cos(x)/x - np.sin(x)/x**2
```

```python
# 用1000个平均分布在[-15, 15]之间的点求值
x = np.linspace(-15, 15, 1000)
f = sinc(x)
df = d_sinc(x)
```

```python
# 绘制sinc函数和它的导数
fig, ax = plt.subplots()
ax.plot(x, f, color="red", label=r"$sinc(x)$")
ax.plot(x, df, color="blue", ls="--", label=r"$\frac{d(sinc(x))}{dx}$")


ax.set_title("Example Notebook Plot")
ax.set_xlabel(r"$x$ [radians]")

ax.grid(True)
ax.legend();
```

注意这个记事本界面很方便对图表做修改。你可以轻松地修改图表的颜色或线条风格并重新绘制，而不需要重新计算这些函数的解。你可以直接重新执行含有绘图代码的小节。这在计算要绘图的数据很昂贵时极其有用。


## 熟悉Jupyter记事本
在这一段，我们会列出常用的键盘快捷键和记事本功能来帮助你有效地使用记事本。一个记事本有两种不同的模式：

**命令模式 (敲击** `<ESC>` **来激活)** 

在命令模式中，你可以使用键盘快捷键来创建/删除/剪接/粘贴记事本小节或更改小节的类型（如代码或Markdown）。当你在命令模式中，你选择的小节会有一个*蓝色*的边框。你可以点击记事本上方的 `Help > Keyboard Shortcuts` （帮助 > 键盘快捷键）来查看所有的快捷键。最重要的快捷键是：

- 在当前小节上方创建一个新的小节：`a`
- 在当前小节下方创建一个新的小节：`b`
- 删除当前小节：`dd`
- 重启记事本壳（杀死所有进程并删除所有定义的变量）：`00`
- 将当前小节的类型改为“代码”：`y`
- 将当前小节的类型改为“Markdown”：`m`

**编辑模式（敲击** `<Enter>` **来激活）** 

编辑模式允许你去在选择的小节中输入文本。在编辑模式中，当前选择的小节会有一个*绿色*边框。记事本提供两个执行小节的命令：

- 执行当前小节：`<CTRL>+<ENTER>`
- 执行当前小节并在下方创建新小节：`<SHIFT>+<ENTER>`

小节在创建时默认为代码小节，意味着它的内容会被当作Python代码格式化和执行。就如IPython命令台一样，`<TAB>` 可以用来自动完成代码。同时，当你的光标在代码中某个Python函数名字上方时，`<SHIFT>+<TAB>` 将打开一个包含该函数的说明字符串的小窗口。这将*很*有用。

![Looking up code documentation in a notebook](attachments/jp_docstring.PNG)

### Markdown小节
当一个小节被设置为Markdown类型时，它的内容不会被当成Python代码，反而会被渲染成风格化的文本。参考[此页](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet)来学习如何使用不同的Markdown语法。使用Markdown小节会允许你在记事本中创建章节和字章节标题。你可以[渲染LaTeX数学公式](http://jupyter-notebook.readthedocs.io/en/stable/examples/Notebook/Working%20With%20Markdown%20Cells.html#LaTeX-equations)，在记事本中嵌入照片和视屏，以及更多。事实上，这个网站的每一页都是用Jupyter记事本创建的！Markdown类型的小节对创建高质量，详细记录的Jupyter文件夹极其有用。它们可以完美应对实验汇报类的作业。

<div class="alert alert-warning">

**注意小节类型**:

如果你发现你的Python代码没有被正确地格式化，且在执行小节时根本不运行（你看不到任何输入或者错误），那很有可能你一不小心将小节设置为了Markdown类型！进入命令模式（`<ESC>`）并敲击 `y` 以将小节改回代码类型。

</div>

### 将Jupyter记事本用于其它语言
Jupyter记事本不只支持Python。你可以在Jupyter中为任何编程语言开发一个“壳”（Kernel）。其实很多重要的编程语言都有着自己相对应的壳：

- [R](https://github.com/IRkernel/IRkernel)
- [Julia](https://github.com/JuliaLang/IJulia.jl)
- [Ruby](https://github.com/SciRuby/iruby)
- [C++](https://github.com/QuantStack/xeus-cling)
- [Haskell](https://github.com/gibiansky/IHaskell)
- [Go](https://github.com/gopherdata/gophernotes)

这时刻增长的可用Jupyter壳列表在[这里](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)列出。请注意并不是所有的这些项目都很成熟。比如说，虽然Python和Julia的壳都比较强健，Haskell的壳不能在Windows机器上本地化运行，而C++的壳在编写本文时还在前期开发中。

### Visual Studio Code的Jupyter记事本支持
Visual Studio Code在近期[开始官方支持Jupyter记事本](https://devblogs.microsoft.com/python/announcing-support-for-native-editing-of-jupyter-notebooks-in-vs-code/)。这意味着你可以在[Visual Studio Code IDE](https://cn.pythonlikeyoumeanit.com/Module1_GettingStartedWithPython/Getting_Started_With_IDEs_and_Notebooks.html)中直接编辑Jupyter记事本，并可以享受如代码完成，调试，和查看变量的额外功能。