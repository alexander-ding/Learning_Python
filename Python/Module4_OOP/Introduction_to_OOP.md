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
   :description: Topic: Introducing object oriented programming in python, Difficulty: Easy, Category: Section
   :keywords: class, type, creation, definition, intro, overview, basics, meaning
```

<!-- #region -->
# 面向对象编程简介

我们首次对Python基础的探索向我们介绍了[基础对象类型：数字，字符串，和列表](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html)。相似的，我们对NumPy的讨论也是围绕着[N维数组](http://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/IntroducingTheNDarray.html)展开的。这些对象类型之间很大的区别和它们绑定的不同函数。绑定（bound）到数组上的函数叫做**方法**（method）。比如说，字符串有着多个旨在操作其储存的字符序列的方法，而NumPy数组有着很多操作数组内置数字数据的方法。

```python
# 不同类型的对象可以储存不同的方法

>>> string = "hello world"
>>> string.capitalize() # 使用字符串方法 `capitalize`
'Hello world'

>>> import numpy as np
>>> array = np.array([[0, 1, 2],
...                   [3, 4, 5]])
>>> array.sum()  # 使用数组方法 `sum`
15
```
<!-- #endregion -->

<!-- #region -->
更加广义来讲，对象可以拥有总结关于对象信息的**属性**（attribute）。比如说，数组属性 `ndim` 和 `shape` 提供了关于数组数字数据索引布局的信息。

```python
# 访问对象的属性
>>> array.ndim
2
>>> array.shape
(2, 3)
```
<!-- #endregion -->

<!-- #region -->
在本模组中，我们将学习如何定义我们自己的，可自定义的，有着不同集合的属性和方法的对象类型。通过这么做·，我们将将Python作为一个“面对对象”（object-oriented）编程语言使用；这将大幅提升我们作为Python用户的能力并让我们更加深入地理解Python这门语言。

作为一个预览，让我们创建我们自己的名为 `Rectangle` 的类（class）：

```python
class Rectangle:
    """ 一个描述长方形特性的Python对象 """
    def __init__(self, width, height, center=(0.0, 0.0)):
        """ 设置某个 `Rectangle` 实例的属性。

            Parameters
            ----------
            width : float
                这个长方形实例的x长度。

            height : float
                这个长方形实例的y长度。

            center : Tuple[float, float], optional (default=(0, 0))
                这个长方形的中心的 (x, y) 位置"""
        self.width = width    
        self.height = height  
        self.center = center
    
    def __repr__(self):
        """ 返回在打印某个长方形时使用的字符串。"""
        return "Rectangle(width={w}, height={h}, center={c})".format(h=self.height,
                                                                     w=self.width,
                                                                     c=self.center)

    def compute_area(self):
        """ 返回长方形的面积

            Returns
            -------
            float"""
        return self.width * self.height

    def compute_corners(self):
        """ 计算这个长方形的四角的 (x, y) 位置，首先返回“左上”角，然后顺时针继续。

            Returns
            -------
            List[Tuple[float, float], Tuple[float, float], Tuple[float, float], Tuple[float, float]]"""
        cx, cy = self.center
        dx = self.width / 2.0
        dy = self.height / 2.0
        return [(cx + x, cy + y) for x,y in ((dx, dy), (dx, -dy), (-dx, -dy), (-dx, dy))]
```

这个 `Rectangle` 类的一个实例（instance）是一个*属性*包括长度，高度，和中心坐标的单个长方形。同时，我们可以使用这个长方形的*方法*（它为函数的属性）来计算它的面积和四角的坐标。
<!-- #endregion -->

<!-- #region -->
```python
# 创建一个长度为4，高度为0，中心为 (0, 0) 的长方形
# 这将执行 __init__ 方法并设置它的width/height/center属性
>>> rect1 = Rectangle(4, 10)  

# __repr__ 方法定义了长方形实例在命令行中将如何显示
>>> rect1  
Rectangle(width=4, height=10, center=(0, 0))

# 计算这个长方形的面积
>>> rect1.compute_area()
40

# 计算这个长方形的四角坐标
>>> rect1.compute_corners()
[(2.0, 5.0), (2.0, -5.0), (-2.0, -5.0), (-2.0, 5.0)]
```
<!-- #endregion -->

就像我们遭遇过的任何其它Python对象一样，我们可以将我们的 `Rectangle` 实例放在字符串中，存储为词典的值，作为参数输入到函数中，使用多个变量引用它们，等等。

流行的科学，数据分析，和机器学习Python模组都很依赖定义Python自定义类的能力。比如说，[pandas](https://pandas.pydata.org/)定义了一个类似表格的 `DataFrame` 类；[PyTorch](https://pytorch.org/)，[MXNet](https://mxnet.incubator.apache.org/)，和[TensorFlow](https://www.tensorflow.org/)都定义了能够[自动求导数](https://en.wikipedia.org/wiki/Automatic_differentiation)的张量类；这对训练神经网络极其重要。理解Python的类系统会将大幅提升你使用像以上这些的模组（无耻广告：如果你对自动求导的进行纯Python/NumPy实现的模组感兴趣，请查阅[MyGrad](https://mygrad.readthedocs.io)模组——译者注：这是作者自己写的模组，很厉害的哦！）。

在本模组，我们将会讨论*类定义*的基础；这将允许我们定义自己的对象类（class）（也就是类型（type））。然后，我们将学习如何创建某个对象类型的不同的*实例*并定义方法。这将导致我们初次见到*特殊方法*；它们将允许我们修改我们对象类型在配合各种Python操作符时的行为。比如说，我们可以定义 `+` 操作符会如何和我们的对象互动。最后，我们将快速地讨论类继承（class inheritance）的概念。

<div class="alert alert-info">

**经验**：

本模组旨在理解如何定义和使用我们自己的Python对象类。这将极大地成熟化我们对Python作为一门面向对象语言的理解并提升我们完整使用Python所有功能的能力。 

</div>

## 类vs类型：关于术语的重要注释
在我们深入讨论之前，我们应该花一些时间来了解术语“类型”（type）和“类”（class）在Python的实际操作中是同义的。到现在为止，我们仅仅使用过术语“类型”来将对象互相区分。比如说，`1` 术语类型 `int`，而 `"cat"` 术语类型 `str`。但是，我们将马上学习用来创造新类型对象的*类*定义并学习如 `issubclass` 的函数。虽然如此，请记住，*类*和*类型*指的是同一个东西！存在两个不同术语的历史遗留，但是[Python 2.2之后](https://www.python.org/download/releases/2.2/descrintro/)类型和类的概念被完全统一了。

在实际操作中，人们一般使用“类型”一词来指内置的类型（如 `int` 和 `str`），而“类”则指用户定义的类型。最后重申一遍，在Pythond的现代版本中，这两次并没有任何实际的区别。


<div class="alert alert-info">

**经验**：

 两个术语“类型”和“类”是同义的；它们都指着对某一类型/类的Python对象的包装定义。虽然它们在Python语言中不能互换——我们将编写类定义，而不是类型定义，而我们将使用 `type` 而不是 `class` 来检查对象——但这些区别仅仅是早期Python版本的遗留物而已。

</div>


## 官方说明文档链接

- [Python教程：类的初体验](https://docs.python.org/3/tutorial/classes.html#a-first-look-at-classes)
