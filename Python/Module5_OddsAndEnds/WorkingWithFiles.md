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
   :description: Topic: Working with aths and files, Difficulty: Medium, Category: Section
   :keywords: open file, read file, pathlib, join directory, context manager, close file, rb, binary file, utf-8, encoding, pickle, numpy, load, archive, npy, npz, pkl, glob, read lines, write, save
<!-- #endraw -->

# 操作文件
本节将会讨论编写读写文件的最佳做法。我们将学习内置的 `pathlib.Path` 对象，其将帮助我们保证我们编写的代码在不同的操作系统（OS）（如Windows，MacOS，Linux）都可以直接使用。我们也将介绍*环境管理器*（context manager）`open`。这将允许我们安全地读写文件；在这里，“安全”的意思是我们可以确信每个我们打开的文档都会早晚被正确地关闭，这样就算我们的代码出错文件也不会被损害。接下来，我们将学习如何“glob”文件，也就是说我们会学习如何搜索并列出名字符合具体规则的文件。最后，我们将快速地过学习允许我们向和从电脑文件系统中分别储存（或“pickle”，“腌制”）和读取Python对象的 `pickle` 模组。

<!-- #region -->
## 操作路径
假设你在编辑一个分析储存在你机器上的数据的Juyter记事本。你自然会需要提供数据在你的机器文件系统中的具体位置来读取该数据。让我们假设这个记事本存在于文件夹 `my_folder` 中，且路径中有一个存储了几个数据文本文件的文件夹 `data`。如此，你的路径架构如下：

```
my_folder/
  |-notebook.ipynb
  |-data/
     |-data1.txt
     |-data2.txt
```
如果你的机器运行Linux或MacOS，那么 `data1.txt` 相对于记事本的路径为：`./data/data1.txt`。请注意，符号 `/` 在这里作为分隔符来区分路径中的文件夹父子关系。在一个Windows机器上，分隔符是 `\`，所以到你数据的路径会写为 `.\data\data1.txt`。我们想要我们代码在不同的操作系统上可以不做修改就直接使用。Python棒棒的 `pathlib` 模组就可以在这里帮上你的忙了。

### pathlib.Path

标准库的[pathlib模组](https://docs.python.org/3/library/pathlib.html)提供了一系列帮助你在不同操作系统中处理文件路径的类。我们只将讨论 `pathlib.Path` 类，因为它会处理我们所有做紧要的需求。这个类允许我们用单一的方式编写我们所有的路径相关的代码，且它会在幕后将我们的路径转化成符合当前操作系统的格式。

让我们首先创建一个指向包含当前记事本的文件夹的 `Path`：

```python
# 创建一个指向当前路径的Path对象
>>> from pathlib import Path
>>> root = Path(".") # “.”意味着：当前代码存在于的文件夹
```

因为我在一台Windows机器中运行此代码，所以程序自动创建了一个 `WindowsPath` 对象：

```python
>>> root
WindowsPath('.')
```

如果我在一台Linux或MacOS机器上运行此代码，它则会创建一个 `PosixPath` 对象。幸运得是，这些类会自动处理这些细节，所以我们不需要担心它们！`Path` 类有着很多我们可以使用的有用方法。首先，请注意，它自动重载了 `/` 操作符（通过实现[特殊方法](http://www.pythonlikeyoumeanit.com/Module4_OOP/Special_Methods.html)）来允许我们创建到子文件夹的路径。让我们看看这是怎么用的：

```python
# 创建指向在子文件夹 'data' 中的 'data1.txt' 文件的路径
>>> path_to_data1 = root / "data" / "data1.txt"
>>> path_to_data1
WindowsPath('data/data1.txt')
```
请注意，`/` 操作符在和一个 `Path` 实例一起使用时会创建含有适合当前OS的路径分隔符的路径。这很方便！

让我们继续探索 `Path` 提供的其它一些方法。这些方法允许我们检查路径和文件，创建新路径，列出路径中所有的文件，读写文件，以及其它。你可以在[这里](https://docs.python.org/3/library/pathlib.html#methods-and-properties)和[这里](https://docs.python.org/3/library/pathlib.html#methods)找到这些方法的完整列表；在这里我强烈建议你花些时间读它们。


```python
>>> root = Path(".")
>>> path_to_data1 = root / "data" / "data1.txt"

