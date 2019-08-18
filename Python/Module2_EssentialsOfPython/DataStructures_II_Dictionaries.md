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
   :description: Topic: Introduction to Dictionaries, Difficulty: Medium, Category: Section
   :keywords: dictionary, complexity, key, value, iteration, get item, hashing, lookup, interview preparation
```

# 数据结构（第二部分）：词典
Python的词典（dictionary）允许你储存键值对（key-value pair），然后向词典输入键可以快速地获取其对应的值。具体来讲，你通过描述从键到值的单向对应关系来创建词典。**每个键应该只对应一个值**，也就是说每个键都应该是唯一的。


<!-- #region -->
让我们来创建以下食物到价格的对应关系：

- "cheese" $\rightarrow$ 2.53, 
- "milk" $\rightarrow$ 3.40, 
- "frozen pizza" $\rightarrow$ 8.01

```python
# 使用一个词典来将食物来对应价格：名字 -> 价格
>>> items_to_prices = {"cheese": 2.53, "milk": 3.40, "frozen pizza": 8.01}

# 查看 "frozen pizza" 的价格
>>> items_to_prices["frozen pizza"]
8.01
```

Pythond的词典是其数据结构中一颗闪耀的星；它紧凑，快速，通用，并非常有用。你可以用它来创建多种多样的对应关系。

```python
# 记录某3D坐标是否处于空间中某区域之中
# 将 (x, y, z) 坐标对应到“是否在区域中”：(x, y, z) -> True/False
>>> point_to_region = {(0.1, 2.2, 3):False, (-10., 0, 4.5):True, (4.3, 1.0, 9.5):False}
>>> point_to_region[(-10., 0, 4.5)]
True

# 将学生名字对应到考试成绩中：名字 -> 成绩
>>> name_to_scores = {"Ryan S": [65, 50, 80], "Nick S": [100, 99, 90]}
>>> name_to_scores["Ryan S"]
[65, 50, 80]
```

我们应该立刻指出：在词典中查找某键对应的值的速度*并不会根据词典大小改变。*也就是说，词典查找的复杂度为 $\mathcal{O}(1)$！它通过一个知名的[哈希算法](https://en.wikipedia.org/wiki/Hash_function)技巧来达到这一点。

以下为所有内置 `dict` 类型的实例:
```python
>>> type(items_to_prices)
dict
```
我们会在下文讨论词典的基础。本文强烈建议你阅读Python官方说明文档来查看[所有词典可用的函数](https://docs.python.org/3/library/stdtypes.html#dict).
<!-- #endregion -->

<!-- #region -->
## 词典基础
### 创建词典
你可以通过在“花括号”中输入键值对来创建词典：`{key1:value1, key2:value2, ...}`。作为范例，让我们创建一个将食物名字对应到 "fruit" 或 "vegetable" 的词典吧。我们首先将 "apple" 对应到 "fruit"，"carrot" 对应到 "vegetable"
```python
# 使用 `{key1:value1, key2:value2, ...}` 来创建词典：
#  "apple" -> "fruit"
# "carrot" -> "vegetable
>>> fruit_or_veggie = {"apple":"fruit", "carrot":"vegetable"} 

# 创建空词典
>>> {}
{}
```
你也可以使用 `dict` 来作为构造器来创建词典。你可以向其输入有着键值对（由类似元组的序列代表）的可迭代物。

```python
# 使用 `dict` 来创建词典：
#  "apple" -> "fruit"
# "carrot" -> "vegetable
>>> fruit_or_veggie = dict([("apple", "fruit"), ("carrot", "vegetable")])

# 使用 `dict` 来创建空词典
>>> dict()
{}
```
最后，Python也支持词典理解语法，其与本模组之前讨论的生成器/列表理解语法类似：

```
{key:value for key, value in <iterable of key-value pairs> [if bool(<condition>)]}
```

```python
# 使用“词典理解”语法来创建词典
#  "apple" -> "fruit"
# "carrot" -> "vegetable
>>> {k:v for k,v in [("apple", "fruit"), ("carrot", "vegetable")]}
{'apple': 'fruit', 'carrot': 'vegetable'}
```

### 返回键对应的值
现在我们可以使用词典来“查找”某项目是水果还是蔬菜。词典支持和列表/元组一样的方括号“获取项目”语法，但与其使用索引我们使用一个合法的键：
```python
# 获取键 "apple" 对应的值
>>> fruit_or_veggie["apple"]
"fruit"
```

尝试查找不存在的键会导致 `KeyError`：
```python
# "grape" 并没有作为键被定义
>>> fruit_or_veggie["grape"]
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-30-74c002a67890> in <module>()
----> 1 fruit_or_veggie["grape"]

