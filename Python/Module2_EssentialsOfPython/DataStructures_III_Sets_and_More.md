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
   :description: Topic: Introduction to Sets, Difficulty: Medium, Category: Section
   :keywords: set, complexity, comparison, union, intersection, membership, hashing, lookup, interview preparation
```


# 数据结构（第三部分）：集和集合模组
## 集数据结构
`set` （集）类是一个成员为*互不相同*的对象的*无序*集合。它在以下情况有用：

- 过滤集合中“重复”的成员并仅仅返回其中不重复的成员。
- 快速（$\mathcal{O}(1)$）检查对象是否为成员。
- 高效地对比两个集；如检查一个集是否为另外一个的“子集”（subset）。

集使用“哈希”（hashing）算法来记录它的成员。因此，像词典的键一样，集只能储存*不可变*的对象来保证对象的哈希值不会无效。和我们遇到的其它数据结构不同，*你无法从集中获取单独的成员*。也就是说，没有任何索引或键可以用来获取集中的单个成员。

Python的集也支持了常见的集合代数操作，如并集（union）或交集运算，如下所见。你可以咨询[集的官方说明文档](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)来查看所有可用的集函数。

<!-- #region -->
### 创建集
你可以用以下语法初始化集：`{item1, item2, ...}`。请注意这和使用冒号表达键值对的词典初始化语法不同：

```python
# 初始化有着各种不可变对象的集
>>> {1, 3.4, "apple", False, (1, 2, 3)}
{False, 1, (1, 2, 3), 3.4, 'apple'}
```
你可以使用生成器理解语法来创建一个集：
```python
# 使用集理解来初始化
>>> {i**2 for i in range(5) if i != 3}
{0, 1, 4, 16}
```

同时，和 `list`，`tuple`，和 `dict` 类型类似，`set` 类型可以接受可迭代物来创建集。请注意你必须使用 `set()` 来创建一个空集，因为 `{}` 会创建一个空*词典*：
```python
# 介绍 `set` 类型
>>> type({2, 4, 6})
set

# 使用 `set` 消耗一个可迭代物来创建集
>>> set(range(4))
{0, 1, 2, 3}

# 创建空集
>>> set()  # 如果使用 `{}` 则会创造一个*词典*
set()
```

在创建或往集里面添加内容时会“无视”重复的项目。所以说，*创建集是一个很棒的从集合中抽取独特对象的方法*：
```python
# 通过输入到集中过滤集合中重复的对象
>>> x = [1, 2, 1, 2, 1, "moo", "moo"]
>>> set(x)
{1, 2, 'moo'}
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：创建集**

使用集来找到字符串 `"The cat in the hat"` 中所有的独特的字母。无视所有不是字母的字符，并将所有字母小写化。

</div>

<!-- #region -->
### 集操作
集支持成员检测为（$\mathcal{O}(1)$）以及迭代（$\mathcal{O}(n)$）。注意集是无序的；所以迭代顺序实际上是随机的：
```python
# 检查集成员
>>> 2 in {1, 2, 3}
True

# 迭代集（迭代顺序是随机的）
>>> [i for i in {"a", "b", "c"}]
['b', 'c', 'a']
```
Python也支持集合代数操作：并集（union），交集（intersectino），和等值（equality）和子集（subset）关系。你可以通过操作符或通过显性调用集函数来进行这些操作。

请[查阅Python官方说明文档](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)来阅读集函数的完整列表。

```python
# 演示集对比操作
>>> x = {"a", "b", "c", "d"}
>>> y = {"a", "b", "e"}

# 并集：在x或y或全部两个集当中的项目
>>> x | y  # 等值于 x.union(y)
{'a', 'b', 'c', 'd', 'e'}

# 交集：在x和y中的项目
>>> x & y  # 等值于 x.intersection(y)
{'a', 'b'}

# 差：在x而不在y中的项目
>>> x - y  # 等值于 x.difference(y)
{'c', 'd'}

# 对称差：在x或y但不是全部两个的项目
>>> x ^ y  # 等值于 x.symmetric_difference(y)
{'c', 'd', 'e'}

# 检查set_1是否为set_2的超集（superset）
>>> {1, 2, 3, 4} >= {1, 2}
True

# 检查set_1和set_2是否为相同的集
>>> {1, 2, 3, 4} == {1, 2}
False
```

