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
   :description: Topic: Dictionary Merge Exercise, Difficulty: Easy, Category: Practice Problem
   :keywords: dictionary, merge, practice problem
<!-- #endraw -->

<!-- #region -->
# 合并两个词典
> 合并两个[词典](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html#数据结构（第二部分）：词典)，在词典有着共同的键时使用*更大*的对应值。

以下为一个例子：

```python
# 合并两个词典，在遇到共同的键时优先使用更大值
>>> dict1 = {'bananas': 7, 'apples': 3, 'pears': 14}
>>> dict2 = {'bananas': 3, 'apples': 6, 'grapes': 9}
>>> merge_max_mappings(dict1, dict2)
{'bananas': 7, 'apples': 6, 'pears': 14, 'grapes': 9}
```

编写一个接受两个词典并如上合并它们的函数。[词典的键不一定是字符串](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html#词典可以储存什么？)，且它们的值应该是任何可以被排序的数据类型（也就是说可以被不等操作符 `<`，`>` 等对比）。
<!-- #endregion -->

<!-- #region -->
## 简单而有错的解
让我们首先编写一个直接但实际上有错误的解。以下函数将会正确地合并两个输入词典且总体而言写得挺好的，但是在其中潜伏着邪恶...你可以找出该函数可能的错误行为么？

```python
def buggy_merge_max_mappings(dict1, dict2):
    # 创建一个输出词典，其成员包含 `dict1` 的对应关系
    merged = dict1

    # 在以下情况之一满足时向 `merged` 添加 dict2 里的对应关系：
    #   - 键在 `merged` 中不存在
    #   - dict2 的值更大
    for key in dict2:
        if key not in merged or dict2[key] > merged[key]:
            merged[key] = dict2[key]
    return merged
```
让我们首先注意该函数做对的地方。回忆[迭代词典](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html#检查词典)将会一个一个返回词典键的事实。因此，`for key in dict2` 会循环迭代 `dict2` 中所有的键。然后当 `dict2` 的键在 `merged` 中不存在或当 `dict2` 的值大于已经存储的值时，我们在 `merged` 中更新 `dict2` 的键值。因为 `merged` 初始化为 `dict1` 中的对应关系，这是一个正确的根据最大值合并两个词典的方法。

我们这个函数的问题在于我们将 `dict2` 合并*进*了 `dict1`，而不是将两个词典合并成一个*新*词典。词典是[可变](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Variables_and_Assignment.html#多个变量引用一个可变对象)对象，而 `merged = dict1` 语句仅仅赋值一个变量来引用 `dict1`，而不是创建该词典的新的复制品。因此调用该函数将会*改变* `dict1` 的内态，如下所见：
```python
>>> exam_1 = dict(Alice=99, Bob=87, Cindy=65)  # 等值于 {'Alice': 99, 'Bob': 87, 'Cindy': 65}
>>> exam_2 = dict(Alice=77, Bob=90, Cindy=78)
>>> buggy_merge_max_mappings(exam_1, exam_2)
{'Alice': 99, 'Bob': 90, 'Cindy': 78}
```
请注意 `exam_1` 的值也被改变了，且其和函数输出一样：
```python
>>> exam_1
{'Alice': 99, 'Bob': 90, 'Cindy': 78}
```

重申一遍，这是因为在我们函数开头的语句 `merged = dict1` *仅仅创建了一个引用*指向 `dict1`，而不是复制了它的值。在以上范例中 `exam_1` 储存了每个学生的第一轮考试成绩，而在输入到函数内后它现在储存了每个学生在两轮考试中的最大值！

虽然我们大概率会测试我们的函数是否返回了正确的合并词典，但我们很可能不会去测试它是否不会修改其输入参数。这是编写正确代码很重要的一课。
<!-- #endregion -->

<div class="alert alert-info">

**经验**：

- 注意[对象可变性](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Variables_and_Assignment.html#可变和不可变对象)并小心不要无意在函数中修改输入参数或全球域（global scope）变量。
- 在测试函数时，包含一个检查函数是否修改输入的测试。

</div>

<!-- #region -->
## 简单而正确的解

我们可以很轻松地解决以上函数的错误；将 `merged = dict1` 修改为 `merged = dict(dict1)` 或 `merged = dict1.copy()` 将保证 `merged` 引用一个*新*词典。这样我们可以随意更新它：

```python
def simple_merge_max_mappings(dict1, dict2):
    """ 根据对应关系中最大的值来合并两个词典。

    Parameters
    ----------
    dict1 : Dict[Any, Comparable]
    dict2 : Dict[Any, Comparable]

    Returns
    -------
    Dict[Any, Comparable]
        合并的词典
    """
    merged = dict(dict1)
    for key in dict2: 
        if key not in merged or dict2[key] > merged[key]:
            merged[key] = dict2[key]
    return merged
```

注意我们使用的变量名字的简单性和描述性（比如说我们在迭代词典键时使用了函数名 `key`）。这个习惯以及一个好的docstring会使得我们的代码很易读，易懂，和易纠错。同时注意，我们的代码是广义的：这个函数没有对词典的键进行任何假设——词典的键没必要一定是字符串或任何其它某种类型。相同的，函数对词典的值的唯一要求就是它们必须可以相互比较。我们函数的docstring反映了这一点。

请注意我们条件语句顺序的重要性：

```python
if key not in merged or dict2[key] > merged[key]:
```

当我们将其中的项顺序反向会有什么区别？

```python
if dict2[key] > merged[key] or key not in merged:
```

这个反向顺序的问题在于当某个键不存在于 `merged` 中时，`dict2[key] > merged[key]` 会导致 `KeyError`。使用原本的顺序则会导致 `key not in merged` 计算为 `True`；这使得程序跳过会报错的条件语的后半部分并直接返回 `True`（向自己证明 `True or <随便什么>` 永远都会返回 `True`）。
<!-- #endregion -->

<!-- #region -->
## 小改进

如果你的代码大量使用词典合并，且其效率为整个程序的效率瓶颈，那么我们可以实现一个小改进。

假设我们两个词典的大小差距很大：比如说 `dict1` 有一个键，而 `dict2` 有一万个键，那么迭代两个词典中更小的那个会让我们花得时间更少（具体原因可以思考一下；请回忆算法复杂度）。我们可以简单地实现这个小改进；只要将 `merged` 设为两个词典中更大的那个并迭代另外那个词典就行了：

```python
def opt_merge_max_mappings(dict1, dict2):
    """ 根据对应关系中最大的值来合并两个词典。

    Parameters
    ----------
    dict1 : Dict[Any, Comparable]
    dict2 : Dict[Any, Comparable]

    Returns
    -------
    Dict[Any, Comparable]
        合并的词典
    """
    # 我们将会迭代 `other` 来修改 `merged`
    merged, other = (dict1, dict2) if len(dict1) > len(dict2) else (dict2, dict1) 
    merged = dict(merged)

    for key in other: 
        if key not in merged or other[key] > merged[key]:
            merged[key] = other[key]
    return merged
```

在这里，我们通过使用[单行if-else语句](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/ConditionalStatements.html#单行的if-else语句)和[可迭代物解包](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Iterables.html#“解包”可迭代物)来在增加额外逻辑的同时不让代码太过复杂。这行代码：

```python
merged, other = (dict1, dict2) if len(dict1) > len(dict2) else (dict2, dict1) 
```

和以下等值：
```python
if len(dict1) < len(dict2):
    merged = dict1
    other = dict2
else:
    merged = dict2
    other = dict1
```

我们可以在Jupyter记事本或IPython控制台种使用 `timeit` [魔法命令](https://ipython.readthedocs.io/en/stable/interactive/magics.html)（magic command）来记录我们函数花的时间（注：每次 `timeit` 都必须在不同的记事本小节中运行，且 `%%timeit` 必须是小节的第一行命令）。

```python
a = {}
b = dict(zip(range(10000), range(10000)))  # {1 : 1, 2: 2, ..., 9999:9999}
```

```python
%%timeit
simple_merge_max_mappings(a, b)
2.05 ms ± 90.6 µs per loop (mean ± std. dev. of 7 runs, 100 loops each)
```

```python
%%timeit
opt_merge_max_mappings(a, b)
455 µs ± 12.9 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
```

考虑到我们人为建立的例子的极端性，这个速度提升还是比较小的。
<!-- #endregion -->

<!-- #region -->
## 额外问题：合并任意多的词典

我们的函数没有任何理由仅仅合并两个词典。它应该可以合并任意多的词典：

```python
>>> a = dict(a=0, b=100, c=3)
>>> b = dict(a=10, b=10)
>>> c = dict(c=50)
>>> d = dict(d=-70)
>>> e = dict()
>>> merge_max_mappings(a, b, c, d, e)
{'a': 10, 'b': 100, 'c': 50, 'd': -70}
```

在你写答案之前，请考虑以下：
1. 你该如何编写函数签名使其能够接受任意多的词典呢？
2. 你的函数该如何处理0个输入的情况？1个输入呢？
<!-- #endregion -->

<!-- #region -->
## 解
为了解决问题#1，我们将会想要在函数的签名中使用 `*args` 语法来[接受任意多的词典](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Functions.html#支持任意多的位置参数)。因此，所有输入到我们函数中的词典都会被打包成一个元组并可以通过 `args`（或我们使用的随便什么变量名）来访问。

为了解决问题#2，我们可以在收到0个输入时返回空词典。同时，请注意，收到1个输入的情况可能比我们想象的更微妙一点。我们将在展示解之后再多做讨论。

本解创建了一个空词典 `merged`，迭代每一个输入词典的对应关系，并执行像之前一样的合并操作：

```python
def merge_max_mappings(*dicts):
    """ 根据对应关系中的最大值合并任意多的词典。

    Parameters
    ----------
    dicts : Dict[Any, Comparable]

    Returns
    -------
    Dict[Any, Comparable]
        合并的词典
    """
    merged = {}
    for d in dicts:  # `dicts` 是一个储存输入词典的元组
        for key in d:
            if key not in merged or d[key] > merged[key]:
                merged[key] = d[key]
    return merged
```
<!-- #endregion -->

<!-- #region -->
### 处理零个输入
请注意，我们的函数在没有收到任何输入时会返回一个空词典。这是因为 `dicts` 将会是一个空元组，因此我们的循环会直接退出并返回一个空的 `merged`：
```python
>>> merge_max_mappings()
{}
```
虽然你可能会想要在这种情况下返回 `None`，但是返回空词典是一个更加可取的行为，因为使用这个函数的代码如此只需要处理一种输出类型。同时，我们函数的docstring保证它将返回一个词典，而我们应该永远满足这个协议。

### 处理一个输入
当我们函数只收到一个输入词典时，它最后会返回一个该词典的（浅层）复制品。假设我们试图耍聪明并让我们的代码在只收到单个词典时直接返回该词典，如：

```python
def bad_merge_max_mappings(*dicts):
    if len(dicts) == 1:
        return dicts[0]
    merged = {}
    for d in dicts:
        for key in d:
            if key not in merged or d[key] > merged[key]:
                merged[key] = d[key]
    return merged
```

这个解有什么问题呢？这个问题和我们以上考虑过的类似；我们的函数永远应该返回一个和输入参数无联系的词典，也就是说修改合并的词典不应该对输入词典有任何影响。在我们收到单个词典的时候，我们打破了这个规则——在这里“合并的”词典仅仅是输入词典的引用。任何对输出词典的操作都会修改输入词典，且反之亦然。这个意料之外的行为可能会导致你代码中出现很难找出的错误！

当你的简单代码已经足够全面合理时，你没有必要专门处理这样的特殊情况。
<!-- #endregion -->

## 额外挑战
- 为你的函数编写输入键不是字符串且值不是数字的词典的测试。
- 当你想用的值不是最大值，而是某个其它标准决定的值时该怎么做？尝试重写你的函数使其可以接受一个对比函数。请回忆，函数在定义后就是一个对象，其本质与整数和字符串一样——它们都可以作为参数输入进其它函数中。

<!-- #region -->
以下代码是正确的，但其质量极差：

```python
def gross_merge_max_mappings(*dicts):
    """合并词典"""
    merged = {}
    for i in range(len(dicts)):
        for j in dicts[i]: 
            if not (j in list(merged)):
                merged[j] = dicts[i][j]
            elif dicts[i][j] > merged[j]:
                merged[j] = dicts[i][j]
            else:
                continue
    return merged
```
将以上代码和我们提供的解进行对比，并枚举在此所有的风格错误。体会一下我们编写的代码相比之下有多么简单易懂，并向自己承诺永远不要编写这样的垃圾代码。
<!-- #endregion -->
