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
   :description: Topic: Understanding imports modules and packages, Difficulty: Easy, Category: Section
   :keywords: custom package, module, script, import, setup, pip, conda, relative import, absolute import 
<!-- #endraw -->

<!-- #region -->
# 导入：模组和包

在之前学习Python和NumPy基础时，我们需要经常使用 `import` 语句。这允许我们访问标准库和NumPy提供的函数和对象。

```python
# 访问标准库中 `collections` 模组的 `defaultdict`
from collections import defaultdict

# 导入整个numpy包并将给其代名词“np”
import numpy as np
```

尽管我们经常食用 `import` 语句，我们到现在为止都还没有讨论它具体的运作细节。在这里，我们将给予其足够的注意力并讨论Python的导入系统，也就是代码被组织成模组（module）和包（package）的方法。模组是单独的 `.py` 文件，而我们可以从模组中导入函数和对象；包则是这些模组的集合。具体讨论这个包系统不仅仅将帮助我们理解标准库和其它Python代码集的组织方法，这也将允许我们创建我们自己的代码包。

作为本节的总结，我们将演示在系统安装一个Python包的过程；假设你编写了你自己的Python包，安装该包将允许你从系统的任何地方导入它。我们将会简短地讨论两个最常用的向全世界提供Python包储存和下载功能的服务：Python Package Index（PyPI）和 Anaconda.org。

