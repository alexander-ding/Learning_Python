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
   :description: Topic: Class inheritance, Difficulty: Easy, Category: Section
   :keywords: inherit, object oriented, overwrite, sub class, issubclass
```

<!-- #region -->
# 继承
我们对面向对象编程的讨论的最后一个话题就是继承（inheritance）的概念。使用继承将提供强大的抽象概念和优雅的代码重运用——它允许某类继承另外一类的属性并以其为基础增加额外的功能。

让我们立刻考虑一个使用继承的例子。让我们重新讨论在本模组介绍中编写的 `Rectangle` 类。

```python
class Rectangle:
    """ 一类描述长方形特征的Python对象"""
    def __init__(self, width, height, center=(0, 0)):
        self.width = width    
        self.height = height  
        self.center = center

    def __repr__(self):
        return "Rectangle(width={w}, height={h}, center={c})".format(h=self.height,
                                                                     w=self.width,
                                                                     c=self.center)

    def compute_area(self):
        return self.width * self.height
```

现在，假设我们也想编写一个 `Square` 类，使得用户只需提供一个变长就能决定其大小。注意，正方形只是一种特殊的长方形而已——它的长和宽相同。在注意到这一点后，我们应该利用为 `Rectangle` 编写的代码。我们可以定义一个为 `Rectangle` *子类*（subclass）的 `Square` 类。这意味着 `Square` 将会*继承* `Rectangle` 的所有属性，包括它的方法。让我们来编写这个子类：

```python
# 创建 Square，Rectangle 的子类
class Square(Rectangle):
    def __init__(self, side, center=(0, 0)):
        # 等值于 `Rectangle.__init__(self, side, side, center)`
        super().__init__(side, side, center)
```

编写 `class Square(Rectangle)` 将会标示 `Square` 是 `Rectangle` 的子类，并因此会使其继承 `Rectangle` 的属性。接下来，请注意，我们重写了 `Square` 继承的 `__init__`；与其接受一个高和宽，`Square` 应该通过提供单个边长描述。在这个新的 `__init__` 方法中，我们将收到的单个边长为左 `Rectangle.__init__` 中的长和宽输入。`super` 永远引用着某一类的“父类”或“超类”（super class），因此 `super` 在这里指 `Rectangle`。

定义完我们的子类后，我们可以直接使用 `Rectangle` 的其它方法。让我们看看 `Square` 是如何工作的：

```python
# 创建一个边长为 2 的正方形
>>> my_square = Square(2)

# 使用继承的 `get_area` 方法
>>> my_square.get_area()
4

# 正方形是一个有着相同长宽的长方形
>>> my_square
Rectangle(width=2, height=2, center=(0.0, 0.0))

>>> my_square.width == my_square.height
True
```

<!-- #endregion -->

<!-- #region -->
内置的 `issubclass` 函数允许我们确定 `Square` 和 `Rectangle` 之间的关系。

```python
# `Square` 和 `Rectangle` 是不同的类
>>> Square is not Rectangle
True

# `Square` 是 `Rectangle` 的子类
>>> issubclass(Square, Rectangle)
True

# `my_square` 同时是 `Square` 和 `Rectangle` 实例
>>> isinstance(my_square, Square)
True

>>> isinstance(my_square, Rectangle)
True
```
<!-- #endregion -->

## 继承总结

<!-- #region -->
一般而言，如果你有类 `A`，那么你可以通过以下定义子类 `A`：

```python

class A:
    attr = 0
    
    def method(self):
        return 0
    
# `B` 是 `A` 的子类
class B(A):
    # 继承 `attr` 和 `method`
    b_attr = -2  # 和 `B` 不同的类属性
    
    def method(self):
        # 重写继承的 `method`
        return -1
```

`B` 将会继承 `A` 的所有属性和方法。在 `B` 中定义属性和方法将重写 `A` 中已经存在的同名属性。`B` 也可以随意定义属于它自己的不同属性和方法，并和 `A` 无关。

```python
>>> issubclass(B, A)
True

>>> A.attr
0

>>> A().method()
0

>>> B.attr
0

>>> B().method()
-1

>>> B.b_attr
-2
```
<!-- #endregion -->

我们仅仅摸到了类继承这一话题的皮毛。虽然如此，本节向读者展示了类继承的基本功能和用法。


## 官方说明文档链接

- [官方教程：继承](https://docs.python.org/3/tutorial/classes.html#inheritance)
