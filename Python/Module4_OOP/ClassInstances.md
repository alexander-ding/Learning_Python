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
   :description: Topic: Class instances versus objects, Difficulty: Medium, Category: Section
   :keywords: instance, class creation, init, self, isinstance
<!-- #endraw -->

<!-- #region -->
# 类实例

到现在为止，我们学习了定义新对象类，提供其名字，属性，和方法（也就是函数属性）的语法。当我们离开类定义的作用域外，一个类对象将被创建——定义创建的*类对象*（class object）是单个包装我们类定义的对象。我们很少会想要在类对象创建后将其作为参数输入或操作它。反而，我们将想要使用它来创建该类的一个个*实例*（instance）。具体来讲，`list` 是一个类对象（请记住，“类”和“类型”是一个意思）——它和 `class` 定义执行后创建的对象是同一种类的。就如你在[模组2](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Lists)中看到的一样，我们可以使用这个类对象来创建列表类单独的*实例*，而每个实例有着属于自己的对象序列。

```python
# 使用类对象 `list` 来创建列表实例
>>> list()  # 创建一个空的 `list` 实例
[]

>>> list((1, 2, 3))  # 创建一个包括1，2，和3的 `list` 实例
[1, 2, 3]

# `a` 和 `b` 是两个不同的列表类/类型实例，虽然它们储存着相同的
# 整数序列
>>> a = list((1, 2, 3))
>>> b = list((1, 2, 3))
>>> a is b
False

>>> isinstance(a, list)
True

>>> isinstance(b, list)
True

# 对 `a` 调用append方法只会影响该列表实例。
>>> a.append(-1)
>>> a
[1, 2, 3, -1]

>>> b
[1, 2, 3]
```

以上每一个实例都共享着属性 `append`，`count`，`reverse`，等等，如被 `list` 类对象包装的列表类定义描述得一般。虽然如此，每个列表的内容是一个属于该列表实例的属性；也就是说，列表的成员是一个*实例属性*（instance attribute）而不是一个类属性（class attribute）。到现在为止，我们还没有办法创建实例级别的属性。让我们改变这一事实。

假设我们想要创建自己的 `Person` 类。每个人都应该有着自己的名字，所以名字应该是实例属性。我们将学习如何定义一个特殊的初始化方法来允许我们定义并设置实例属性。在 `Person` 的情况下，这将允许我们为每一个人提供自己的名字：

```python
>>> class Person:
...    def __init__(self, name):
...        self.name = name

>>> emmy = Person("Emmy")
>>> niels = Person("Niels")

>>> emmy.name
'Emmy'

>>> niels.name
'Niels'

>>> isinstance(emmy, Person)
True
```

<!-- #endregion -->

我们接下来会马上学习 `__init__` 方法和奇怪的 `self` 参数。首先，我们将学习如何使用类对象来创建实例对象。

<!-- #region -->
## 对象身份和创建实例

我们将在这里学习创建对象实例的基本方法。设以下简单的类定义：

```python
class Dummy:
    x = 1
```

我们可以对 `Dummy` 使用“调用”语法 `Dummy()` 来创建该类的单独实例：

```python
# 创建一个为 Dummy 类实例的对象
>>> d1 = Dummy()

>>> isinstance(d1, Dummy)
True
```

请回忆，`is` 操作符检查两个变量是否引用了电脑内存中一样的对象。同时，请回忆，内置的 `isinstance` 函数会检查一个对象是否是某一类/类型的实例。这些函数将帮助我们理解类对象，类实例，和对象引用之间的关系。

```python
# `Dummy` 是一个包含我们类定义的类对象
>>> Dummy
__main__.Dummy

# `d1` 是我们 Dummy 类的一个实例。
# 这个实例存在于某内存地址中（0x2ae8f68f2e8）
>>> d1
<__main__.Dummy at 0x2ae8f68f2e8>

# d1 不是 Dummy；它是 Dummy 的一个实例
>>> d1 is Dummy 
False

>>> isinstance(d1, Dummy)
True
```
请注意，`Dummy` 和 `d1` 的关系就像 `list` 和 `[1, 4, "a"]` 的关系一样。