[官方Python教程](https://docs.python.org/3/tutorial/modules.html)提供了很棒的有关本节材料的总结以及在这里没有讨论的细节。
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-warning">

**自动重载**：

当你在Jupyter记事本中跟随本节的操作时，请将以下代码包含在记事本的顶端：

```python
%load_ext autoreload
%autoreload 2
```

执行这些“魔法命令”将会告知你的记事本在任何导入的模组和包被修改后重新导入它们。如果你不执行这些命令，你的记事本将不会“看到”你对已经导入的模组进行的修改，除非你重新启动该记事本的壳。

</div>
<!-- #endregion -->

<!-- #region -->
## 模组

Python“模组”指包含函数定义和变量赋值语句的单个 `.py` 文件。导入一个模组将执行这些命令并通过被导入的模组使得命令产生的对象可以被访问。

让我们创建属于自己的模组并将其导入到一个互动式的Python进程中吧。在你计算机中的某个路径中打开一个Jupyter记事本或IPython命令行。使用[IDE](http://www.pythonlikeyoumeanit.com/Module1_GettingStartedWithPython/Getting_Started_With_IDEs_and_Notebooks.html#Integrated-Development-Environments)或简单的文本编辑器（不要用像Microsoft Word这样的软件！）在进程的路径中创建一个名为 `my_module.py` 的文本文件。`my_module.py` 的内容应为：

```python
"""
我们的第一个Python模组。这个开头的字符串是模组级别的说明文档字符串（docstring）。
它并不是该模组必要的一部分，但它对描述你的模组的目的很有用。
"""

print("I am being executed!")

some_list = ["a", 1, None]

def square(x):
    return x ** 2

def cube(x):
    return x ** 3
```

回到我们的互动式Python进程，我们可以将这个模组导入到该进程中。因为它就在当前的路径中，Python能够直接“找到”这个模组——我们会在之后对此多做讨论。导入 `my_module.py` 将会从上到下执行它所有的代码，并会创建一个名为 `my_module` 的Python对象；这是内置的 `module` 类型的一个实例。请注意，我们在导入语句中没有包含 `.py` 后缀。

```python
# 将 my_module 导入到我们的互动进程中
>>> import my_module
I am being executed!

# 创建了一个模组类的实例对象
>>> my_module
<module 'my_module' from 'usr/my_dir/my_module.py'>

>>> type(my_module)
module
```
<!-- #endregion -->

<!-- #region -->
正如所料，导入我们的模组导致 `print` 语句被执行，因此 `'I am being executed!'` 被打印到了命令行中。然后，对象 `some_list`，`square`，和 `cube` 在剩余的代码被执行时也被定义了。*我们可以通过模组对象来访问它们*。

```python
# 我们可以通过模组对象访问模组中定义的所有变量
>>> my_module.some_list
['a', 1, None]

>>> my_module.square
<function my_module.square(x)>

>>> my_module.square(2)
4

>>> my_module.cube(2)
8
```
<!-- #endregion -->

<!-- #region -->
你一定要理解这就是模组内容能够被导入它的环境访问的方法。知道这点后，了解模组的成员的一个好办法是使用IDE和互动命令行的[自动完成](http://www.pythonlikeyoumeanit.com/Module1_GettingStartedWithPython/Informal_Intro_Python.html#Dabbling-with-Numbers)功能来列出模组对象的所有属性。内置的 `help` 函数可以用来总结模组的内容：

```python
>>> help(my_module)
Help on module my_module:

NAME
    my_module

DESCRIPTION
    我们的第一个Python模组。这个开头的字符串是模组级别的说明文档字符串（docstring）。
    它并不是该模组必要的一部分，但它对描述你的模组的目的很有用。

FUNCTIONS
    cube(x)

    square(x)

DATA
    some_list = ['a', 1, None]

FILE
    c:\users\ryan soklaski\desktop\learning_python\python\module5_oddsandends\my_module.py
```
<!-- #endregion -->

<div class="alert alert-info">

**经验**：

模组仅仅是一个包含.py后缀的文本文件，其内容为Python代码。你可以在互动命令行环境（如Jupyter记事本）或另外一个模组中导入模组。导入模组会执行模组的代码并创建一个模组类型的对象实例。任何在导入时赋值的变量都被绑定到这个模组对象上。

</div>


<div class="alert alert-info">

**阅读理解：创建简单模组**

创建一个名为 `basic_math.py` 的简单数学模组。它应该提供无理数 $\pi$ 和 $e$，以及函数 `deg_to_rad`，其将角度数转化成弧度数。

然后，导入这个模组，计算 $e^{i\pi}$，并将45度转化为对应的弧度。
</div>

<!-- #region -->
## 导入语句

Python提供了导入模组和具体模组成员的灵活框架。我们已经在关于NumPy的讨论中见到了我们可以在导入语句提供一个*代名词*（alias）这一点；这在导入长名字的模组时非常方便：

```python
# NumPy的模组对象由名为 `np` 的变量代表
>>> import numpy as np
>>> np.array([2., 3.])
array([2., 3.])
```

一般而言，我们可以如此为导入的模组设置代名词：`import <module_name> as <alias_name>`。

然后，语法 `from <module_name> import <thing1>, <thing2>, ...` 允许我们从模组导入具体的对象，而不是整个模组。让我们从 `basic_module.py` 导入 `square` 和 `some_list`：

```python
>>> from my_module import square, some_list
I am being executed!

>>> some_list
['a', 1, None]

>>> square(2)
4
```
请注意，模组依然整个被执行，但是与其创建模组实例 `my_module`，这个导入语句仅仅返回了模组中定义的对象中在我们导入语句中提供名字的那些。

最后，你可以提供 `*` 来代表模组的所有属性。

```python
# 导入 `my_module` 的所有成员
>>> from my_module import *
```

你可以在模组中提供一个名为 `__all__` 的列表，其将属性名储存为字符串，来限制 `*` 代表的属性。也就是说，如果我们在 `my_module.py` 中包含了 `__all__ = ["cube", "some_list"]`，那么 `from my_module import *` 将只会导入 `cube` 和 `some_list`，而不会导入 `square`。

最后，我们也可以在这种风格的导入中使用代名词：

```python
>>> from my_module import cube as my_cube
>>> my_cube(2)
8
```
<!-- #endregion -->

<!-- #region -->
## 包

在大项目中工作时，你经常会想要将你的代码组织成多个模组。比如说，假设我们在编写脸部识别的软件。我们可以会需要一个摄像机模组来获取照片，一个脸部识别模组来储存能够识别人脸的类，以及一个数据库模组来储存和更新“见过的”脸。这些模组可以在一个共同的*包*（package）中。

Python包是一个包含名为 `__init__.py` 的文件的路径，以及其它Python模组和子包（也就是说，有着属于自己的 `__init__.py` 文件和有关模组的子路径）。`__init__.py` 文件有着特殊的重要性——它用来标示其所在的路径应作为包处理。作为一个例子，让我们创建一个最基础的包，其文件结构如下：

```
- 你当前的Jupyter记事本进程/命令行进程
- a_dir/
    |--__init__.py
```

请注意，你的互动Python进程（如Jupyter记事本）应该在和 `a_dir/` 相同的路径下活跃。包含 `__init__.py` 文件的路径名字就是包的名字，因此这个包的名字为 `a_dir`。假设 `__init__.py` 的内容如下：

```python
def sum_func(x, y):
    return x + y

def divide_func(x, y):
    return x / y
```
<!-- #endregion -->

<!-- #region -->
和模组一样，导入这个包会执行 `__init__.py` 的内容并将 `sum_func` 和 `divide_func` 提供为创建的模组对象的属性：

```python
# 导入Python包
>>> import a_dir
>>> a_dir.divide_func(1, 2)
0.5
```

让我们讨论一个更加成熟的包，其包含模组和子包。下面的包，`face_detection`，包含了模组 `utils`，`database`，和 `model`。它也包含了子包 `camera`，其包含了 `config` 模组和 `calibration` 模组。

```
- face_detection/
    |-- __init__.py
    |-- utils.py
    |-- database.py
    |-- model.py
    |-- camera/
        |-- __init__.py
        |-- calibration.py
        |-- config.py
```

我们可以通过 `<package>.<module>`，`<package>.<subpackage>.<module>`，和类似的语法来访问这些模组的内容。如以下范例：

```python
# 从 `database` 模组导入一个函数
>>> from face_detection.database import load_database

# 导入整个 `model` 模组
>>> from face_detection import model

# 从 `camera` 子包导入一个函数
>>> from face_detection.camera.config import restore_default
```
<!-- #endregion -->

请注意，`.` 语法允许我们相对顶层包一层层深入其中的模组和子包。

<!-- #region -->
<div class="alert alert-info">

**阅读理解：包**

假设我们在使用一个名为 `mail` 的包。路径 `mail/` 包含了一个 `__init__.py` 模组，其内容为：

```python

def send_mail(x):
    return x

phrase_of_the_day = "get that package delievered!"
```

它也包含了一个模组 `delivery.py`，其内容为：

```python
def get_zip():
    """just a dummy function"""
    return 871092
```

1. 创建这个包。
2. 导入 `send_mail` 函数。
3. 导入 `delivery` 模组并执行它的 `get_zip` 函数

</div>
<!-- #endregion -->

<!-- #region -->
## 模组内导入

包中的模组可以互相导入；比如说，假设 `face_detection.database` 和 `face_detection.camera.calibration` 都想要使用 `face_detection.utils` 模组。请回忆本包的结构：

```
- face_detection/
    |-- __init__.py
    |-- utils.py
    |-- database.py
    |-- model.py
    |-- camera/
        |-- __init__.py
        |-- calibration.py
        |-- config.py
```

我们有两种导入风格来进行模组之间的导入：绝对导入和相对导入。

### 绝对导入
绝对导入风格允许你通过提供模组相对顶层包的绝对位置来选择模组。假设我们想要在 `model` 模组中通过绝对导入来导入 `utils` 模组，那么代码将是：

```python
import face_detection.utils
```

或，使用代名词：

```python
import face_detection.utils as utils
```

假设我们也想要在 `face_detection.camera.calibration` 中导入 `utils` 模组；绝对导入的语句和之前一样，因为 `utils` 相对顶层包的相对位置不会根据我们导入它的路径改变。

作为一个额外的例子，在包中任何位置导入 `camera` 的 `config` 模组的绝对导入语句如下：

```python
import face_detection.camera.config
```

绝对导入语法支持我们上方枚举的所有变化，如代名词和 `from <module> import <object>`。

### 相对导入
相对导入使用点来表达被导入模组的相对于执行导入命令的模组的位置。比如说，假设我们想要在 `model` 模组中使用相对导入来导入 `utils` 模组：

```python
from . import utils
```

请注意，`.` 用来表达“当前包”。而 `..` 代表着“当前包上一级的包”。因此，从 `face_detection/camera/calibration.py` 相对导入 `utils` 会是：

```python
from .. import utils
```
我们使用 `..` 的原因是 `utils` 并不和 `calibration.py` 在同一个包中，而在它往上一层的包中。我们也可以如此导入模组中的具体内容：

```python
from ..utils import some_util_func
```

相对导入风格对比绝对导入风格限制更多。它只能包含 `from <module> import <thing1>, <thing2>` 格式的导入语句。虽然它的格式有所限制，相对导入在有着复杂的嵌套子包时可能很方便。
<!-- #endregion -->

<!-- #region -->
## 安装包

### PYTHONPATH和site-packages

到现在为止，我们的Python包阅读都确保了所有我们编写的模组和包都和我们的互动Python进程在同一个路径下。那么我们是如何能够在不知道包的位置的情况下在任何进程或模组中导入NumPy呢？这是因为我们已经安装了*NumPy*；这意味着这个包已经被放在了我们的“Python路径”中，其由 `PYTHONPATH` 代表。

`PYTHONPATH` 描述了Python在导入模组时会搜索的路径。你可以使用 `sys.path` 来查看你的 `PYTHONPATH`：

```python
# 查看 `PYTHONPATH`
>>> import sys
>>> sys.path
['',
 '/home/TerranceWasabi/miniconda3/bin',
 '/home/TerranceWasabi/miniconda3/lib/python36.zip',
 '/home/TerranceWasabi/miniconda3/lib/python3.6',
 '/home/TerranceWasabi/miniconda3/lib/python3.6/lib-dynload',
 '/home/TerranceWasabi/miniconda3/lib/python3.6/site-packages'
]
```

请注意，`PYTHONPATH` 的第一个成员为 `''`，意味着Python直译器会在试图导入时首先查看当前路径。如果它没有找到任何满足导入语句的包或模组，那么它会检查 `PYTHONPATH` 的下一个成员。这就是为什么我们在之前将我们的模组和包创建在和当前Python进程中一样的路径。

现在，请注意 `PYTHONPATH` 的最后一个路径：“site-packages”（站点包）。*site-packages是所有Python包安装的默认路径*。我们可以在任何路径导入NumPy的原因是因为在安装时它被放入了“site-packages”中，而Python直译器将会在试图满足导入语句时检查“site-packages”。
<!-- #endregion -->

<!-- #region -->
如果你不想打印 `PYTHONPATH` 的话，你可以直接检查你的“site-packages“的位置：

```python
# 检查你的site-packages
>>> import site
>>> site.getsitepackages()
['/home/TerranceWasabi/miniconda3/lib/python3.6/site-packages']
```

我们在这里必须指出我们其实省略了一些细节。安装NumPy并不会将它的各个模组和包完整复制到site-packages中。虽然如此，我们并不会更加细致地讨论包安装的技术性细节。你只需要知道包安装的位置以及Python直译器在导入时检查的路径就行了。
<!-- #endregion -->

### 安装你自己的Python包

假设我们对 `face_detector` 项目的工作很满意，并想要安装这个包——将其放置到我们的“site-packages”路径来允许我们无论Python直译器的工作路径都可以导入它。在这里，我们将会创建一个基础的安装脚本来允许我们达成这一点。

我们将直接指出，本模组的目的仅仅只是为你提供安装包的最少指令集。我们不将会讨论幕后发生的事情。请查看[Distutils简介](https://docs.python.org/3/distutils/introduction.html#an-introduction-to-distutils)和[包装你的项目](https://packaging.python.org/tutorials/packaging-projects/#packaging-your-project)来阅读本话题的更多细节。

回到我们的工作，我们将在*和我们包相同的路径下*创建一个设置脚本 `setup.py`。也就是说，我们的路径结构如下：

```
- setup.py
- face_detection/
    |-- __init__.py
    |-- utils.py
    |-- database.py
    |-- model.py
    |-- camera/
        |-- __init__.py
        |-- calibration.py
        |-- config.py
```

<!-- #region -->
最简单的为安装准备你的包的构建脚本 `setup.py` 如下：

```python
# setup.py 的内容
import setuptools

setuptools.setup(
    name="face_detection",
    version="1.0",
    packages=setuptools.find_packages(),
)
```

<!-- #endregion -->

如果你阅读了以上链接的额外材料，你将会注意你可以向这个设置脚本提供更多的可选信息，如作者名字，包的安装要求，等等。

有了这个脚本后，我们可以在本地机器上安装我们的包了！在你的命令行中，导航到包含这个设置脚本和被安装的包的路径中。运行：

```shell
python setup.py install
```

就像这样，你的包 `face_detection` 将被安装到“site-packages”中。你现在可以从计算机的任何路径中导入这个模组。在命令行中执行以下代码来从你的机器上卸载这个包：

```shell
pip uninstall face_detection
```

我们在这里提供最后一个重要细节。你的包的被安装的版本将不会“看到”它的源代码。也就是，当你对你的代码进行修改时，你将会需要首先卸载并重装你的包来使得你的修改在整个系统可见。你可以将你的包在开发（develop）模式下安装来使得程序在“site-packages”路径下创建一个指向源代码的符号链接（symbolic link）。如此，任何对源代码进行的修改都会立刻在全系统都反映出来。因此，与其运行 `python setup.py install`，执行以下代码来在开发模式下安装一个包：

```shell
python setup.py develop
```


## pip和conda：包管理器

你可以在在全世界共享Python包。市面上有两个常用的包管理器，`pip` 和 `conda`。`pip` 从[The Python Package Index (PyPI)](https://pypi.org/)（Python包索引）下载并安装包, 而 `conda` 从Anaconda Cloud下载并安装包。`conda` 和 `pip` 都已经作为[Anaconda分布](http://www.pythonlikeyoumeanit.com/Module1_GettingStartedWithPython/Installing_Python.html#Installing-Anaconda)的一部分被安装在你的机器上了。

执行以下来使用 `pip` 安装包：

```shell
pip install <package_name>
```

执行以下来使用 `conda` 安装包：

```shell
conda install <package_name>
```

两个管理器都会向你的“site-packages”路径安装包。

使用 `conda` 而不是 `pip` 来安装包有着显著的好处。首先，`conda` 有着一个强大的“环境计算器”来记录Python包之间的互相依赖性。因此，它会试图按需安装，升级，并降级包来满足你安装的需求。同时，`conda` 默认提供的包列表由Continuum Analytics，也就是Anaconda的创造者，提供并维持。这样做的好处之一在于，通过 `pip` 安装NumPy会为你提供标准的NumPy版本，而 `conda` 将会安装[mkl优化过的NumPy版本](https://software.intel.com/en-us/articles/numpyscipy-with-intel-mkl)，其执行速度快很多。最后，`conda` 也能作为一个[环境管理器](https://conda.io/docs/user-guide/tasks/manage-environments.html)，其支持你维持多个，互相不冲突的环境来储存不同的Python包安装情况以及甚至不同的Python版本。

话说如此，使用 `pip` 也是有一些好处的。PyPi易用也容易上传包；这很可能是向全世界分布Python包最简单的方法。因此，`pip` 提供了更广阔的包选择。但是，你也可以使用 `conda` 来从自定义频道安装包——如此为你提供Anaconda分布的包列表之外的包。这已经成为了一个流行的安装如PyTorch和TensorFlow的机器学习库的方法。


你可以使用 `conda` 来安装一部分包，而用 `pip` 来安装其它包。唯一需要注意的是不要不小心使用两个管理器安装相同的包——这会搞出大麻烦。


## 官方说明文档链接

- [Python教程：模组](https://docs.python.org/3/tutorial/modules.html)
- [Distutils简介](https://docs.python.org/3/distutils/introduction.html#an-introduction-to-distutils)
- [包装你的项目](https://packaging.python.org/tutorials/packaging-projects/#packaging-your-project) 
- [PyPi](https://pypi.org/)


## 阅读理解答案

<!-- #region -->
**创建简单模组：解**

创建一个名为 `basic_math.py` 的简单数学模组。它应该提供无理数 $\pi$ 和 $e$，以及函数 `deg_to_rad`，其将角度数转化成弧度数。

`basic_math.py` 的内容应为：

```python
"""基本的数学常量和函数"""

pi = 3.141592653589793
e = 2.718281828459045

def deg_to_rad(angle):
    return (pi / 180) * angle
```

```python
>>> import basic_math

# Euler公式：e**(i * pi) = -1
>>> basic_math.e ** (basic_math.pi * complex(0,1))
(-1+1.2246467991473532e-16j)

>>> basic_math.deg_to_rad(45)
0.7853981633974483
```
<!-- #endregion -->

<!-- #region -->
**包：解**

1. 创建这个包。

```
   mail/
       |-- __init__.py
       |-- delivery.py
```

2. 导入 `send_mail` 函数

```python
>>> from mail import send_mail
```

3. 导入 `delivery` 模组并执行它的 `get-zip` 函数

```python
>>> from mail import delivery
>>> delivery.get_zip()
871092
```
<!-- #endregion -->
