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
   :description: Topic: Integrated Development Environments, Difficulty: Easy, Category: Tools
   :keywords: python, introduction, IDE, PyCharm, VSCode, Jupyter, recommendation, tools
<!-- #endraw -->

# 设置开发环境
## 你将学到什么

- 集成开发环境（IDE）是一套允许你编辑，运行，和调试代码的复杂的文本编辑器。
- Python壳（shell）是一个用以输入和直接在计算机命令行中运行Python代码的接口。
- IPython壳是一个更加完善的Python壳版本——它提供语法高亮，自动完成，和其它功能。
- Jupyter记事本是一个强大的编写，实验新代码的工具。它同时也可以用来可视化数据和编写格式化的文本。我们将会在本资源中经常使用它。


## 集成开发环境（IDE）
在本模组的第一节中，我们了解了Python脚本仅仅是包含Python代码的文本文件这一事实。除了使用 `.py` 为文件后缀名之外，Python脚本对比其它文本文件没有什么特殊的特征。虽然如此，一般来说用简单的文本编辑器来编写Python代码并不有效（而使用类似Microsoft Word的文字处理软件也是万万不可取的）。我们需要一个“集成开发环境”（IDE）来帮助我们开发代码。

首先，一个好的IDE会提供一个拥有如下功能的文本编辑器：

- 检查你的代码以找出并标记语法错误（拼错的函数名，调用不存在的变量，等等）
- 彩色化你的代码使其更容易分辨。比如说，这可以帮助你区分数字和字符串。
- 让你可以轻松地查阅函数的定义和说明文件。
- 在你输入变量和函数时自动完成它们的名字。

一个IDE同时也经常提供调试工具来帮助你测试你的代码；它也经常会提供一个和像Git这样的版本控制软件（version-control software）的交互接口来帮助你在修改代码的同时跟踪代码的版本。虽然它们很有用，我们在这里并不会讨论这些更高级的功能。

### 推荐的IDE
市面上有很多很棒的IDE可以调试以配合Python使用。我们在这里推荐两个IDE：
 
[PyCharm](https://www.jetbrains.com/pycharm/download): 一个专门针对Python的强大IDE。

**好处**

- 下载即用，无需太多设置就很好用
- 有职业人员的长期支持，因此很可靠
- 可以进行深层次的设置
- 全功能

**坏处**

 - 特别对手提电脑来说可能很耗资源
 - 对新用户而言可能会难易上手（但它有很好的教程和说明文件）
 
[Visual Studio Code](https://code.visualstudio.com/) 以及它的 [Python拓展](https://code.visualstudio.com/docs/languages/python): 一个轻便，宜定制的IDE。

**好处**

- 轻便优雅 
- 支持很多不同的语言，这样你只需要学习一个IDE
- 有极其多的拓展可供下载以提供编辑器新功能；这些拓展是由一个大社区的开源开发者编写的。

**坏处**

- PyCharm暂时比起更完善和强大，但注意Microsoft现在正式支持Python拓展了
- 可能需要细微的调试才能使用某些功能


<div class="alert alert-info">

**经验**:

集成开发环境（IDE）提供强大的工具来帮助你编写格式化和无拼写错误的代码。本资源建议你使用PyCharm Community Edition或Visual Studio Code（加上Python拓展）作为你的Python IDE。
</div>


<div class="alert alert-warning">

**Jupyter Lab**:

[Jupyter Lab](https://jupyterlab.readthedocs.io/en/stable/getting_started/overview.html)是由Jupyter Notebook团队开发的一个全新的IDE。它的设计宗旨是为了将传统IDE的完善和强大和记事本环境的方便和好用。在编写本文时，Jupyter Lab还在beta测试期。鉴于Jupyter Notebook的流行度，Jupyter Lab也很有可能会很快成为一个常用的IDE。

</div>