KeyError: 'grape'
```

### 添加额外的键值对应
在词典创建后，你可以通过 `my_dict[new_key] = new_value` 在其中设定额外的的键值对：
```python
# 设定对应 "banana" -> "fruit"
>>> fruit_or_veggie["banana"] = "fruit"
>>> fruit_or_veggie
{'apple': 'fruit', 'banana': 'fruit', 'carrot': 'vegetable'}
```
如果键已经存在的话，那该键对应的值将被更新。

你可以使用 `update` 函数来一次性添加多个键值对。该函数接受另外一个词典或成员为键值对序列的可迭代物
```python
# 向词典添加多个键值对
>>> fruit_or_veggie.update([("grape", "fruit"), ("onion", "vegetable")])
>>> fruit_or_veggie
{'apple': 'fruit',
 'banana': 'fruit',
 'carrot': 'vegetable',
 'grape': 'fruit',
 'onion': 'vegetable'}
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：词典基础**

设含有学生名的元组 `("Ashley", "David", "Edward", "Zoe")` 以及他们对应的考试成绩 `(0.92, 0.72, 0.88, 0.77)`，创建一个词典来对应：名字 $\rightarrow$ 成绩。然后，将Zoe的成绩更新为 `.79`。最后添加一个新学生Ryan，其成绩为0.34。

译者注：原作者名为Ryan，是位自黑达人。

</div>

<!-- #region -->
## 词典可以储存什么？
虽然前文的例子中仅仅将字符串对应字符串，但是*词典的键和值的类型可以相异*：

```python
# 演示词典可以使用的多种多样的键值类型
>>> example_dict = {-1:10, "moo":True, (1, 2):print, 3.4:"cow", False:[]}
>>> example_dict[-1]
10

>>> example_dict["moo"]
True

>>> example_dict[(1, 2)]
<function print>

>>> example_dict[3.4]
"cow"

>>> example_dict[False]
[]
```

具体来讲，词典的内容应遵循以下规则：

