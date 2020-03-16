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
   :description: Topic: The different kinds of class methods, Difficulty: Medium, Category: Section
   :keywords: instance, class method, static method, property, abstract method, class funtion
<!-- #endraw -->

<!-- #region -->
# 方法

请回忆，方法就是一个是函数的类属性。比如说，“append”是一个为 `list` 类定义的方法，而“capitalize”是 `str`（字符串）类的方法。

```python
# 创建 `list` 类/类型的实例并调用实例方法 `append`
>>> a = [1, 2, 3]
>>> a.append(-1)
>>> a
[1, 2, 3, -1]

# 创建 `str` 类/类型的实例并调用实例方法 `capitalize`
>>> b = "moo"
>>> b.capitalize()
'Moo'
```

在这里我们会遭遇三种不同的方法：

- 实例方法（instance method）
- 类方法（class method）
- 静态方法（static method）

它们之间的区别相对而言很小，但是这些区别很重要。函数“append”和“capitalize”都是实例方法的例子，因为它们旨在分别被某个列表实例和字符串实例调用。

我们已经见过实例方法 `__init__` 了。因为它被Python保留，所以它是一个特殊方法，会在实例类初始化时被自动调用。相似的，特殊实例方法 `__add__` 将告诉对象如何和 `+` 操作符交互。比如说，`float.__add__` 指示了 `+` 将会将两个 `float` 实例的值相加，而 `list.__add__` 指示了 `+` 会将两个 `list` 实例粘连在一起。我们将会通过了解一些这种特殊方法来为我们对方法的讨论结尾——这将大幅度地提升我们定义方便，对用户友善地类的能力。
<!-- #endregion -->

<!-- #region -->
## 实例方法
*实例方法*（instance method）是任何在类主体代码中的函数定义。我们必须要撇清一个看起来琐碎但是很重要的细节：'`self`' 是实际上任何实例方法的第一个参数。这是我们在操作 `__init__` 时遇到的一个细节。让我们单纯地编写代码以遭遇一个常见的错误来演示以上细节的重要性。我们首先创建一个有着一个接受一个参数并直接返回该参数的实例方法的类：

```python
class Dummy:
    def func(x): 
        """ 一个直接 `x` 的实例方法。 
            这是一个有问题的实例方法版本！"""
        return x
```   

我们可以通过类对象 `Dummy` 本身调用这个函数，而它的行为会和定义的一样：
```python
>>> Dummy.func(2)
2
```
但是当我们试图从 `Dummy` 的实例调用 `func` 时发生了一些奇怪的事情：
```python
# 从 `Dummy` 的实例调用 `func` 导致了一个预期之外的错误
>>> inst = Dummy()
>>> inst.func(2)
TypeError: func() takes 1 positional argument but 2 were given
```
第一眼看上去，这个错误信息完全不讲道理。`func` 确实只接受一个参数——我们在函数定义时让其接受一个参数 `x`。但调用 `inst.func(2)` 怎么会提供*两个*参数呢？看起来，我们仅仅向我们的方法输入了 `2`。在这里藏着一个重要的细节：
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-warning">

**重要**！

当你从一个实例对象（如 `inst`）调用一个实例方法（如 `func`）时，Python会在用户提供的参数之外自动将该实例对象作为第一个参数输入到函数中。

</div>

根据以上，`inst` 被作为参数 `x` 被输入，而我们在试图将 `2` 作为第二个参数输入到方法中；这解释了向 `func` 输入两个函数的错误。根据这个逻辑，我们应该能够调用 `a.func()` 并发现 `inst` 是作为参数 `x` 输入——请回忆，`func` 的定义使得它直接返回 `x`。让我们确定这个：

