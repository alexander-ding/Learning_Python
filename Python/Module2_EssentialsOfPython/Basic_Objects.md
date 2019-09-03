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
   :description: Topic: Basics of Python Objects, Difficulty: Easy, Category: Section
   :keywords: integers, booleans, floats, floating point precision, lists, strings, fundamentals
```

<!-- #region -->
# 基本对象类型

<div class="alert alert-warning">

**注**：

在本资源各处会有一些阅读理解练习。这些题目旨在帮助读者活学活用文中的知识点。练习题的答案可以在本页底部找到。
</div>

本文会经常使用“对象”这一单词。在Python中，“对象”包含了很多东西：数字，字符串，列表，函数，等等。简单来讲，一个Python对象是任何可以赋值给变量的东西。尽管如此，对象是分*类型*（type）的：比如说在Python眼中，整数（integer）和字符串（string）的*类型*是不一样的。

不同类型的对象为了不同目的而存在，而因此也有着不同的内置函数。在这里，我们将复习Python自带的一些基本类型并以此写一些代码。我们将会讨论：

- 数字（整数，浮点数，复数）（number（integer，float-point number，complex number））
- 布尔值（真或否）（boolean）
- “空”值类（the "null" type）
- 字符串（string）
- 列表（list）

Python自带的 `isinstance` 函数允许我们检测对象是否为某一类型。你也可以用自带的 `type` 函数来查看对象的类型。比如说，下面的代码会检测一个对象是否是整数：

```python
# 对变量 `x` 赋值整数 1
>>> x = 1  

# 查看 `x` 的类型
>>> type(x)
int

# 检测 `x` 是否是整数类（integer）
>>> isinstance(x, int)
True
```

在之后的模组中，你将学习“面对对象”的编程模式。这将允许你创建你自己的定制对象种类！
<!-- #endregion -->

<!-- #region -->
## 数字类型
Python有三种基本的数字类型：整数（integer），浮点数（floating-point number），和复数（complex number）。你可以使用你熟悉的数学运算符来对这三种数字进行算术操作（像“大于”这种比较性的运算符没有定义如何处理复数）:

| 操作  | 描述 |
| ---- |:----:|
| `x + y` | 求两数相加的结果 |   
| `x - y` | 求两数相减的结果 |   
| `x * y` | 求两数相乘的结果 |   
| `x / y` | 求两数相除的结果 |   
| `x // y` | 求两数相除的整数商 | 
| `x % y` | 求两数的模（`x` modulo: `y`，也就是 `x / y` 的余数）|
| `x ** y` | 求 `x` 的幂 `y` |
| `-x` | 求负数 |
| `abs(x)` | 求数的绝对值 |
| `x == y` | 检测两数是否等值 |
| `x != y` | 检测两数是否不等值 |
| `x > y` | 检测 `x` 是否大于 `y` |
| `x >= y` | 检测 `x` 是否大于或等于 `y` |
| `x < y` | 检测 `x` 是否小于 `y` |
| `x <= y` | 检测 `x` 是否小于或等于 `y` |

这些运算符和你平时在数学课上见到的运算顺序一样，在没有括号的情况下遵循先乘除后加减。

```python
# 先乘除后加减
>>> 1 + 2 * 3
7

# 用括号更改运算顺序
>>> (1 + 2) * 3
9

# 求余数
>>> 11 % 5
1

# 检测不等式是否成立
>>> (2 ** 3) < (2 ** 4)
True
```

请注意：在很多其他的编程语言中（包括Python 2），两个整数相除永远都会返回一个整数——就算数学上的结果应该是个分数。在Python 3中，*两个整数相除永远都会返回一个浮点数*（一个包含小数点的数字）：

```python
# 在很多别的语言中，3 / 2 会返回整数 1
# 在Python 3中，除法返回一个浮点数
>>> 3 / 2
1.5

>>> 4 / 2
2.0
```

运算符 `//` 英文叫做“floor-divide”（直译“地板除”）：它取两数相除之后去尾（去掉浮点数小数点后的数字）得到的整数（因为浮点数取整可以往上入——4.5入到5——或者往下舍——4.5舍到4——，所以去尾被称为地板除）。这可以用来进行很多其他语言中传统的整数相除：

```python
# “地板除“
>>> 1 // 3  # 0.3333.. -> 0
0
>>> 3 // 2  # 1.5 -> 1
1
```
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：理解模运算符**

