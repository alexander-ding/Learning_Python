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
   :description: Topic: Applications of object-oriented programming, Difficulty: Medium, Category: Section
   :keywords: summary, tutorial, python shopping list, object oriented, method, attribute
<!-- #endraw -->

# 面向对象编程的应用

我们花了不少时间学习类，类对象，实例，和方法的语法和定义。让我们花一些时间来总结我们的知识并创建一个有用的类。这将帮助我们发展对面向对象编程如何能为我们所用的理解。我们将试图推荐一些决定我们何时该和何时不该定义我们自己的类的守则。

## 购物清单
让我们创建一个购物清单类，其实例将纯粗一列表的要购买的物品名字（字符串）和一列表的已经购买的物品名字。我们将编写一个 `__init__` 函数来接受一个或多个字符串来加入到购物清单中。然后，我们将创建方法来允许我们：

- 向购物清单添加新物品
- 将列表的物品标记为“已购买”
- 从列表中删除物品，不管其是否已经购买
- （根据字母顺序）列出需要购买的物品名字
- （根据字母顺序）列出已经购买的物品名字

我们不想要在购物清单中有着重复的物品——如果有人输入了“apple”两次，我们应该仅仅列该物品一次。因此，我们将使用存储不重复成员的[集](http://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_III_Sets_and_More.html#The-%E2%80%9CSet%E2%80%9D-Data-Structure)来存储列表中的成员。

最后，我们想要我们的方法来接受一个为单个物品名（字符串）或多个物品名（一列表/元组/可迭代物的字符串）的变量。为了容纳这两种情况，如果我们仅仅收到了一个字符串，我们将首先将其放入一个列表中。这将保证我们永远都会操作成员为字符串的可迭代物。




```python
class ShoppingList:
    def __init__(self, items):
        """ Parameters
            ----------
            items : Union[str, Iterable[str]]
                要加入到购物列表的成员为物品名字的可迭代物"""
        if isinstance(items, str):
            items = [items]
        self._needed = set(items)
        self._purchased = set()
    
    def add_new_items(self, items):
        """ Add more items to the shopping list 
            
            Parameters
            ----------
            items : Union[str, Iterable[str]]
                要加入到购物列表的成员为物品名字的可迭代物"""
        if isinstance(items, str):
            items = [items]
        # set.update 将可迭代物中的成员添加到集中
        self._needed.update(items) 
        
    def mark_purchased_items(self, items):
        """ 提供用来标记成“已购买”的物品的名字
            
            Parameters
            ----------
            items : Union[str, Iterable[str]]"""
        if isinstance(items, str):
            items = [items]
        # 仅仅将已经在列表中的物品标记为已购买
        self._purchased.update(set(items) & self._needed)
        # 从未购买的集中删除删除所有已购买物品
        self._needed.difference_update(self._purchased)
    
    def list_purchased_items(self):
        """ 返回已购买物品的排序列表"""
        return sorted(self._purchased)

    def list_unpurchased_items(self):
        """ 返回未购买物品的排序列表"""
        return sorted(self._needed)
```

<!-- #region -->
让我们创建一个有几个物品的购物列表：

```python
# 创建一个购物清单
>>> my_list = ShoppingList(["apples", "apples", "grapes", "peaches", "milk", "bread"])
>>> my_list.list_unpurchased_items()
['apples', 'bread', 'grapes', 'milk', 'peaches']
>>> my_list.list_purchased_items()
[]
```

请注意，我不小心在列表中输入了“apples”两次。但是，我们在幕后使用的集能够优雅地处理这个错误并自动删除所有重复的物品。假设我们带着购物清单并购买了几个物品，让我们如此标记。

```python
# 在购物列表中标记几个物品为已购买
>>> my_list.mark_purchased_items(["grapes", "pineapples"])
>>> my_list.list_purchased_items()
['grapes']
>>> my_list.list_unpurchased_items()
['apples', 'bread', 'milk', 'peaches']
```

请注意我们在未购买组中正确的删除了“grapes”并将其加入到了购买组中。同时，我们的实现优雅地忽略了将“pineapple”标记为已购买的请求，因为我们本来就没有在列表中加入“pineapple”。

在下一节中我们将会为我们的 `ShoppingList` 类添加一些特殊方法来使得我们可以更加简单地浏览我们购买和未购买物品的列表。
<!-- #endregion -->

<div class="alert alert-warning">

**注意：负责地使用类定义**

我们 `ShoppingList` 类的主要功能是为我们管理两个物品集，使得记录未购买和购买物品的逻辑简单易懂。如果我们不在乎记录物品是否已经购买，那么我们根本不应该定义一个类。我们应该直接将物品名存入一个 `list` 中并使用它的方法来添加和删除物品。

当可以的时候，我们应当优先使用Python的内置类型，而不是定义一个新类。这么做会使得你的代码简单，轻便，并和其它代码兼容。

</div>
