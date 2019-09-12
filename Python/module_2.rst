模组2：Python基础
=================
本模组旨在向你介绍Python最基础的一些元素。我们会首先学习Python自带的核心对象。这将允许我们对数字，文本，和可以存储多个对象的容器进行操作。列表（list），元组（tuple），和字符串（string）都可以存储一*序列*的对象（字符串存储的对象就是字符），所以Python提供了一个用来操作这些对象的通用接口；操作这些数据序列的技能是进行几乎任何Python科学工作的奠基石。

在介绍过Python基本的对象类型和操作序列的方法后，我们会正式讨论给这些对象设置变量的过程。你将会经常见到一个对象对应多个变量的情况；Python处理同一个对象对应多个“引用”（reference）的方法将在这里被讨论解释。在这个过程中我们将会区分*可变*对象和*不可变*对象。

学会了更多的Python对象以及用变量引用Python对象的操作方法后，我们将会学习如何控制代码中的逻辑流程。我们将使用“if”，“else”，和“else-if”语句来让计算机只在某些条件达成时才执行代码的某个枝干（如，当学生A的成绩低于65分时，执行代码来向学生发一封警告信电邮）。“while-loop”（while循环）和“for-loop”（for循环）则允许我们重复执行代码区块（如，为列表中的每一个学生计算平均成绩）。这些叫做控制流（control-flow）的工具将会大幅提升我们写高效代码的能力。

讨论完for循环后，我们将花些时间探索Python语言针对可迭代（iterable）（你可以暂时理解为将“可迭代”定义为“可以通过for循环迭代的”）对象提供的一些方便功能。说方便其实有点轻描淡写了；这些功能将会极大幅度提升我们编写干净，简短，和高效率的代码的能力。我们将会仔细探讨生成器理解语句（generator comprehension statement）。这将允许我们处理长序列的数据而不需要将整套数据都存在内存中。我相信在最后你会庆幸你学习了这些技能和工具。

回到更传统的编程语言模式，我们会学习如何定义我们自己的函数。这将允许我们包装并在未来随时通过调用（call）函数来运行其中的代码。函数允许我们将代码模组化并利用它们创造强大的算法。

最后，我们会回到最初的任务：学习不同类型的Python内置对象。这次，我们会额外学习词典（dictionary），集合（set），和其它类型的集（collection）。在提升了我们对Python的理解后，我们可以考虑一些更加微妙但极其重要的事情。其中之一就是不同算法在不同数据结构下的效率。比如说，你将学到检查一个对象是否在某集中比检查是否在某列表中快*很多*。对任何一个任务采取正确的工具会对你的效率产生显著的影响。

虽然本模组不可能面面俱到，但它会帮助你熟悉很多最重要最基础的Python元素，以及使得这门语言易用的一些方便功能。本模组最终的目的还是帮助读者了解编写易读有效的代码——特别是和数据科学应用有关的代码——所需的工具。编写代码的过程不应该感觉像是将一个个不明所以的黑箱叠加在一起；因此，本模组会包含很多的细节。我希望在本文平衡了易读性和深度，来帮助读者带走最重要的一些知识点。

.. toctree::
   :maxdepth: 2
   :caption: Contents:

   Module2_EssentialsOfPython/Basic_Objects.md
   Module2_EssentialsOfPython/SequenceTypes.md
   Module2_EssentialsOfPython/Variables_and_Assignment.md
   Module2_EssentialsOfPython/Introduction.md
   Module2_EssentialsOfPython/ConditionalStatements.md
   Module2_EssentialsOfPython/ForLoops.md
   Module2_EssentialsOfPython/Iterables.md
   Module2_EssentialsOfPython/Generators_and_Comprehensions.md
   Module2_EssentialsOfPython/Itertools.md
   Module2_EssentialsOfPython/Functions.md
   Module2_EssentialsOfPython/Scope.md
   Module2_EssentialsOfPython/DataStructures.md
   Module2_EssentialsOfPython/DataStructures_II_Dictionaries.md
   Module2_EssentialsOfPython/DataStructures_III_Sets_and_More.md