# 检查文件或文件夹是否存在
>>> path_to_data1.exists()
True

>>> (root / "bogus_path").exists()
False

# 获取文件或文件夹的“绝对”路径
>>> path_to_data1.absolute()
WindowsPath('C:/Users/TerranceWasabi/Desktop/PLYMI/Module5_OddsAndEnds/data/data1.txt')

# 获取路径指向的文件的名字
>>> path_to_data1.name
'data1.txt'

# 在当前文件夹中创建一个新文件夹
>>> root.mkdir("new_folder")

# 使用“glob”来返回所有符合某规律的文件的生成器
# 比如说，某文件夹中所有的.txt文件的路径
>>> list((root / "data").glob("*.txt"))
[WindowsPath('data/data1.txt'), WindowsPath('data/data2.txt')]

# 将一个路径对象转化为符合当前OS风格的字符串
>>> str(path_to_data1)
'data\\data1.txt'
```

<!-- #endregion -->

<div class="alert alert-info">

**经验**：

每当你在代码中处理文件系统路径时，你永远都应该使用 `pathlib.Path`。重申一边——这将保证你的代码在不同操作系统都可以直接使用，使得你的路径处理易读，并为你提供大量方便的功能。

</div>


<div class="alert alert-warning">

**注意**：

`pathlib` 在Python 3.4中加入。虽然很多第三方模组已经更新了它们的文件读写功能来接受字符串和 `pathlib.Path` 对象（如，`numpy.save` 可以接受一个 `Path` 对象来描述在哪里储存一个NumPy数组），但是某些模组依然仅仅接受字符串为路径。在这种情况下，你可以直接通过对其调用 `str` 来将你的 `Path` 实例转化为一个字符串，并将得到的字符串路径输入到文件读写函数中。在此我也友善地建议你在编写文件读写函数时接受 `pathlib.Path` 对象！

</div>

<!-- #region -->
## 打开文件
我在此建议你查看[官方Python教程](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)来阅读简单的文件读写教程。

每当你指示你的代码打开文件来进行读写时，你必须要保证该文件在代码结束前会被关闭。这将使得它的数据不被意外地损坏或修改。Python提供了 `open` 环境管理器（context manager），其旨在保证就算代码报错时打开的文件都会被关闭。

以下代码打开文件“file1.txt”来对其写数据：

```python
# 演示 `open` 环境管理器的使用方法

# 我们将向在当前文件夹中的名为“file1.txt”的文件写入数据
path_to_file = Path("file1.txt")
with open(path_to_file, mode="w") as f:
    # 这里的缩进表示我们进入了打开文件的“环境”（context）中。
    # 离开这个缩进的空间将退出打开文件的环境，迫使该文件被关闭。
    # 就算我们在缩进中的代码导致错误，文件也将被关闭
    f.write('this is a line.\nThis is a second line.\nThis is the third line.')

# 文件再次被关闭。
```

语法 `with <context_manager>() as <context_variable>:` 示意使用对象 `<context_variable>` 来创建一个环境。在这个例子中，`open` 是环境管理器，且我们命名为 `f` 的变量是在这个由缩进空格限定的环境中打开的文件对象。你也可以直接从 `Path` 实例调用 `open`：

```python
with path_to_file.open(mode="w") as f:
    f.write('this is a line.\nThis is a second line.\nThis is the third line.')
