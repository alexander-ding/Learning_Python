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
   :description: Topic: Controlling behavior with special methods, Difficulty: Medium, Category: Section
   :keywords: dunder method, special method, operator overload, repr, getitem, custom syntax, __init__
```

<!-- #region -->
# 特殊方法
在本节中，我们将学习一系列各种Python保留的影响对象高层次行为和与操作符交互行为的实例方法。这些方法被称为特殊方法（special method）。`__init__` 是其中之一；请回忆，它控制在创建类实例的过程。相似的，比如说，我们将看到 `__add__` 如何控制对象在被 `+` 符号操作时的行为。一般而言，特殊方法的名字遵循格式 `__<name>__`；在名字前后各有两个下划线。相应的，我们可以将特殊方法读作“dunder”（double underscore）方法。学习如何使用这些特殊方法将允许我们设计优雅和强大的对象类。

这些方法为我们提供了对各种我们用来和对象交互的高层次接口的完全控制。让我们设一个行为随心的简单类来演示我们控制类行为的能力：

```python
# 演示特殊方法的（错误）使用方法
class SillyClass:
    def __getitem__(self, key):
        """ 设定 `self[key]` 的行为 """
        return [True, False, True, False]
    
    def __pow__(self, other):
        """ 设定 `self ** other` 的行为 """
        return "Python Like You Mean It"
```

```python
>>> silly = SillyClass()

>>> silly[None]
[True, False, True, False]

>>> silly ** 2
'Python Like You Mean It'
```
本节并不旨在为特殊方法的完整讨论，因为这会使得我们超出本文所期望的复杂度。[官方Python说明文档](https://docs.python.org/3/reference/datamodel.html#special-method-names)提供了一个严谨但有点难读的关于特殊方法的讨论。[Dive into Python 3](http://www.diveintopython3.net/special-method-names.html)提供了一个很棒的关于特殊方法的附录。本文强烈建议读者咨询此资源。
<!-- #endregion -->

<!-- #region -->
## 对象的字符串表达
以下方法决定了对象在各种情况下该如何作为字符串表达。比如说，本文经常向Python命令行输入对象来导致命令行打印对象对应的字符串表达式。比如说：

```python
>>> x = list(("a", 1, True))
>>> x
['a', 1, True]
```

在幕后，每当一个对象的在命令行/笔记本中像这样被显式，Python会调用特殊方法 `x.__repr__` 来获取对象的字符串表达式。这个方法返回了字符串 `"['a', 1, True]"`，然后Python将其打印到命令行中。这对创建可以方便地在Python命令行或Jupyter笔记本中查看的类对象游泳。相似的，`__str__` 返回在 `str` 为此对象调用时对象转化成的字符串。

|方法| 签名 | 解释 |
|---|---|---|
|返回对象打印时的字符串表达|`__repr__(self)`|`repr(x)` 调用 `x.__repr__()`。这也在对象被命令行返回时被调用|
|返回对象的字符串表达|`__str__(self)`|`str(x)` 调用 `x.__str__()`|
<!-- #endregion -->

一个实现得好的 `__repr__` 方法可以极大提升操作该类时的方便程度。比如说，让我们为在上一节写的的 `ShoppingList` 类添加这个方法；`__repr__` 将会将我们的购物清单物品转化成一个列表，其中已经购买的物品将被划掉：

<!-- #region -->
```python
def strike(text):
    """ 为字符串的每个字符添加删除线。
        
        `strike('hello world')` -> '̶h̶e̶l̶l̶o̶ ̶w̶o̶r̶l̶d'
        
        Notes
        -----
        \u0336 是一个特殊的删除线unicode符号；它并不是Python独有的。"""
    return ''.join('\u0336{}'.format(c) for c in text)

class ShoppingList:
    def __init__(self, items):
        self._needed = set(items)
        self._purchased = set()

    def __repr__(self):
        """ 返回格式化过的购物清单。已购买的物品将会被划掉。
            
            Returns
            -------
            str"""
        if self._needed or self._purchased:
            remaining_items = [str(i) for i in self._needed]
            purchased_items = [strike(str(i)) for i in self._purchased]
            # 你不会在你的键盘中找到 • 符号。我直接谷歌了“unicode bullet point”
            # 并将其复制/粘贴到了这里。
            return "• " + "\n• ".join(remaining_items + purchased_items)
        
    def add_new_items(self, items):  
        self._needed.update(items) 

    def mark_purchased_items(self, items):
        self._purchased.update(set(items) & self._needed)
        self._needed.difference_update(self._purchased)
