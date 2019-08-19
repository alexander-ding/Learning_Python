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
   :description: Topic: For-Loop Exercise, Difficulty: Easy, Category: Practice Problem
   :keywords: for loops, list, function, list comprehension, practice problem
```


<!-- #region -->
# 扇出差

设一列表的数字，为每个数字生成它与它在列表后 $n_{fanout}$（叫做**扇出**（fanout）值）个数字之间的差的列表。返回一个列表，其成员为为每一个数字产生的列表（也就是说返回的是成员为列表的列表）。如果列表成员之后的成员数小于 $n_{fanout}$ 的话，计算尽可能多的差。比如说，假设我们想要计算列表 `[3, 2, 4, 6, 1]` 的扇出差，且扇出值为3，那我们应该计算：

 - $3 \rightarrow [2 - 3, 4 - 3, 6 - 3]$
 - $2 \rightarrow [4 - 2, 6 - 2, 1 - 2]$
 - $4 \rightarrow [6 - 4, 1 - 4]$
 - $6 \rightarrow [1 - 6]$
 - $1 \rightarrow []$
 
``` Python
# 范例行为
>>> difference_fanout([3, 2, 4, 6, 1], 3)
[[-1, 1, 3], [2, 4, -1], [2, -3], [-5], []]
```

你会想要先阅读本文对[列表](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Lists)，[索引和切片](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Introducing-Indexing-and-Slicing)列表，以及[for循环](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/ForLoops.html)的讨论，然后再解决本问题。

如果想要获得额外分（以及额外的乐趣！）的话，尝试仅仅使用[列表理解](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html#List-&-Tuple-Comprehensions)来编写你的函数。

## 解：使用for循环计算扇出差
我们将会使用最直接了当的嵌套for循环来解决本问题。外层for循环会迭代列表中的每一个数字。我们这个数字将其称为“基础数”（base number）。我们将想要内层for循环迭代基础数之后的数字，这样我们可以计算基础数和它 $n_{fanout}$ 的邻居们的差。我们将会为每一个基础数重新初始化中间的列表，不然每个差都会被附加到一个很长的列表后。
```python
def difference_fanout(l, fanout):
    """ 返回一列表，每个成员为值和在它后面的成员之间的差
        
        Parameters
        ----------
        l: List[Number]
            基础数的输入列表
            
        fanout: int
            和多少个邻居计算差
        
        Returns
        -------
        List[List[Number]]
    """
    all_fanouts = []  # 会储存每一个扇出差的列表
    for i, base_number in enumerate(l):
        # `base_fanout` 会储存基础数和它之后邻居的差
        base_fanout = []  
        for neighbor in l[i+1: i+1+fanout]:
            base_fanout.append(neighbor - base_number)
            
        all_fanouts.append(base_fanout)
    return all_fanouts
```

请注意我们使用了[enumerate](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Iterables.html#Enumerating-iterables)；这允许我们同时访问我们的基础数（来求差）以及它在列表 `l` 中的索引（来确定它的邻居是哪些）。

你可能会担心我们的内循环会试图迭代列表结尾之后的数字。考虑 `base_number` 为 `l` 的最后成员的情况。那么 `l[i+1: i+1+fanout]` 等值于 `l[len(l): len(l)+fanout]`——这个切片的结尾索引明显超过了 `l` 的长度（假设 `fanout > 0`）。幸运的是，这并不是我们的疏忽。虽然索引列表范围外的成员会导致错误，但是请回忆：[切片会自动将其限制在序列的边界之中](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Handling-out-of-bounds-indices)。也就是说，`l[i+1: i+1+fanout]` 其实行为和 `l[min(i, len(l)-1): min(len(l), i+1+fanout)]` 一样（假设我们仅仅在处理正索引和非空的列表）。因此我们的内循环会自然地限制自身。在 `base_number` 是 `l` 最后成员的情况下，内循环会立刻退出，使得 `base_fanout` 为空。虽然有一点费解，但是这是一个值得记忆的Python切片特征。

## 解：使用列表理解计算扇出差
我们可以审慎地使用嵌套[列表理解](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html#List-&-Tuple-Comprehensions)来简化我们的答案。虽然这语法可能第一眼看上去有一点复杂，但是它允许我们不用担心初始化多个列表并在嵌套for循环中正确的地方向它们附加内容

``` Python
def difference_fanout(l, fanout):
    """ 返回一列表，每个成员为值和在它后面的成员之间的差
        
        Parameters
        ----------
        l: List[Number]
            基础数的输入列表
            
        fanout: int
            和多少个邻居计算差
        
        Returns
        -------
        List[Number]
    """
    return [[neighbor - base for neighbor in l[i+1:i+1+fanout]] 
            for i,base in enumerate(l)]
```

注意最外层的列表理解迭代了基础数，如上一个答案的外层for循环一般，而里层的列表理解达成了和上一个答案的里层for循环一样的目的。

在本解中可能的错误点比前一个答案少，因为其简短性去除了之前答案需要处理的“中间部分”。这应该演示了理解表达式语法的强大。

## 扩展
回忆[前文](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Functions.html#Functions-are-Objects)，函数在幕后实际上就是有一些允许你“调用”函数的特殊操作的对象。这意味着你可以将函数作为参数输入到别的函数中。它极其强大，因为这将允许我们扩大我们函数的运用目的。比如说，我们不需要限制我们的函数来仅仅计算成员和之后项目的**差**；我们可以使用**任何***二元运算*。与其计算差，我们可以计算和或乘积或甚至在字符串列表的情况下粘连字符串。我们有着无限的可能性。

记住这知识之后，我们可以归纳我们的代码。
```Python
def apply_fanout(l, fanout, op):
    """ 返回一个列表，每个成员为值和在它后面的成员之间
        进行二元运算的结果
        
        Parameters
        ----------
        l: List[Any]
            输入列表
        
        fanout: int
            和多少个邻居进行运算
        
        op: Callable[[Any, Any], Any]
            任何用来对 `l` 中对象组成的扇出对操作的二元运算
        
        Returns
        -------
        List[List[Any]]
    """
    return [[op(neighbor, base) for neighbor in l[i+1:i+1+fanout]] 
            for i,base in enumerate(l)]
```
现在，我们可以简单地重写 `difference_fanout` 为：
``` Python
def subtract(a, b): 
    return a - b

def difference_fanout(l, fanout):
    return apply_fanout(l, fanout, subtract)
```
我们可以轻松地将 `subtract` 换成其它函数来达到完全不同的目的。
<!-- #endregion -->
