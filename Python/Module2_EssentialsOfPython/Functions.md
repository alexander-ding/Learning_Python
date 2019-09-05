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
   :description: Topic: Functions and Function Signatures, Difficulty: Medium, Category: Section
   :keywords: functions, *args, **kwargs, signature, default parameter, docstring, return, vowel count, syntax, basics
```

<!-- #region -->
# 函数基础
<div class="alert alert-warning">

**注**：

在本资源各处会有一些阅读理解练习。这些题目旨在帮助读者活学活用文中的知识点。练习题的答案可以在本页底部找到。
</div>

定义函数将允许我们包装代码片段，描述进入和离开其代码的信息。你可以在各种情况下重复使用这个“代码胶囊”。比如说，假设你想要数一个字符串中有多少元音，你可以定义以下的函数来达到目的：

```python
def count_vowels(in_string):
    """ 返回 `in_string` 中有多少元音 """
    num_vowels = 0
    vowels = "aeiouAEIOU"
    
    for char in in_string:
        if char in vowels:
            num_vowels += 1  # 等值于 num_vowels = num_vowels + 1
    return num_vowels
```

执行此代码会定义*函数* `count_vowels`。这个函数会期待一个对象作为*输入参数*（input argument），由 `in_string` 代名，并会*返回*（return）该对象中元音的数量。使用 `count_vowels` 并向其输入一个对象的过程叫做*调用*（call）该函数：

```python
>>> count_vowels("Hi my name is Ryan")
5
```

最棒的一点在于你可以重复使用这个函数！

```python
>>> count_vowels("Apple")
2

>>> count_vowels("envelope")
4
```

在本节中，我们将会了解一下定义和调用Python函数的语法。
<!-- #endregion -->

<div class="alert alert-info">

**定义**：

Python**函数**（function）是一个包装代码的对象。*调用*（call）函数将会执行包装的代码并*返回*（return）一个对象。你可以定义函数使其接受*参数*（argument），也就是输入进包装的代码的对象。 
</div>

<!-- #region -->
## `def` 语句
类似于 `if`，`else`，和 `for`，Python保留 `def` 语句来预示函数（以及我们会在之后讨论的几个其它东西）的定义。以下是定义Python函数的一般形式：

```
def <function name>(<function signature>):
    """ 说明字符串 """
    <encapsulated code>
    return <object>
```

- `<function name>` 可以是任何合法的变量名，在这之后*必须*有一对括号和一个冒号。
- `<function signature>` 描述函数的输入参数。如果函数不接受任何变量，你可以在这一部分留白（你仍然需要输入括号，但是光光括号并不会包装任何参数）。
- 说明字符串（documentation string）（一般叫做“docstring”）可以有多行长并解释了函数的目的。它是可选的。
- `<encapsulated code>` 可以包含一般的Python代码，且其由相对于 `def` 语句的缩进在标记。
- `return` 当被包装的代码遇到这个标示后，会导致函数返回标记的对象并立刻终止函数的执行。
 
Python同样保留 `return` 语句来表达函数的终止；如果遇到了 `return`，那程序将立即终止函数的执行并返回 `return` 右边的对象。

请注意，和if语句和for循环一样，`def` 语句必须由冒号结尾，且函数的主体代码必须[由空格限定](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Introduction.html#Python使用空格来限定区域)：
<!-- #endregion -->

<!-- #region -->
```python
# 错误的缩进
def bad_func1():
x = 1
    return x + 2
```
***
```python
# 错误的缩进
def bad_func2():
    x = 1
return x + 2
```
***
```python
# 漏了冒号
def bad_func3()
    x = 1
    return x + 2
```
***
```python
# 漏了括号
def bad_func4:
    x = 1
    return x + 2
```

***
```python
# 这个没问题
def ok_func():
    x = 1
    return x + 2
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：编写简单函数**

写一个名为 `count_even` 的函数。它应该接受一个名为 `numbers` 的参数，其为一个成员为整数的可迭代物。函数应该返回列表中的偶数数量。注意包含一个合理的docstring。

</div>

<!-- #region -->
## `return` 语句
一般来讲，`return` 语句可以返回任何Python对象。同时，你可以使用一个**空的** `return` 语句或在函数中完全不使用**返回**语句。这两种情况下，*函数都会返回 `None` 对象*。

```python
# 本函数返回 `None`
# 一个“空”的返回语句
def f():
    x = 1
    return
```