```
<!-- #endregion -->

<!-- #region -->
`open` 的完整说明文档可以在[这里](https://docs.python.org/3/library/functions.html#open)找到。

### 提供打开模式
输入 `mode='w'` 表示我们将会从头开始向文件写数据——如果该文件已经有内容了，那么该内容会在写入之前被删除。以下为可用的打开文件的“模式”：

|模式| 解释 |
|---|---|
|`r`| 打开文件以读取其文本 |
|`w`| 打开文件，**删除其内容**，并重新写入文本 |
|`a`| 打开文件以在其现有内容结尾写入文本，所以它在对文件“附加”（append）|
|`x`| 打开文件以写入文本，但如果文件已经存在该操作会失败 |
|`+`| 打开文件以读取和写入其文本 |

这些模式会默认使用unicode（utf-8）解码/编码方法。也就是说，当你从你的文件系统用 `mode='r'` 读取数据时，Python将会自动根据utf-8*解码*在你机器上的二进制数据，而utf-8会将二进制数据转化为用字符串表达的文本。相似的，在“w”，“a”，“x”，或“+”模式向文件写入字符串时，Python会认为该字符串应根据utf-8编码规则被编码为二进制（当然，储存为文件的数据必须编码为二进制）。

你可以通过向这些模式添加一个 `'b'` 来逼迫Python直接读写二进制数据而不进行编码：`'rb'`，`'wb'`，`'ab'`，`'xb'`，`'+b'`。理解这个二进制模式极其重要。比如说，如果你在储存一个NumPy数组，你应该使用“wb”或“xb”模式来打开文件来使其期待二进制数据；显然，在储存一个NumPy数字数组时我们并不是在储存文本。

```python
# 将NumPy数组储存到文件“array.npy”中
>>> import numpy as np
>>> x = np.array([1, 2, 3])

# 我们必须用二进制写入模式打开该文件，因为
# 我们不在储存文本
>>> with open("array.npy", mode="wb") as f:
...     np.save(f, x)

```

### 操作文件对象
当我们调用 `open` 来打开一个文件时，环境管理器将返回一个打开的文件（file）对象。该文件的方法为我们提供读写被打开的文件的能力（假设我们使用了合适的模式来打开它）。

```python
# 演示文件对象的 `read` 方法
>>> with open(path_to_file, mode="r") as var:
...     # 将文件的全部内容读取为字符串
...     content = var.read() 

>>> content
'this is a line.\nThis is a second line.\nThis is the third line.'
>>> print(content)
this is a line.
This is a second line.
This is the third line.
```

以下总结了文件对象可用的一些方法：

- `read()`：将文件的全部内容读取为字符串或比特（具体由打开的模式决定）
- `readline()`：读取文件的下一行文本，包含结尾的 `'\n'` 字符
- `readlines()`：读取文件的所有行的文本，并将每一行存为列表中的一个字符串。
- `write(x)`：将 `x`（一个字符串）写入文件。
- `writelines(x)`：接受成员为字符串的可迭代物，将每个字符串当作一行文本写入到文件中（`readlines` 的逆向操作）。

同时，请注意，你可以*迭代*文件对象，且每一轮迭代会返回文件文本中的一行。这是一行一行读取整个文件的最佳做法。
<!-- #endregion -->

<!-- #region -->
## 范例：读写文本文件

设以下字符串：

```python
# 请回忆：三引号可以用来编写多行长的字符串
>>> some_text = """A bagel rolled down the hill.
I mean *all* the way down the hill.
A lady watched it roll.
Way to help me out."""

>>> some_text
'A bagel rolled down the hill.\nI mean *all* the way down the hill.\nA lady watched it roll.\nWay to help me out.'
```

在当前路径瞎，将该字符串写入名为“a_poem.txt“的文件：

```python
# 使用模式x来保证如果文件已经存在，我们不会覆盖它
with open("a_poem.txt", mode="x") as my_open_file:
    my_open_file.write(some_text)
```

现在，让我们读取文件的每一行并（为了让这个范例更加复杂一点）*在该行首字母为“A”*时将它附加到列表 `out` 的结尾：

```python
with open("a_poem.txt", mode="r") as my_open_file:
    # 请回忆：迭代文件对象将一次返回文件的一行内容
    out = [line for line in my_open_file if line.startswith("A")]
```

```python
# 确认输出和我们期待的一样
>>> out
['A bagel rolled down the hill.\n', 'A lady watched it roll.\n']
```
<!-- #endregion -->

## 使用glob寻找文件

很多情况下，我们会想要创建用来迭代的文件列表。比如说，如果我们有多个数据文件，那么创建一个可以迭代并顺序处理的文件列表会很有用。达成这个目的的一个方法为手动创建这个文件列表：

``` python
my_files = ['data/file1.txt', 'data/file2.txt', 'data/file3.txt', 'data/file4.txt']
```

但是，这么做非常麻烦且易错，因为你可能会拼写错误或漏掉文件。另外一个与此相比强大很多的创建文件列表的方法叫做globbing。`glob` 是符合某规律的文件名字集。在glob文件的过程中，我们使用特殊的通配符（wildcard）来配对所有包含某一文件名部分的文件。在我们的情况下，`*` 会是我们最常用的通配符——它对应任何字符。提供一个例子会让以上内容好理解很多。在下面我们将见到一些glob和它们配对的规律：

```
# 配对任何开头为 `file` 且结尾为 `.txt` 的文件名，如
# file1.txt，filefilefile.txt，file.txt，file12345.txt，...
file*.txt

