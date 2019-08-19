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
   :description: Topic: Palindrome Exercise, Difficulty: Easy, Category: Practice Problem
   :keywords: string, palindrome, practice problem
```

<!-- #region -->
# 回文判断
> 回文（palindrome）是一个从左到右和从右到左读起来都一样的字符串。像 `racecar` 和 `Live on time, emit no evil` 都是回文。请注意，我们仅仅考虑字母和数字，且不分大小写。在得到输入字符串后，请判断它是否是回文。

```python
# 范例行为
>>> is_palindrome("Step on no pets!")
True
>>> is_palindrome("'Tis not a palindrome")
False
>>> is_palindrome("Hi, I am Mai Ih")
True
```

## 提示
[str.isalnum](https://docs.python.org/3/library/stdtypes.html#str.isalnum)返回某字符串是否纯粹由数字和字母字符组成（对单个字符的字符串也可以用哦）。
```python
>>> "I love Python".isalnum()
False
>>> "IlovePython".isalnum()
True
```

尝试将其与 `str.lower` 一起使用来过滤所有不是数字字母的符号并将所有字符都小写化，然后再进行回文判断。
<!-- #endregion -->

<!-- #region -->
## 解
本题最简单的解如下。我们使用了 `str.join` 函数以及步距为负的切片：

```python
def is_palindrome(input_str):
    """ 判断某字符串是否为回文。忽略空格，
        字符大小写，以及非字母数字的字符
        
        Parameters
        ----------
        s: str
            输入字符串
        
        Returns
        -------
        bool
    """
    filtered_str = "".join(c.lower() for c in input_str if c.isalnum())
    return filtered_str == filtered_str[::-1]
```

注意 `(c.lower() for c in input_str if c.isalnum())` 使用了[有过滤条件的生成器理解](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Generators_and_Comprehensions.html#Creating-your-own-generator:-generator-comprehensions)。因此，

```python
"".join(c.lower() for c in input_str if c.isalnum())
```
等值于以下的代码块：

```python
filtered_str = ""
for char in input_str:
    if char.isalnum():
        filtered_str += char.lower()
```
生成器表达式不仅更加简短易读，且其对 `str.join` 的使用使得创建新列表更加高效。长代码中每次调用 `filtered_str += c.lower()` 都会在内存中创建一个新字符串，而 `str.join` 在它消耗可迭代物时仅仅使用单个字符串。

接下来，[请回忆](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/SequenceTypes.html#Slicing)，`seq[::-1]` 使用的步距-1的切片将会返回序列的*反向*版本。因此，`filtered_str == filtered_str[::-1]` 允许我们对比 `filtered_str` 中的第一个字符和原本字符串的最后一个，如此继续下去。因此，这等值于：

```python
is_equal = True
for i in range(len(filtered_str)//2): # 请回忆： 5//2 -> 2, 6//2 -> 3
    if filtered_str[i] != filtered_str[-(i+1)]:
        is_equal = False
        break
```

使用切片来进行这个对比的唯一坏处在于它需要复制一份 `filtered_str`，而显性的for循环不需要。

在这里指出的效率考量仅仅在 `is_palindrome` 可能是我们整体代码的效率瓶颈时才值得进行。虽然我们希望读者能够发展编写高效Python代码的直觉，但我们也不推荐读者为了过早地优化代码以至于代码变得难懂。
<!-- #endregion -->