```

```python
# 演示 `ShoppingList.__repr__`
>>> l = ShoppingList(["grapes", "beets", "apples", "milk", "melon", "coffee"])
>>> l.mark_purchased_items(["grapes", "beets", "milk"])
>>> l
• melon
• apples
• coffee
• ̶g̶r̶a̶p̶e̶s
• ̶m̶i̶l̶k
• ̶b̶e̶e̶t̶s
```

这个简单的方法使得我们在命令行/笔记本环境下检查购物清单的内态简单很多。
<!-- #endregion -->

## 为数学操作符接口
以下特殊方法控制某个对象如何和 `+`，`*`，`**`，和其它数学操作符交互。完整的模仿数字类的特殊方法列表可以在[这里](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)找到。

|方法| 签名 | 解释 |
|---|---|---|
|加|`__add__(self, other)`|`x + y` 调用 `x.__add__(y)`|
|减|`__sub__(self, other)`|`x - y` 调用 `x.__sub__(y)`|
|乘|`__mul__(self, other)`|`x * y` 调用 `x.__mul__(y)`|
|除|`__truediv__(self, other)`|`x / y` 调用 `x.__truediv__(y)`|
|乘方|`__pow__(self, other)`|`x ** y` 调用 `x.__pow__(y)`|

你可能在好奇为什么除法有着奇怪的名字 `__truediv__`，而其它操作符的名字都很正常。这是从Python 2转型到Python 3的一个产物；[默认的整数除法被浮点数除法提到了](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Number-Types)，所以 `__div__` 被 `__truediv__` 代替以保证两个版本的兼容性。

<!-- #region -->
让我们给 `ShoppingList` 一个 `__add__` 方法来使得我们可以使用 `+` 操作符合并两个购物清单。与其重新定义整个 `ShoppingList` 类，我们可以直接将其定义为一个函数并使用 `setattr` 来将其加入到我们先有的类中。

```python
def __add__(self, other):
    """ 将另外一个购物清单的未购买和已购买物品添加到这个清单。
        
        Parameters
        ----------
        other : ShoppingList
            另外那个购物清单，其物品我们将添加到这个清单中。
        Returns
        -------
        ShoppingList
            添加了物品的本购物清单。"""
    new_list = ShoppingList([])
    # 为 new_list 添加 `self` 和 `other` 的物品
    for l in [self, other]:
        new_list.add_new_items(l._needed)

        # 首先将已购买物品添加到列表中，然后将它们标为已购买
        new_list.add_new_items(l._purchased) 
        new_list.mark_purchased_items(l._purchased) 
    return new_list
```

```python
# 将 `__add__` 设为 `ShoppingList` 的一个方法
>>> setattr(ShoppingList, "__add__", __add__)
```

现在，让我们创建几个购物清单并合并它们：
```python
>>> food = ShoppingList(["milk", "flour", "salt", "eggs"])
>>> food.mark_purchased_items(["flour", "salt"])

>>> office_supplies = ShoppingList(["staples", "pens", "pencils"])
>>> office_supplies.mark_purchased_items(["pencils"])

>>> clothes = ShoppingList(["t-shirts", "socks"])

# 合并所有三个购物清单
>>> food + office_supplies + clothes
• t-shirts
• eggs
• pens
• milk
• staples
• socks
• ̶f̶l̶o̶u̶r
• ̶s̶a̶l̶t
• ̶p̶e̶n̶c̶i̶l̶s
```
重载（overload）`+` 操作符为我们提供了合并多个购物清单的简单易读的方法。`food + office_supplies + clothes` 等值于调用 `(food.__add__(office_supplies)).__add__(clothes)`。很明显，前者更加可取。
<!-- #endregion -->

## 创建类容器类
以下特殊方法允许我们给我们的类提供一个像词典，集，或列表所有的一样的容器接口。这些方法的完整列表和讨论可以在[这里](https://docs.python.org/3/reference/datamodel.html#emulating-container-types)找到。

|方法| 签名 | 接受 |
|---|---|---|
|长度|`__len__(self)`|`len(x)` 调用 `x.__len__()`|
|获取成员|`__getitem__(self, key)`|`x[key]` 调用 `x.__getitem__(key)`|
|设置成员|`__setitem__(self, key, item)`|`x[key] = item` 调用 `x.__setitem__(key, item)`|
|是否拥有|`__contains__(self, item)`|`item in x` 调用 `x.__contains__(item)`|
|迭代器|`__iter__(self)`|`iter(x)` 调用 `x.__iter__()`|
|下一个|`__next__(self)`|`next(x)` 调用 `x.__next__()`|

<!-- #region -->
为了对这些方法产生直观理解，让我们创建一个实现了大部分列表交互界面的类。我们将会列表作为类的一个属性来记录其内容，但我们会通过实现特殊方法来复制列表的交互界面。

```python
class MyList:
    def __init__(self, *args):
        if len(args) == 1 and hasattr(args[0], '__iter__'):
            # 处理 `MyList([1, 2, 3])
            self._data = list(args[0])
        else:
            # 处理 `MyList(1, 2, 3)`
            self._data = list(args)

    def __getitem__(self, index):
        out = self._data[index]
        # 切片应返回一个 `MyList` 实例
        # 不然的话，我们应该直接返回单个成员
        return MyList(out) if isinstance(index, slice) else out
    
    def __setitem__(self, key, value):
        self._data[key] = value

    def __len__(self): 
        return len(self._data)

    def __repr__(self): 
        """ 使用字符 | 来限定我们的列表"""
        # `self._data.__repr__()` 返回 '[ ... ]'，
        # 因此我们可以切片字符串的成员并去除两边的方括号，并
        # 使用我们自己的限定符号来代替它们
        return "|" + self._data.__repr__()[1:-1] + "|"

    def __contains__(self, item): 
        return item in self._data

    def append(self, item):
        self._data.append(item)
```
<!-- #endregion -->

<!-- #region -->
让我们体会一下这个简单的类提供的成熟行为：

```python
# MyList 接受任何可迭代物为其第一个（也是唯一的）
# 输入参数
>>> x = MyList("hello")
>>> x
|'h', 'e', 'l', 'l', 'o'|

# MyList 接受任意数量的参数
>>> x = MyList(1, 2, 3, 4, 5)
>>> x
|1, 2, 3, 4, 5|

>>> len(x)
5

# 获取一个成员
>>> x[0]
1

# 切片返回一个 MyList 实例
>>> x[2:4]
|3, 4|

# 设置一个成员
>>> x[0] = -1
>>> x
|-1, 2, 3, 4, 5|

# 检查是否为成员
>>> 10 in x
False

>>> MyList()
||
```
<!-- #endregion -->

## 官方说明文档链接

- [特殊方法](https://docs.python.org/3/reference/datamodel.html#special-method-names)
- [模仿数字类](https://docs.python.org/3/reference/datamodel.html#emulating-numeric-types)
- [模仿容器类](https://docs.python.org/3/reference/datamodel.html#emulating-container-types)
