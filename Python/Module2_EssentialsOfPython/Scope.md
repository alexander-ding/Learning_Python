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
   :description: Topic: variable score and namespaces in python, Difficulty: Medium, Category: Section
   :keywords: variable, namespace, function, scope, shadowing
<!-- #endraw -->

# 作用域

<!-- #region -->
函数“封装”（encapsulation）提供的一大好处在于你无法在函数外面“看到”或访问参数变量或其中定义的任何其它变量。也就是说，这些变量的**作用域**（scope）是有限的。

<div class="alert alert-info">

**定义**：

一个变量的**作用域**指其在Python直译器眼中可见/可访问的环境。
</div>

直到我们理解语句和函数的讨论之前，我们仅仅遇到过有**文件作用域**（file scope）的变量。这意味着被定义后，该变量对任何相同文件中的代码而言都可见。有着文件作用域的变量甚至可以*在函数内*使用。相比文件作用域，函数中定义的变量或函数的输入参数则有着**有限作用域**（restricted scope）——它们只能在函数内使用：

```python
x = 3  # `x` 有着文件作用域。就算它没有作为参数输入到函数中，
       # 它也可以在其中被访问

# `my_func` 在其被定义后有着文件作用域
def my_func(y): 
    func_var = 9 + x  # `x` 的值将为3
    # 变量 `y` 和 `func_var` 的作用域限制在本函数中
    return y

# `func_var` 和 `y` 在此不存在
print(func_var)  # 导致 NameError：名字 `func_var` 未定义
print(y)         # 导致 NameError：名字 `y` 未定义
```

Python的作用域规则其实相比其他如C++的语言相当直接。在大部分情况下，Python会给予变量文件作用域。让我们快速总结一下定义变量的不同环境以及它们相对应的作用域规则。假设以下代码代表着Python脚本“example_scope.py”的全部内容：
```python
# 演示变量在不同情况下的作用域
# 本文件并不计算任何有意义的结果

from itertools import combinations  # `combinations` 有文件作用域
 
# `my_func` 有文件作用域
# `in_arg1` 有有限作用域
# `in_arg2` 有有限作用域
# `func_block` 有有限作用域
def my_func(in_arg1, in_arg2="cat"):
    func_block = 1
    return None 

# `file_var` 有文件作用域
# `comp_var` 有有限作用域
file_var = [comp_var**2 for comp_var in [-1, -2]]

# `if_block` 有文件作用域
if True:
    if_block = 2
else:
    if_block = 3

# `it_var` 有文件作用域
# `for_block` 有文件作用域
for it_var in [1, 2, 3]:
    for_block = 1

# `while_block` 有文件作用域
while True:
    while_block = None
    break
```

在以上代码中，以下变量有着*文件作用域*：

- `combinations`
- `my_func`
- `file_var`
- `if_block`
- `it_var`
- `for_block`
- `while_block`

而以下变量有着*有限作用域*：

- `in_arg1`
- `in_arg2`
- `func_block`
- `comp_var`
<!-- #endregion -->

在C++中，变量 `if_block`，`it_var`，`for_block`，和 `while_block` 会有着有限作用域——这些函数在它们对应的if/for/while区块外不会被定义。

<div class="alert alert-info">

**经验**：

在函数中定义的变量有着*有限作用域*。这将导致它们在函数外不存在。大部分其它情况下定义的Python变量都有着*文件作用域*（也就是说它们在定义后可以被同文件的任何代码使用）。
</div>

<!-- #region -->
## 变量重影
当一个文件作用域的变量和一个函数作用域的变量有着同样名字时会发生什么？这种情况叫做**变量重影**（variable shadowing）。Python通过优先使用可用作用域中*最受限制的作用域*的变量来解决这个冲突：

```python
x = 2
y = 3

def func(x):
    # 输入参数 `x` 凌驾于文件作用域版本的 `x` 之上
    y = 5  # 凌驾于文件作用域版本的 `y` 之上
    return x + y

# `x` 在这里是 2
# `y` 在这里是 3

print(func(-5))  # 打印 0
print(x, y)      # 打印 2 3
```

相似的：

```python
it = "cow"

def func():
    it = "dog" # 凌驾于文件作用域版本的 `it` 之上
    my_list = [it**2 for it in [1, 2, 3]]  # 在列表理解中，函数作用域版本的 `it` 被覆盖了
    # `it` 在这里是 "dog"
    return None

# `it` 在这里是 "cow"
```
<!-- #endregion -->

## 官方说明文档链接

- [作用域和命名空间](https://docs.python.org/3/tutorial/classes.html#python-scopes-and-namespaces)
- [Python的执行模型](https://docs.python.org/3/reference/executionmodel.html)
