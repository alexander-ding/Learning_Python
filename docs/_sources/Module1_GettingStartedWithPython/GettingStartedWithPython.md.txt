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
   :description: Topic: Basic description of the Python programming language, Difficulty: Easy, Category: Background
   :keywords: python, install, basics, scripts, interpreter, foundations
<!-- #endraw -->

<!-- #region -->
# 介绍Python编程语言

在本节中我们将学习

 - Python是什么。
 - “安装Python”具体意味着什么。
 - 如何写和执行Python代码。

## Python是什么？
**Python是一门编程语言**。具体来讲，Python为我们提供一组严谨的语法规则；每条规则都对应一组计算机会执行的明确命令。这将允许我们编写一些即易读，又能提供具体命令给计算机的文本。所谓的Python**代码**其实就是符合Python语法的文本。

比如说，以下的文本就符合Python的语法规则：

```python
x = 2 + 3
print('2 + 3 = {}'.format(x))
```

根据Python的定义，它将指令计算机去：

 - 计算 2 + 3
 - 在内存中设定一个函数 `x` 为计算的结果（5）
 - 访问 `x` 的值，并在计算机屏幕上显示：“2 + 3 = 5”

<!-- #endregion -->

<!-- #region -->
一个**不符合**Python规则的例子：

```python
x = 2 ~ 3
```
原因是，根据Python定义，夹在两个整数中间的符号 `~` 没有任何意义。

因此，“学习Python”意味着学习Python语言的语法规则以及这些规则对应的计算机指令。同时，你也需要了解已经“提前写好”的，极其方便的Python自带的工具；这些工具是所谓Python标准库的一部分。

有了对Python语言的基本理解后，我们现在要学习如何使用我们的Python代码去给计算机发送指令。最基本的方法是：

 - 写Python“脚本”：一个包含Python代码的文本。
 - 将这个文本送到**Python直译器**（Python Interpreter）中。直译器将告诉计算机去执行代码描述的工作。

<!-- #endregion -->

## Python脚本
你可以用任意一个文本编辑器将前文的能用的代码存到一个文本文件中。就像这样，你写好了一个**Python脚本**：一个包含Python代码的文本文件。标准情况下，Python代码的后缀都是 `.py`（比如说 `my_code.py`），而不是常见的 `.txt`（例如 `my_text.txt`）。这个 `.py` 后缀没有任何特殊的；这仅仅是为了帮助用户区分包含Python代码的文本文件和其他正常的包含普通文章的文本文件。

虽然你可以使用基本的文本编辑器去写Python脚本（如notepad（Windows），TextEdit（Mac），nano（Linux）），但是你也可以使用更加复杂的提供“集成开发环境”（Integrated Development Environment，也就是IDE）的编辑器来写代码。一个为Python配置的IDE会警告不符合Python语法的代码，就像Word这类文字处理软件会提醒你的错误拼写一样。我们会在之后的一节里详细讨论IDE。

<div class="alert alert-warning">

**警告**：

不要使用像Microsfot Word这样的传统文字处理程序来编写代码。这些程序会“静默”地对你打出的字符进行修改，如双引号的种类。这会导致你的代码出现错误。

译者注：在写Python代码时一定要记得使用英文输入法。Python直译器并不会理解中文的双引号 “” 或单引号 ‘’
</div>

在有一个Python脚本后，该如何让电脑读取并执行脚本的指示呢？你需要在你的计算机上安装一个**Python直译器**。当人们说你需要“安装Python”的时候，这就是他们具体的意思。


## Python直译器是什么？“安装Python”意味着什么？

Python直译器泛指任何可以执行以下的电脑程序：

 - 读取文本文件（如 `my_code.py`）。
 - 解析其文本并反馈文本是否符合Python语言的语法规则（如果不符合，直译器会报错）。
 - 指示计算机去执行文本描述的任务。
 - 总体而言，Python直译器将从上到下执行脚本。