让我们创建另外一额 `Dummy` 的实例。理解这个新实例和之前创建的实例是*不同的*很重要。

```python
# `d2` 是我们 Dummy 类的一个新实例。
# 它存在于某内存地址中（0x2ae8f666f60）
>>> d2 = Dummy()
>>> d2
<__main__.Dummy at 0x2ae8f666f60>

>>> d2 is d1  # `d2` 和 `d1` 是 `Dummy` 的不同实例
False

>>> isinstance(d2, Dummy)
True
```

Python的[变量引用对象的规则](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Variables_and_Assignment.html)在此依然成立：向某个变量赋值一个对象，不管是类对象还是其实例，都不会创建该对象的复制品。这个变量仅仅会引用该对象并作为它的一个代名词。

```python
# `A` 引用 `Dummy`
>>> A = Dummy

>>> A is Dummy
True

# 使用 `A` 来创建 `Dummy` 的实例
>>> dummy_instance = A()  
>>> dummy_instance 
<__main__.Dummy at 0x2ae8f65fcf8>

>>> isinstance(dummy_instance, A)  # 等值于：`isinstance(dummy_instance, Dummy)`
True

# `var` 引用 Dummy 的实例 `dummy_instance`
>>> var = dummy_instance

>>> var is dummy_instance
True

# 向 `var.x` 设置一个新值等值于向 `dummy_instance.x`
# 设置一个新值
>>> var.x = 22
>>> dummy_instance.x
22

```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：类初始化**

使用以上定义的 `Dummy` 类，创建一个包含10个这一类的*不同*实例的列表。编写代码来显式检查每一个成员互不相同，以及每个成员都是 `Dummy` 类的实例。

然后，创建一个包含*一个* `Dummy` 实例被分别储存10次的元组。编写代码来显式检查每一个成员都引用相同的对象，以及每个成员都是 `Dummy` 类的实例。

</div>

<!-- #region -->
<div class="alert alert-info">

**阅读理解：术语**

设：
```python
>>> class Cat:
...     pass

>>> x = Cat
>>> y = Cat()
>>> x1 = x
>>> x2 = x()
>>> y1 = y
```

`x` 和 `Cat` 共享着什么关系？

`y` 和 `Cat` 共享着什么关系？

`x` 和 `y` 共享着什么关系？

`x2` 和 `y` 共享着什么关系？

`y` 和 `y1` 共享着什么关系？
<!-- #endregion -->

接下来，指出以下每个对象是类对象还是类实例（如果是实例，是哪个类的实例）？

 - `"hello world"`
 - `True`
 - `int`
 - `{"a" : 22}`
 - `tuple`

</div>


在学习了如何创建类实例，理解类对象和实例的关系，并理解了不同实例之间的区别后，我们可以学习如何为我们的类创建实例级别的属性了。

<!-- #region -->
## 定义实例级别的属性：`__init__` 方法

就像这一节之前用 `Person` 类和本模组简介节定义的 `Rectangle` 类演示的一样，Python提供了一个特殊方法 `__init__` 来允许我们定义类的实例级别的属性。这是一个极其重要的，我们会经常使用的方法。请注意这个方法的名字是：“下划线-下划线-init-下划线-下划线”，而你可以将其读作“dunder-init”（“dunder”代表着“double-underscore”）。

设以下稍微修改过的 `Person` 定义，其同时包含了类属性 `x`：

