模组3：NumPy基础
==============
NumPy就是Python能和R，Matlab，和Julia这一级别的语言在进行科学有关的数据处理工作相提并论的原因。它作为一个第三方模组（也就是说它不是Python标准库的一部分）为用户提供了一个多功能的数组对象和强大的操作这些数组的数学函数；这些都用来帮助用户使用Python进行数字工作。NumPy通过矢量化（vectorization）这一过程来高度优化它很多的功能并取得Python语言正常情况下无法达到的计算效率。

NumPy对Python数据处理界的影响毋庸置疑。不管你是在matplotlib中绘制数据，通过`pandas <https://pandas.pydata.org>`_和`xarray <https://xarray.pydata.org/en/stable>`_分析数据，使用`OpenCV <https://opencv.org>`_处理图片和视屏，用astropy <www.astropy.org>`_进行天文学研究，或是通过`Scikit-Learn <https://scikit-learn.org/stable/index.html>`_和`MyGrad <https://mygrad.readthedocs.io>`_来尝试机器学习，你走在使用受到NumPy深远影响的Python模组。在它们的最核心，这些模组都依赖着NumPy的N维数组和它高效的矢量化功能。NumPy也从根本性上影响了这些模组的设计以及它们对用户开放的接口。因此，熟练使用这些工具并进行Python科学工作都建立在对NumPy扎实的理解上。

本模组向我们介绍NumPy的基础。我们将首先定义维度（dimensionality）一词的具体含义并发展对零，一，二，和N维数组以及它们为什么对数据科学应用如此重要的直观理解。然后，我们将会讨论数组遍历顺序的不明确点以及NumPy默认使用的行优先排序。然后我们将会讨论极其重要的矢量化，其描述了NumPy向发送对数组进行的数学操作的方法。这也将帮助我们理解NumPy如何得到它如此高的计算效率。最后，我们将会深入讨论NumPy的高级功能。这包括它为不同形状的数组之间的数学操作进行广播（broadcast）的规则，以及通过基础和进阶索引来访问和更新数组的内容的方法。有了这些技巧折后，我们将能够使用NumPy和Python很多其它的科学模组来编写短小精悍的数组处理代码！



.. toctree::
   :maxdepth: 2
   :caption: Contents:

   Module3_IntroducingNumpy/IntroducingTheNDarray.md
   Module3_IntroducingNumpy/AccessingDataAlongMultipleDimensions.md
   Module3_IntroducingNumpy/BasicArrayAttributes.md
   Module3_IntroducingNumpy/FunctionsForCreatingNumpyArrays.md
   Module3_IntroducingNumpy/ArrayTraversal.md
   Module3_IntroducingNumpy/VectorizedOperations.md
   Module3_IntroducingNumpy/Broadcasting.md
   Module3_IntroducingNumpy/BasicIndexing.md
   Module3_IntroducingNumpy/AdvancedIndexing.md
