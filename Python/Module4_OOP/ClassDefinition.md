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
   :description: Topic: The rules for defining classes in python, Difficulty: Easy, Category: Section
   :keywords: class definition, scope, class object, attribute, method
<!-- #endraw -->


<!-- #region -->
# 定义新对象类

本节将介绍定义新Python对象类（也就是类型）的基础语法。请回忆，`def` 关键词用来标示函数的定义。相似的，`class` 关键词用来标示类定义的开始。类定义的主体部分，也就是 `class` 语句下方的锁进区域，则用来定义类的各种**属性**。

以下定义了一类新的对象，名叫 `MyGuy`，并提供4个属性 `x`，`y`，`z`，和 `f`

```python
# 定义一类/类型新的对象
class MyGuy:
    x = 1 + 2
    y = [2, 4, 6]
    z = "hi"
    
    def f():
        return 3

# 离开锁进区域将终止类定义
```

当这个新对象类的定义被执行后，你可以在代码中引用该对象。在这里，我们将访问 `MyGuy` 的各种属性。

```python
>>> MyGuy.x
3

>>> MyGuy.y
[2, 4, 6]

>>> MyGuy.z
"hi"

>>> MyGuy.f
<function __main__.MyGuy.f>
```

请注意，所有的属性都可以通过“点”语法访问：`object.attribute_name`。属性 `f` 是一个函数，所以我们可以调用它来让它如预期一般执行：

```python
# 调用属性 f
>>> MyGuy.f()
3
```

同时是函数的对象属性被称为**方法**。所以，`f` 是 `MyGuy` 的一个方法。

`MyGuy` 是单个包含我们类定义的类对象。它类似于 `list`，`str`，和 `int`。我们将使用 `MyGuy` 来创建为我们类*实例*的对象，就像 `"cat"` 是 `str` 的实例一样。我们将在之后对此深入讨论。
<!-- #endregion -->

<div class="alert alert-info">

**经验**：

`class` 表达式标示了新对象类的定义，而定义类代表着定义类的属性。一个属性可以向该类“绑定”其它Python对象（整数，字符串，列表，等等），包括函数。为函数的属性叫做*方法*。语法 `obj.attr` 是用来从名为 `obj` 的对象“获取”名为 `attr` 的属性的点语法。

</div>

<!-- #region -->
## 类定义的一般形式
类定义的一般形式就是属性定义——要么是变量赋值或函数定义——的集合。这将创建一类新的对象并提供它的属性和方法：

```python
class ClassName:
    """ 类的说明字符串（docstring）"""
    <statement-1>
    .
    .
    .
    <statement-N>
```

每个 `<statement-j>` 为该类对象定义一个属性（例：`z = "hi"` 定义属性 `z`，而函数定义则创建一个方法）。

就像函数定义一样，类定义在可以包含基本任何Python代码，且其定义有着属于自己的[作用域](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Scope.html)；但是，*任何*在类定义中赋值的变量都可以作为属性来调用。

```python
# 任何在类定义中赋值的变量将作为对象类的属性可用。
# 就算是在for循环中定义的变量也将成为类的属性。

class Dummy:
    cnt = 0
    
    for i in range(5, 11):
        # i = 5
        # i = 6
        # ...
        # i = 10
        cnt += i
    
    # 最后一次迭代赋值了 i = 10
    # 因此 i 是 Dummy 的属性之一，其值为 10
```

```python
>>> Dummy.cnt  # cnt = 0 + 5 + 6 + 7 + 8 + 9 + 10
45

>>> Dummy.i
10
```
<!-- #endregion -->

<div class="alert alert-warning">

**命名对象类**：

命名新对象类/类型的传统是使用“驼峰式大小写风格“（camel-casing）。因此如果我想叫我的对象类”pizza shop“，那么我会使用名字 `PizzaShop`。这和使用小写字母和下划线替代空格的蛇大小写风格（snake-case）变量名，函数名，和类对象的*实例*（即将介绍）的名字不同。

译者注：camel-case直接将多个单词首字母大写拼接在一起而不使用空格，而snake-case用下划线串联多个小写单词。如：pizza shop的camel case是PizzaShop，而它的snake case是pizza_shop。

</div>


<div class="alert alert-info">

**阅读理解：创建你自己的对象类**

创建名为 `Dog` 对象类的定义。这个类应该有两个属性：“name”和“speak”。“name”属性应该向对象绑定一个字符串（也就是狗的名字）。“speak”属性应该是一额*方法*，其接受一个字符串并返回该字符串在两边添加了 `"*woof*"` 的版本（如 `"hello"` -> `"*woof* hello *woof*"`）。

</div>

<!-- #region -->
## 操作对象属性
试图访问对象未定义的属性将会返回一个 `AttributeError`：

```python
>>> MyGuy.apple
AttributeError: type object 'MyGuy' has no attribute 'apple'
```

我们可以使用内置的函数 `hasattr` 来检查对象是否有某个属性：

```python
# 演示 `hasattr`
>>> hasattr(MyGuy, "apple")  # MyGuy.apple 未定义
False

>>> hasattr(MyGuy, "x")      # MyGuy.x 已定义
True
```

除了使用点语法来访问属性之外，内置的 `getattr` 函数也可以用在达成一样的结果：

```python
# 演示 `getattr`
>>> MyGuy.x
3

>>> getattr(MyGuy, "x")
3
```

你可能会惊讶于你可以在对象类定义*之后*向其绑定（或“设置”（set））新属性的事实。你可以通过内置函数 `setattr` 达到这个目的：

```python
# 使用 `setattr` 来将属性 `apple` 绑定到 `MyGuy` 上
>>> hasattr(MyGuy, "apple")  # MyGuy.apple 未定义
False

>>> setattr(MyGuy, "apple", "red")
>>> MyGuy.apple
'red'
```

你可以使用简单的赋值语法更加不正式地定义/设置属性：

```python
>>> hasattr(MyGuy, "grape")  # MyGuy.grape 未定义
False

# 向 `MyGuy` 设置  `grape`
>>> MyGuy.grape = "purple"  # 定义且设置属性 'grape'
>>> MyGuy.grape
'purple'

>>> MyGuy.x = -1  # 为属性 'x' 设置一个新值
>>> MyGuy.x
-1
```

因为你可以如此随意地向对象设置属性，类定义可能看起来仅仅只是一个正式仪式了。虽然Python为支持这种很随意的代码而出名，但是请注意在对象类定义区域外面创建新属性一般来说是不好的形式。
<!-- #endregion -->

<div class="alert alert-info">

**经验**：

`hasattr`，`getattr`，和 `setattr` 是支持我们通过属性名字来分别检查属性是否存在，访问属性值，和设置属性值的内置函数。Python对象十分灵活，因为它们支持在类定义区域外面创建属性，但是我们应该文明做人，并在可能的情况下将类定义的区域作为正式的合约看待。

</div>


## 官方说明文档链接

- [Python教程：类对象](https://docs.python.org/3/tutorial/classes.html#class-objects)


## 阅读理解答案：

<!-- #region -->
**创建你自己的对象类：解**

创建名为 `Dog` 对象类的定义。这个类应该有两个属性：“name”和“speak”。“name”属性应该向对象绑定一个字符串（也就是狗的名字）。“speak”属性应该是一额*方法*，其接受一个字符串并返回该字符串在两边添加了 `"*woof*"` 的版本（如 `"hello"` -> `"*woof* hello *woof*"`）。

```python
class Dog:
    name = "Charlie"
    
    def speak(input_string):
        return "*woof* " + input_string + " *woof*"
```
<!-- #endregion -->
