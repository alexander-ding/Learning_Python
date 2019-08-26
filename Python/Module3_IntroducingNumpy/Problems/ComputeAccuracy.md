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
   :description: Topic: Measuring classification accuracy, Difficulty: Easy, Category: Practice Problem
   :keywords: numpy, vectorization, practice, machine learning, classifier
```

<!-- #region -->
# 测量分类模型的精度
假设我们在做一个使用某种模型来处理图片并分类其内容的项目。比如说，我的 `cat_dog_goose_other` 函数将试图分类某张图片是否是猫（类型0），狗（类型1），鹅（类型2），或其它（类型3）。我们想要测量分类器的*精确度*。也就是说，我们想要向函数输入一序列我们知道其类型的图片并数模型的分类和实际分类相同的次数。这样做的精度就是模型分类正确的图片的比例。

为我们输入给 `cat_dog_goose_other` 模型的每一张图片，模型都会返回四个**分数**（score）——每个分数对应一类型。模型设计的结果就是分数最高的类型就是模型预测的类型。比如说，如果模型处理一张图片，它将会返回一个形状为 $(1, 4)$ 的分数数组：

```python
>>> scores = cat_dog_goose_other(image)
# 处理一张图片将返回一个1x4的分类分数数组
>>> scores
array([[-10, 33, 580, 100]])
```
在这里，我们的模型预测这张图片是一只鹅，因为类型2对应的分数（`scores[2]`）是最大的值。总体而言，如果我们向 `cat_dog_goose_other` 输入一个有着 $N$ 张图片的数组，它将返回一个形状为 $(N, 4)$ 的分类分数数组——$N$ 张图片的每一张都对应着 $4$ 个分数。

因为我们在测量模型的精准度，我们将输入一组我们已经知道其真实分类的图片。也就是说，我们有着每张图片的正确**标签**（label），以类型ID的形式存储着。比如说，一张猫的图片会对应着标签 `0`，一张狗的图片会对应标签 `1`，如此重复。所以，一打 $N$ 张图片会对应着一个形状为 $(N,)$ 的整数标签数组，而每个标签都在 $[0, 4)$ 之内。

假设我们向我们的模型输入了5张图片，而它返回了以下分数：
```python
# `cat_dog_goose_other` 对5张图片产生的分类分数
# 一个形状为 (5, 4) 的数组
>>> import numpy as np
>>> scores = np.array([[ 30,   1,  10,  80],  # 预测：其它
...                    [-10,  20,   0,  -5],  # 预测：狗
...                    [ 27,  50,   9,  30],  # 预测：狗
...                    [ -1,   0,  84,   3],  # 预测：鹅
...                    [  5,   2,  10,   0]]) # 预测：鹅
``` 

并假设这5张图片的正确标签为：
```python
# 正确：猫，狗，狗，鹅，其它
>>> labels = np.array([0, 1, 1, 2, 3])
```

那么我们的模型在5张图片中正确分类了3张；因此，我们的精确度函数应当返回0.6:
```python
>>> classification_accuracy(scores, labels)
0.6
```

为了归纳这个问题，假设你的分类器会分类 $K$ 个类型（而不是 $4$ 个）。填完以下函数。

**提示**：你会想要使用[numpy的argmax函数](https://docs.scipy.org/doc/numpy/reference/generated/numpy.argmax.html#numpy.argmax)。

```python
def classification_accuracy(classification_scores, true_labels):
    """
    返回一组N个预测的分类精确度。

    Parameters
    ----------
    classification_scores : numpy.ndarray, shape=(N, K)
        一组N个数据（图片）的K个类型的分数。
    true_labels : numpy.ndarray, shape=(N,)
        一组中每个数据的正确标签：每个标签是一个在 [0, K) 之中的整数。

    Returns
    -------
    float
        正确数 / N
    """
    # 你的代码
    pass

```
<!-- #endregion -->

<!-- #region -->
### 未矢量化的解
此问题的一个简单处理方法就是首先迭代我们分类分数的每一行。我们知道每一行数据储存着一个数据点每个类型的分数，而每一行最高分数的*索引*将会为我们提供该数据点（如我们例子中的图片）的预测标签。然后我们可以直接将预测的标签和正确的标签进行对比来计算精确度。

我们可以使用函数 `numpy.argmax` 来获取每个数据点的最高分数的索引并由此获得其预测的类型ID。请回忆，NumPy数组使用[行优先遍历顺序](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/ArrayTraversal.html#How-to-Traverse-an-Array:-Row-major-%28C%29-vs-Column-major-%28F%29-Traversal-Ordering)，所以for循环 `classification_scores` 将会一次返回数组的一行。

```python
pred_labels = []  # 将会储存N个预测的类型ID
for row in classification_scores:
    # 储存每个数据点最大值对应的索引
    pred_labels.append(np.argmax(row))  