世界上第一个Python直译器是用C编程语言写的，因此其被命名为CPython。所以**CPython直译器是最官方的Python语言直译器**。任何新的Python特征或规则一定会在[CPython代码库](https://github.com/python/cpython)中实现。

<div class="alert alert-warning">

**注**：

当然，Python直译器也存在于其他不是C的编程语言中，如Java（Jython）和Go（Grumpy）。这些直译器并不能保证实现最新的Python规范，因此很少有人会将它们和CPython直译器对比。
</div>

<div class="alert alert-warning">

**关于安装Python**：

不要从python.org下载和安装Python。这样做没有任何错，但是本文后面的一节将提供详细的Python安装指示。

</div>

如果你从[python.org](https://www.python.org/downloads/release/python-363/)“在你的计算机上安装了Python”，你在做的基本就是下载一个满足Python直译器规范的可执行文件。比如在Windows上，这个程序叫做 `python.exe`。这个程序就是之前提到的CPython代码的产物，其可以执行任何Python直译器的工作。同时，你也在下载一大组可以用于你自己代码里的工具和函数。这些工具和函数叫做Python标准库；花一些时间去了解[标准库](https://docs.python.org/3/library/index.html#the-python-standard-library)的内容。

当你在计算机上安装了一个Python直译器后，使用它去执行Python脚本并不困难。为简化我们的讨论，假设叫做 `python` 的直译器程序和 `my_script.py` 都在你的计算机中同一个文件夹内（叫做 `folder`）。那么，在命令行（对Windows而言就是 `cmd.exe`）中，你可以执行以下指令：

```shell
python my_script.py
```

这将指示Python直译器程序 `python` 去读取你的文本文件 `my_script.py`，确保里面的代码符合所有Python语言的语法规则，并指示你的计算机去执行和文本相应的指令。如果你的脚本仅仅包含代码 `print("hello world")`，那么你的命令行自然会显示字符串 `hello world`。

在实际使用中，不管你从任何文件夹中执行 `python my_script.py`，你的计算机也应该会知道如何去找到名为 `python` 程序。相应的设置会在安装过程中完成。

理解Python语言是被一个用另外语言编写的程序来直译的事实可能比较绕脑。如果是这样，那另外那个语言是怎么被直译的呢？在CPython的情况下，答案就是C代码并不会被直译；有程序存在于Windows，Mac，和Linux上可以直接将C代码翻译成电脑直接执行的机器指令。

<!-- #region -->
## 为什么选择Python？

Python已经成为了一个极其流行的编程语言。事实上，它可能是[最流行的大学编程入门语言](https://cacm.acm.org/blogs/blog-cacm/176450-python-is-now-the-most-popular-introductory-teaching-language-at-top-u-s-universities/fulltext)。首先，Python的句法非常易懂易读。比如说，接下来的代码会求数字 0-9 的和，并打印结果：
```python
a = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(sum(a))
```

同样的工作可以用以下的C++代码达成，但是这难懂很多：
```cpp
#include <iostream>
#include <vector>
#include <numeric>

int main() {
    std::vector<int> a = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
    std::cout << std::accumulate(a.begin(), a.end(), 0) << std::endl;
}
```

因此，Python作为一门语言很方便快速编写和测试新代码。同时，Python是开源的；你可以免费使用它，任何人都可以参与到维持或改善这门语言中，且大家创造了很多给Python提供了大量功能的第三方模组。因此，Python在科学家，工程师，和其他研究者中极其流行。

在本资源中我们会使用Python和高度优化的数字工作模组NumPy。
<!-- #endregion -->

## 总结

- Python是一门编程语言——它为我们提供一组简单的语法规则来编写既可以让人看懂也可以准确翻译到计算机指令的文本。
- Python代码是任何符合Python语言标准的文本。
- Python脚本是一个包含Python代码的文本文件。这种文件根据命名标准都使用后缀 `.py`。
- Python直译器是一个能够读取和分析文本文件并根据Python语言的规则将其中的代码翻译成计算机指示的程序。
- "安装Python”归根结底就是将Python直译器下载到你的计算机上。
- 官方的Python直译器是一个用C语言编写的程序，叫做CPython直译器。
- Python"标准库“是和CPython直译器一起包装的一组庞大的工具和函数。
