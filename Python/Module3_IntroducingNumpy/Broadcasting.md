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
   :description: Topic: Numpy array broadcasting, Difficulty: Medium, Category: Section
   :keywords: broadcasting, vectorization, rules, mismatched shapes, distances
<!-- #endraw -->

<!-- #region -->
# 数组广播
NumPy提供了用来广播（broadcast）对*不同*形状的数组进行数学操作的功能：

```python
>>> import numpy as np

# 形状为 (3, 4) 的数组
>>> x = np.array([[-0. , -0.1, -0.2, -0.3],
...               [-0.4, -0.5, -0.6, -0.7],
...               [-0.8, -0.9, -1. , -1.1]])

# 形状为 (4,) 的数组
>>> y = np.array([1, 2, 3, 4])

# 使用形状为 (4,) 的数组乘到形状为 (3, 4) 的数组
# `y` 被 `x` 的每一行乘上
>>> x * y
array([[-0. , -0.2, -0.6, -1.2],
       [-0.4, -1. , -1.8, -2.8],
       [-0.8, -1.8, -3. , -4.4]])
```

从结果上讲，NumPy将 `y` 在一个新的维度广播，使其成为形状是 (3, 4) 的二维数组，这样它的形状就和 `x` 相对应，支持相乘了：

\begin{equation}
\left( \begin{array}{*{3}{X}}
  -0.0 & -0.1 & -0.2 & -0.3 \\
  -0.4 & -0.5 & -0.6 & -0.7 \\
  -0.8 & -0.9 & -1.0 & -1.1
\end{array} \right)
% 
\cdot \left( \begin{array}{*{3}{X}}
   1 & 2 & 3 & 4
\end{array}\right)
% 
\rightarrow \left( \begin{array}{*{3}{X}}
  -0.0 & -0.1 & -0.2 & -0.3 \\
  -0.4 & -0.5 & -0.6 & -0.7 \\
  -0.8 & -0.9 & -1.0 & -1.1
\end{array} \right)
%
\cdot \left( \begin{array}{*{3}{X}}
  1 & 2 & 3 & 4 \\
  1 & 2 & 3 & 4 \\
  1 & 2 & 3 & 4
\end{array}\right)
\
\end{equation}

请注意，NumPy并不会真的在幕后创建这个广播过的 `y` 版本；NumPy能够在不浪费地将它的成员复制到形状为 (3,4) 的数组的情况下进行必须的运算。因为这么做会浪费内存和计算时间。虽然如此，这个不存在的复制过程能够表达数组之间的数学运算，所以之前的图表假装这个复制过程存在。你在也可以假装内容被复制了来方便理解广播的结果。

广播不仅仅作用域在1维和2维数组之间的操作，因此有些时候操作中的两个数组都需要被广播。虽然如此，但是不是所有对的数组都可以通过广播来兼容的（叫做广播不兼容——broadcast-incompatible）。


```python
# 广播形状为 (3, 1, 2) 的数组和形状为 (3, 1)
# 的数组的乘法。
>>> x = np.array([[[0, 1]],
...
...               [[2, 3]],
...
...               [[4, 5]]])

>>> y = np.array([[ 0],
...               [ 1],
...               [-1]])

# 形状 (3, 1, 2) 广播相乘形状 (3, 1)
# 返回形状 (3, 3, 2)
>>> x * y
array([[[ 0,  0],
        [ 0,  1],
        [ 0, -1]],

       [[ 0,  0],
        [ 2,  3],
        [-2, -3]],

       [[ 0,  0],
        [ 4,  5],
        [-4, -5]]])

# 一个广播不兼容数组对的例子
# 形状为 (2,) 的数组和形状为 (3,) 的数组
>>> np.array([1, 2]) * np.array([0, 1, 2])
ValueError: operands could not be broadcast together with shapes (2,) (3,) 
```
<!-- #endregion -->

<div class="alert alert-info"> 

**定义：数组广播** 

数组广播（array broadcasting）是NumPy用来支持形状不同但兼容的多个数组之间的矢量化数学操作的功能。具体来讲，NumPy会假装数组的内容在某个合适的维度被复制，使得这个复制出来，更高维度的数组能够完成这里的数学操作。

</div>


现在我们将总结检查两个数组是否广播兼容的规则，以及在进行数学操作后得到的数组的形状。


