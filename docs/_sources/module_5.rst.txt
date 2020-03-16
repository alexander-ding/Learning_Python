模组5：琐碎话题
===============
本模组包含了并不是Python和NumPy核心，但依然对做日常工作极其重要的话题。

第一节介绍了一些编写“好代码”的指导方针。具体来讲，它将为你指向很多Python社区的用户使用的风格指南。它也介绍了相对新但越来越流行的Python功能，叫做类型暗示（type hinting）。它允许我们提供类说明文档注释来完善我们代码的可读性。读者也将接触NumPy和Google的编写好docstring（说明字符串）的指南。

本模组的第二节介绍matplotlib，一个允许我们绘制并视觉上查看数据的模组。在这里，我们将会具体学习如何使用matplotlib的面向对象API，而不是它的函数API，来创建散点图，线图，直方图，和图片绘制。

之后一节将演示使用Python操作文件的“最佳做法“。这包括使用环境管理器（context manager）来读写文件。我们将学习使用强大的 :code:`pathlib.Path` 类来对路径进行优雅并独立于平台的操作。最后，我们将复习一些关键的文件工具，如使用 :code:`glob` 来搜索文件，使用 :code:`pickle` 来储存文件，和储存NumPy数组。

在这之后，我们将学习Python的包装系统。这将让我们理解 :code:`import` 语句具体的功能。这将自然而然地使得我们讨论在机器上安装Python模组具体意味着什么。我们将复习 :code:`pip` 和 :code:`conda` 这两个常用的在机器上安装和管理模组的包管理器。这节对任何想要从只会使用Jupyter记事本的Python用户成熟为可以创造自己的可安装的Python项目的程序员至关重要。这也将大幅提升你诊断在你机器上Python相关问题的能力。

在未来我们将向本模组添加更多节。

.. toctree::
   :maxdepth: 2
   :caption: 内容:

   Module5_OddsAndEnds/Writing_Good_Code.md
   Module5_OddsAndEnds/Matplotlib.ipynb
   Module5_OddsAndEnds/WorkingWithFiles.md
   Module5_OddsAndEnds/Modules_and_Packages.md