集是一个*可变*对象；你可以在创建后修改它：
```python
# 集是可变的

# 往 `x` 中添加一个成员
>>> x.add("dog")

# 通过添加可迭代物中的成员来更新 `x`
>>> x.update([1, 2, 3])

# 删除 `x` 的一个成员
>>> x.remove("a")
>>> x
{1, 2, 3, 'b', 'c', 'd', 'dog'}
```
因为集是可变的，它不能作为词典的键，也不能作为另外一个集的成员。Python提供了一个不可变版本的集，叫做 `frozenset`。它有着集所有除了变化集的操作函数：
```python
# `frozenset` 是Python集的不可变版本
>>> frozenset(x)
frozenset({1, 2, 3, 'b', 'c', 'd', 'dog'})
```

<div class="alert alert-info"> 

**经验**：

Python的集的成员为一集合的互不相同，不可变的对象。它是从项目集合中获取不重复的成员很棒的方法。集提供了 $\mathcal{O}(1)$ 的成员检测以及一组对比集的集合代数操作。`frozenset` 是集的不可变版本。
</div>
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**阅读理解：对比集**

设课A和课B的学生，寻找在同时上两个课的同学名单。返回排序过的名字列表。

```python
>>> classA = ["Bohr", "Curie", "David", "Euler", "Fermi", "Feynman", "Gauss", "Heisenberg", "Noether"]
>>> classB = ["Bohm", "Bohr", "Einstein", "Fermi", "Gauss", "Hopper", "Montalcini"]  
```

</div>
<!-- #endregion -->