## 广播的规则
数组广播并不能支持任何组合的数组形状。比如说，一个形状为 (7,5) 的数组就不和形状为 (11,3) 的数组兼容。试图将这两个数组相加会导致一个 `ValueError`。以下规则可以用来决定两个数组是否广播兼容：

<div class="alert alert-warning"> 

**定义：广播的规则**：

为检测两个数组是否广播兼容，将两个数组的形状靠尾对应，然后检查每对对应的维度是否满足以下条件之一：

- 对应的维度大小一样
- 其中一个维度的大小是1

如果每对对应的维度都满足以上之一，那么这两个数组广播兼容。

</div>


请注意，如果一个数组的维度更高并导致前面有些维度没有和另外一个数组对应，这是没有问题的。任何大小为1的维度或”缺失“的维度都会通过广播来填充。

请考虑前一小节的范例。请根据以上规则思考为什么形状为 (4,3) 和形状为 (3,) 的数组满足广播兼容的规则：
```
     array-1: 4 x 3
     array-2:     3
result-shape: 4 x 3
```

让我们来考虑各种数组形状的对并理解它们为何广播（不）兼容：

```
     array-1:         8
     array-2: 5 x 2 x 8
result-shape: 5 x 2 x 8

     array-1:     5 x 2
     array-2: 5 x 4 x 2
result-shape: 不兼容

     array-1:     4 x 2
     array-2: 5 x 4 x 2
result-shape: 5 x 4 x 2

     array-1: 8 x 1 x 3
     array-2: 8 x 5 x 3
result-shape: 8 x 5 x 3

     array-1: 5 x 1 x 3 x 2
     array-2:     9 x 1 x 2
result-shape: 5 x 9 x 3 x 2

     array-1: 1 x 3 x 2
     array-2:     8 x 2 
result-shape: 不兼容

     array-1: 2 x 1
     array-2:     1  
result-shape: 2 x 1
```