```python
# 确认 `inst` 是被作为第一个参数输入到实例方法 `func`

# 请注意 Dummy 实例 `inst` 的内存地址
>>> inst
<__main__.Dummy at 0x284f0008da0>

# `inst.func()` 自动接受 `inst` 为输入参数，并直接返回它
>>> inst.func()
<__main__.Dummy at 0x284f0008da0>

# `inst` 确实被输入到并返回于 `func`
>>> out = inst.func()
>>> inst is out
True
```

*请注意，这个“幕后”的行为仅仅在方法通过实例调用的情况才会发生*；这就是为什么我们通过 `Dummy` 调用 `func` 时没有遇到这个问题——`Dummy` 是一个类对象，不是一个实例。因此，`inst.func()` 等值于 `Dummy.func(inst)`：

```python
>>> out = Dummy.func(inst)
>>> out is inst
True
```

在现在的情况下，我们没有任何办法在从 `Dummy` 实例调用 `func` 时向其输入一个参数。为了解决这个问题，我们将重构 `func` 的定义来预知实例对象将作为第一个参数输入的这一点。
<!-- #endregion -->

<!-- #region -->
### `self` 参数
我们将想要定义我们的实例方法来提前预知Python会自动将实例对象作为第一个参数输入到方法这一点。因此，如果我们想要我们的方法接受 $N$ 个外部参数，我们应该定义它的签名来有着 $N+1$ 个参数，因为我们知道Python会将实例作为第一个参数输入。广泛使用的传统是将这个第一个参数叫做 `self`。这个名字没有任何特殊的意义，仅仅是它是Python用户广泛使用的传统；“self”旨在指出实例对象在将自己作为第一个参数输入到方法中一事。设以下范例：

```python
# 演示 `self` 在实例参数中的作用
class Number:
    def __init__(self, value):
        self.value = value
    
    def add(self, new_value):
        return self.value + new_value
```

```python
# 调用 __init__，设 self.value = 4.0
>>> x = Number(4.0)

# `x` 输入为 `self` 参数
>>> x.add(2.0)
6.0

# 从类对象调用这个实例方法。我们必须显式地为 `self` 输入一个对象
>>> Number.add(x, 2.0)
6.0
```

请注意将 `self` 自动作为参数输入到 `__init__` 和 `add` 的有用之处。实例方法旨在能够访问调用它的实例对象——当你从一个字符串实例调用 `capitalize` 时，你当然想要它大小*该*字符串。如果Python不自动这么做，每次输入实例本身将会很乏味和重复。

接下来，我们将注意我们也可以定义类方法和静态方法。类方法会自动将*类对象*作为第一个参数输入，而静态方法不会自动在幕后输入任何对象。
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：调用实例方法**

重写 `Dummy` 使得它的实例方法 `func` 接受两个参数：Python自动输入的实例对象和我们想要 `func` 直接返回的参数 `x`。创建一个 `Dummy` 的实例，从这个实例调用 `func`，并向其输入字符串 `"hi"`。这会返回什么呢？当你试图调用 `Dummy.func("hi")` 时会发生什么呢？为什么？我们该如何修改这个直接从 `Dummy` 进行的调用来使得这个方法会像我们希望的一样操作呢？

</div>

<!-- #region -->
## 类方法
一个类方法类似于实例方法，但它会自动将*类对象*作为第一个参数输入。请回忆，当实例方法从实例对象中调用时，它会自动将实例对象作为方法的第一个参数输入。相比之下，当一个*类方法*从类对象或实例对象调用时，它将自动将类对象作为方法的第一个参数输入。与其叫这个第一个参数 `self`，传统是将其称为 `cls`。

为了定义一个类方法，你必须使用一个特殊的内置装饰器 `classmethod` 来*装饰*（decorate）该方法定义。我们还没有讨论装饰器。现在你只需要知道它们会“标记”该方法，使得Python知道该将其作为一个类方法而不是实例方法对待。以下演示了装饰的过程：

```python
class Dummy:
    
    @classmethod
    def class_func(cls):
        """ 定义来直接返回 `cls` 的类方法
        """
        return cls
```