```python
class Person:
    x = 1  # 这设置了一个类级别的属性，通用于所有 `Person` 实例
    
    def __init__(self, name):
        """ 本方法在每次创建一个新 `Person` 实例时将被执行。
            `self` 指向被创建的对象实例。"""
        self.name = name   # 为 Person 实例 `self` 设置属性 `name`
        
        # __init__ 不能反悔任何除了 `None` 以外的对象。它的唯一目的是修改
        # `self`，也就是被创建的 `Person` 实例。
```

调用 `Person()` 其实在“幕后”调用 `__init__()`，且任何输入到 `Person()` 中的参数会被送到 `__init__` 中。乍一眼看 `__init__` 的定义，我们好像需要向这个方法输入两个参数：`self` 和 `name`。第一个参数 `self` 其实代表着正在被创建的 `Person` 对象实例。Python会自动向 `__init__` 的 `self` 输入相应对象，所以我们只需要担心向其输入 `name` 的值。

让我们创建 `Person` 类的一个实例，并使用字符串 `"Kamasi"` 来作为名字：

```python
# 创建实例 `self`，将其和 `"Kamasi"` 输入到 `Person.__init__` 中，
# 然后返回被创建的实例对象
>>> p = Person("Kamasi")  

>>> p.name  # 访问实例属性 `name`
'Kamasi'
>>> p.x     # 访问类属性 `x`
1
```

当我们创建 `Person` 的一个实例时，以下是“幕后”发生的事情（**这很重要**）：

- 调用 `Person("Kamasi")` 首先会假装 `__init__` 方法不存在一样创建 `Person` 的一个实例。返回的对象在这时并没有 `name` 属性。它只拥有类级别的属性 `x`。
- 然后，`Person` 的那个实例会作为参数 `self` 被输入到 `__init__` 中，而我们显式提供的 `"Kamasi"` 将会作为参数 `name` 被输入。
- 有了这些参数后，`Person.__init__(self, "Kamasi")` 将会执行它的主体代码。具体来讲，`self.name = name` 将使用值 `"Kamasi"` 为 `self` 设置属性。
- 在执行完 `__init__` 方法后，`Person("Kamasi")` 会返回被创建的实例对象。

我们现在有了在实例级别上定义和设置属性的能力！理解这个过程对掌握Python的面向对象编程至关重要。让我们创建多个 `Person` 实例并将它们存储在一个列表中：

```python
# 创建多个 `Person` 的实例
>>> list_of_people = [Person(n) for n in ("Fermi", "Noether", "Euler")]

>>> for person in list_of_people:
...    print(person.name)
...    print(person.x)
Fermi
1
Noether
1
Euler
1
```

更新 `Person` 的类级别的属性 `x` 会影响所有 `Person` 的实例：
```python
# 为类属性 `x` 设置一个新值
>>> Person.x = 22 

# 这会影响 `Person` 的所有实例
>>> for person in list_of_people:
...    print(person.name)
...    print(person.x)
Fermi
22
Noether
22
Euler
22
```
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**阅读理解：实例属性**

定义类 `Tmp`，其有着三个实例属性：`x`，`y`，和 `z`。`x` 和 `y` 应为根据在实例创建时输入的值设定的数字，而 `z` 应为这两个值的乘积。

比如说：
```python
>>> tmp = Tmp(2.1, 3.0)
>>> tmp.x
2.1

>>> tmp.y
3.0

>>> tmp.z
6.3
```

</div>
<!-- #endregion -->

现在，你应该对特殊的 `__init__` 方法能如何用来为你的类定义和设置实例级别的属性有一定的了解了。同时，调用类实例化会如何创建实例对象并将其作为 `self` 参数自动输入到 `__init__` 的过程也应该很明显易懂。在接下来的一节，我们将会遇到三种不同的方法：实例方法，类方法，和静态（static）方法。我们还会遭遇类似于 `__init__` 的所谓“特殊方法”（special method）；我们可以使用它们来更加广泛地描述你的类的行为和它如何和Python的操作符交互。


## 阅读理解答案：

<!-- #region -->
**解：类初始化**

