模组2：Python基础
=================
本模组旨在像你介绍Python最基础的一些元素。我们会首先学习Python自带的最基本的对象。这将允许我们来对数字，文本，和可以存储复数对象的容器进行操作。列表（list），元组（tuple），和字符串（string）都可以存储一*序列*的对象（字符串存储的对象就是字符），所以Python提供了一个用来操作这些对象的通用接口；操作这些数据序列的技能是几乎任何Python科学类工作的奠基石。

在介绍过Python基本的对象类型和操作序列的方法好，我们会正式讨论给这些对象设置变量的过程。你将会经常见到将同一个对象对应多个变量的情况；Python对待同一个对象拥有多个“引用”（reference）的方法将在这里被解释清楚，在这个过程共我们将会区分*可变*对象和*不可变*对象。

学会了越来越多对Python对象的操作方法，以及我们对如何用变量引用Python对象的理解后，我们将会学习如何控制我们代码中的逻辑流程。我们将使用“if”，“else”，和“else-if”语句来让计算机只在某些用户设置的条件达成时才执行代码的某个枝干（例，当学生A的成绩低于65分时，执行代码来发一篇email以警告学生）。“while-loop”（while循环）和“for-loop”（for循环）则允许我们重复执行代码区块（例，为列表中的每一个学生计算该学生的平均成绩）。这些叫做控制流（control-flow）的工具将会大幅提升我们写有效代码的能力。

讨论完for循环后，我们将花些时间探索一些Python语言对于可迭代（iterable）（你可以非正式地理解为“可以通过一个for循环迭代的”）对象提供的一些方便之处。说方便其实有点轻描淡写了；这些小技巧将会极大幅度提升我们编写干净，简短，和高效率的代码。我们将会仔细探讨发生器理解语句（generator comprehension statement）。这将允许我们处理长序列的数据而不需要将整套数据都存在内存中。我相信在最后你会庆幸你会这些技能和工具。

回到更传统的编程语言模式，我们会学习如何定义我们自己的函数。这将允许我们包装并在未来碎尸通过调用（call）函数来运行其中的代码。函数将允许我们将代码模组化并利用这些函数创造强大的算法。

最后，我们会回到我们最初的任务：学习不同种类的Python自带对象。这次，我们会额外学习词典（dictionary），集合（set），和其它类型的集（collection）。现在我们对Python的理解应该提升了很多，所以我们可以思考一些更加微妙但极其重要的事情。其中之一就是我们会讨论不同算法在不同数据结构下的效率。比如说，你将学到查看一个对象是否在某集合中比查看是否在某列表中快*很多*。对任何一个任务采取正确的工具会对你的效率产生明确的影响。

虽然本模组不可能面面俱到，但它会帮助你熟悉很多最重要最基础的Python元素，以及那些使得这门语言易用的一些方便点。最终的目的还是帮助读者了解编写易读有效的代码所需的工具，特别是和数据科学应用有关的代码。在写代码时你不应该感觉像是在将一个个你不懂为什么会可用的黑箱叠加在一起；因此，本模组会包含很多的细节。我希望在本文中平衡了易读性和深度，来帮助读者带走最重要的一些知识点。

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