```python
# `Dummy` 会自动作为 `cls` 输入。
#  我们定义 `class_func` 使其直接返回 `cls` 而不做修改
>>> Dummy.class_func()
__main__.Dummy

# `Dummy.class_func()` 返回 `Dummy`
>>> out = Dummy.class_func()
>>> out is Dummy
True

# 就算我们从实例中调用 `class_func`，`Dummy` 也会自动作为
# `cls` 被输入
>>> inst = Dummy()
>>> inst.class_func()
>>> inst.class_func()
__main__.Dummy
```
<!-- #endregion -->

<!-- #region -->
`dict.fromkeys` 是一个接受可迭代物并返回键为可迭代物成员，值都默认为 `None` 的词典的类方法。

```python
>>> dict.fromkeys("abcd")
{'a': 2.3, 'b': 2.3, 'c': 2.3, 'd': 2.3}
```

这作为一个类方法而不是实例方法是很合理的，因为这个方法会凭空创建一个新词典。它只需要能够调用 `dict` 对象（也就是 `cls` 参数）就能创建这个词典。以下是如果我们自己定义 `dict` 类的话 `fromkeys` 的可能实现：

```python
class dict:
    # 假设所有其他的词典方法都在这里定义了
    @classmethod
    def fromkeys(cls, iterable, value=None):
        """ 创建一个词典，其键为 `iterable` 的成员。所有值这些键都是`value`。
        
        Parameters
        ----------
        iterable: Iterable[Hashable]
            成员为合法词典键（也就是任何可哈希的对象）的可迭代物。
        
        value : Optional[Any]
            所有键会对应的值。默认是 `None`。
        
        Returns
        -------
        dict """
        new_dict = cls()  # 等值于 `dict()`：创建一个新的词典实例
        for key in iterable:
            new_dict[key] = value
        return new_dict
```
<!-- #endregion -->

<!-- #region -->
## 静态方法
静态方法就是所有参数都需要用户显式输入的方法。也就是说，Python不会向静态方法输入任何参数。你可以使用内置的装饰器 `staticmethod` 来将一个静态方法和实例方法区分。

```python
class Dummy:

    @staticmethod
    def static_func():
        """ 永远返回 `'hi'` 的静态方法
        """
        return 'hi'
```

```python
# 静态方法可以从类对象或实例对象调用；没有任何自动向其输入的参数。
>>> Dummy.static_func()
'hi'

>>> inst = Dummy()
>>> inst.static_func()
'hi'
```
<!-- #endregion -->

## 阅读理解答案：

<!-- #region -->
**调用实例方法：解**

重写 `Dummy` 使得它的实例方法 `func` 接受两个参数：Python自动输入的实例对象和我们想要 `func` 直接返回的参数 `x`。

> 我们将重写 `func` 来接受名为“self”的参数，其将接受“幕后”输入的实例对象和“x”。就像你在阅读中所见的一样，参数的名字“self”仅仅是传统而已。

```python
class Dummy:
    def func(self, x): 
        return x
```

创建一个 `Dummy` 的实例，从这个实例调用 `func`，并向其输入字符串 `"hi"`。

```python
>>> inst = Dummy()
>>> inst.func("hi")  # `inst` 被输入到参数 `self` 中
'hi'
```

当你试图调用 `Dummy.func("hi")` 时会发生什么呢？为什么？

> 这将导致一个错误，其反应 `func` 期待两个参数但只收到了一个。确实，我们仅仅向其输入了对象 "hi"。Dummy是一个类对象，而不是实例对象。因此，在调用Dummy.func时Python不会自动在“幕后”做任何特别的事情。我们必须为self参数输入些什么。因为这个方法不会使用self做任何事情，我们可以向其输入None，或其它任何对象。

```python
# Dummy.func("hi") 会导致一个错误
>>> Dummy(None, "hi")
'hi'
```
<!-- #endregion -->