```

接下来，我们需要数多有少比例的预测类型ID和正确标签相同。

```python
num_correct = 0
for i in range(len(pred_labels)):
    if pred_labels[i] == true_labels[i]:
        num_correct += 1
```

我们也可以使用[生成器理解](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html#Creating-your-own-generator:-generator-comprehensions)和[itertools](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Itertools.html)来简化以上代码：

```python
# 请回忆：int(True) -> 1, int(False) -> 0
num_correct = sum(p == t for p, t in zip(pred_labels, true_labels)) 
```

我们可以将以上代码正式写入一个函数中：

```python
def unvectorized_accuracy(classification_scores, true_labels):
    """
    返回一组N个预测的分类精确度。

    Parameters
    ----------
    classification_scores : numpy.ndarray, shape=(N, K)
        一组N个数据（图片）的K个类型的分数。
    true_labels : numpy.ndarray, shape=(N,)
        一组中每个数据的正确标签：每个标签是一个在 [0, K) 之中的整数。

    Returns
    -------
    float
        正确数 / N
    """
    pred_labels = []  # 将会储存N个预测的类型ID
    for row in classification_scores:
        pred_labels.append(np.argmax(row))
    
    num_correct = 0
    for i in range(len(pred_labels)):
        if pred_labels[i] == true_labels[i]:
            num_correct += 1
    return num_correct / len(true_labels)
```
用之前例子的数据进行测试：
```python
>>> unvectorized_accuracy(scores, labels)
0.6
```
耶！我们有一个可用的精确度函数了！但是，这个函数可以通过[矢量化](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/VectorizedOperations.html)大幅度地简化和优化。
<!-- #endregion -->

<!-- #region -->
### 矢量化的解
`numpy.argmax` 是NumPy的一个[矢量化序列函数](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/VectorizedOperations.html#Sequential-Functions)。如此，它接受[关键词参数axis](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/VectorizedOperations.html#Specifying-the-axis-Keyword-Argument-in-Sequential-NumPy-Functions)。这意味着，与其对 `classification_scores` 的每一行通过for循环调用 `np.argmax`，我们可以直接通过提供 `axis=1` 来告诉 `np.argmax` 去对*数组每一行的列*进行操作。

```python
# 返回 `classification_scores` 每行中最大值列的索引
pred_labels = np.argmax(classification_scores, axis=1)
```
这个简单的表达式完全代替了我们的第一个for循环。

然后，我们可以使用NumPy的*矢量化逻辑操作符* `==` 来获取一个布尔数组，其在预测标签等于正确标签时储存 `True`，而不等于时储存 `False`。请回忆，`True` 的行为和 `1` 一样，而 `False` 的行为和 `0` 一样。所以，我们可以通过对返回的布尔数组调用 `np.mean` 来计算正确预测除以总预测数的比例。我们可以如此矢量化第二个for循环：

```python
# 计算正确预测标签的比例
frac_correct = np.mean(pred_labels == true_labels)
```

综上，聪明地使用矢量化允许我们在*一行代码内*编写我们的分类精度函数。

```python
def classification_accuracy(classification_scores, true_labels):
    """
    返回一组N个预测的分类精确度。

    Parameters
    ----------
    classification_scores : numpy.ndarray, shape=(N, K)
        一组N个数据（图片）的K个类型的分数。
    true_labels : numpy.ndarray, shape=(N,)
        一组中每个数据的正确标签：每个标签是一个在 [0, K) 之中的整数。

    Returns
    -------
    float
        正确数 / N
    """
    return np.mean(np.argmax(classification_scores, axis=1) == true_labels)
```

这不仅仅看起来干净很多，它也更加简单且在编写时更难出错。同时，它相比我们之前未矢量化的版本快很多——设 $N=10,000$ 个数据点和 $K=100$ 个类型，我们的矢量化解大约快 $40\times$。 

（以下的“time-it”代码块必须在Jupyter记事本或IPython命令行的独立小节中运行——`%%timeit` 必须是小节顶端的第一行命令）

```python
>>> N = 10000
>>> K = 100
>>> scores = np.random.rand(N, K)
>>> labels = np.random.randint(low=0, high=K, size=N)
```
```python
>>> %%timeit
... unvectorized_accuracy(scores, labels)
39.5 ms ± 1.2 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
```

```python
>>> %%timeit
... classification_accuracy(scores, labels)
1.6 ms ± 7.04 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```
<!-- #endregion -->
