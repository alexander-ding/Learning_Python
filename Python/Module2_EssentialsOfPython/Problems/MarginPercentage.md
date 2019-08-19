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
   :description: Topic: Within Margin Exercise, Difficulty: Medium, Category: Practice Problem
   :keywords: function, control flow, comparisons, practice problem
```

<!-- #region -->
# 错误范围内百分比

>我们需要设计一个算法来测试某工厂制造的零件有多少百分比属于设计规格的可接受范围内。函数接受一列表的零件度量，一列表的设计规格的理想度量，以及一个可以接受的错误范围。返回有多少部分的零件属于安全范围内（`<=`）。

``` Python
# 范例行为
>>> within_margin_percentage(desired=[10.0, 5.0, 8.0, 3.0, 2.0],
...                          actual= [10.3, 5.2, 8.4, 3.0, 1.2],
...                          margin=0.5)
0.8
```

注意 $4/5$ 的值都处于错误范围内：$1.2$ 离 $2$ 差的超过 $0.5$。

完成以下的函数；请考虑 `desired` 和 `actual` 是空列表的边缘情况。

```python
def within_margin_percentage(desired, actual, margin):
    """ 计算多少部分的值属于期望值的错误范围内
        
        Parameters
        ----------
        desired: List[float]
            期望值
        
        actual: List[float]
            对应的实际值
            假设 `len(actual) == len(desired)`
        
        margin: float
            可接受的错误范围
        
        Returns
        -------
        float
            多少部分的值满足 |actual - desired| <= margin
    """
    # 你的代码
    pass
```

你会需要熟悉[对比操作符](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/ConditionalStatements.html#Comparison-Operations)，[控制流](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Introduction.html)，和[列表索引](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Introducing-Indexing-and-Slicing)来解决本问题。

## 解
本题可以通过简单地循环实际和可接受值对并数多少对属于安全范围内来解决：
``` Python
def within_margin_percentage(desired, actual, margin):
    """ 计算多少部分的值属于期望值的错误范围内
        
        Parameters
        ----------
        desired: List[float]
            期望值
        
        actual: List[float]
            对应的实际值
            假设 `len(actual) == len(desired)`
        
        margin: float
            可接受的错误范围
        
        Returns
        -------
        float
            多少部分的值满足 |actual - desired| <= margin
    """
    count = 0  # 计算多少值属于范围内
    total = len(desired)
    for i in range(total):
        if abs(desired[i] - actual[i]) <= margin:
            count += 1  # 等值于 `count = count + 1`
    return count / total if total > 0 else 1.0
```

注意我们处理了 `desired` 和 `actual` 是空列表的边缘情况：[单行if-else语法](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/ConditionalStatements.html#Inline-if-else-statements) `count / total if total > 0 else 1` 将在 `total` 为0时返回1:
```python
>>> within_margin_percentage([], [], margin=0.5)
1.0
```
这个行为按理说是这种情况下的正确行为（因为没有任何值在安全范围外面）。如果我们没有处理这个边缘情况，`within_margin_percentage([], [], margin=0.5)` 会导致 `ZeroDivisionError`。

你也可能会使用内置的 `sum` 函数和[生成器理解](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html#Creating-your-own-generator:-generator-comprehensions)来过滤在安全范围外的项目对：

```python
def within_margin_percentage(desired, actual, margin):
    total = len(desired)
    count = sum(1 for i in range(total) if abs(actual[i] - desired[i]) <= margin)
    return  count / total if total > 0 else 1.0
```

后者是否强于前者是有待商榷——这取决于你和任何其它会阅读你的代码的人有多熟悉生成器理解语法。
<!-- #endregion -->