<!-- #region -->
NumPy提供函数[broadcast_to](https://docs.scipy.org/doc/numpy/reference/generated/numpy.broadcast_to.html#numpy.broadcast_to)来将数组广播到某个用户提供的形状。这可以用来帮助我们发展对广播的直观理解。让我们将形状为 (3,4) 的数组广播成一个形状为 (2,3,4) 的数组：

```python
# 演示 `np.broadcast_to`
>>> x = np.array([[ 0,  1,  2,  3],
...               [ 4,  5,  6,  7],
...               [ 8,  9, 10, 11]])

# 显式将形状为 (3,4) 的数组广播成 
# 形状为 (2,3,4) 的数组
>>> np.broadcast_to(x, (2, 3, 4))
array([[[ 0,  1,  2,  3],
        [ 4,  5,  6,  7],
        [ 8,  9, 10, 11]],

       [[ 0,  1,  2,  3],
        [ 4,  5,  6,  7],
        [ 8,  9, 10, 11]]])
```
<!-- #endregion -->

<div class="alert alert-info"> 

**阅读理解：广播兼容性**

设以下数组形状对，判断广播后得到的形状是什么并标注形状广播不兼容的对。

1\. `7 x 2` 和 `7`

2\. `4` 和 `3 x 4`

3\. `1 x 3 x 1` 和 `8 x 1 x 1`

4\. `9 x 2 x 5` 和 `2 x 5`

5\. `3` 和 `3 x 3 x 2`

</div>

<!-- #region -->
## 数组广播的简单应用
我们在此提供一个简单的使用广播的现实世界范例。假设你有着6个学生的成绩簿，每个成绩簿有着3个考试的成绩，你将这些成绩储存在一个形状为 (6,3) 的数组中：

```python
# 6个学生3次考试的成绩
# 轴0（行）：学生
# 轴1（列）：考试
>>> import numpy as np
>>> grades = np.array([[ 0.79,  0.84,  0.84],
...                    [ 0.87,  0.93,  0.78],
...                    [ 0.77,  1.00,  0.87],
...                    [ 0.66,  0.75,  0.82],
...                    [ 0.84,  0.89,  0.76],
...                    [ 0.83,  0.71,  0.85]])
```

我们可能会想要知道每个成绩和该考试的平均成绩是如何对比的。根据上一节的讨论，我们可以轻松地计算每个考试的平均成绩（四舍五入到小数点后两位）：

```python
# 计算每个考试的平均值（四舍五入到小数点后两位）
>>> mean_exam_scores = grades.mean(axis=0)
>>> mean_exam_scores = np.round(mean_exam_scores, 2)
>>> mean_exam_scores
array([ 0.79,  0.85,  0.82])
```

`grades` 是一个形状为 (6,3) 的数组，而 `mean_exam_scores` 是一个形状为 (3,) 的数组，而我们想要计算每个考试成绩和对应平均值的差。我们的第一反应可能是迭代 `grades` 的每一行并减去对应的 `mean_exam_scores` 来计算差：
```python
# 使用for循环来计算成绩差。

# 形状 (6,3) 的数组，用来储存每个考试的（成绩-平均值）
score_offset = np.zeros_like(grades)

# 迭代 `grades` 的每一行
for n, scores_per_student in enumerate(grades):
    # `scores_per_student` 是一个形状为 (3,) 的单个学生的考试成绩数组
    # 这和 `mean_exam_scores` 相同，所以我们可以进行减法
    score_offset[n] = scores_per_student - mean_exam_scores
```

根据我们上一节对矢量化操作的讨论，你应该立刻因为进行数组算数代码中的for循环而感到作呕。都这样了干嘛不把我们的算盘和表格纸页拿出来。幸运的事，我们可以使用广播来简单，矢量化地计算这些差：
```python
# 使用广播来计算形状为 (6,3) 的数组和形状为 (3,) 的数组的差。
>>> score_offset = grades - mean_exam_scores
>>> score_offset
array([[ 0.  , -0.01,  0.02],
       [ 0.08,  0.08, -0.04],
       [-0.02,  0.15,  0.05],
       [-0.13, -0.1 ,  0.  ],
       [ 0.05,  0.04, -0.06],
       [ 0.04, -0.14,  0.03]])
```
<!-- #endregion -->

根据以上的广播规则，当你调用 `grades - mean_exam_scores` 时，NumPy会发现 `mean_exam_scores` 和 `grades` 的每一行有着相同的形状，因此它会 `grades` 的*每*一行减去  `mean_exam_scores`。也就是说，`mean_exam_scores` 的内容被*广播*来填满一个形状为 (6,3) 的数组，使得对应成员的减法可以进行。我们再次强调，NumPy并不会真的不必要的复制 `mean_exam_scores` 的数据，且我们理解广播的模型仅仅是为了表达这过程中进行的数学运算。


<div class="alert alert-info"> 

**阅读理解：基础广播**

使用 `np.random.rand` 生成一个成员为10,000个2维点的随机数组。计算这些点的“质量中心”（center of mass），也就是这10,000个点的平均x坐标和平均y坐标。然后，使用广播来计算储存这些点*相对于*质量中心的坐标的形状为 (10000,2) 的数组。比如说，如果质量中心是 $(0.5, 1)$，一个点的绝对坐标为 $(2, 3)$，那么那个点*相对于*质量中心的位置就是 $(2, 3) - (0.5, 1) = (1.5, 2)$

</div>

<!-- #region -->
## 大小为1的轴和 `newaxis` 对象 

### 向数组插入大小为1的维度
广播规则也有提到，大小为1的维度因为可以广播成任何大小是特殊的。我们将在这里学习如何向数组中插入大小为1的维度来满足广播的需求。

你可以在不改变数组总体大小（也就是数组中总共的成员数）的情况下插入大小为1的维度。所以我们可以随意通过 `reshape` 函数添加大小为1的维度。让我们将一个形状为 (3,) 的数组重塑成形状为 (1, 3, 1, 1) 的数组：
```python
>>> import numpy as np

# 通过重塑数组来插入大小为1的维度
# 在插入大小为1的维度前后数组的大小都是3.
>>> np.array([1, 2, 3]).reshape(1, 3, 1, 1)
array([[[[1]],

        [[2]],

        [[3]]]])
```

就像这样，一个有着3个成员的1维数组就被重塑成了4维数组：“一打三页，每页有着一行一列”。我们有另外一种插入大小为1的维度的方法。NumPy为此提供了 `newaxis`  对象。让我们立刻演示一下 `np.newaxis` 可以如何使用：
```python
# 演示 `numpy.newaxis` 对象得使用方法
>>> x= np.array([1, 2, 3])
>>> y= x[np.newaxis, :, np.newaxis, np.newaxis]
>>> y
array([[[[1]],

        [[2]],

        [[3]]]])
>>> y.shape
(1, 3, 1, 1)
```

将 `x` 索引为 `x[np.newaxis, :, np.newaxis, np.newaxis]` 会返回一个 `x` 作为在轴0，2，和3中插入了大小为1的维度4维数组的视阈。得到的数组不是 `x` 的复制品；它指向和 `x` 一样的内置数据，但它仅仅提供了一个不一样的索引框架。这和我们通过是重塑形状得到的结果没有任何区别：`x.reshape(1, 3, 1, 1)`。
<!-- #endregion -->

<!-- #region -->
### 使用大小为1的维度来进行广播
是时候回答一个更重要的问题了：我们在什么情况下会想要向数组中插入这些看起来没什么用的维度呢？让我们通过一个例子来了解大小为1的维度的作用。

假设我们想要将以下两个数组的所有可能的对组合相乘：`array([1, 2, 3])` 和 `array([4, 5, 6, 7])`。也就是说，我们想要进行12次乘法，并能够访问每个结果。第一眼看，形状为 (3,) 的数组和形状为 (4,) 的数组似乎广播不兼容；我们似乎一定要被迫像使一群史前穴居人一样用嵌套的for循环。幸运的是，我们可以通过聪明地使用大小为1的维度来矢量化地进行这个计算。

让我们向 `x` 插入大小为1的维度：

```python
# 向 `x` 和 `y` 插入大小为1的维度来准备广播
>>> x_1d = np.array([1, 2, 3]) 
>>> x = x_1d.reshape(3, 1)
>>> x
array([[1],
       [2],
       [3]])

>>> y = np.array([4, 5, 6, 7])
```

`x` 现在是一个形状为 (3, 1) 的数组，而 `y` 是一个形状为 (4,) 的数组。根据广播的规则，这两个数组现在广播兼容，并将乘出一个形状为 (3, 4) 的数组。让我们看看将这两个数组相乘是如何会返回我们想要的刚好12个数字的：

```python
# 广播相乘 `x` 和 `y`
>>> x * y
array([[ 4,  5,  6,  7],
       [ 8, 10, 12, 14],
       [12, 15, 18, 21]])
```
<!-- #endregion -->

\begin{equation}
\left(
\begin{array}{*{1}{X}}
  1 \\
  2 \\
  3
\end{array} \right)
% 
\cdot \left( \begin{array}{*{4}{X}}
  4 & 5 & 6 & 7
\end{array}\right)
% 
\rightarrow \left( \begin{array}{*{4}{X}}
  1 & 1 & 1 & 1 \\
  2 & 2 & 2 & 2 \\
  3 & 3 & 3 & 3
\end{array}\right)
%
\cdot \left( \begin{array}{*{4}{X}}
  4 & 5 & 6 & 7 \\
  4 & 5 & 6 & 7 \\
  4 & 5 & 6 & 7
\end{array}\right)
%
= \left( \begin{array}{*{4}{X}}
  1\cdot4 & 1\cdot5 & 1\cdot6 & 1\cdot7 \\
  2\cdot4 & 2\cdot5 & 2\cdot6 & 2\cdot7 \\
  3\cdot4 & 3\cdot5 & 3\cdot6 & 3\cdot7
\end{array}\right)
\
\end{equation}


请注意，返回数组的成员 `(i, j)` 对应着 `x_1d[i] * y[j]`。

通过简单的重塑来精明地插入大小为1的维度使得我们可以逼迫NumPy来进行我们想要的组合乘法。同时，我们对广播的准确理解让我们可以对广播运算的结果形状有着清晰的解释。也就是说，如果我将 `x` 重塑为形状是 $(M, 1)$ 的数组，`y` 的形状为 $(N,)$，那么（根据广播的规则）`x * y` 将会返回一个形状是 $(M, N)$ 的数组，其储存着每个 `x` $M$ 个成员之一和每个 `y` $N$ 个成员之一的乘积。

<!-- #region -->
<div class="alert alert-info"> 

**R阅读理解：基础广播II**

设以下形状为 (2,3,4) 的数组：
```python
>>> x = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7],
...                [ 8,  9, 10, 11]],
...
...               [[12, 13, 14, 15],
...                [16, 17, 18, 19],
...                [20, 21, 22, 23]]])
```

标准化 `x`，使其*每页中每行的数据的和都是1*。使用序列函数 `np.sum`，但你只需要调用它一次，并使用广播除法。

</div>
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info"> 

**阅读理解：基础广播III**

一张电子图片其实就是一个数字数组，其指示显示器的像素根据数组的值去发射具体颜色的光。

因此，一张RGB图片可以作为一个3维NumPy数组储存，其形状为 $(V, H, 3)$。$V$ 是竖向的像素数量，$H$ 是横向的像素数量，大小为3的维度储存了像素的红，蓝，绿颜色。因此，一个 $(32, 32, 3)$ 的数组将会是一张32x32的RGB图片。

我们经常会需要处理图片集。假设我们想要将N张图片储存为一个数组，那么我们可以将其当作一个形状为 $(N, V, H, 3)$ 的4维数组。方便起见，让我们先产生成员为随机数字的4维数组来代替真实的图片数据。我们将会生成500张48x48的RGB图片：

```python
>>> images = np.random.rand(500, 48, 48, 3)

```

使用序列函数 `np.max` 和广播，标准化 `images`，使其*每张图片的每个颜色频道*的最大值是1。

</div> 
<!-- #endregion -->

<!-- #region -->
## 广播的高级运用：距离对
我们将通过演示一个重要，相对复杂的数组广播例子来结束本节。在这个例子中，我们会发现最直接的广播方法*不*一定是我们问题的正确解决方法；我们也会注意到在广播之前首先重构数学公式的重要性。具体来讲，我们会注意到我们最初方法的内存使用效率低的问题。

假设我们有着两个2维数组。`x` 的形状为 $(M, D)$，而 `y` 的形状为 $(N, D)$。我们想要计算两个数组之间*每一对*行的欧氏距离（Euclidean distance，也叫 $L_2$-distance）。也就是说，如果 `x` 的任何一行数据为 $D$ 个数字 $(x_0, x_1, \ldots, x_{D-1})$，而 `y` 的一行数据为 $(y_0, y_1, \ldots, y_{D-1})$，且我们想要计算这两行之间的欧氏距离：

\begin{equation}
\sqrt{(x_{0} - y_{0})^2 + (x_{1} - y_{1})^2 + \ldots + (x_{D-1} - y_{D-1})^2} = \sqrt{\sum_{i=0}^{D-1}{(x_{i} - y_{i})^2}}
\end{equation}

为每一对行组合进行这个计算将会总共产生 $M\times N$ 个距离。之前一小节演示了一个很相似，但低一维度的计算。让我们通过以下三种方法进行这个计算吧：

1. 使用显式for循环
2. 直接地使用广播
3. 首先重构问题然后再使用广播

为了让例子更加实际，我们将会计算以下两个数组之间所有行对的欧氏距离：
```python
# 形状为 (5, 3) 的数组
>>> x = np.array([[ 8.54,  1.54,  8.12],
...               [ 3.13,  8.76,  5.29],
...               [ 7.73,  6.71,  1.31],
...               [ 6.44,  9.64,  8.44],
...               [ 7.27,  8.42,  5.27]])

# 形状为 (6, 3) 的数组
>>> y = np.array([[ 8.65,  0.27,  4.67],
...               [ 7.73,  7.26,  1.95],
...               [ 1.27,  7.27,  3.59],
...               [ 4.05,  5.16,  3.53],
...               [ 4.77,  6.48,  8.01],
...               [ 7.85,  6.68,  6.13]])

```

因此我们会为 `x` 和 `y` 的每一对行计算总共30个距离。
<!-- #endregion -->

<!-- #region -->
### 使用for循环计算距离对
使用for循环来计算距离如下：

```python
def pairwise_dists_looped(x, y):
    """ 使用for循环计算距离对

        Parameters
        ----------
        x : numpy.ndarray, shape=(M, D)
        y : numpy.ndarray, shape=(N, D)

        Returns
        -------
        numpy.ndarray, shape=(M, N)
            `x` 和 `y` 每一行对的欧氏距离。"""
    # `dists[i, j]` 将会存储 `x[i]` and `y[j]`
    # 的欧氏距离
    dists = np.empty((5, 6))

    for i, row_x in enumerate(x):     # 循环遍历 `x` 的行
        for j, row_y in enumerate(y): # 循环遍历 `y` 的行
            # 将两行相对应的成员相减，求每个差的平方，然后求平方差的和。
            # 这和之前给出的欧氏距离公式完全一样（我们会在之后再求
            # 平方根）
            dists[i, j] = np.sum((row_x - row_y)**2)

    # 我们还需要求所有数字的平方根
    return np.sqrt(dists)
```

请一步一步理解此代码，并注意到 `dists` 存储 `x` 和 `y` 每一对行的欧氏距离这一事实。
<!-- #endregion -->

<!-- #region -->
### 使用广播计算距离对（未优化）
现在，让我们使用矢量化来进行这个距离计算。首先，请注意，我们将要使用的方法对内存的运用是不高效的。我们将会在本小节结尾仔细地处理此问题。

我们首先将向 `x` 和 `y` 精明的地方插入大小为1的维度来让我们可以对它们长度为 $D$ 的行对进行 $M \times N$ 个减法。*这将创建一个形状为* $(M, N, D)$ *的数组*。

```python
# 将形状为 (5, 1, 3) 的数组和形状为 (1, 6, 3) 的数组相减
# 来产生形状为 (5, 6, 3) 的数组
>>> diffs = x.reshape(5, 1, 3) - y.reshape(1, 6, 3)
>>> diffs.shape
(5, 6, 3)
```

请注意，通过广播，`diffs[i, j]` 现在储存着 `x[i] - y[j]`。因此我们需要将 `diffs` 的每个成员平方，求每行列在最后一轴的和，并最后平方根所有数字来返回 $M \times N$ 个欧氏距离：

```python
# 创建欧氏距离
>>> dists = np.sqrt(np.sum(diffs**2, axis=2))
>>> dists.shape
(5, 6)
```

就这么简单！我们通过矢量化方法创建了这些距离。现在，让我们将其正式编为一个函数：

```python
def pairwise_dists_crude(x, y):
    """ 使用矢量化计算距离对

        Parameters
        ----------
        x : numpy.ndarray, shape=(M, D)
        y : numpy.ndarray, shape=(N, D)

        Returns
        -------
        numpy.ndarray, shape=(M, N)
            `x` 和 `y` 每一行对的欧氏距离。"""
    # 在这里使用的 `np.newaxis` 达到和我们之前运用 `reshape`
    # 函数一样的结果
    return np.sqrt(np.sum((x[:, np.newaxis] - y[np.newaxis])**2, axis=2))
```

可惜的是，我们这里进行的矢量化计算有一个明显的问题。请对比在for循环版本中创建的大小最大的数组和在这个矢量化计算中创建的最大数组。for循环版本仅仅需要创建一个形状为 $(M, N)$ 的数组，而矢量化版本会创建一个形状为 $(M, N, D)$ 的过渡数组（也就是 `diffs`）。这个过渡数组甚至会在函数中的一行代码中被创建。当 $D$ 是一个大数时这个过渡数组会非常大！

比如说，假设你在试图计算分辨率为  $32 \times 32$  的RGB图片之间的欧氏距离（来计算图片是否相似）。那么在这个情况下，每张图片被 $D = 32 \times 32 \times 3 = 3072$（$32^2$ 个像素，且每个像素有3个值：红，绿，蓝）个数字描述。那么计算一打5000张图片和一打100张图片之间的所有距离对会创建一个形状为 $(5000, 100, 3072)$ 的过渡数组。虽然这个大数组只会暂时存在，但是它会需要超过6GB的内存！for循环版本只需要 $\frac{1}{3027}$ 多的内存（大约2MB）。

是否已经木已成舟了？我们是否必须要在缓慢的for循环和内存使用效率低的矢量化中做选择呢？不！小孩子才做选择题，成年人当然是全都要！我们可以重构欧氏距离的数学公式来避免创建巨大的过渡数组。
<!-- #endregion -->

### 优化过的距离对
在之前的计算中，对 `x` 和 `y` 的对应行进行减法是创建过大过渡数组的罪魁祸首。因此，我们想要重写欧氏距离公式来使得没有任何项需要广播到大于 $M \times N$ 的数组。

暂时抛开平方根，欧氏距离公式的每一项平方差可以被重构为如下：

\begin{equation}
\sum_{i=0}^{D-1}{(x_{i} - y_{i})^2} = \sum_{i=0}^{D-1}{x_{i}^2} + \sum_{i=0}^{D-1}{y_{i}^2} - 2\sum_{i=0}^{D-1}{x_{i} y_{i}}
\end{equation}

请注意，我们必须为 `x` 和 `y` 的每一对行计算这个差。我们将会发现这个版本的公式允许我们使用矩阵乘法来避免创建形状为 $(M, N, D)$ 的过渡数组。

<!-- #region -->
这个公式的前两项可以很直接地去计算并相加，而仅仅会创建形状为 $(M, N)$ 的数组。我们平方 `x` 和 `y` 的每一项并在每一行求和：

```python
# 计算重构后的欧氏距离公式的前两项

# 创建一个形状为 (5,) 的数组
>>> x_sqrd_summed = np.sum(x**2, axis=1)

# 创建一个形状为 (6,) 的数组
>>> y_sqrd_summed = np.sum(y**2, axis=1)
```

我们需要向 `x` 插入一个大小为1的维度来让我们将相加形状为 $(M, 1)$ 和 $(N,)$ 数组的每一对数。这将为所有 $M \times N$ 个行对计算 $\sum_{i=0}^{D-1}{x_{i}^2} + \sum_{i=0}^{D-1}{y_{i}^2}$。

```python
# 将形状为 (5, 1) 和 (6, ) 的数组相加来创建形状为 (5, 6) 的数组
>>> x_y_sqrd = x_sqrd_summed[:, np.newaxis] + y_sqrd_summed
>>> x_y_sqrd.shape
(5, 6)
```

现在，我们只剩下第三项需要计算。我们留给读者来证明计算 `x` 和 `y` 每一行对的乘积的和等值于矩阵乘法 $-2\;(x \cdot y^{T})$，其中 `y` 被转制为形状 $(D, N)$。形状为 $(M, D)$ 的数组和形状为 $(D, N)$ 的数组的矩阵乘法将创建一个形状为 $(M, N)$ 的数组。因此，我们可以在不创建更大的过渡数组的情况下计算这最后一项。

因此，我们所有 $M \times N$ 个行对的公式第三项 $-2\sum_{i=0}^{D-1}{x_{i} y_{i}}$ 为：

```python
# 计算所有行对的距离公式第三项
>>> x_y_prod = -2 * np.matmul(x, y.T)  # 你也可以使用 `np.dot` 来达成一样的效果
>>> x_y_prod.shape
(5, 6)
```

在计算完全部三项后，我们终于可以计算欧氏距离了：

```python
# 计算所有距离
>>> dists = np.sqrt(x_y_sqrd + x_y_prod)
>>> dists.shape
(5, 6)
```
总结一下，我们成功地使用了矢量化方法来计算所有距离对，而过程中仅仅使用了形状为 $(M, N)$ 的数组！这方法既高效地使用了内存，又是矢量化版本——理想世界不过如此。让我们编写完整进行以上计算的函数。

```python
def pairwise_dists(x, y):
    """ 使用省内存的矢量化方法计算距离对。

        Parameters
        ----------
        x : numpy.ndarray, shape=(M, D)
        y : numpy.ndarray, shape=(N, D)

        Returns
        -------
        numpy.ndarray, shape=(M, N)
            `x` 和 `y` 每一行对的欧氏距离。"""
    dists = -2 * np.matmul(x, y.T)
    dists +=  np.sum(x**2, axis=1)[:, np.newaxis]
    dists += np.sum(y**2, axis=1)
    return  np.sqrt(dists)
```


<!-- #endregion -->

<div class="alert alert-info">

**经验**：

公式的具体格式可能会对NumPy矢量化的实现的内存使用量有着巨大的影响。在你可以确定数组形状不会导致明显的内存使用时可以安全地无视这个问题。但如果你不能确定，那么花些时间思考一下公式的格式来看看你是否能够重写它来减缓内存使用的瓶颈。

</div>


<div class="alert alert-info"> 

**阅读理解：检查三个距离对公式是否等值**

使用函数[numpy.allclose](https://docs.scipy.org/doc/numpy/reference/generated/numpy.allclose.html)来检查以上三种计算距离对的公式返回的结果是一样的。

</div>


## 官方说明文档链接

- [广播基础](https://docs.scipy.org/doc/numpy/user/basics.broadcasting.html#broadcasting)
- [广播常规](https://docs.scipy.org/doc/numpy/reference/routines.array-manipulation.html#changing-number-of-dimensions)


## 阅读理解答案

**广播兼容性：解**

1\. 不兼容

2\. `3 x 4`

3\. `8 x 3 x 1`

4\. `9 x 2 x 5`

5\. 不兼容

<!-- #region -->
**基础广播：解**

生成成员为10,000个2维点的数组并计算它们的“质量中心”。
```python
# 计算10000个点的平均x和y坐标
>>> pts = np.random.rand(10000, 2)
>>> center_of_mass = pts.mean(axis=0)  # -> array([mean_x, mean_y])
>>> center_of_mass.shape
(2,)

# 使用广播来计算每个点相对质量中心的坐标。通过广播减法来为所有10000个点
# 减去质量中心的坐标
>>> relative_pos = pts - center_of_mass # shape-(10000,2) w/ shape-(2,)
>>> relative_pos.shape
(10000, 2)
```
<!-- #endregion -->



<!-- #region -->
**基础广播II：解**

标准化 `x`，使其*每页中每行的数据的和都是1*。使用序列函数 `np.sum`，但你只需要调用它一次，并使用广播除法。

```python
# 形状为 (2, 3, 4) 的数组
>>> x = np.array([[[ 0,  1,  2,  3],
...                [ 4,  5,  6,  7],
...                [ 8,  9, 10, 11]],
...
...               [[12, 13, 14, 15],
...                [16, 17, 18, 19],
...                [20, 21, 22, 23]]])

# 求每一页三行中的和
>>> summed_rows = x.sum(axis=2)
>>> summed_rows
array([[ 6, 22, 38],
       [54, 70, 86]])

# 这个形状为 (2, 3) 的数组可以广播来于 `x` 的每一页和行相减。
# 我们只需要在和数组的维度2中插入一个大小为1的轴来代替之前的行就行
>>> x_norm = x / summed_rows[:, :, np.newaxis]

# 检查答案
>>> x_norm.sum(axis=2)
array([[1., 1., 1.],
       [1., 1., 1.]])
```
<!-- #endregion -->

<!-- #region -->
**基础广播III：解**

```python
# 500张48x48RGB图的集合
>>> images = np.random.rand(500, 48, 48, 3)

# 计算每张图每个颜色频道的最大值
>>> max_vals = images.max(axis=(1,2))
>>> max_vals.shape
(500, 3)

# 我们可以通过插入大小为1的维度来广播相除这些最大值
# 和图片中的像素。
# 为相除形状 (500, 48, 48, 3) 将最大值数组广播为形状 (500, 1, 1, 3)
>>> normed_images = images / max_vals.reshape(500, 1, 1, 3)

# checking that all the max-values are 1
>>> normed_images.max(axis=(1,2))
array([[ 1.,  1.,  1.],
       [ 1.,  1.,  1.],
       [ 1.,  1.,  1.],
       ..., 
       [ 1.,  1.,  1.],
       [ 1.,  1.,  1.],
       [ 1.,  1.,  1.]])

# 严格的检查
>>> np.all(normed_images.max(axis=(1,2)) == 1)
True
```
<!-- #endregion -->

<!-- #region -->
**检查三个距离对公式是否等值：解**

`numpy.allclose` 当两个数组所有对应的成员基本相同时返回 `True`。

```python
>>> x = np.array([[ 8.54,  1.54,  8.12],
...               [ 3.13,  8.76,  5.29],
...               [ 7.73,  6.71,  1.31],
...               [ 6.44,  9.64,  8.44],
...               [ 7.27,  8.42,  5.27]])

>>> y = np.array([[ 8.65,  0.27,  4.67],
...               [ 7.73,  7.26,  1.95],
...               [ 1.27,  7.27,  3.59],
...               [ 4.05,  5.16,  3.53],
...               [ 4.77,  6.48,  8.01],
...               [ 7.85,  6.68,  6.13]])

>>> np.allclose(pairwise_dists_looped(x, y), pairwise_dists_crude(x, y))
True

>>> np.allclose(pairwise_dists_crude(x, y), pairwise_dists(x, y))
True
```
<!-- #endregion -->
