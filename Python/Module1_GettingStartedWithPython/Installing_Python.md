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
   :description: Topic: Installing Python with Anaconda, Difficulty: Easy, Category: Tutorial
   :keywords: python, anaconda, instructions, environments, beginner, data science, introduction
```

## 安装Python

废话不多说，让我们提供安装Python和其它有用的Python模组的步骤。安装Anaconda平台会安装以下的内容：

 - Python；具体来讲是之前一节提到的CPython直译器。
 - 几个有用的Python模组，如matplotlib，NumPy，和SciPy。
 - Jupyter，用以提供用来编写新代码的互动式“记事本”环境。
 - conda：一个帮助你安装并升级Python和其它Python模组以及自动处理兼容性问题的包管理器。
 
请注意通过Anaconda安装Python**不会**破坏通过其他途径安装的Python。详阅[我刚对我的电脑做了什么？](#我刚对我的电脑做了什么？)。

有些Anaconda提供的模组，如NumPy，是被[优化](https://docs.anaconda.com/mkl-optimizations/)过的，而因此比起单独安装运行得快很多。

<div class="alert alert-info">

**经验**: 

“Anaconda”是个包含CPython直译器和其它流行Python数据科学模组的集合。它同时提供一个用来下载和更新Python模组的包管理器，以及一个用以同时维持多个独立Python安装的环境管理器。
</div>

### 安装Anaconda

1. 点开[本链接](https://www.anaconda.com/download/)，并单击**Python 3**对应的“Download”按钮。
2. 当下载完成后，安装下载的文件。安装过程中会有个选择：`Add Anaconda to the system PATH environment variable`（将Anaconda加到系统PATH的环境变量中）；我建议你**选择**这个选项（懂行的用户：见下以了解关于这个选择的注意事项）。
3. 完成20分钟的["Getting Started"](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html)（入门）教程来熟悉 `conda`。这很重要！译者注：这里是英文的文档，有能力的读者可以用谷歌机翻。如果实在看不懂，原文中关于环境管理的内容在本节后面会有包含。

你将会需要知道如何打开一个命令行（Windows用户是cmd.exe），以及如何在命令行中切换文件夹。如果你不知道如何做到这些，请阅读一个和你操作系统相对应的教程。译者注：Windows：[文库](https://wenku.baidu.com/view/35c6365c7cd184254a353526.html)（注意要下载券）或[经验](https://jingyan.baidu.com/article/adc815138b23eef722bf7348.html)（不全面，但够用）。

<!-- #region -->
### 我刚对我的电脑做了什么？

这创建了一个叫做 `Anaconda3`（或类似）的文件夹。它包含了所以和CPython直译器有关的文件，所有Python标准库的模组，之前提到过的Anaconda包含的第三方模组（例，NumPy，SciPy，Jupyter，iPython），和 `conda` 包管理器。它也包含了这些所有的程序的可执行文件。默认的Anaconda安装路径为：

- (Linux): `/home/<your_username>/Anaconda3`
- (Windows): `C:\Users\<your_username>\Anaconda3`
- (Mac): `/Users/<your_username>/Anaconda3`

如果你跟随了上文的安装指示，那么Anaconda安装器应该*将这个文件夹输入到了你系统的“path”环境变量*。让我们快速地描述一下这具体意味着什么。系统的path仅仅是一列表的文件夹。每当你从你计算机的命令行中执行任何指令，你的计算机都会快速搜寻“path”包含的文件夹下面同名的可执行文件；*计算机将会执行它找到的第一个同名可执行文件*。因此，将 `Anaconda3` 文件夹放在path列表的最顶端会导致你的系统最优先使用Anaconda的Python可执行文件，而不是任何其它安装的Python版本，因为系统会首先找到Anaconda版本的可执行文件。

对Linux和Mac用户而说，你的系统很有可能已经安装了某个版本的Python。*千万不要尝试卸载，删除，或修改系统自带的Python*。这些操作系统用它们自带的Python去执行系统的一些服务。这些服务会直接专门调用系统自带的Python可执行文件——它们不会意外地使用Anaconda版本的Python。所以你要知道的就是你可以安装Anaconda而不担心以上这些细节。

**对用其它编程语言写代码的人的重要事项：**Anaconda使用它自己的 `lib` 和 `bin` 文件夹来存储模组文件和二进制文件。虽然这让用户可以简单地安装复杂的使用C库的Python模组，但是这也意味着你的系统会首先使用Anaconda的文件而不是你的系统的 `lib` 和 `bin`。如果你编写其它语言的代码，这可能会是个大问题。

最简单的解决方案就是*不要*让Anaconda安装器将Anaconda包含在你的path里面。你可以创建一行alias（代名）来手动前置Anaconda到你的path里。比如说，在Linux上你可以在 `~/.bashrc` 文件中添加如下的alias：

```shell
alias anaconda="export PATH=/home/<your_username>/anaconda3/bin:$PATH"
```

设置好这个alias后，你可以从命令台调用 `anaconda` 来将Anaconda暂时添加到path的顶端——这只维持到你当前命令台被关闭之前。

### 快速入门Conda环境

`conda` 不仅仅是一个包管理器，它也同时是一个很强大的环境管理器。在了解它之前，让我们先看看为什么我们会需要一个环境管理器：

>PLYMI建议你使用最新版本的Python（Python 3.X）来使用本资源。但是，很多时候其他的课程或项目会需要你使用Python 2.7，一个已经不被更新或维护的Python版本。有没有一个简单干净的方法来快速在Python 3和Python2.7环境中切换呢？有！使用conda环境可以完美解决这个问题。

假设你的Anaconda安装包含了Python 3，那你的*root*（默认）conda环境就是那个Python 3环境。你现在可以创建一个包含Python 2.7和所有Anaconda自带的第三方模组的conda环境。在你的命令行中执行以下指令：

```shell
conda create -n py27 python=2.7 anaconda
```

当安装成功后，你将可以激活（activate）这个叫做 `py27` 的环境。在命令行中执行以下命令：
```shell
conda activate py27
```

激活一个环境仅仅更新你的系统path变量。在本例中这意味着 `Anaconda3` 会被更新为 `Anacond3/envs/py27`。这样你的系统就会在path中找到Python 2.7的可执行文件还有该版本相对应的模组。请注意这个变化仅仅作用于**当前的命令行进程**。任何其它的命令行进程都会默认使用叫做root的conda环境。

当你激活名为 `py27` 的环境后，你可以使用Python命令行，iPython命令行，Jupyter记事本，执行Python脚本，等等。这些现在都会使用Python 2.7。你可以用 `conda`（和 `pip`）来在这个环境中安装和Python2.7兼容的模组。只要你的path指向的是 `Anaconda3/envs/py27` 而不是 `Anaconda3`，这些指令的行为就会如同Python 2.7是计算机上唯一的Python版本一般。

冻结（`deactivate`）这个环境将将你返回到名为root的Python 3环境。这意味着conda将重新修改path，让它包含 `Anaconda3` 而不是 `Anaconda3/envs/py27`。执行以下指令：

```shell
conda deactivate
```

就这样，conda环境提供给你像招魂师一样的能力，轻便地在活的（Python 3）和死的（Python 2.7）之间切换。

除了在Python 3和2之间切换，conda环境有着很多其它的作用。很多人喜欢为每一个新的大规模项目创建一个新的conda环境。这样他们可以随意安装任何该项目需要的模组而不用担心和其他项目模组的冲突。读者应当注意学习和使用conda环境。

我非常建议你去阅读[这个管理conda环境的教程](https://conda.io/docs/user-guide/tasks/manage-environments.html)。译者注：同样是英文的文档，可以考虑不看。
<!-- #endregion -->