## collections模组
Python在其[“collections”模组](https://docs.python.org/3/library/collections.html)提供一系列优化过的实用数据结构。本文建议读者花一些时间去细读该模组。我们将在这里快速展示一下其中数据结构的用处。

请查阅[官方说明文档](https://docs.python.org/3/library/collections.html)来阅读所有这些数据结构可用的函数。

<!-- #region -->
### Named-Tuple
一个命名元组（namedtuple）允许你创建成员有名字的元组。因此你可以通过名字或者索引来访问其中的成员。除此之外命名元组和正常元组的行为完全一样。这将帮助你编写干净易读的代码。

比如说，假设你想要储存某事件的3D坐标以及时间，你可以使用命名元组来“根据名字”调用其空间和时间坐标。通过这个方式，代码的读者不需要记住元组的索引3代表着时间坐标这一设定：

```python
# 演示命名元组
>>> from collections import namedtuple

# 定义储存时空坐标的元组。
# 我们如此顺序定义元组的四个成员，名字为
# 'x'，'y'，'z'，和 't'。
>>> space_time_coord = namedtuple("space_time_coord", ['x', 'y', 'z', 't'])

# `r` 是某一个时空点（我们定义的命名元组的一个实例）
>>> r = space_time_coord(1.5, 2.3, 5.1, 100.2)

>>> r.x  # “通过名字”访问x坐标；这比 `r[0]` 更加易懂
1.5

>>> r.y
2.3

>>> r.z
5.1

>>> r.t  
100.2

# 你也可以通过索引/切片来访问其内容
>>> r[3]  
100.2

>>> r[:]
(1.5, 2.3, 5.1, 100.2)
```
<!-- #endregion -->

<!-- #region -->
### 默认词典
默认词典（defaultdict）允许你提供一个Python函数 $f$ 来作为该词典的“默认值”，其默认值为 $f()$ 返回的值。也就是说，每当你试图访问在词典中不存在的键时，词典会创建 $key \rightarrow f()$ 的对应关系而不会导致 `KeyError`：

```python
# 演示 `defaultdict` 的行为
>>> from collections import defaultdict

>>> example_default_dict = defaultdict(list)  # 会将任何不存在的键对应到 `list()`
>>> example_default_dict  # 空默认词典
defaultdict(list, {})

# "apple" 并不是一个存在的键，所以词典将创建默认对应关系 "apple" -> list()
# 并返回对应的值
>>> example_default_dict["apple"]  
[]

# 现在这个对应关系在词典中存在
>>> example_default_dict 
defaultdict(list, {'apple': []})
```

假设你想要将一个词典当作成绩簿来使用，其对应关系为 $名字 \rightarrow 成绩（多个）$。在使用标准词典时，你会需要担心第一次输入某学生成绩的情况：
```python
# 使用标准词典来储存：名字 -> 成绩列表
student = "Ryan" # 译者注：这是原作者的名字
grade = 52  # 我不合格...

# 标准词典的用法
gradebook = {}

# 如果学生不在成绩簿来使用，将学生先作为空列表输入
if student not in gradebook:
    gradebook[student] = []

gradebook[student].append(grade)  # 在列表最后添加学生的成绩
```
默认词典的行为（当你向初始化函数提供 `list` 时）完全满足以上的初始化过程：

```python
# 使用默认词典来储存：名字 -> 成绩列表
>>> gradebook = defaultdict(list)

# 因为 "Susan" 在词典中并不存在
# `list()` 创建一个空列表来作为默认值
# 然后我们可以立刻向其添加她的成绩
>>> gradebook["Susan"].append(84)

>>> gradebook
defaultdict(list, {'Susan': [84]})
```
<!-- #endregion -->

<!-- #region -->
### 计数器
Python的计数器（counter）数据结构旨在计算它遇到的独特的对象数。它基本创建了词典，其对应：$对象 \rightarrow 遭遇次数$。假设你想要了解在某文本中单词的分布，计数器是此应用完美的解决方案：
```python
# 演示 `Counter` 数据结构
>>> from collections import Counter

# 注：我们将会“标准化“我们的文本，使其全部都为小写。
# 我们将会根据空格区分单个单词，并将得到的单词存在列表中
# 如果我们在使用真正的文本，我们也会想要删除标点符号
>>> text_1 = "The cat in the hat"
>>> text_1 = text_1.lower().split()
>>> text_1
['the', 'cat', 'in', 'the', 'hat']

>>> word_distr = Counter(text_1)  # 数 `text_1` 中独特对象的出现次数
>>> word_distr
Counter({'cat': 1, 'hat': 1, 'in': 1, 'the': 2})

# 利用“update”来输入更多的对象
>>> text_2 = "The apple in the tree"
>>> text_2 = text_2.lower().split()
>>> word_distr.update(text_2)
>>> word_distr
Counter({'apple': 1, 'cat': 1, 'hat': 1, 'in': 2, 'the': 4, 'tree': 1})

# 获取2个最常见的单词以及它们的出现次数
>>> word_distr.most_common(2)
[('the', 4), ('in', 2)]

# 获取单词“tree”的出现次数
>>> word_distr["tree"]
1
```

`Counter` 接受任何成员为不可变对象的可迭代物：
```python
>>> Counter([0, 0, "moo", (None, None), (None, None), (None, None)])
Counter({(None, None): 3, 0: 2, 'moo': 1})
```
请查阅[官方说明文档](https://docs.python.org/3/library/collections.html#counter-objects)来阅读 `Counter` 可用的棒棒的函数。
<!-- #endregion -->

<!-- #region -->
### 双队列
像列表一样，Python的双队列（deque）是一个可变的序列性的数据结构。双队列不同的点在于：

 - 修改双队列的开头或者结尾都是 $\mathcal{O}(1)$，而修改列表开头是 $\mathcal{O}(n)$。因此，deque有着独特的函数来利用这一优势，如 `appendleft`。
 - 以上功能的坏处在于*获取双队列靠近中间的对象是* $\mathcal{O}(n)$，而列表和元组是 $\mathcal{O}(1)$。

你可以在[这里](https://docs.python.org/3/library/collections.html#deque-objects)找到双列表的完整功能。

双列表属于Python `collections` 模组的成员，因此你需要导入它：
```python
>>> from collections import deque
>>> my_deque = deque([1,2,3])
>>> my_deque.appendleft(0)
>>> my_deque
deque([0, 1, 2, 3])
```
<!-- #endregion -->

## 官方说明文档链接

- [集和冻集](https://docs.python.org/3/library/stdtypes.html#set-types-set-frozenset)
  - [集教程](https://docs.python.org/3/tutorial/datastructures.html#sets)
- [collections模组](https://docs.python.org/3/library/collections.html)


## 阅读理解答案

<!-- #region -->
**创建集：解**

使用集来找到字符串 `"The cat in the hat"` 中所有的独特的字母。无视所有不是字母的字符，并将所有字母小写化。

我们可以使用内置的字符串函数[isalpha](https://docs.python.org/3/library/stdtypes.html#str.isalpha)和[lower](https://docs.python.org/3/library/stdtypes.html#str.lower)来过滤非字母字符并小写化所有字母。

```python
>>> sentence = "The cat in the hat"
>>> {char.lower() for char in sentence if char.isalpha()}
{'a', 'c', 'e', 'h', 'i', 'n', 't'}
```
<!-- #endregion -->

<!-- #region -->
**对比集：解**

根据课A和课B的名单表，找到同时在两个班的学生名单。返回排序过的名字列表。

```python
>>> classA = ["Bohr", "Curie", "David", "Euler", "Gauss", "Heisenberg", "Noether"]
>>> classB = ["Bohm", "Bohr", "Einstein", "Fermi", "Gauss", "Hopper", "Montalcini"]  
```
我们可以通过使用这两个列表创建集然后求交集来找出两个列表通用的名字。结果为一个集，而集是一种可迭代物。因此我们可以将其输入进内置的函数[sorted](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Iterables.html#Functions-that-act-on-iterables)并返回排序过的名字列表。

```python
>>> sorted(set(classA) & set(classB))
['Bohr', 'Gauss']
```
<!-- #endregion -->
