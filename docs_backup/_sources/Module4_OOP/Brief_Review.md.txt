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
   :description: Topic: Brief review of object oriented programming, Difficulty: Easy, Category: Tutorial
   :keywords: class definition, simple, examples, overview, init, initialize, type, object
```

<!-- #region -->
## 术语和概念的快速总结

让我们快速地复习一下我们已经讨论的一些概念和术语。以下代码是一个*类定义*（class definition），其描述了属于类/类型 `Example` 对象的*属性*（attribute）。

```python
class Example:
    a = (1, 2, 3)
    
    def __init__(self):
        self.b = "apple"
```

在执行之后，以上代码将创建*类对象* `Example`，其包装了以上的定义并可以被用来创建这个类/类型的实例对象。`Example.a` 和 `Example.__init__` 都是该类的属性。`Example.__init__` 更加仔细来讲是一个特殊方法；当创建这类的实例时会自动调用它。

以下代买创建了 `Example` 的一个*实例*，并将这个实例赋值给变量 `ex`。这意味着这个对象术语类型（也就是类）`Example`。

```python
>>> ex = Example()

>>> Example.a
(1, 2, 3)

>>> ex.a
(1, 2, 3)

>>> isinstance(ex, Example)
True

>>> type(ex)
__main__.Example
```

在实例化后，实例属性 `b` 会通过 `__init__` 方法的执行而被定义。而Python会自动向 `__init__` 方法将被创建的对象作为参数 `self` 输入。所以，`b` 是一个*实例级别*（instance level）的属性，而实例属性属于 `Example` 类对象本身。

```python
>>> Example.b
AttributeError: type object 'Example' has no attribute 'b'

>>> ex.b
'apple'
```

<!-- #endregion -->