# 配对所有在“data”路径下的.txt文件
data/*.txt

# 配对任何文件名
*

# 配对所有png图片文件
*.png

# 配对任何文件名包含“test”的文件
*test*

# 配对所有包含“number”的.py文件
*number*.py
```

`pathlib` 模组提供了glob文件的方便功能。当我们创建了一个 `Path` 后，我们可以直接对其调用 `glob()` 并提供一个glob字符串。这将返回一个生成所有符合规律的文件的[生成器](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html#Introducing-Generators)。

``` python
# glob当前路径下所有开头为“test”和结尾为“.txt“的文本文件
>>> root_dir = Path('.')
>>> files = root_dir.glob('test*.txt')  # 这返回一个生成器
<generator object Path.glob at 0x00000146CE118620>

# 获取这些路径的排序列表
>>> sorted(files)
[PosixPath('test_0.txt'),
 PosixPath('test_1.txt'),
 PosixPath('test_apple.txt')]

# 直接迭代这个生成器
>>> for file in root_dir.glob('test*.txt'):
>>>     with open(file, 'r') as f:
...         # 进行一些操作
...         pass
```

请查询[官方说明文档](https://docs.python.org/3/library/pathlib.html#pathlib.Path.glob)来阅读更多关于glob的细节。

<div class="alert alert-info">

**阅读理解：基础glob规律**

为以下每个要求编写一个glob规律：

- glob所有在 `./files` 路径中的.txt文件
- glob所有文件名中包含“quirk”的文件
- glob所有文件名开头为“data”的文件
- glob所有文件名首字母为“q”，包含一个“w”，且结尾为“.npy“后缀的文件

</div>


`*` 通配符并不是唯一可用的规律。有时我们会想要配对一组字符。比如说，我们可能仅仅想要配对开头为数字的文件。如果我们仅仅只能使用 `*` 通配符，那这是不可能的。幸运的是，这些常见的使用情况都被支持。

我们可以使用方括号来配对一组字符：`[abc]*` 会配对任何开头为“a”，“b”，或“c“的文件名，但它不会配对任何其它名字。我们也可以在方括号中使用一个”-“来glob字符组。比如说：

```
# 配对任何开头为数字的文件
[0-9]*.txt

# 配对任何名字中一个元音的文件
*[aeiou]*

# 配对任何开头为小写字母的文件
[a-z]*
```

<div class="alert alert-info">

**阅读理解：更多glob规律**

为以下每个要求编写一个glob规律：

- 任何名字有奇数的文件
- 所有名字中有”q“或”z“的txt文件

</div>

<!-- #region -->
## 储存并读取Python对象：pickle
假设你创建了一个作为你在教的课的成绩簿的词典：
```python
>>> grades = {"Albert": 92, "David": 85, "Emmy": 98, "Marie": 79}  
```
你应该如何储存这个词典来允许你在未来重新使用它呢？Python的标准库包含了[pickle](https://docs.python.org/3/library/pickle.html)（腌制）模组。它提供了在硬盘上存储和读取Python对象的函数。让我们“腌制”这个词典并将其存到当前路径下的”grades.pkl“文件中：

```python
import pickle

# 腌制词典
with open("grades.pkl", mode="wb") as opened_file:
    pickle.dump(grades, opened_file)
```
`pickle.dump` 创建了我们词典的序列化表达方法并通过我们提供的文件对象将其写入了打开的文件中。请注意，我们使用了二进制写的打开方式，因为我们在写入二进制数据，而不是需要被编码为二进制数据的文本数据。请同时注意，我们使用了”.pkl“后缀来表示该文件为被Python的pickle协议编写的二进制数据。你不需要使用这个后缀，但是这么做是个好习惯。

`pickle.load` 将会从硬盘反腌制（unpickle）我们的Python对象以允许我们继续操作我们的成绩簿。

```python
# 反腌制词典
with open("grades.pkl", mode="rb") as opened_file:
    my_loaded_grades = pickle.load(opened_file)
```

```python
>>> my_loaded_grades
{'Albert': 92, 'David': 85, 'Emmy': 98, 'Marie': 79}
```

`pickle.dump` 和 `pickle.load` 能够解决我们绝大部分的对象腌制需求。你可以如此储存非常多的Python对象，包括我们定义的函数和自定义类的实例。请查看[官方说明文档](https://docs.python.org/3/library/pickle.html#what-can-be-pickled-and-unpickled)来阅读哪些Python对象可以被腌制和哪些Python对象不可以。
<!-- #endregion -->

<!-- #region -->
## 储存和读取NumPy数组
NumPy提供了它自己的储存和读取数组的函数。虽然这些数组可以被腌制，但你应该使用NumPy的文件读取函数。NumPy用于储存数组数据的标准二进制文件类型叫做“.npy”文件。NumPy用以在单个文件中储存多个数组的二进制档案格式叫做“.npz”格式。

让我们将数组 `x = np.array([1, 2, 3])` 储存到二进制文件（不是文本文件）“my_array.npz”。`numpy.save` 和 `numpy.load` 会储存和读取数组并为你处理打开和关闭文件。因此，在使用这些函数时没有必要使用一个环境管理器。

```python
>>> import numpy as np
>>> x = np.array([1, 2, 3])

# 将NumPy数组存入硬盘
>>> np.save("my_array.npy", x)

# 从硬盘读取NumPy数组
>>> y = np.load("my_array.npy")

>>> y
array([1, 2, 3])
```

我们可以使用 `numpy.savez` 来将多个数组储存到一个档案文件“my_archive.npy”中。在这里，我们将向这个档案储存三个数组。我们可以关键词参数提供这些数组的名字来允许我们在读取档案文件时区分它们。

```python
# 将三个数组储存到NumPy档案文件中
a0 = np.array([1, 2, 3])
a1 = np.array([4, 5, 6])
a2 = np.array([7, 8, 9])

# 我们使用关键词参数 `soil`，`crust`，和 `bedrock` 来
# 作为档案中对应数组的名字。
np.savez("my_archive.npz", soil=a0, crust=a1, bedrock=a2)
```

从档案中读取数组比读取单个数组更加麻烦一些；我们将使用一个环境管理器来打开档案文件并读取我们想要的数组。你可以使用 `np.load` 来代替 `open` 作为一个环境管理器。它创建的文件对象就是我们的NumPy数组档案，且其提供一个像词典的界面来让我们访问其中的数组：

```python
# 打开档案并通过名字访问每个数组
with np.load("my_archive.npz") as my_archive_file:
    out0 = my_archive_file["soil"]
    out1 = my_archive_file["crust"]
    out2 = my_archive_file["bedrock"]
```
```python
>>> out0
array([1, 2, 3])
>>> out1
array([4, 5, 6])
>>> out2
array([7, 8, 9])
```
<!-- #endregion -->

## 官方说明文档链接

- [“pathlib”模组](https://docs.python.org/3/library/pathlib.html)
- [“open”函数](https://docs.python.org/3/library/functions.html#open)
- [官方教程：读写文件](https://docs.python.org/3/tutorial/inputoutput.html#reading-and-writing-files)
- [glob文件](https://docs.python.org/3/library/pathlib.html#pathlib.Path.glob)
- [“pickle”模组](https://docs.python.org/3/library/pickle.html)
  - [哪些文件（不）可以被腌制？](https://docs.python.org/3/library/pickle.html#what-can-be-pickled-and-unpickled)


## 阅读理解答案

**基础glob规律：解**

为以下每个要求编写一个glob规律：

- glob所有在 `./files` 路径中的.txt文件（解：`./files/*.txt`）
- glob所有文件名中包含“quirk”的文件（解：`*quirk*`）
- glob所有文件名开头为“data”的文件（解：`data*`）
- glob所有文件名首字母为“q”，包含一个“w”，且结尾为“.npy“后缀的文件（解：`q*w*.npy`）

**更多glob规律：解**

为以下每个要求编写一个glob规律：

- 任何名字有奇数的文件（解：`*[13579]*`）
- 所有名字中有”q“或”z“的txt文件（解：`*[qz]*.txt`）
