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
   :description: Topic: String Encoding Exercise, Difficulty: Medium, Category: Practice Problem
   :keywords: function, string, casting, practice problem
<!-- #endraw -->

# 编码为字符串
有时，我们需要在函数中根据输入类型的不同进行不同的操作。本题将考察你对类型，控制流，词典，和更多概念的掌握。

>我们想要将一序列的Python对象编码为一个字符串。以下描述了对每种对象类型的编码规则。每个对象的翻译结果应由 `" | "` 隔开，并在最后函数应返回一个长字符串。

- 如果对象是整数，通过拼写它在10进制下每一位数字将其转化为字符串，如下：
`142` $\rightarrow$ `one-four-two`；`-12` $\rightarrow$ `neg-one-two`。
/ 如果对象是浮点数，通过相同的规则转化其整数部分（通过取整得到）并在后面添加字符串`"and float"`：
`12.324` $\rightarrow$ `one-two and float`。
- 如果对象是字符串，直接附加不需要额外操作。
- 如果对象是任何其它类，附加`'<OTHER>'`。


``` Python
# 范例行为
>>> s = concat_to_str([12,-14.23,"hello", True,
...                    "Aha", 10.1, None, 5])
>>> s
'one-two | neg-one-four and float | hello | <OTHER> | Aha | one-zero and float | <OTHER> | five'
```

**提示**：查阅在[这里](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html)介绍的 `isinstance` 函数来处理不同的数据类型。同时，你可以考虑创建一个辅助函数来将整数编码到我们特殊格式的字符串，因为我们在处理整数和浮点数时都要用到它。在任何情况下，将重复进行的工作抽取为函数都是一个好习惯。你将会需要硬编码（hardcode）每个数字到它英文名字的转化表。

<!-- #region -->
## 解
我们的解总分化为三个简单的函数。`int_to_str` 用来将整数转化成英语单词。`item_to_transcript` 将任何类型的对象转化为根据本题描述的字符串格式。最后，`concat_to_str` 迭代输入列表，调用另外两个辅助函数，将每个对象转化为字符串表达方法，并由 `' | '` 链接这些字符串。

```python
def int_to_str(n):
    """
    接受一个整数并将其格式化为字符串：
        e.g. 142 -> "one-four-two"
             -12 -> "neg-one-two"
    """
    mapping = {"0": "zero", "1": "one", "2": "two", "3": "three",
               "4": "four", "5": "five", "6": "six", "7": "seven",
               "8": "eight", "9": "nine", "-": "neg"}
    return "-".join(mapping[digit] for digit in str(n))

def item_to_transcript(item):
    """ Any -> str """
    if isinstance(item, bool): return '<OTHER>'
    if isinstance(item, int): return int_to_str(item)
    if isinstance(item, float): return int_to_str(int(item)) + " and float"
    if isinstance(item, str): return item
    return '<OTHER>'

def concat_to_str(l):
    """
    将一列表的对象转化成字符串表达方式并粘连在一起

    Parameters
    ----------
    l: List[Any]
        输入对象的列表

    Returns
    -------
    str

    Examples
    --------
    >>> concat_to_str([1, None, 'hi', 2.0])
    one | <OTHER> | hi | two and float
    """
    return " | ".join(item_to_transcript(item) for item in l)
```

我们在本答案中多次使用 `str.join` 函数和生成器理解。请回忆：
```python
"<hi>".join(x for x in some_iterable_of_strings)
```
等值于以下更长的代码：
```python
out = ""
for x in some_iterable_of_strings:
    out += "<hi>" + x
```

`int_to_str` 为了将整数的每位数转化成字符串使用了一个小技巧——它用 `str` 将整数转化为字符串。而字符串是一个[序列](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html)。这允许我们访问整数的每一位数，甚至迭代它们：

```python
# 将整数转化为字符串让我们可以通过索引/迭代来
# 访问它的符号和位数
>>> x = str(-123)
>>> x
'-123'
>>> x[0]
'-'
>>> x[-1]
'3'
```
因此，`"-".join(mapping[digit] for digit in str(n))` 将整数转化为字符串，迭代其每一位数，并使用函数中定义的[词典](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html)来将其转换成对应的单词。

`item_to_transcript` 是一个特别骚操作的函数。首先，它并不使用elif语句，但这：
```python
def item_to_transcript(item):
    """ Any -> str """
    if isinstance(item, bool): return '<OTHER>'
    if isinstance(item, int): return int_to_str(item)
    if isinstance(item, float): return int_to_str(int(item)) + " and float"
    if isinstance(item, str): return item
    return '<OTHER>'
```
和以下完全等值：
```python
def item_to_transcript_alt(item):
    """ Any -> str """
    if isinstance(item, bool): 
        return '<OTHER>'
    elif isinstance(item, int): 
        return int_to_str(item)
    elif isinstance(item, float): 
        return int_to_str(int(item)) + " and float"
    elif isinstance(item, str): 
        return item
    else:
        return '<OTHER>'
```
后者使用了熟悉的[if-elif-else](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/ConditionalStatements.html)语句，且为一个完全合理可接受的版本。但请注意在 `item_to_transcript` 中的[多个返回语句](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Functions.html#Multiple-return-Statements)实现了一样的逻辑。也就是说如果某个条件被满足，它对应的值会被返回，且后面的任何代码都会被跳过。比如说，如果 `item` 是一个整数，那么第二个if条件会运算为真并返回 `int_to_str(item)`，同时，程序立即从函数主体跳出。

在两个函数中选哪个“更好”终归是一个风格上的偏好。你可能注意到了单行的 `if-return` 表达式。这也仅仅是一个风格的选择。这：
```python
if isinstance(item, int): return int_to_str(item)
```
和以下没有区别：
```python
if isinstance(item, int): 
    return int_to_str(item)
```
在 `item_to_transcript` 中使用单行的 `if-return` 表达式强调了该函数像词典一般的行为：代码的格式很好地对应了其功能。但就算如此，这样的单行语句也要谨慎使用。有些人可能认为这是一个可爱的小技巧。我也觉得它很可爱。这代码很可爱。我写的代码很可爱。

最后，你可能在代码中注意到一个像是重复的部分：我们的第一个 `if` 语句当 `item` 为 `True` 或 `False`（布尔值）时返回 `'<OTHER>'`，而我们的最后一行代码在以上所有情况都没有满足时（也就是当 `item` 不是 `bool`，`int`，`float`，或 `str` 类型对象时）会返回 `'<OTHER>'`。为什么我们不将最后一行和第一个 `if` 句段合并呢？不这样做的原因是 `True` 和 `False` 不仅仅是布尔类型的实例，它们也是整数！`True` 的行为类似于整数 `1` 而 `False` 像 `0`：

```python
>>> isinstance(True, int) and isinstance(True, bool)
True

>>> 3*True + True - False
4
```

因此，如果我们没有首先检查布尔值，`True` 和 `False` 会被分别转化成 `'one'` 和 `'zero'`，而不是 `'<OTHER>'`。这是一个比较微妙难抓的边界情况。
<!-- #endregion -->