模运算符 `%` 在数学教材中并不常见。但是它在我们手中是很有用的一个运算符。`x % y` (读作 x “模”（mod）y）返回 `x / y` 的余数。比如说：

- $\frac{3}{2} = 1 + \frac{1}{2}$. 3 中可以去除一个 2，最后余下 1，所以 `3 % 2` 返回 `1`
- $\frac{9}{3} = 3$. 9 中可以去除三个 3，什么都不余下。所以 `9 % 3` 返回 `0`

根据以上对模运算符的解释，先自己猜测下方表达式的结果，并用IPython命令行检查你的答案:

1. `1 % 5`
2. `2 % 5`
3. `22 % 1`
4. `22 % 2`
5. `22 % 3`
6. `22 % 4`
7. `22 % 5`
6. `22 % 6`

假设 `n` 是任何整数，`n % 2` 可能返回哪些数值？尝试简明地解释 `n % 2` 的行为。

</div>

<!-- #region -->
### Python的math模组
标准库的math模组为我们提供更多的数学函数，像对数（logarithm）和三角函数。完整的列表[可以在Python官网上找到](https://docs.python.org/3/library/math.html#number-theoretic-and-representation-functions)。要使用math模组里的函数，你必须先导入它。

```python
# 使用 `math` 模组里面的数学函数
>>> import math
>>> math.sqrt(4.)
2.0

# 以10为底的对数
>>> math.log10(10.)
1.0

# 4! = 4*3*2*1
>>> math.factorial(4)
24
```
<!-- #endregion -->

<!-- #region -->
### 整数
像传统数学一样，一个整数是任何没有小数点的数字：$\dots, -3, -2, -1, 0, 1, 2, 3, \dots$。

整数属于内置类 `int`，而 `int` 则可以用来将其它对象转化成整数：

```python
>>> type(-3)
int

# `1.3` 不是整数对象
>>> isinstance(1.3, int)
False

# 将字符串转化成整数
>>> int("10")
10

# 将浮点数转化成整数
>>> int(1.3)
1
```

整数想多大都可以；Python会分配相应该数字所需的内存来完全存储它（而不会丢失精度）。当然，说到底这还是受有多少内存可用的限制:

```python
# 你可以创建一个很大的整数
>>> large_int = 281938481039848500192847576920
```

整数有一些内置函数可用。完整的介绍可以在[官方说明文档](https://docs.python.org/3/library/stdtypes.html#additional-methods-on-integer-types)中找到。很多这些函数的作用可能对新手而言比较难以理解。现在你只需要知道这些函数存在就行。
<!-- #endregion -->

<!-- #region -->
### 浮点数
一个浮点数（floating-point number，或float）是有小数点的数字。这可以用来表达任何数字，但其精度受数字位数的限制。

这些对象属于内置类 `float`，而 `float` 则可以用来将其它对象转化成浮点数：

```python
# 几个浮点数的例子
>>> 100. ** 0.5
10.0

>>> 1 / 3
0.3333333333333333

>>> 1 / 2
0.5

>>> type(-2.1)
float

# 整数 10 并不是浮点数
>>> isinstance(10, float)
False

# 但加上一个小数点会让其成为浮点数
>>> isinstance(10., float)
True

# 将字符串转化成浮点数
>>> float("10.456")
10.456

# 将整数转化成浮点数
>>> float(-22)
-22.0
```

浮点数有几个内置的函数可用。具体内容在[官方说明文件](https://docs.python.org/3/library/stdtypes.html#additional-methods-on-float)中可以查阅。
<!-- #endregion -->

<!-- #region -->
#### 科学计数法
浮点数也可以用常见的科学计数法来创建。字符 `e` 用来代表 $\times 10$，而在这字符后的数字是幂。以下为几个传统的科学计数法例子和它们对应的Python表达式：

$1.38 \times 10^{-4} \rightarrow$ `1.38e-04`

$-4.2 \times 10^{10} \rightarrow$ `-4.2e10`

Python将自动用科学计数法来显示位数多的浮点数：
```python
# Python会用科学计数法来显示多位浮点数
>>> 0.0000001  # 7个前置0
1e-07
```
<!-- #endregion -->

<!-- #region -->
#### 理解数字精度
虽然Python整数无大小限制，浮点数的精度*受限于它能存储多少位数*。具体来讲，你的电脑只会用固定量的内存（在大部分机器上是8字节，也就是32比特）来储存一个浮点数的值。

实际操作时，这意味着一个浮点数大概只能存储16位有效数字的*数值精度*。计算机并不能保证存这8字节之外的位数精度。比如说，接下来的代码定义了一个100位的整数。但当这个整数被转化成浮点数后，它只能保存16位的有效数字：

```python
# 演示浮点数的有限精度

# 一个100位的整数
# Python会分配足够多的的内存来存储这个整数
>>> int("1"*100)  # 创建一个含有100个“1”的字符串并将其转化成整数
1111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111

# 当将同样的字符串转化成浮点数时，
# 它只能存储16位的有效数字
# 这也就是8字节内存能存储的大小
>>> float("1"*100)  # 创建一个含有100个“1”的字符串并将其转化成浮点数
1.111111111111111e+99
```
计算机无法存储最后84位数字的原因是这样做会需要远超于8字节的内存去存储整个浮点数。假如你在数星星的话，用浮点数而不是整数来存储这个数字会让你漏掉其中整整 $1\times10^{83}$ 个！

因此，试图用超过浮点数精度的数值去修改该浮点数并不会有任何效果：

```python
# 用超过浮点数精度的数值去修改该浮点数并不会有任何效果
>>> 1. + 1e-16
1.0
```

就算有了以上对浮点精度的讨论做铺垫，你也很可能会惊讶于以下浮点运算的结果：

```python
# 这令人意外的结果源自浮点数的有限精度
>>> 0.1 + 0.1 + 0.1 - 0.3 == 0.
False

# 浮点数的有限精度的效果
>>> 0.1 + 0.1 + 0.1 - 0.3
5.551115123125783e-17
```
这并不是什么Python的奇怪特征；这是一个已经被[完全理解](https://docs.python.org/3/tutorial/floatingpoint.html)的操作有限精度的浮点数的现象。为了应对这个问题，你不应该检查两个浮点数是否“相同”，而应该检查两个浮点数是否“够相近”。
让我重点强调一下：

**你永远不应该检查两个浮点数是否完全相同。你应该检查两个浮点数是否足够相近。**

`math` 模组有一个解决这个需求的函数；`math.isclose` 会检查两个数字之间的差是否小于 $1 \times 10^{-9}$。你可以修改这个函数使用的容忍值；详见此函数的[说明文档](https://docs.python.org/3/library/math.html#math.isclose)。因为在之前的例子中我们对比的数值都很接近0，我们将检查它们的差的绝对值是否够小：

```python
# 检查两个浮点数是否“足够接近”
>>> import math

# 检查
# | (0.1 + 0.1 + 0.1 - 0.3) - 0 | < 1x10^{-9}
>>> math.isclose((0.1 + 0.1 + 0.1 - 0.3), 0., abs_tol=1e-9)
True
```
如果你不记住这一课，你未来绝对会导致一些严重，难查的bug。在Python（以及任何其它编程语言）中做和数字有关的工作时，你一定要理解浮点数的有限精度是一个错误源，就像测量工具的不精确一样。因此在错误分析的时候要考虑到它（如果你需要做错误分析的话）。

你可以用Python的[decimal模组](https://docs.python.org/3.0/library/decimal.html)来定义比标准8字节的浮点数更高（或更低）精度的数字。任何使用这个模组的数学计算都是*完全精确*的，也就是说 `0.1 + 0.1 + 0.1 - 0.3` 正好是 `0.`。Python还有一个自带的[fractions模组](https://docs.python.org/3/library/fractions.html#module-fractions)提供精确表达和操作有理数的工具。虽然我们不会在本资源中使用这些模组，但是你也应该知道这些模组的存在以及浮点数并不是唯一的选择这事实。
<!-- #endregion -->

<!-- #region -->
### 复数
在数学中，一个“复数”（complex number）是任何 $a + bi$ 形式的数字（$a$ 和 $b$ 为实数，$i$ 的定义为满足 $i^2 = -1$ 关系的数）。因为没有任何实数满足上述关系，$i$ 被称为“虚数”。

奇奇怪怪的电气工程师用符号 $j$ 来代替 $i$，所以Python会将复数 $2 + 3i$ 打印成 `2+3j`（实际原因是 $i$ 一般用来表达电流；我们还是要爱护电气工程师的）。

除了 `a + bj` 格式之外，内置的 `complex` 类可以用来创建复数：

```python
# 创建复数
>>> 2 + 3j
(2+3j)

>>> complex(2, 3)
(2+3j)

>>> complex(0, 1)**2
(-1+0j)

>>> type(2+3j)
complex

>>> isinstance(2-4j, complex)
True
```

请注意，`j` 自身并不是代表 $i$ 的特殊字符。只有在一个数字常量（numerical literal）（不是变量的数字）后的 `j` 才会被Python直译器理解为复数。

```python
# 仅仅 'j' 本身并没有什么特殊含义
>>> j
NameError: name 'j' is not defined（名字 'j' 未定义）

# 而 `1j` 会被理解为虚数
>>> (1j) ** 2
(-1+0j)
```

你分别可以获取 `a + bj` 中的 `a` 和 `b`，也就是复数的实数（real）和虚数（imaginary）部分。
```python
# 获取复数的实数和虚数部分
>>> x = complex(1.2, -3.4)
>>> x.real
1.2
>>> x.imag
-3.4
```

`cmath`（“complex math”）模组定义了一组复数的数学函数。完整的列表可以在[官方说明文件](https://docs.python.org/3/library/cmath.html#module-cmath)找到。
<!-- #endregion -->

<!-- #region -->
<div class="alert alert-info">

**阅读理解：操作Python数字**

1\. 在Python中，对两个整数进行如加或乘的算术会返回一个整数，而对两个浮点数进行运算会返回一个浮点数：
```python
>>> 2 * 3
6

>>> 2.0 * 3.0
6.0
```

上述语句对 `+ - * / **` 这些运算符中哪个*不*成立？
<!-- #endregion -->

2\. 对一个整数和一个浮点数进行数学运算会返回哪种数字？是否所有的运算符都遵循这一规律？请在命令行中进行尝试以得到答案。

3\. 定义 $f(x) = e^{|x - 2|}$。使用 `math` 模组来运算 $f(-0.2)$。

4\. 使用Python的科学计数法语法来确认一万亿除以十亿等于一千。
</div>

<!-- #region -->
### 赋值语句的简写
Python提供了一些方便你使用数学运算符更新变量的方式。假设你想要将 `x` 的值增加 1，你可能会如此操作：
```python
# 将 `x` 增加 1
>>> x = 5
>>> x = x + 1
>>> x 
6
```

我们可以利用一个特殊的赋值符 `+=` 来简写这个更新操作。
```python
# 使用 `+=` 来将 `x` 增加 1
>>> x = 5
>>> x += 1  # 这和 `x = x + 1` 等值
>>> x
6
```

`+=` 是一种*增强赋值语句*（augmented assignment statement）。总的来讲，一个增强赋值语句会对一个函数进行某种数学运算，并用运算的结果更新该函数。每个算术运算符都有着相对应的增强的赋值语句。假设 `x` 和 `n` 为数字，以下列表将总结我们可以用 `n` 对 `x` 进行的增强算术赋值语句：

- `x += n` $\rightarrow$ `x = x + n`
- `x -= n` $\rightarrow$ `x = x - n`
- `x *= n` $\rightarrow$ `x = x * n`
- `x /= n` $\rightarrow$ `x = x / n`
- `x //= n` $\rightarrow$ `x = x // n`
- `x **= n` $\rightarrow$ `x = x ** n`
<!-- #endregion -->

<!-- #region -->
### 提升数字的可读性
Python 3.6版本提供了在数字位数之间插入下划线来视觉上划分它们的功能。这个符号可以用来提升多位数的可读性。比如说，利用 `_` 来区分每三位的数字，`662607004` 可以被重写成为 `662_607_004`。下划线不能出现在数字首位，尾位，或另一个下划线旁边，但除此以外这个符号可以出现在数字常量中间的任何位置。

```python
# 利用 `_` 来视觉上划分数字
>>> 1_000_000  # 这很实用！
1000000

# 虽然看起来挺恶心的，但这是合法的Python代码
>>> 2_3_4.5_6_7  
234.567

# 你可以在任何类型的数字中插入下划线
>>> 10_000j  
10000j
```

<div class="alert alert-warning">

**兼容性警告**

在数字常量中插入下划线 `_` 的功能是在Python 3.6版本加入的。如果你使用此功能，那你的代码会和Python 3.5和更早的版本不兼容。

</div>
<!-- #endregion -->

<!-- #region -->
## 布尔类
Python有两种布尔类（boolean）的对象：`True`（真）和 `False`（否）；它们属于内置的 `bool` 类型。我们在之前见到 `isinstance` 函数根据对象是否是某类的实例（instance）而返回 `True` 或 `False`。

```python
# 两种布尔类的对象：`True` 和 `False`
>>> type(True)
bool

# `False` 是一个布尔类的对象
>>> isinstance(False, bool)
True
```
在Python中，`True` 和 `False` 必须要首字母大写。请不要将它们和字符串搞混；注意布尔值并两边没有引号。

### 逻辑运算符
Python提供常见的布尔逻辑的操作符：

| 逻辑运算名称 | 运算符号 |
| ------------ | -------- |
| `and`（逻辑与）     | `&`                 | 
| `or` （逻辑或）     | <code>&#124;</code> |          
<!-- #endregion -->

<!-- #region -->
```python
# 演示布尔逻辑运算符
>>> True or False
True

>>> True and False
False

>>> not False 
True
```

译者注：原文因为 `not`（逻辑否）在英文中很好理解没有多讨论。读者只需知道：`not` 返回输入的布尔值的反值。

你也可以使用 `and` 和 `or` 对应的运算符号：
```python
# 演示逻辑运算符
>>> False | True  # 和 `False or True` 等值
True

>>> False & True  # 和 `False and True` 等值
False
```
一般来讲，使用运算名称，而不是符号，是更加“Python风格“的做法（也就是说这个方法在Python编程者之间更时兴）。

你可以在同行使用多个逻辑运算符，并用括号来细述运算顺序：
```python
>>> (True or False) and True
True
```

基本的比大小语句自然会返回布尔类的对象。
```python
>>> 2 < 3
True

>>> 10.5 < 0
False

>>> (2 < 4) and not (4 != -1)
False
```

`bool` 类有着其它方便操作的方法。我们将在“条件语”小节讨论它们。
<!-- #endregion -->

<!-- #region -->
### 布尔值对象其实就是整数
`True` 和 `False` 这两个布尔值对象正式而言也是 `int` 类型的成员，其分别对应 `1` 和 `0`：

```python
>>> isinstance(True, int)
True

>>> int(True)
1

>>> isinstance(False, int)
True

>>> int(False)
0
```

因此，在数学运算中它们和 `1` 和 `0` 的作用一样：
```python
>>> 3*True - False  # 和 3*1 + 0 等值
3

>>> True / False  # 和 1 / 0 等值
---------------------------------------------------------------------------
ZeroDivisionError                         Traceback (most recent call last)
<ipython-input-4-f8487d9d0863> in <module>()
----> 1 True / False

ZeroDivisionError: division by zero
（因为除以零所以直译器报错）
```

为何 `True` 和 `False` 同时也是整数的原因超出了本节的纲领。熟悉这一特征会让你在写Python代码的时候避免一些错误。
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：布尔表达式**

1\. 假设 `x` 是整数类。编写一个表达式，在 `x` 是偶数时返回 `True`，其它情况返回 `False`。（提示：回忆 `%` 运算符的功能）

2\. 假设 `x` 和 `y` 都是实数（也就是不是复数的数字）。编写一行代码，在以下情况中返回 `False`：`x` 和 `y` 相差小于0.9，且 `x` 大于零。（提示：尝试先编写在这个情况中会返回 `True` 的代码，然后再取反）

3\. 编写一个表达式，当 `x` 是布尔类型或浮点类型对象时返回 `True`。

</div>

<!-- #region -->
## None类型
`NoneType`（None类型）仅仅只有一个对象：`None`。`None` 用来表达“无”/“空”，也就是什么都没有的意思。
```python
# `None` 是*唯一*属于NoneType的对象
>>> type(None)
NoneType
```
因此，与其检查某对象是否属于NoneType，你应该直接检查该对象是否就是 `None`。Python保留 `is` 为检查两个对象是否相同的运算符。这和检查两个对象是否有着同样的值或态的 `==` 运算符不一样：
```python
# 检查对象是否*是*（is）None，而不是
# 检查对象是否是NoneType类的实例
>>> x = 22

>>> x is None
False

>>> x is not None
True

>>> y = None

>>> y is None
True
```
译者注：`x is not None` 和 `not (x is None)` 等值，但前者因更符合英文语法而也被支持。

`None` 经常作为占位符使用。以下代码将演示 `None` 在实际操作中的用法；如果你并不能完全理解这代码，不要担心：

```python
# 演示将 `None` 作为占位符的用法

# 在本代码中，我们想要找到列表中第一个大于10的物件
# 如果该物件不存在，我们将如实告诉用户

large_num = None

for number in [1, 2, 3, 4]:
    if number > 10:
        large_num = number
        break

if large_num is None:
    print("The list did not contain any number larger than 10")

```
<!-- #endregion -->

<!-- #region -->
## 字符串
### 字符串类型简介
字符串（string）类型被用于存储多个字符。你可以用一下语法创建字符串：

- 单引号：`'Hello world'`
- 双引号：`"Hello world"`
- 三引号：`"""Hello world"""` 或 `'''Hello world'''`

```python
# 字符串可以存储任何字符，
# 不仅仅是英文字母表中的字母！
>>> "hello, 你好, Olá, 123"
'hello, 你好, Olá, 123'
```
Python 3默认使用[UTF-8 unicode](https://docs.python.org/3/howto/unicode.html#unicode-howto)来表达这些多样化的字符。暂时没必要太过关注这个细节，知道它存在就行。

字符串属于内置的 `str` 类型。`str` 则可以用来将非字符串对象转化成字符串。
```python
# `str` 类型
>>> type("hello")
str

>>> isinstance("83", str)
True

# 利用 `str` 类型来将非字符串对象转化成字符串
>>> str(10.34)
'10.34'

>>> str(True)
'True'
```
当一个字符串被创建后，它的值无法被改变（除非你创建一个全新的字符串）。因此，任何一个字符串都无法被“改变”——字符串是*不可变*（immutable）的对象。

因为Python存储一*序列*（sequence）的字符，Python提供了调用字符串中单个字符和子字符串的工具：
```python
>>> sentence = "The cat in the hat."
>>> sentence[0]
'T'
>>> sentence[0:3]
'The'
```
字符串并不是Python中唯一的序列类；列表（list）和元组（tuple）也同样是序列的一种。我们在未来将专门花一节课来讨论Python提供的通用序列操作。这包括了以上展示的“索引“（indexing）和”切片“（slicing）。

### 字符串基础
我们只会学习一些字符串的基础。如果你想要一个更全面的非正式字符串教程，请查阅[官方Python教程](https://docs.python.org/3/tutorial/introduction.html#strings)。

在字符串中，`\n` 被视为一个字符。它代表着字符串的新行（new-line）。在打印时它也会如此显示。相似的 `\t` 会显示为一个tab字符。

```python
# 使用 `\n` 来创建新行
>>> print("hi...\n...bye")
hi...
...bye
```

三引号将允许你编写一整块字符串，也就是说你可以在多行中编写同一个字符串：

```python
# 使用三引号来编写一个多行字符串
>>> x = """ I am a string.
I am part of the same string.
    me... too!"""

>>> x 
' I am a string.\nI am part of the same string.\n    me... too!'
 
```

Python的字符串有着大量很有用的内置函数。熟悉了解这些函数*极其重要*。详见[官方说明文件](https://docs.python.org/3/library/stdtypes.html#string-methods)。以下是其中一些函数的演示：
```python
# 演示几个字符串的内置函数
>>> "hello".capitalize() # 译者注：首字母大写
'Hello'

# 使用“...”来讲多个字符串连接在一起
>>> "...".join(["item1", "item2", "item3"])
'item1...item2...item3'

# 每当“, ”出现时切分字符串
>>> 'item1, item2, item3'.split(", ")
['item1', 'item2', 'item3']

# 该字符串是否以“.py”结尾?
>>> "script.py".endswith(".py")
True

# 该字符串是否以“sc”打头?
>>> "script.py".startswith("sc")
True

# 在占位符 {} 中将对象转化为字符串并插入
>>> "x: {}, y: {}, z: {}".format(3.2, 8.4, -1.0)
'x: 3.2, y: 8.4, z: -1.0'

# 该字符串的字符是否时数字位数？
>>> "7".isdigit()
True

```

### 格式化字符串
Python提供多种设置字符串格式的语法；利用这些语法，我们可以轻松地将变量数值插入到字符串中，利用空格对齐行列，并控制字符串显示小数点后几位数。本节旨在向读者展示这些不同的字符串格式。

如果你想要完整（但仍然易懂）的Python字符串格式教程，请查阅[pyformat.info](https://pyformat.info)。你也可以查阅[官方教程](https://docs.python.org/3/library/string.html#format-examples)。

在Python 3中，你可以使用 `format` 方法来格式化字符串：

```python
# 使用 `format` 来用函数值代替占位符
>>> "{name} is {age} years old".format(name="Bruce", age=80)
'Bruce is 80 years old'

# 以下语法会前置空格来保证字符串至少有8字符长
>>> "{item:>8}".format(item="stew")
'   stew'
```
你有时可能会见到别人用 `%` 运算符来格式化字符串：

```python
# 使用 `%` 来格式化字符串（注意不要使用此方法）
>>> name = "Selina"
>>> "My name is %s" % name
'My name is Selina'
```
这是Python 2的遗留物；你应注意避免使用此语法。

如果你在用Python 3.6和之后的版本，你可以使用f-string（f字符串），一种很方便格式化字符串的方法。以下是一个f字符串的范例：

```python
# “f字符串”范例
>>> batman = 12
>>> catwoman = 10
>>> f"Batman has {batman} apples. Catwoman has {catwoman} apples. Together, they have {batman + catwoman} apples"
'Batman has 12 apples. Catwoman has 10 apples. Together, they have 22 apples'
```

请注意f字符串是一种特殊语法；在第一个引号前输入小写字符f来使用f字符串：
```python
# 这是一个常见的空字符串
>>> ""
''

# 这是一个空的f字符串
>>> f""
''
```

f字符串的特殊之处在于它允许我们在字符串*中*编写Python代码；任何在 `{}` 中的表达式都会作为Python代码被执行，其结果将会被转化成字符串并插入在 `{}` 在f字符串中的位置。

```python
>>> x = 7.9
>>> f"x is a {type(x)}-number. Its value is {x}. The statement 'x is greater than 5' is {x > 5}"
"x is a <class 'float'>-number. Its value is 7.9. The statement 'x is greater than 5' is True"
（“x是一个浮点数类数字。它的值为7.9。表达式‘x大于5’为真”）
```

如例中所见，这将允许我们优雅地在字符串创建时插入变量并甚至调用函数的输出。

<div class="alert alert-warning">

**f字符串兼容性**：

f字符串是Python 3.6才加入的新语法。它在之前的Python版本不可用。
</div>

### 字符串的官方说明文档
本文强烈建议读者花些时间了解所有字符串的自带函数。

- [字符串自带函数](https://docs.python.org/3/library/stdtypes.html#string-methods)
- [格式化字符串](https://docs.python.org/3/library/string.html#format-examples)
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：字符串**

为了回答以下问题，你会需要查阅字符串内置函数的说明文档。你可能需要进行一些尝试才能理解说明文档的“[]”格式（用于表达函数的可选输入）。

1\. 使用一个函数，在输入字符串 `"cat"` 时返回字符串 `"    cat    "`（输出的长度为11个字符；这包含字符c，a，t）。之后，修改你调用该函数的方法以返回 `"----cat----"`。

2\. 用空格代替以下字符串的前三个句号：`"I.am.aware.that.spaces.are.a.thing"`


3\. 去除该字符串两边的所有空格：`"  basket    "`。

<!-- #region -->
4\. 创建一个打印时会如下显示的字符串（第二行打头的是一个tab符号）：
```
Hello
	over there
```

5\. 将整数 `12` 转化成字符串 `"12"`。

6\. 只有13岁和以上的孩子可以观看电影《反斗智多星》。设变量 `name`（字符串）和 `age`（整数）。使用f字符串来显示："NAME is old enough to watch the movie: BOOL"（“NAME可以看这电影：BOOL”）。NAME应该用孩子的名字来代替，而BOOL在孩子13岁或以上时为 `True`，不然为 `False`。

</div>
<!-- #endregion -->

<!-- #region -->
## 列表
一个 `list`（列表）是一个允许我们存储一序列Python对象的对象。列表的一大功能就是我们在未来可以更新列表中的内容。

你可以用方括号来创建列表，并用逗号区分其中的成员：`[item1, item2, ..., itemN]`。它的成员可以是各种不同的类型。

译者注：注意使用英文的方括号和逗号。

```python
# 列表对象可以存储一序列的其它对象
>>> [3.5, None, 3.5, True, "hello"]
[3.5, None, 3.5, True, 'hello']

>>> type([1, 2, 3])
list

>>> isinstance([1, 2], list)
True

# 空列表
>>> []
[]

# 只有一个成员的列表
>>> ["hello"]
["hello"]
```

你也可以在创建列表时使用变量，公式，和其它Python表达式；Python会计算这些表达式并用计算的结果作为列表的成员。
```python
# 创建列表时会计算表达式并储存计算后的输出对象
>>> x = "hello"
>>> [2 < 3, x.capitalize(), 5**2, [1, 2]]
[True, 'Hello', 25, [1, 2]]
```

内置的 `list` 类型可以用来将其它任何类型的序列（更广义来讲，任何*可迭代*的对象——我们会在之后对其进行讨论）转化成列表：
```python
# `list` 利用其它序列的内容来创建新列表
>>> list("apple")
['a', 'p', 'p', 'l', 'e']
```
### 列表是序列
就像字符串一样，列表内容的排列顺序是很重要的。这也就是说，列表是序列性（sequential）的。
```python
# 列表是分顺序的
>>> [1, "a", True] == [1, True, "a"]
False
```

因此，像字符串一样，你可以通过索引（indexing）和切片（slicing）来访问列表的内容。我们将在下一节中更具体地介绍索引和切片。
```python
# 通过索引和切片来访问列表的内容
>>> x = [2, 4, 6, 8, 10]

# `x` 里有5个物件
>>> len(x)
5

# 通过索引来访问列表的第0个物件
>>> x[0]
2

# 通过切片来访问列表的子序列
>>> x[1:3]
[4, 6]
```

### 列表是可变的
我们会在之后遇到其它的Python容器。使得列表不同的是*列表的内容可以在列表创建后被修改*。因此，列表是*可变*对象的一种。
```python
# 在列表被创建后修改它
>>> x = [2, 4, 6, 8, 10]
>>> y = [2, 4, 6, 8, 10] 

# 将列表 `x` 第1位的物件“设置”（set）成字符串 'apple'
>>> x[1] = "apple"
>>> x
[2, 'apple', 6, 8, 10]

# 修改 `y` 的子序列
>>> y[1:4] = [-3, -4, -5]
>>> y
[2, -3, -4, -5, 10]
```

自带的列表函数“append”和“extend”允许我们分别在列表结尾添加一个和多个的成员：
```python
>>> x = [2, 4, 6, 8, 10]

# 用 `append` 在列表结尾添加一个对象
>>> x.append("moo")
>>> x
[2, 4, 6, 8, 10, 'moo']

# 用 `extend` 在列表结尾添加一序列的对象
>>> x.extend([True, False, None])
>>> x
[2, 4, 6, 8, 10, 'moo', True, False, None]
```

“pop”和“remove”函数允许我们分别根据成员在列表中的位置和值来将它们从列表中移出。
```python
>>> x = ["a", "b", "c", "d"]

# 将列表中位置1的物件“pop”（跳）出来
# `pop` 会返回被移出的物件
>>> x.pop(1) 
'b'

>>> x
['a', 'c', 'd']

# 将对象 `"d"` 从列表中移出
>>> x.remove("d")
>>> x
['a', 'c']
```

### 列表的官方说明文档
本文强烈建议读者花些时间了解列表的所有自带函数。这些函数都旨在帮助我们访问或修改列表的内容。

- [列表的内置函数](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)
<!-- #endregion -->

<div class="alert alert-info">

**阅读理解：列表**

为了回答以下问题，你会需要查阅列表内置函数的说明文档。

1\. 创建一个列表，其唯一的物件是 `None` 对象。

2\. 向变量 `k` 赋值一个内容为一个整数，一个布尔值，和一个字符串（顺序也是如此）的列表。然后在列表结尾添加两个项目：一个浮点数和一个复数。

3\. 根据字母顺序排序以下列表：`["Jane", "Adam", "Ryan", "Bob", "Zordon", "Jack", "Jackenzie"]`。

</div>

<!-- #region -->
## 总结
“对象”（object）在Python中泛指任何可以赋值于一个变量的东西。对象根据其“类型”（type）不同会有不同的行为。

我们复习了Python几种不同的基本对象类型：

- `int`，`float`，`complex`：数字类型
- `bool`：布尔（boolean）类型，其唯二的布尔类对象为 `True` 和 `False`
- `NoneType`：“空”类；`None` 是*唯一的这类对象*
- `str`：字符串类
- `list`：列表类

Python自带的 `type` 函数允许我们查看任何对象的类型：
```python
>>> type(3.2)
float

>>> type(True)
bool
```

我们应该用自带的 `isinstance` 函数来查看一个对象是否属于某类型：
```python
>>> x = 2 + 3
>>> isinstance(x, int)
True
```
唯一例外的是当你想要检查某个对象是否属于 `NoneType` 类型，因为这类型只有 `None` 一个对象；因此，如下直接检查会更加“干净”：
```python
>>> x = None
>>> x is None
True
```

不同类型的对象有着不同的内置函数可用：
```python
>>> x = "I am a farmer.. moo"
>>> x.upper()
'I AM A FARMER.. MOO'

>>> y = 0.5
>>> y.as_integer_ratio()
(1, 2)

>>> z = [1, 2]
>>> z.append(3)
>>> z
[1, 2, 3]
```

当你在试图寻找操作某类型对象的最佳方法时，你应当查阅官方说明文档（本节多次提供其链接）。

最后，Python整数类可以存储任意多的位数，而浮点数只能存储有限的“有效”位数。因此，你可以直接检查两个整数是否完全相等：
```python
# 检查两个整数是否完全相等没有任何问题！
>>> 2 + 2 == 4
True
```
但你不应该检测两个浮点数是否完全相等。你应该检查它们的值是否足够接近：
```python
# 检查两个浮点数是否完全相等会有一些奇怪的结果！
>>> 0.1 + 0.1 + 0.1 - 0.3 == 0.
False

>>> abs((0.1 + 0.1 + 0.1 - 0.3) - 0.) < 1e-12
True
```

切记浮点数的有限精度问题。
<!-- #endregion -->

## 官方说明文档链接

- [整数](https://docs.python.org/3/library/stdtypes.html#additional-methods-on-integer-types)
- [浮点数](https://docs.python.org/3/library/stdtypes.html#additional-methods-on-float)
- [标准库的math模组](https://docs.python.org/3/library/math.html#number-theoretic-and-representation-functions)
- [标准库的复数math模组](https://docs.python.org/3/library/cmath.html#module-cmath)
- [字符串](https://docs.python.org/3/library/stdtypes.html#string-methods)
   - [字符串教程](https://docs.python.org/3/tutorial/introduction.html#strings)
   - [字符串格式](https://docs.python.org/3.4/library/string.html#format-examples)
- [列表](https://docs.python.org/3/tutorial/datastructures.html#more-on-lists)


## 阅读理解答案


**理解模运算符：解**

如果 `n` 是偶数，那么它将被2整除，所以余数为0。如果 `n` 是单数，那 `n / 2` 的余数为1。所以：

- 当 `n` 是偶数时 `n % 2 ` = 0
- 当 `n` 是单数时 `n % 2 ` = 1

<!-- #region -->
**操作Python数字：解**

1\. 除法操作符 `/` 永远都会返回浮点数，就算它在将两个整数相除。

2\. 所有算术运算符 `+ - * / **` 对一个整数一个浮点数操作时都会返回一个浮点数：
```python
>>> 2 * 3.0
6.0

>>> -1.0 + 2
1.0
```

3\. 设函数 $f(x) = e^{|x - 2|}$。用 `math` 模组来计算 $f(-0.2)$。
```python
>>> from math import exp
>>> x = -0.2
>>> exp(abs(x - 2))
9.025013499434122
```

4\. 使用Python的科学计数法语法来确认一万亿处以十亿等于一千。

> 如上文所警示的一样，我们应该避免检查两个浮点数是否完全相等，而应当检查两浮点数是否足够相近。注意在Python中用科学计数法写出的数字会是浮点类的。

```python
>>> from math import isclose
>>> isclose(1e12 / 1e9, 1e3)
True
```
<!-- #endregion -->

<!-- #region -->
**布尔表达式：解**
```python
# 1. 编写一个表达式，在 `x` 是偶数时返回 `True`，
#    其它情况返回 `False`。
x%2 == 0
```

```python
# 2. 编写一行代码，在以下情况中返回 `False`：
#    `x` 和 `y` 相差小于0.9，且 `x` 大于零。
not (abs(x - y) < 0.9 and 0 < x)
# 或者
abs(x - y) > 0.9 or 0 > x
```

```python
# 3. 编写一个表达式，当 `x` 是布尔类型
#    或浮点类型对象时返回 `True`。
isinstance(x, bool) or isinstance(x, float)
```
<!-- #endregion -->

<!-- #region -->
**字符串：解**

```python
# 1.  使用一个函数，在输入字符串 `"cat"` 时
#     返回字符串 `"    cat    "`
>>> "cat".center(11)
'    cat    '

>>> "cat".center(11, "-")
'----cat----'
```

```python
# 2. 用空格代替以下字符串的前三个句号：
>>> "I.am.aware.that.spaces.are.a.thing".replace(".", " ", 3)
'I am aware that.spaces.are.a.thing'
```

```python
# 3. 去除该字符串两边的所有空格：`"  basket    "`
>>> "  basket    ".strip()
'basket'
```

```python
# 4.
>>> print("Hello\n\tover there")
Hello
	over there
```

```python
# 5. 将整数 `12` 转化成字符串 `"12"`。
>>> str(12)
'12'
```

只有13岁和以上的孩子可以观看电影《反斗智多星》。假设你有函数 `name`（字符串）和 `age`（整数）。使用f字符串来显示："NAME is old enough to watch the movie: BOOL"（“NAME可以看这电影：BOOL”）。NAME应该用孩子的名字来代替，而BOOL在孩子至少13岁以上时应为 `True`，不然为 `False`。为测试我们使用 `name = "Alfred"`，`age = 10`.

```python
# 6. 只有13岁和以上的孩子可以观看电影《反斗智多星》。
#    假设你有函数 `name`（字符串）和 `age`（整数）。
#    使用f字符串来显示："NAME is old enough to watch 
#    the movie: BOOL"（“NAME可以看这电影：BOOL”）。
#    用NAME应该用孩子的名字来代替，而BOOL在孩子至少13岁以上
#    时应为 `True`，不然是 `False`。
>>> name = "Alfred"
>>> age = 10
>>> f"{name} is old enough to watch the movie: {age >= 13}"
'Alfred is old enough to watch the movie: False'
```

<!-- #endregion -->

<!-- #region -->
**列表：解**

```python
# 创建一个列表，其唯一的物件是 `None` 对象。
>>> [None]
[None]
```

```python
# 2. 向变量 `k` 赋值一个内容为一个整数，一个布尔值，
#    和一个字符串（顺序也是如此）的列表。然后在列表结尾
#    添加两个项目：一个浮点数和一个复数。
>>> k = [4, False, "moo"]
>>> k.extend([3.14, complex(9, -2)])
>>> k
[4, False, 'moo', 3.14, (9-2j)]
```

```python
# 3. 根据字母顺序排序以下列表：
>>> names = ["Jane", "Adam", "Ryan", "Bob", "Zordon", "Jack", "Jackenzie"]

# `sort` 的说明文档说排序是“in-place”（当场）的，
# 这意思就是原本的列表会被新的排序好的列表代替。
# 如果不是“in-place”的话，那可能 `sort` 会返回一个排序好
# 的列表，而不修改原本名为 `names` 的列表。
>>> names.sort()
>>> names
['Adam', 'Bob', 'Jack', 'Jackenzie', 'Jane', 'Ryan', 'Zordon']
```
<!-- #endregion -->