```python
# 本函数返回 `None`
# 完全没有用返回语句
def f():
    x = 1
```
<!-- #endregion -->

<!-- #region -->
所有Python函数都会返回*某物*。就算是内置的 `print` 函数在打印后也会返回 `None`！

```python
# `print` 函数返回 `None`
>>> x = print("hi")
hi

>>> x is None
True
```
<div class="alert alert-warning">

**警告**！

请注意不要*错误地*漏掉或使用空的返回语句。你任然能调用你的函数，但是它在任何情况下都会返回 `None`！
</div>

函数并不是一定要有返回语句之外的额外代码的。比如说，我们可以使用`sum` 和生成器理解（见本模组前一节）来简短我们的 `count_vowels` 函数：

```python
# 你可以直接描述函数返回的对象
def count_vowels(in_string): 
    """ 返回 `in_string` 中元音的数量 """
    return sum(1 for char in in_string if char in "aeiouAEIOU")
```
<!-- #endregion -->

<!-- #region -->
### 多个 `return` 语句
你可以在一个函数内使用多余一个的 `return` 语句。在处理边缘情况或优化代码时这可能会有用。假设你想要你的函数通过[泰勒级数](https://en.wikipedia.org/wiki/Taylor_series#Exponential_function)（Taylor series）来模糊计算$e^{x}$，当 $x = 0$ 时函数应该立刻返回 `1.0`：

```python
def compute_exp(x):
    """ 使用泰勒级数来计算 e^x """
    if x == 0:
        return 1.0

    from math import factorial
    return sum(x**n / factorial(n) for n in range(100))
```

如果 `x==0` 为 `True`，那么程序将执行第一个 `return` 语句，返回 `1.0` 并立刻“终止”该函数，而不执行其下方的代码。
<!-- #endregion -->

如上所述，就算后面有额外的代码，`return` 语句也会导致函数立刻终止而不执行之后的代码。*在一次函数调用中，程序不可能遇到多个 `return` 语句*。因此，如果你想要返回多个项目，那么你的函数必须返回单个包含这些项目的容器，如列表会元组。

<!-- #region -->
```python
# 函数返回多个项目
def bad_f(x): # 函数名字大意为：不好的函数
    """ 返回 x**2 和 x**3"""
    return x**2 
    # 此代码永远都不会被执行！
    return x**3

def good_f(x): # 函数名字大意为：好的函数
    """ return x**2 and x**3"""
    return (x**2, x**3)
```
```python
>>> bad_f(2)
4

>>> good_f(2)
(4, 8)
```
<!-- #endregion -->

<!-- #region -->
## 单行函数
你可以在一行内定义只有一个返回语句的函数：

```python
def add_2(x):
    return x + 2
```

可以被重写为：

```python
def add_2(x): return x + 2
```

尽量只在函数极其简单到不需要docstring就能理解时才使用此功能。不要滥用。
<!-- #endregion -->

<!-- #region -->
## 参数
你可以在函数签名（function signature）部分通过一序列由逗号隔开的变量名来描述函数的*位置性*参数（positional argument）。比如说，以下代码为函数 `is_bounded` 提供了 `x`，`lower`，和 `upper` 三个输入参数：

```python
def is_bounded(x, lower, upper):
    return lower <= x <= upper
```

你可以用几种不同的方法来像函数提供其参数：

### 通过位置提供参数
输入到 `is_bounded` 中的对象会根据它们的位置来赋值给它的输入变量。也就是说，`is_bounded(3, 2, 4)` 会赋值 `x=3`，`lower=2`，和 `upper=4`，根据函数输入参数的位置顺序来对应：

```python
# 计算：2 <= 3 <= 4
# 通过位置来提供输入
>>> is_bounded(3, 2, 4)
True
```

向函数输入太少或太多参数会导致 `TypeError`
```python
# 输入太少：报错
is_bounded(3)

# 输入太多：报错
is_bounded(1, 2, 3, 4)
```

### 通过关键词提供参数
在顺序不重要的时候，你也可以在给函数输入参数时提供它们的关键词（也就是名字）来对应对象和参数。这会帮助你编写易读和灵活的代码：
```python
# 计算：2 <= 3 <= 4
# 通过参数名字来提供输入对象
>>> is_bounded(lower=2, x=3, upper=4)
True
```

你可以混合关键词参数和位置参数，但位置参数应该在前：

```python
# 计算：2 <= 3 <= 4
# `x` 是位置参数
# `lower` 和 `upper` 关键词参数
>>> is_bounded(3, upper=4, lower=2)
True
```

请注意，如果你提供了一个关键词参数，那么它之后所有的参数都应该是关键词参数：

```python
# 你不可以在关键词参数后使用位置参数
>>> is_bounded(3, lower=2, 4)
SyntaxError: positional argument follows keyword argument
```
<!-- #endregion -->

<!-- #region -->
### 有默认值的参数
你可以提供参数的默认值。如果用户在调用函数时没有提供此参数，那么函数将会使用定义的默认值。请回忆我们的 `count_vowels` 函数。假设我们想要提供将“y”算为元音的选择，但因为我们知道人们一般不会将“y”算为元音，所以我们可以默认不算“y”：

```python
def count_vowels(in_string, include_y=False): 
    """ 返回 `in_string` 中元音的数量"""
    vowels = "aeiouAEIOU"
    if include_y:
        vowels += "yY"  # 将 "y" 加到元音列表中
    return sum(1 for char in in_string if char in vowels)
```

现在，如果在调用 `count_vowels` 时只有 `in_string` 被提供了，那么 `include_y` 会使用默认值 `False`：

```python
# 使用默认值：不将y算为元音
>>> count_vowels("Happy")
1
```

我们可以提供默认值之外的输入：
```python
# 不使用不认值：将y算为元音
>>> count_vowels("Happy", True)
2

# 你依然可以通过名字来描述输入参数
>>> count_vowels(include_y=True, in_string="Happy")
2
```

在函数签名中，有默认值的输入参数必须在所有位置参数之后：
```python
# 这没问题
def f(x, y, z, count=1, upper=2):
    return None
```

```python
# 这会导致语法错误
def f(x, y, count=1, upper=2, z):
    return None
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：函数和参数s**

编写一个函数 `max_or_min`。它接受两个位置参数 `x` 和 `y`（将会被赋值为数字）以及一个 `mode` 变量，其默认值为 `"max"`。

此函数应该根据 `mode` 返回 `min(x, y)` 或 `max(x, y)`。如果 `mode` 既不是 `"max"` 也不是 `"min"` 的时候让函数返回 `None`。

包含一个描述性的docstring。

</div>

<!-- #region -->
### 支持任意多的位置参数
Python为我们提供了定义可以接受任意多位置参数的函数的语法。使用 `def f(*<var_name>)` 语法来定义这类输入。

```python
# * 符号表明了调用 `f` 时可以向 `args` 
# 输入任意多的参数。
def f(*args):
    #  所有向 `f` 输入的参数都会被“打包”成一个元组
    #  并赋值给变量 `args`。
    # `f()` 会赋值 `args = tuple()`
    # `f(x, y, ...)` 会赋值 `args = (x, y, ...)`
    return args
```

因为Python不能提前知道 `f` 会收到多少个参数，因此它所有的输入参数都会被*打包成一个元组*并赋值给变量 `args`：

```python
# 向 `f` 输入0个参数
>>> f()
()

# 向 `f` 输入1个参数
>>> f(1)           
(1,)

# 向 `f` 输入3个参数
>>> f((0, 1), True, "cow")  
((0, 1), True, "cow")
```

你可以将此语法与位置参数和默认参数混合使用。任何在被打包的变量后的变量*必须提供参数名*：
```python
def f(x, *seq, y):
    print("x is: ", x)
    print("seq is: ", seq)
    print("y is: ", y)
    return None
```
```python
>>> f(1, 2, 3, 4, y=5)  # 必须提供 `y` 的名字
```
```
x   是： 1
seq 是： (2, 3, 4)
y   是： 5
```
```python
>>> f("cat", y="dog")  # 没有输入任何额外的位置参数
```
```
x   是： "cat"
seq 是： ()
y   是： "dog"
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：任意多的参数**

编写一个名为 `mean` 的函数，其接受任意多的数字参数并计算所有数字的平均值。如，`mean(1, 2, 3)` 应返回 $\frac{1 + 2 + 3}{3} = 2.0$

如果没有收到任何输入，函数应返回 `0.`。记得测试你的函数并编写一个docstring。

</div>

<!-- #region -->
如我们所见，在函数定义的签名中使用 `*` 意味着*将任意多的参数打包成一个元组*。同时，在调用函数时 `*` 也意味着*解包可迭代物*并将其作为位置参数输入到函数中：

```python
# 在调用函数时使用 `*` 来解包可迭代物，并将其
# 成员作为位置参数输入到函数中

def f(x, y, z):
    return x + y + z

>>> f(1, 2, 3)
6

# `*` 意味着：解包 [1, 2, 3] 的内容并将每个
# 物件分别输入为x，y，和z
>>> f(*[1, 2, 3])  # 等值于：f(1, 2, 3)
6
```
<!-- #endregion -->

在以下范例中，我们使用 `*` 来：

   1. 定义一个接受任意多参数并将其打包为元组的函数
   2. 通过解包可迭代物来调用函数并输入任意多的参数

<!-- #region -->
```python
def number_of_args(*args):
    return len(args)
```
```python
>>> number_of_args(None, None, None, None)
4

>>> some_list = [1, 2, 3, 4, 5]

# 将列表本身作为唯一的参数输入
>>> number_of_args(some_list)
1

# 将列表的5个成员解包并作为多个参数输入进函数
>>> number_of_args(*some_list)
5
```
<!-- #endregion -->

<!-- #region -->
### 支持任意多的关键词参数
我们可以使用 `def f(**<var_name>)` 语法来定义一个接受任意多的*关键词*（keyword）参数的函数。

注意单个星号 `*` 用来代表任意多的*位置*参数，而 `**` 代表着任意多的*关键词*参数。

```python
# ** 符号意味着在调用 `f` 时可以向 `args` 输入
# 任意多的关键词参数。
def f(**args):
    # 所有输入到 `f` 的关键词参数都会被“打包”成一个词典
    # 并赋值到变量 `args` 中
    # `f()` 会赋值 `args = {}`（一个空词典）
    # `f(x=1, y=2, ...)` 会赋值 `args = {"x":1, "y":2, ...}`
    return args
```

因为Python无法预先知道 `f` 会收到多少个关键词参数，所以所有收到的关键词参数都会被打包成一个*词典*（dictionary）。词典允许你通过关键词（将其转化成字符串）查询并设置关键词对应的值。这个词典被赋值到变量 `args` 上。我们会在[后面一节](https://cn.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/DataStructures_II_Dictionaries.html)专门讨论词典。

```python
>>> f()            # 向 `f` 输入0个参数
{}

>>> f(x=1)           # 向 `f` 输入1个参数
{'x': 1}

>>> f(x=(0, 1), val=True, moo="cow")  # 向 `f` 输入3个参数
{'moo': 'cow', 'val': True, 'x': (0, 1)}
```

这一语法可以和位置参数和默认参数混合使用。在函数定义签名中，`**` 后不能有任何额外的参数：
```python
def f(x, y=2, **kwargs):
    print("x 是：", x)
    print("y 是：", y)
    print("kwargs 是：", kwargs)
    return None

# 译者注：kwargs是keyword arguments的缩写，也就是关键词参数。
# 译者注：此命名方式算是传统，所以尽量遵循
```
```python
# passing arbitrary keyword arguments to `f`
>>> f(1, y=9, z=3, k="hi")
```
```
x 是： 1
y 是： 9
kwargs 是： {'z': 3, 'k': 'hi'}
```
```python
# 没有输入任何额外的关键词参数
>>> f("cat", y="dog")  
```
```
x is:  cat
y is:  dog
kwargs is:  {}
```

以下函数接受任意多的位置参数*和*任意多的关键词参数：

```python
# 接受任意多的位置和关键词参数
def f(*x, **y):
    # 所有位置参数都打包成元组 `x`
    # 所有关键词参数都打包成词典 `y` 
    print(x)  
    print(y)
    return None

>>> f(1, 2, 3, hi=-1, bye=-2, sigh=-3)
```
```
(1, 2, 3)
{'hi': -1, 'bye': -2, 'sigh': -3}
```
<!-- #endregion -->

<!-- #region -->
如上所见，在函数定义的签名中 `**` 意味着*将任意多的关键词参数打包成一个词典*。同时，在调用函数时 `**` 意味着*解包词典*并将其的键值对（key-value pair）作为函数关键词参数输入：

```python
# 在调用函数时使用 `**` 来解包词典并将其成员作为
# 关键词参数输入到函数中
def f(x, y, z):
    return 0*x + 1*y + 2*z

>>> f(z=10, x=9, y=1)
21

>>> args = {"x": 9, "y": 1, "z": 10}
>>> f(**args)  # 等值于：f(x=9, y=1, z=10)
21
```
<!-- #endregion -->

在以下范例中，我们用 `**` 来：

   1. 定义一个接受任意多关键词参数的函数并将其打包成词典。
   2. 调用函数并通过解包词典向其输入任意多的关键词参数。

<!-- #region -->
```python
def print_kwargs(**args):
    print(args)
```
```python
>>> print_kwargs(a=1, b=2, c=3, d=4)
{'a': 1, 'b': 2, 'c': 3, 'd': 4}

>>> some_dict = {"hi":1, "bye":2}

# 解包词典的键值对并将其作为关键词参数输入到函数中
>>> print_kwargs(a=2, umbrella=True, **some_dict)
{'a': 2, 'umbrella': True, 'hi': 1, 'bye': 2}
```
<!-- #endregion -->

<!-- #region -->
## 函数也是对象
在定义之后，函数和任何其它Python对象，如列表或字符串或整数，的行为差不多。你可以将函数赋值到变量上：
```python
>>> var = count_vowels  # `var` 现在引用函数 `count_vowels`
>>> var("Hello")        # 你现在可以“调用” `var`
2
```

你可以将函数存储到一个列表中：
```python
my_list = [count_vowels, print]

for func in my_list:
    func("hello")
    
# 迭代0：调用 `count_vowels("hello")`
# 迭代1：调用 `print("hello")`
```

你也可以在代码任何地方调用函数，且它的返回值会在原地返回：
```python
if count_vowels("pillow") > 1:
    print("that's a lot of vowels!")
```

当然，这在列表理解表达式中也成立：
```python
>>> sum(count_vowels(word, include_y=True) for word in ["hi", "bye", "guy", "sigh"])
6
```

“打印”一个函数并不会揭露些什么。这仅仅打印此函数对象在内存中的地址：
```python
>>> print(count_vowels)
<function count_vowels at 0x000002A32898C6A8>
```
<!-- #endregion -->

## 官方说明文档链接

- [“函数”的定义](https://docs.python.org/3/library/stdtypes.html#functions)
- [定义函数](https://docs.python.org/3/tutorial/controlflow.html#defining-functions)
- [定义参数值](https://docs.python.org/3/tutorial/controlflow.html#default-argument-values)
- [关键词参数](https://docs.python.org/3/tutorial/controlflow.html#keyword-arguments)
- [提供任意多的参数](https://docs.python.org/3/tutorial/controlflow.html#arbitrary-argument-lists)
- [解包参数](https://docs.python.org/3/tutorial/controlflow.html#unpacking-argument-lists)
- [docstring](https://docs.python.org/3/tutorial/controlflow.html#documentation-strings)
- [函数注解](https://docs.python.org/3/tutorial/controlflow.html#function-annotations)

<!-- #region -->
## 阅读理解答案：

**编写简单函数：解**

```python
def count_even(numbers):
    """ 返回可迭代物中偶数的数量"""
    total_even = 0
    for num in numbers:
        if num % 2 == 0:
            total += 1
    return total
```
或，通过使用生成器理解：

```python
def count_even(numbers):
    """ 返回可迭代物中偶数的数量"""
    return sum(1 for num in numbers if num % 2 == 0)
```

**函数和参数：解**

```python
def max_or_min(x, y, mode="max"):
    """ 根据 `mode` 参数返回 `max(x,y)` 或 `min(x,y)`。
        
        Parameters
        ----------
        x : Number
   
        y : Number
   
        mode : str
            'max' 或 'min'
        
        Returns
        -------
        两值的最大或最小值。如果mode不合法那么会返回 `None`。"""
    if mode == "max":
        return max(x, y)
    elif mode == "min":
        return min(x, y)
    else:
        return None
```

请注意你其实可以在 `mode` 输入不正确时让你的函数报错（raise an “exception”）。事实上，这应该才是这种情况下更加合理的函数行为。

这种解决方案如下：
```python
def max_or_min(x, y, mode="max"):
    if mode == "max":
        return max(x, y)
    elif mode == "min":
        return min(x, y)
    else:
        raise Exception("`mode` was passed an invalid value: {}".format(mode))
```

**任意多的参数：解**

```python
def mean(*seq):
    """ 返回函数参数的平均值 """
    if len(seq) == 0:
        return 0
    
    total = 0 
    for num in seq:
        total += num
    return total / len(seq)
```

或，我们可以利用以下两地来做一下骚操作：

- 当 `seq` 为空时 `bool(seq)` 是 `False` 的事实
- 单行if-else语法

```python
def mean(*seq):
    """ 返回函数参数的平均值 """
    return sum(seq) / len(seq) if seq else 0
```
<!-- #endregion -->