- 词典的*键*必须是*不可变*的对象（更加精确地讲，他必须是[可哈希](https://docs.python.org/3/glossary.html#term-hashable)（hashable）的；不要太在意这个细节）。
- 词典的*值*可以是任何对象（甚至可以是同一个词典！试试看，这挺酷的！）

<div class="alert alert-warning">

**回忆**：

可变对象在被创建后是可以被修改的。不可变对象则不能被修改。
</div>

所以合法的键可以是以下类型：

 - 数字（整数，浮点数，复数）
 - 字符串
 - 元组（但元组中的成员也必须是不可变对象）
 - 布尔值
 - [冻集](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_III_Sets_and_More.html#Set-operations)（frozenset）对象
 
使用可变对象作为键的问题在于该对象在作为键使用*之后*可以被改变。因此，词典会需要“检测”到这个变化并为变化后的键重新创建它的“查找方式”。而值可以是可变对象，因为值对象的细节并不影响词典如何查找该值。
 
如上所述，如果你试图使用列表来作为键，你会收到一个 `TypeError`，因为列表是可变的：
```python
# 试图将列表作为键使用
# 因为列表是可变的，这会报错
>>> bad_dict = {[]:1}
---------------------------------------------------------------------------
TypeError                                 Traceback (most recent call last)
<ipython-input-50-e7cf39509d06> in <module>()
----> 1 bad_dict = {[]:1}

TypeError: unhashable type: 'list'
```

### 数字精度和词典键
在使用浮点数作为词典键时需小心，因为浮点数的[精度有限](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Understanding-Numerical-Precision)。和你不应该检查两个浮点数是否完全相同的原因一样，你不能保证两个浮点数作为键时会有相同的哈希值：

```python
# 将浮点数作为词典键使用导致的尴尬情况
>>> x = {}
>>> x[(0.1 + 0.1 + 0.1) - 0.3] = "apple"
>>> x[0.0]
KeyError: 0
```

根据使用情况，一种可以接受的使用浮点数作为值的方法是先将浮点数四舍五入到一个更低的精度：

```python
# 现将浮点数四舍五入再将其用为键
>>> x = {}

# 将浮点数四舍五入到小数点后第二位
>>> float_key = round((0.1 + 0.1 + 0.1) - 0.3, 2)
>>> x[float_key] = "apple"
>> x[0.0]
'apple'
```

<div class="alert alert-info">

**经验**：

词典的键必须是不可变对象。词典的值可以是任何对象。
</div>
<!-- #endregion -->

<!-- #region -->
### 检查词典
词典提供了一些检查和迭代其键和值的工具。我们在范例中将会使用以下词典：

```python
>>> example_dict = {"key1":"value1", "key2":"value2", "key3":"value3"}
```

**检查词典的键**

你可以直接使用词典本身来迭代它的键：
```python
# 迭代词典会迭代它的键
>>> [i for i in example_dict]
['key1', 'key2', 'key3']
```

你也可以使用词典本身来检测某对象是否为它的键：

```python
# 检查某对象是否是词典的键
>>> "key3" in example_dict
True

# 你*不能*用这个方法检查词典的值
>>> "value3" in example_dict
False
```
`len` 会返回词典有多少个键：
```python
>>> len(example_dict)
3
```

`example_dict.keys()` 也会返回一个词典键的可迭代物，也因此可以用来达到和之前范例直接使用词典一样的效果。

**检查词典的值**

迭代 `example_dict.values()` 会提供词典的值：
```python
# 迭代词典的值
>>> [i for i in example_dict.values()]
['value1', 'value2', 'value3']
```

你也可以使用这个函数来检查某对象是否为词典的值：

```python
# 检查某对象是否为词典的值
>>> "value1" in example_dict.values()
True
```

**检查词典的键值对**

迭代 `example_dict.items()` 会返回词典的键值对（它们被打包在元组中）：
```python
# 迭代词典的items会返回它的键值对
>>> [i for i in example_dict.items()]
[('key1', 'value1'), ('key2', 'value2'), ('key3', 'value3')]
```

你也可以用这个函数来检测某对象是否为词典的键值对：

```python
# 检查某对象是否为词典的键值对
>>> ('key1', 'value1') in example_dict.items()
True
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：反向词典**

编写一个函数来将词典反向。比如说，如果函数收到 `x = {'k1': 'v1', 'k2': 'v2', 'k3': 'v3'}`，那你应返回词典 `inverted_x = {'v1': 'k1', 'v2': 'k2', 'v3': 'k3'}`。

</div>


<div class="alert alert-info">

**阅读理解：检查词典**

假设我们收到一个值为*互不相同*的数字的词典，编写一个返回对应词典中*最大*值的*键*的函数。

然后，修改你的函数来处理值可能重复的词典。返回包含所有对应最大值的键的元组。
</div>


### 词典函数的时间复杂性
除了灵活和通用之外，词典的函数也相对高效。以下为词典常见操作（在CPython实现中）的时间复杂性——请注意 $\mathcal{O}(1)$ 操作的数量！

设 `example_dict` 为一个有 $n$ 个键值对的词典。

以下为 $\mathcal{O}(1)$ 的操作：

- 返回词典键的数量：`len(example_dict)` 
- 检查某对象是否为词典的键，如果是的话返回它对应的值：`example_dict[key]` 
- 设定一个键值对应：`example_dict[key] = value` 
- 检查某对象是否为词典的键：`obj in example_dict` 
- 检查某对对象是否为词典的键值对：`(obj1, obj2) in example_dict.items()`

以下为 $\mathcal{O}(n)$ 的操作：

- 检查某对象是否为词典的值：`obj in example_dict.values()`
- 迭代词典所有的键/值/键值对

<!-- #region -->
### 词典是否有序？一句警告
和Python的序列不同的是，词典没有任何内在的顺序，但这是在Python 3.6之前了。

在Python 3.6之前，词典并没有任何顺序。如果你迭代一个词典的键，值，或键值对，你不会得到任何项目*顺序*的保证。`[i for i in example_dict]` 可能在每次运行时返回顺序不同的键列表；你唯一得到的保证是列表会包含词典所有的键：

```python
# 在Python 3.5和之前，词典是无序 的
>>> example_dict = {"key1":"value1", "key2":"value2", "key3":"value3"}

# 这可能会返回排序不同的列表
>>> [i for i in example_dict]
["key1", "key3", "key2"]

>>> [i for i in example_dict]
["key2", "key1", "key3"]

>>> [i for i in example_dict]
["key1", "key2", "key3"]

...
```

词典在[Python 3.6中重新实现](https://docs.python.org/3/whatsnew/3.6.html#new-dict-implementation)，使得它相比以前会少使用大约25%的内存（这是一个很大的提升！）。随之而来的变化就是新的实现导致了词典的各种可迭代物（如 `dict.keys()`，`dict.values()`，`dict.items()`）每次都会根据添加到词典中顺序返回对象。

```python
# 在Python 3.6和之后词典根据键值对添加的顺序来排序
>>> example_dict = {"key1":"value1", "key2":"value2", "key3":"value3"}

# 这永远都会返回一样顺序的键
>>> [i for i in example_dict]
["key1", "key2", "key3"]
```

这很棒，对吧？不对！如果你在Python 3.6中编写基于词典排序的代码，那么你的算法在Python 3.5和以前几乎一定会导致错误的结果！最差的是，这很可能在你的代码中根本不报错，所以这个bug会静默地持续下去——这很难抓到！

**如果你没有明显标记你的代码和Python 3.6之前的版本不兼容，编写代码的时候假装词典并不归序！**

如果你真的想要使用一个顺序的词典，用 `collections.OrderedDict`。它的行为和正常的词典一模一样，但它不管什么Python版本都保证顺序。

```python
from collections import OrderedDict
>>> ordered = OrderedDict([('key1', 'value1'), ('key2', 'value2'), ('key3', 'value3')])

# 这永远都会返回相同的结果，不论Python版本
>>> [i for i in ordered]
["key1", "key2", "key3"]
```

<div class="alert alert-info">

**经验**：

不管你在用什么Python版本，在编写代码的时候假装Python词典是无序的。如果你真的想要一个有序的词典，你的代码应该使用 `collections.OrderedDict`。
</div>
<!-- #endregion -->

## 官方说明文档链接

- [词典](https://docs.python.org/3/library/stdtypes.html#mapping-types-dict)
  - [词典教程](https://docs.python.org/3/tutorial/datastructures.html#dictionaries)
- [“可哈希”的定义](https://docs.python.org/3/glossary.html#term-hashable)
- [词典视阈对象](https://docs.python.org/3/library/stdtypes.html#dictionary-view-objects)（dictionary view objects）


## 阅读理解答案

<!-- #region -->
**词典基础：解**

设包含学生名字的元组 `(Ashley, David, Edward, Zoe)` 以及对应的成绩 `(0.92, 0.72, 0.88, 0.77)`，创建一个词典：name $\rightarrow$ grade。

然后，将Zoe的成绩更新为 `.79`。最后，添加一个新学生Ryan，其成绩为 0.34。

```python
names = ("Ashley", "David", "Edward", "Zoe")
scores = (0.92, 0.72, 0.88, 0.77)
```

这是一个基本但冗长的解法。它太长，且其逻辑使因为使用了 `index` 而没必要地复杂化了。

```python
# basic solution for creating `grades`
grades = {}
for index in range(len(names)):
    name = names[index]
    value = scores[index]
    grades[name] = value
```

你应该使用函数[zip](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Itertools.html#zip)来将名字和成绩打包到一个可迭代物中国呢，并利用理解表达式来创建词典。

```python
# 好很多的创建 `grades` 的方式
grades = {student:value for student, value in zip(names, scores)}

# 更新Zoe的成绩
grades["Zoe"] = 0.79

# 添加Ryan的成绩
grades["Ryan"] = 0.34
```
<!-- #endregion -->

<!-- #region -->
**反向词典：解**
```python
# 简单解：使用for循环
x = {'k1': 'v1', 'k2': 'v2', 'k3': 'v3'}
inverted_x = {}
for key, value in x.items():
    inverted_x[value] = key
```

```python
# 更好的解：使用词典理解
inverted_x = {value:key for key, value in x.items()}
```
<!-- #endregion -->

<!-- #region -->
**检查词典：解**

假设我们收到一个值为*互不相同*的数字的词典，编写一个返回对应词典中*最大*值的键的函数。

你应该能够写出的解为：

```python
# solution
def max_key(x): 
    max_val = max(x.values())
    for key, value in x.items():
        if value == max_val:
            return key
```
```python
>>> max_key({'a': 0, 'b': 2, 'c': 200, 'd': 0})
'c'
```

这个函数的不利之处在于它迭代了 `x` 两次；一次由 `max` 一次由for循环。这个问题的最优解仅仅迭代一次，但它用到了在这里提供的材料之外的进阶概念。我们在此为子孙后代提供最优解：

你可以为 `max` 函数提供“key”参数。这个参数接受用来计算每轮迭代的“值”（这个值就是 `max` 函数用来找出最大值的参考）的函数。在此，我们向其输入内置的词典函数[get](https://docs.python.org/3/library/stdtypes.html#dict.get)。它接受词典的键为参数并返回对应的值。所以 `max` 会迭代 `x` 的每一个值并通过对比每个 `x.get(key)`（这和 `x[key]` 一样有效率）来寻找最大值。

```python
# 最优解（在此为完整性提供）
def max_key_optimal(x):
    return max(x, key=x.get)
```
```python
>>> max_key_optimal({'a': -1, 'b': 30, 'c': 10, 'd': 500})
'd'
```

你可以在[这里](https://docs.python.org/3/howto/sorting.html#key-functions)阅读 `key` 参数。

接下来，让你的解可以处理词典有重复值的情况。返回成员为对应最大值的键的元组。

```python
def get_maxes(dictionary):
    max_val = max(dictionary.values())
    return tuple(k for k,v in dictionary.items() if v == max_val)

>>> get_maxes(dict(a=1, b=2,c= 2, d=1))
('b', 'c')
```


<!-- #endregion -->