使用以上定义的 `Dummy` 类，创建一个包含10个这一类的*不同*实例的列表。

```python
# 会在每次迭代中调用 `Dummy()` 一次
>>> list_of_dummies = [Dummy() for i in range(10)] 

# 请注意以下不相同的内存地址
>>> list_of_dummies
[<__main__.Dummy at 0x1d50de89940>,
 <__main__.Dummy at 0x1d50de896d8>,
 <__main__.Dummy at 0x1d50de897b8>,
 <__main__.Dummy at 0x1d50de89a20>,
 <__main__.Dummy at 0x1d50de89ac8>,
 <__main__.Dummy at 0x1d50de89a58>,
 <__main__.Dummy at 0x1d50de899e8>,
 <__main__.Dummy at 0x1d50de89a90>,
 <__main__.Dummy at 0x1d50de89b00>,
 <__main__.Dummy at 0x1d50de89b38>]
```

编写代码来显式检查每一个成员互不相同，以及每个成员都是 `Dummy` 类的实例。

```python
>>> from itertools import combinations
# `combinations(list_of_dummies, 2)` 会循环 `list_of_dummies` 中所有
# 成员对
>>> all(a is not b for a,b in combinations(list_of_dummies, 2))
True

>>> all(isinstance(a, Dummy) for a in list_of_dummies)
True
```

创建一个包含*一个* `Dummy` 实例被分别储存10次的元组。请注意在这里，我们仅仅初始化了 `Dummy` 一次，而元组理解语句仅仅向元组中输入了10次相同的实例。
```python
>>> dummy = Dummy()  # 一个 `Dummy` 实例
>>> tuple_of_dummy = tuple(dummy for i in range(10))

# 请注意所有的内存地址都是相同的
>>> tuple_of_dummy
(<__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>,
 <__main__.Dummy at 0x1d50de887b8>)
```

编写代码来显式检查每一个成员都引用相同的对象，以及每个成员都是 `Dummy` 类的实例。

```python
>>> all(dummy is i for i in tuple_of_dummy)
True

>>> all(isinstance(a, Dummy) for a in tuple_of_dummy)
True
```
<!-- #endregion -->

<!-- #region -->
**解：术语**

设：
```python
>>> class Cat:
...     pass

>>> x = Cat
>>> y = Cat()
>>> x1 = x
>>> x2 = x()
>>> y1 = y
```

`x` 和 `Cat` 共享着什么关系？：`x` 和 `Cat` 引用相同的类对象。

`y` 和 `Cat` 共享着什么关系？：`y` 是 `Cat` 类的一个实例。

`x` 和 `y` 共享着什么关系？：`x` 引用 `Cat`，而 `y` 是 `Cat` 的一个实例。因此，`y` 是 `x` 的一个实例。

`x2` 和 `y` 共享着什么关系？：它们是 `Cat` 的不同实例。

`y` 和 `y1` 共享着什么关系？：它们引用相同的 `Cat` 实例。
<!-- #endregion -->

接下来，指出以下每个对象是类对象还是类实例（如果是实例，是哪个类的实例）？

 - `"hello world"`： `str` 类型（也就是类）的一个实例
 - `True`：`bool` 类型的一个实例
 - `int`：一个描述整数的类对象
 - `{"a" : 22}`：`dict` 类型的一个实例
 - `tuple`：一个描述元组的类对象

<!-- #region -->
**解：实例属性**

定义类 `Tmp`，其有着三个实例属性：`x`，`y`，和 `z`。`x` 和 `y` 应为根据在实例创建时输入的值设定的数字，而 `z` 应为这两个值的乘积。

```python
class Tmp:
    def __init__(self, x, y):
        self.x = x
        self.y = y
        self.z = x * y
```

```python
>>> test = Tmp(8, 5)
>>> test.x
8
>>> test.y
5
>>> test.z
40
```
<!-- #endregion -->
