# 流程控制

到目前为止，我们写过的程序都是从第一行开始一行接一行地往下执行，每一行只执行一次. 实际的问题却常常要求程序做出选择，或者反复做同一件事. 程序中语句执行的先后顺序称为控制流（control flow）：分支语句决定执行哪些语句，往程序中引入分支；循环语句让一些语句重复执行，直到满足某个条件才停止，当然也可以永远循环下去.

我们在第 1 节区分表达式和语句，并介绍几种赋值语句的写法，交换两个值、计数加 1 这类写法在后面的分支和循环中随处可见. 第 2 节讲条件的写法，第 3、4 节分别讲分支语句 `#!python if` 和循环语句 `#!python while`、`#!python for`，最后讲在 PTA 上做题时怎样用循环读入和输出数据.

建议读者打开 Thonny，把下面的例子亲手试一遍. 和[输入、输出与字符串](io-string.md)一样，以 `>>>` 开头的行是在 Shell 中输入的代码，没有 `>>>` 的行是 Python 给出的结果. 分支和循环的例子大多写成脚本，也就是在编辑区写好、再整体运行的程序；需要输入的脚本，我们在代码后面写出运行时输入的内容和程序的输出.

## 1. 表达式与语句

### 表达式

在 Shell 中输入 `#!python 1 + 2`，Python 立即给出 `#!python 3`. 像这样能计算出一个值的代码，称为表达式（expression），计算出它的值的过程称为求值. 我们已经写过不少表达式：

- 字面量，如 `#!python 42`、`#!python "Hi"`，它的值就是它本身；
- 名字，如 `x`，它的值是这个名字所绑定的对象；
- 运算，如 `#!python a + b`、`#!python x > 2`；
- 函数调用和方法调用，如 `#!python len(s)`、`#!python input()`、`#!python s.upper()`，它们的值是函数返回的结果.

小的表达式可以组合成大的表达式. 在 `#!python int(input()) * 2` 中，先对 `#!python input()` 求值，得到一个字符串；这个字符串作为参数交给 `#!python int()`，求值得到一个整数；这个整数再参与乘法. 哪一部分先算，由括号和运算符的优先级决定.

```python
>>> 1 + 2
3
>>> len("Python") * 2
12
>>> x = 1 + 2
>>> x
3
>>> x > 2
True
```

`#!python x > 2` 也是一个表达式，它的值是布尔值 `#!python True`. 后面分支和循环中的条件，就是这样的表达式. 中间的 `#!python x = 1 + 2` 却什么也没有显示，因为它不是表达式，而是一条语句.

### 语句

语句（statement）是程序执行的基本单位，程序由一条条语句组成. `#!python x = 1 + 2` 是一条赋值语句：它先对右边的表达式 `#!python 1 + 2` 求值，再把名字 `x` 绑定到结果 `#!python 3` 上. 赋值语句本身不产生值，而 Shell 只自动显示表达式的值，所以这一行什么也没有显示.

表达式也可以单独写成一行，成为一条语句，称为表达式语句. 最常见的是单独一行的函数调用，比如 `#!python print("Hi")`：我们需要的是它在屏幕上输出文字，并不使用它求出的值. 在脚本中，表达式语句的值算出来以后就被丢弃，不会显示出来.

到目前为止，我们写的赋值语句和表达式语句都是简单语句. 本页要讲的 `#!python if`、`#!python while`、`#!python for` 则是复合语句（compound statement）：它们的内部还包含别的语句，用来控制这些语句是否执行、执行几次.

Python 中通常**一行写一条语句**. 一条语句太长时，可以拆成几行：在圆括号里可以直接换行；不在括号里时，要在行末加一个反斜杠 `\`，表示这条语句还没有结束. 井号 `#` 后面直到行末的内容是注释，Python 运行时会忽略它.

```python
total = (90 + 85 + 77
         + 68 + 95)  # 括号里可以直接换行
average = total / \
          5          # 上一行末尾的反斜杠表示语句还没有结束
print(total, average)
```

运行后的输出是：

```
415 83.0
```

也可以用分号 `;` 把几条语句写在同一行，比如 `#!python a = 1; b = 2`，但这样不容易阅读，不建议使用.

### 赋值语句

赋值语句 `#!python x = 1` 把名字 `x` **绑定**到对象 `#!python 1` 上：名字就像一张贴在对象上的标签，而不是一个装着值的盒子. 第 3 章在此基础上介绍了几种赋值的写法.

循环开始之前，常常要把几个名字设成同一个初始值，比如几个计数器都从 0 开始. 这时可以写成 `#!python x = y = 0`，称为链式赋值. 它看起来像是先执行 `#!python y = 0`，再把结果交给 `x`. 但赋值语句不产生值，`#!python y = 0` 没有“结果”可以交给 `x`，真把它当作一个值来用，Python 会报错：

```python
>>> x = (y = 0)
  File "<stdin>", line 1
    x = (y = 0)
         ^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

括号里需要的是一个表达式，`#!python y = 0` 却是一条语句. Python 猜我们想写的是比较运算 `==`，所以给出了这样的提示. 提示中的 `:=` 称为赋值表达式（Python 3.8 加入），能在表达式中完成赋值，课件只是顺带提到，这里不展开.

`#!python x = y = 0` 其实是**一条**赋值语句，等号左边有两个要绑定的目标. 根据[语言参考手册](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#assignment-statements)，Python 先对最右边的表达式求值，只求值一次，得到一个对象，再把左边的名字**从左往右**依次绑定到这个对象上，相当于先算出 `#!python 0`，再依次执行“`x` = 它”和“`y` = 它”. 所以几个名字都贴在同一个对象上：

```python
>>> x = y = 0
>>> x
0
>>> y
0
>>> x = 5
>>> y
0
```

`#!python x = 5` 只是把标签 `x` 揭下来，贴到对象 `#!python 5` 上；`y` 仍然贴在 `#!python 0` 上，不受影响.

!!! note "绑定的顺序"

    左边只有名字时，先绑定哪一个结果都一样，看不出区别. 只有当一个目标要用到另一个目标时，顺序才会显现出来：

    ```python
    >>> x = [0, 0]
    >>> i = 0
    >>> i = x[i] = 1
    >>> i
    1
    >>> x
    [0, 1]
    ```

    从左往右，先把 `i` 绑定到 `#!python 1`，再执行 `#!python x[i] = 1`. 这时 `i` 已经是 1，所以改的是 `#!python x[1]`，结果是 `#!python [0, 1]`. 如果是从右往左，就会先改 `#!python x[0]`，结果应该是 `#!python [1, 0]`. 列表和 `#!python x[i] = ...` 这样的写法后面的章节才会讲；这样的代码也很难读懂，实际编程中不要这样写.

多重赋值则用逗号把几个名字和几个值按位置对应起来，一次完成几个绑定：

```python
>>> x, y = 4, 8
>>> x
4
>>> y
8
```

它最常见的用途是交换两个名字的值. 如果按顺序写两条赋值语句，是交换不成的：

```python
>>> a = 1
>>> b = 2
>>> a = b
>>> b = a
>>> a, b
(2, 2)
```

执行 `#!python a = b` 之后，`a` 和 `b` 都绑定到了 `#!python 2`，原来的 `#!python 1` 已经没有名字能找到它了，第二句 `#!python b = a` 也就无从交换. 许多编程语言要借助第三个名字暂存原来的值，Python 用多重赋值一行就能完成：

```python
>>> a = 1
>>> b = 2
>>> b, a
(2, 1)
>>> a, b = b, a
>>> a
2
>>> b
1
```

`#!python b, a` 也是一个表达式：用逗号连起来的几个值，求值得到一个元组（tuple），Shell 显示它时带着圆括号，前面的 `#!python (2, 2)` 也是这样来的. 元组后面的章节会讲. 多重赋值 `#!python a, b = b, a` 分两步：先对右边求值，得到 `#!python (2, 1)`；再把其中的元素依次分给 `a` 和 `b`. 右边在绑定之前就已经全部算好，所以 `a` 和 `b` 正好交换.

### 解包

上面的第二步，把右边的元素依次分给左边的名字，称为解包（unpacking）. [输入、输出与字符串](io-string.md)“一行中有多个值”一节中的 `#!python a, b = input().split()` 也是解包. 等号右边不一定要用逗号写出几个值，只要它是一个能逐个取出元素的对象，Python 就会把其中的元素依次取出，分给左边的名字. 元组是这样的对象，`#!python split()` 拆出来的列表也是，字符串也是：

```python
>>> a, b = "34"
>>> a
'3'
>>> b
'4'
>>> a, b, c = "34"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a, b, c = "34"
    ^^^^^^^
ValueError: not enough values to unpack (expected 3, got 2)
```

左边名字的个数必须和元素的个数相同. 这里左边有 3 个名字，`#!python "34"` 只有 2 个字符，所以报错说值不够（not enough values）：需要 3 个（expected 3），实际只有 2 个（got 2）. 输入、输出与字符串一页中遇到的 `too many values to unpack` 则是值多了.

如果元素的个数不确定，可以在某一个名字前面加星号 `*`，让它收集剩下的元素：

```python
>>> a, *b = "1234"
>>> a
'1'
>>> b
['2', '3', '4']
>>> *a, b = "1234"
>>> a
['1', '2', '3']
>>> b
'4'
>>> a, *b, c = "1234"
>>> b
['2', '3']
>>> a, *b, c = "12"
>>> b
[]
>>> a, *b, c = "1"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a, *b, c = "1"
    ^^^^^^^^
ValueError: not enough values to unpack (expected at least 2, got 1)
```

可以把解包想象成把一队小朋友按顺序分给几个名字. 不带星号的名字从队伍两头领人，每个名字领一个：星号前面的名字依次领走队首的人，星号后面的名字依次领走队尾的人. 带星号的名字领走中间剩下的所有人，结果总是一个列表；中间一个人也不剩时，它领到空列表 `#!python []`. 人数连不带星号的名字都分不够时，就会报 `#!python ValueError`：`#!python a, *b, c` 至少需要 2 个元素（expected at least 2）.

如果写两个带星号的名字，中间剩下的人该怎么分就说不清了，所以 Python 直接报语法错误：

```python
>>> *a, *b = "1234"
  File "<stdin-0>", line 1
    *a, *b = "1234"
    ^^^^^^
SyntaxError: multiple starred expressions in assignment
```

在 PTA 上，这种写法可以把一行中的第一个值和其余的值分开：

```python
>>> name, *scores = "Alice 90 85 77".split()
>>> name
'Alice'
>>> scores
['90', '85', '77']
```

和 `#!python split()` 拆出来的其他结果一样，`scores` 中的元素仍然是字符串.

### 增强赋值

循环中经常要在一个名字原来的值上再加一些，比如计数加 1、把新读到的数累加起来. `#!python j = j + 3` 可以简写成 `#!python j += 3`，称为增强赋值（augmented assignment）：先用 `j` 原来的值计算 `#!python j + 3`，再把结果重新绑定到 `j` 上. 类似的写法还有 `-=`、`*=`、`/=`、`//=`、`%=`、`**=`.

```python
>>> j = 5
>>> j += 3
>>> j
8
>>> k = 12
>>> k /= j - 2
>>> k
2.0
>>> s = "hello"
>>> s += " world"
>>> s
'hello world'
```

注意，`#!python k /= j - 2` 是先算出右边整个表达式 `#!python j - 2`，再用它去除 `k`，相当于 `#!python k = k / (j - 2)`，而不是 `#!python k = k / j - 2`. 对字符串来说，`+=` 就是拼接.

!!! note "提示"

    学过 C 语言的读者请注意，Python 没有 `++` 和 `--` 运算符，加 1 要写成 `#!python i += 1`：

    ```python
    >>> i = 1
    >>> i++
      File "<stdin>", line 1
        i++
           ^
    SyntaxError: invalid syntax
    ```

赋值语句的完整规则见语言参考手册的[赋值语句](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#assignment-statements)和[增强赋值语句](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#augmented-assignment-statements).

## 2. 条件判断

分支和循环都要先判断一个条件是否成立. 条件是一个表达式，对它求值就得到答案. 最直接的条件是比较运算，它的值是布尔值 `#!python True` 或 `#!python False`.

### 比较运算

比较两个值的大小或是否相等，有以下六种运算符：

| 运算符 | 含义       | 例子              | 结果             |
| ------ | ---------- | ----------------- | ---------------- |
| `==`   | 等于       | `#!python 3 == 2` | `#!python False` |
| `!=`   | 不等于     | `#!python 3 != 2` | `#!python True`  |
| `>`    | 大于       | `#!python 3 > 2`  | `#!python True`  |
| `<`    | 小于       | `#!python 3 < 2`  | `#!python False` |
| `>=`   | 大于或等于 | `#!python 3 >= 2` | `#!python True`  |
| `<=`   | 小于或等于 | `#!python 3 <= 2` | `#!python False` |

判断是否相等要用**两个等号** `==`，一个等号 `=` 是赋值. 比较运算还可以像数学中的 $0 \le x < 10$ 那样连着写，称为链式比较：

```python
>>> x = 5
>>> 0 <= x < 10
True
>>> 0 <= x and x < 10
True
```

`#!python 0 <= x < 10` 的意思是“`#!python 0 <= x` 并且 `#!python x < 10`”，和第二行相同（`#!python and` 就是“并且”，后面会细讲）. 链式比较把相邻的两个比较用“并且”连起来，而不是先算出左边的比较，再拿结果和右边比：

```python
>>> 3 > 2 > 1
True
>>> (3 > 2) > 1
False
```

`#!python 3 > 2 > 1` 表示 `#!python 3 > 2` 并且 `#!python 2 > 1`，结果为真. 加上括号以后，先算出 `#!python 3 > 2` 得到 `#!python True`，再比较 `#!python True > 1`. 课上讲过 `#!python True` 相当于 `#!python 1`，`#!python 1 > 1` 不成立，所以结果为假.

整数和浮点数之间可以直接比较，比较的是数值本身. 字符串之间也能比较大小：从第一个字符开始，逐个比较两边对应字符的编码，也就是 `#!python ord()` 的结果，第一个不同的字符决定大小：

```python
>>> 1 == 1.0
True
>>> "apple" < "banana"
True
>>> "Apple" < "apple"
True
>>> "10" < "9"
True
>>> 10 < 9
False
>>> "5" == 5
False
>>> "5" < 3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    "5" < 3
TypeError: '<' not supported between instances of 'str' and 'int'
```

大写字母 `#!python 'A'` 的编码是 65，小写字母 `#!python 'a'` 是 97，所以 `#!python "Apple"` 比 `#!python "apple"` 小. `#!python "10"` 和 `#!python "9"` 比较的是第一个字符 `#!python '1'` 和 `#!python '9'`，于是 `#!python "10"` 反而更小. 字符串和数字之间，`==` 的结果总是 `#!python False`；用 `<`、`>` 这类运算符比较大小，则直接报 `#!python TypeError`.

这正是忘记转换类型时会发生的事. `#!python input()` 返回的是字符串，下面的程序忘了把它转换成整数：

```python
age = input()
if age >= 18:
    print("成年")
```

输入 `20` 时，输出为：

```
Traceback (most recent call last):
  File "main.py", line 2, in <module>
    if age >= 18:
       ^^^^^^^^^
TypeError: '>=' not supported between instances of 'str' and 'int'
```

报错的最后一行说，`>=` 不能用在 `#!python str` 和 `#!python int` 之间. 如果条件写的是 `#!python age == 18`，程序不会报错，只是这个条件永远不成立，错误反而更难发现.

### 成员运算与身份运算

[输入、输出与字符串](io-string.md)中用过运算符 `#!python in`，它判断一个字符串是否出现在另一个字符串中；`#!python not in` 则判断是否**不**出现. 要判断一个字符是不是某几个字符之一，用它很方便：

```python
>>> ch = "e"
>>> ch in "aeiou"
True
>>> "x" not in "aeiou"
True
```

后面学到列表等类型时，`#!python in` 同样可以判断一个元素是否在其中.

`#!python is` 和 `#!python is not` 是身份运算符，判断两个名字是否绑定到**同一个**对象上. 它和 `==` 不同，`==` 比较的是两个对象的**值**是否相等：

```python
>>> a = [1, 2]
>>> b = [1, 2]
>>> a == b
True
>>> a is b
False
>>> c = a
>>> c is a
True
```

`a` 和 `b` 是两个内容相同的列表，值相等，却是两个不同的对象；`#!python c = a` 则把 `c` 贴到了 `a` 所贴的那个对象上. 每个对象都有一个标识，可以用内置函数 [`#!python id()`](https://docs.python.org/zh-cn/3/builtins/functions.html#id) 查看，`#!python is` 比较的就是标识. 在 CPython（官方的 Python 解释器）中，这个标识就是对象在内存中的地址，但这是实现细节，不是语言的规定. 判断两个值是否相等，一律用 `==`. `==` 和 `#!python is` 的更多区别，可以参阅[类型与值](../faq/typing.md)中“`0` 不是 `False`”一节.

### 逻辑运算

一个条件不够用时，可以用逻辑运算符 `#!python and`（与）、`#!python or`（或）、`#!python not`（非）把几个条件组合起来：

| `x`              | `y`              | `#!python x and y` | `#!python x or y` |
| ---------------- | ---------------- | ------------------ | ----------------- |
| `#!python True`  | `#!python True`  | `#!python True`    | `#!python True`   |
| `#!python True`  | `#!python False` | `#!python False`   | `#!python True`   |
| `#!python False` | `#!python True`  | `#!python False`   | `#!python True`   |
| `#!python False` | `#!python False` | `#!python False`   | `#!python False`  |

`#!python and` 要求两边都为真，`#!python or` 只要有一边为真. `#!python not` 只作用于一个值，把真变成假、假变成真.

```python
>>> score = 85
>>> score >= 60 and score <= 100
True
>>> 60 <= score <= 100
True
>>> not score >= 60
False
```

几种运算符混在一起时，按优先级从高到低依次是：算术运算、比较运算（包括 `#!python in` 和 `#!python is`）、`#!python not`、`#!python and`、`#!python or`. 所以 `#!python not score >= 60` 是先算 `#!python score >= 60`，再取反. 判断闰年是一个典型的例子：能被 4 整除但不能被 100 整除，或者能被 400 整除：

```python
>>> year = 2024
>>> (year % 4 == 0 and year % 100 != 0) or year % 400 == 0
True
>>> year = 1900
>>> (year % 4 == 0 and year % 100 != 0) or year % 400 == 0
False
>>> year = 2000
>>> (year % 4 == 0 and year % 100 != 0) or year % 400 == 0
True
```

2024 能被 4 整除、不能被 100 整除，括号里的前半部分成立；1900 能被 100 整除，又不能被 400 整除，两部分都不成立；2000 能被 400 整除，后半部分成立.

因为 `#!python and` 的优先级比 `#!python or` 高，这里的括号去掉也不影响结果. 不过加上括号，读的人不必再去想优先级，不容易出错，推荐这样写. 完整的优先级表见语言参考手册的[运算符优先级](https://docs.python.org/zh-cn/3/reference/expressions.html#operator-precedence).

### 真值测试

常能看到这样的条件：`#!python if s:`. 条件的位置上只有一个字符串，既没有比较，也没有 `#!python and`、`#!python or`，这是什么意思？条件不一定是 `#!python True` 或 `#!python False`，任何值都可以充当条件，Python 会判断它算“真”还是算“假”. 这称为真值测试，官方文档译作“逻辑值检测”. 在课程会用到的值中，算作假的有：

- `#!python False` 和 `#!python None`（`#!python None` 后面的章节会讲到）；
- 数值零，如 `#!python 0`、`#!python 0.0`；
- 空字符串 `#!python ""`，以及后面会学到的空列表 `#!python []` 等空的容器.

其他的值都算真. 用 `#!python bool()` 可以看到一个值在条件中算真还是算假：

```python
>>> bool(0)
False
>>> bool(0.0)
False
>>> bool("")
False
>>> bool(-3)
True
>>> bool(" ")
True
>>> bool("0")
True
>>> bool("False")
True
```

只含一个空格的字符串 `#!python " "` 不是空字符串，所以算真；`#!python "0"` 和 `#!python "False"` 也一样，Python 不会去看字符串的内容像什么. 所以 `#!python if s:` 的意思是“`s` 不是空字符串”；`#!python if n % 2:` 的意思是“`n` 除以 2 的余数不为 0”，也就是 `n` 是奇数.

对 `#!python input()` 读到的字符串做真值测试时，这一点尤其要注意：

```python
>>> n = input()
0
>>> n
'0'
>>> bool(n)
True
>>> bool(int(n))
False
```

输入的 `0` 读进来是字符串 `#!python '0'`，它不是空字符串，所以算真. 要按数值判断，必须先转换成整数. 完整的规则见官方文档的[逻辑值检测](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#truth-value-testing).

### `#!python and`、`#!python or` 的结果与短路 { #short-circuit }

想判断 `x` 是不是 1 或 2，照着说话的顺序，很容易写成 `#!python x == 1 or 2`. 可是把它写在 `#!python if` 后面，无论 `x` 是多少，条件都成立. 在 Shell 中看看它的值：

```python
>>> x = 5
>>> x == 1 or 2
2
>>> bool(x == 1 or 2)
True
>>> x == 1 or x == 2
False
```

结果既不是 `#!python True` 也不是 `#!python False`，而是 `#!python 2`：`#!python and` 和 `#!python or` 的结果不一定是布尔值. 根据[官方文档](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#boolean-operations-and-or-not)，它们的规则是：

- `#!python x and y`：如果 `x` 算假，结果就是 `x`；否则结果是 `y`.
- `#!python x or y`：如果 `x` 算真，结果就是 `x`；否则结果是 `y`.

`==` 的优先级比 `#!python or` 高，`#!python x == 1 or 2` 的意思是 `#!python (x == 1) or 2`. `#!python x == 1` 为假，所以结果是右边的 `#!python 2`；`#!python 2` 不是零，算真，于是条件总是成立. 正确的写法是把两个比较都写完整：`#!python x == 1 or x == 2`. 如果 `x` 是单个字符，也可以用前面的 `#!python in`，比如 `#!python ch in "ab"`.

按照这个规则，结果总是两边的值之一. 两边都是比较运算时，结果自然是布尔值；两边是别的值时，就能看出区别：

```python
>>> 3 and 0
0
>>> 0 and 3
0
>>> 3 or 0
3
>>> "" or "游客"
'游客'
>>> "Alice" or "游客"
'Alice'
>>> not "Alice"
False
```

有些代码会利用 `#!python or` 的这个特点提供默认值，比如 `#!python name = input() or "游客"`：用户直接按回车时，`#!python input()` 返回空字符串，`name` 就是 `#!python '游客'`. `#!python not` 则不同，它的结果总是布尔值.

上面的规则还意味着：只看 `x` 就能确定结果时，Python **不会计算** `y`. 对 `#!python and` 来说，`x` 算假时结果已经确定；对 `#!python or` 来说，`x` 算真时结果已经确定. 这称为短路（short-circuit）. 短路可以用来避免出错：

```python
>>> x = 0
>>> 10 / x > 1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    10 / x > 1
    ~~~^~~
ZeroDivisionError: division by zero
>>> x != 0 and 10 / x > 1
False
```

`#!python x != 0` 为假，`#!python and` 的结果已经确定是 `#!python False`，后面的 `#!python 10 / x` 没有被计算，也就不会除以零. 写这样的条件时，起保护作用的那一半要放在前面.

这里补充 `#!python and`、`#!python or` 的结果规则，主要是为了判断选择题中 `#!python 3 and 0` 这类表达式的值. 自己写代码时，`#!python and`、`#!python or` 的两边都应当是条件，不要把它们的结果当作数或字符串来用，否则代码很难读懂. 像上面这样用短路避免出错，则是清楚、常见的写法，本页最后的例题也会用到.

## 3. 分支语句

有一个流传很广的笑话：甲对乙说：“回来的时候帮我带四个包子，如果看到卖西瓜的，就买一个”，结果乙只买回来一个包子，理由是“我看到卖西瓜的了”. 歧义在于“就买一个”既没说买什么，也没说它和“带四个包子”是什么关系. 程序中不会这样：条件成立时要做的事必须一条一条写出来，哪几条归这个条件管，由缩进决定.

按照能走的路有几条，分支语句有三种格式：只有 `#!python if` 的单分支，做或不做；`#!python if`-`#!python else` 的二分支，二选一；`#!python if`-`#!python elif`-`#!python else` 的多分支，多选一.

### `#!python if` 语句

先看一个最简单的例子：输入一个整数，输出它的绝对值. 负数要变号，其他的数原样输出：

```python
x = int(input())
if x < 0:
    x = -x
print(x)
```

输入 `-5` 时输出 `5`；输入 `3` 时，条件不成立，`#!python x = -x` 被跳过，输出 `3`.

第 2 行的 `#!python if` 后面是条件，行末必须有冒号；冒号下面**向右缩进**的语句组成一个语句块，缩进通常是 4 个空格. 程序执行到 `#!python if` 时，先对条件做真值测试：条件为真，就执行语句块；条件为假，就跳过整个语句块. 无论哪种情况，之后都接着执行 `#!python if` 语句后面没有缩进的语句，这里是 `#!python print(x)`. 一般地，`#!python if` 语句写成：

```python
if 条件:
    语句块
```

语句块可以有很多行，只要缩进相同，就都归这个 `#!python if` 管.

!!! note "在 Shell 中输入语句块"

    在 Shell 中输入以冒号结尾的一行后，下一行的开头会出现 `...`，表示语句还没有结束，可以继续输入. 输完整个语句块后，再输入一个空行，Python 才会执行它：

    ```python
    >>> x = 3
    >>> if x > 0: print("正数")
    ...
    正数
    ```

    这个例子还说明，语句块只有一条语句时，可以直接写在冒号后面，`#!python if x > 0: print("正数")` 是合法的. 不过为了清楚，我们一般还是换行缩进.

### 缩进决定语句的归属

缩进不是为了美观，而是语法的一部分：**一条语句是否缩进，决定了它是否受 `#!python if` 控制**. 下面的程序处理“满 100 减 20”的优惠：

```python
price = float(input())
if price >= 100:
    price -= 20
    print("满 100 减 20")
print(f"实付 {price:.2f} 元")
```

输入 `150` 时，输出为：

```
满 100 减 20
实付 130.00 元
```

输入 `80` 时，输出为：

```
实付 80.00 元
```

如果把第 4 行的缩进去掉：

```python
price = float(input())
if price >= 100:
    price -= 20
print("满 100 减 20")
print(f"实付 {price:.2f} 元")
```

输入 `80` 时，输出变成了：

```
满 100 减 20
实付 80.00 元
```

去掉缩进之后，`#!python print("满 100 减 20")` 不再属于 `#!python if` 的语句块，无论条件是否成立都会执行. 程序没有报错，结果却不对，所以写完分支语句，要逐行检查每条语句的缩进是否符合本意.

### 常见的语法错误

分支语句的格式写错了，程序一条语句也不会执行，Python 直接报告语法错误. 这类报错没有 `Traceback` 那一行，`File "main.py", line 2` 指出出错的行号，`^` 指出大致的位置，最后一行说明错误的原因. 比如忘了写冒号：

```python
x = int(input())
if x > 0
    print("正数")
```

```
  File "main.py", line 2
    if x > 0
            ^
SyntaxError: expected ':'
```

`^` 指在行末，那里本该有一个冒号. 再如把判断相等的 `==` 写成了赋值的 `=`：

```python
x = int(input())
if x = 5:
    print("五")
```

```
  File "main.py", line 2
    if x = 5:
       ^^^^^
SyntaxError: invalid syntax. Maybe you meant '==' or ':=' instead of '='?
```

条件的位置需要一个表达式，而 `#!python x = 5` 是一条赋值语句，本身不产生值，不能写在这里. 这和第 1 节中 `#!python x = (y = 0)` 报的是同一个错误，Python 猜到我们想写的多半是 `==`.

缩进出错时，报的是 `#!python IndentationError`. 冒号后面的语句忘了缩进：

```python
x = int(input())
if x > 0:
print("正数")
```

```
  File "main.py", line 3
    print("正数")
    ^^^^^
IndentationError: expected an indented block after 'if' statement on line 2
```

同一个语句块中的几行缩进不一致：

```python
x = int(input())
if x > 0:
    print("正数")
  print("大于零")
```

```
  File "main.py", line 4
    print("大于零")
                   ^
IndentationError: unindent does not match any outer indentation level
```

第 4 行只缩进了 2 个空格，既不和语句块中的第 3 行对齐，也不和外面的 `#!python if` 对齐，Python 不知道它属于哪一层. 同一层的缩进要用同样多的空格. 另外，Tab 和空格不要混用，否则可能报 `TabError: inconsistent use of tabs and spaces in indentation`.

这些带有具体提示的报错信息（`expected ':'`、`Maybe you meant '=='`、`after 'if' statement on line 2` 等）是 Python 3.10 才加入的. 如果 Thonny 或 PTA 使用的 Python 版本较早，同样的错误可能只显示 `SyntaxError: invalid syntax`，这时就要自己检查行号附近的冒号、等号和缩进.

### `#!python if`-`#!python else` 语句

条件不成立时也要做点什么，就加上 `#!python else` 子句：

```python
if 条件:
    语句块 1
else:
    语句块 2
```

条件为真时执行语句块 1，为假时执行语句块 2，两者恰好执行一个，然后继续执行后面的语句. `#!python else` 后面没有条件，但同样要写冒号，并且要和 `#!python if` 左对齐.

```python
score = int(input())
if score >= 60:
    print("及格")
else:
    print("不及格")
print("成绩录入完毕")
```

输入 `75` 时，输出为：

```
及格
成绩录入完毕
```

输入 `42` 时，输出为：

```
不及格
成绩录入完毕
```

不用 `#!python else`，写成 `#!python if score >= 60:` 和 `#!python if score < 60:` 两个 `#!python if` 语句，结果也一样. 但这样要判断两次，而且两个条件要我们自己保证恰好相反，改动其中一个时很容易忘了另一个. `#!python else` 则保证两条路恰好走一条.

!!! tip "什么也不做：`#!python pass`"

    有时某个分支暂时还不知道写什么，或者确实什么也不用做. 但冒号后面必须有一个语句块，空着会报错：

    ```python
    n = int(input())
    if n > 0:
    else:
        print("不是正数")
    ```

    ```
      File "main.py", line 3
        else:
        ^^^^
    IndentationError: expected an indented block after 'if' statement on line 2
    ```

    这时可以写一条 `#!python pass` 语句占位，它执行时什么也不做：

    ```python
    n = int(input())
    if n > 0:
        pass  # 正数的情况以后再写
    else:
        print("不是正数")
    ```

    输入 `-1` 时，输出 `不是正数`；输入 `3` 时，没有任何输出. 这个例子直接写成 `#!python if n <= 0:` 更好.

### `#!python if`-`#!python elif`-`#!python else` 语句

情况多于两种时，用 `#!python elif` 子句继续添加条件. `#!python elif` 是 else if 的缩写，可以有任意多个；最后的 `#!python else` 可以省略. 下面的程序按 BMI 判断体重类别：

```python
bmi = float(input())
if bmi < 18.5:
    print("偏瘦")
elif bmi < 24:
    print("正常")
elif bmi < 28:
    print("超重")
else:
    print("肥胖")
```

输入 `17` 时输出 `偏瘦`，输入 `22.5` 时输出 `正常`，输入 `30` 时输出 `肥胖`.

Python **从上往下**依次检查各个条件，执行第一个为真的条件下面的语句块，然后跳过整个 `#!python if` 语句，后面的条件不再检查；所有条件都为假时，执行 `#!python else` 下面的语句块. 所以 `#!python elif bmi < 24` 虽然没有写 `#!python bmi >= 18.5`，能走到这里时，前一个条件 `#!python bmi < 18.5` 一定已经不成立了.

正因为如此，条件的顺序很重要. 把上面的条件倒过来写：

```python
bmi = float(input())
if bmi < 28:
    print("超重")
elif bmi < 24:
    print("正常")
elif bmi < 18.5:
    print("偏瘦")
else:
    print("肥胖")
```

输入 `22.5` 时，输出为：

```
超重
```

`#!python 22.5 < 28` 成立，执行第一个分支以后，后面两个条件不再检查. 所以条件要和顺序配合：从小往大判断时用 `<`，从大往小判断时用 `>=`.

如果把 `#!python elif` 写成了 `#!python if`，问题又不一样：

```python
bmi = float(input())
if bmi < 18.5:
    print("偏瘦")
if bmi < 24:
    print("正常")
if bmi < 28:
    print("超重")
else:
    print("肥胖")
```

输入 `17` 时，输出为：

```
偏瘦
正常
超重
```

现在这是三个独立的 `#!python if` 语句，各自判断，每一个条件成立都会执行. 最后的 `#!python else` 只属于第三个 `#!python if`. **`#!python if`-`#!python elif`-`#!python else` 是一个语句，至多走一个分支；几个 `#!python if` 是几个语句，可能走好几个分支.**

### 嵌套与 `#!python else` 的配对

`#!python else` 总是和它**同一缩进层次上、离它最近的** `#!python if` 配对. 下面的程序计算门票价格：12 岁以下 50 元，60 岁及以上免费，其他人 100 元. 它的写法有问题：

```python
age = int(input())
if age < 12:
    price = 50
if age >= 60:
    price = 0
else:
    price = 100
print(price)
```

输入 `70` 时输出 `0`，输入 `30` 时输出 `100`，都没有问题. 输入 `8` 时，输出却是：

```
100
```

这里有两个独立的 `#!python if` 语句，`#!python else` 属于第二个. 输入 `8` 时，第一个 `#!python if` 把 `price` 设为 `#!python 50`；接着第二个 `#!python if` 语句判断 `#!python age >= 60` 不成立，执行它的 `#!python else`，又把 `price` 改成了 `#!python 100`. 把第二个 `#!python if` 改成 `#!python elif`，三种情况就合成了一个语句，输入 `8` 时输出 `50`.

分支语句的语句块里还可以再写分支语句，称为嵌套. 这时，缩进层次决定了每个 `#!python else` 属于哪个 `#!python if`：

```python
score = int(input())
if score >= 60:
    if score >= 90:
        print("优秀")
    else:
        print("及格")
else:
    print("不及格")
```

分别输入 `95`、`75`、`42` 时，输出依次为 `优秀`、`及格`、`不及格`. 第一个 `#!python else` 缩进了 4 个空格，和里层的 `#!python if score >= 90` 对齐，属于里层；第二个 `#!python else` 没有缩进，属于外层. 如果去掉里层的 `#!python else`：

```python
score = int(input())
if score >= 60:
    if score >= 90:
        print("优秀")
else:
    print("不及格")
```

输入 `75` 时，程序没有任何输出：外层条件成立，进入里层；里层条件不成立，又没有自己的 `#!python else`；剩下的那个 `#!python else` 属于外层，不会执行.

嵌套的层数多了，代码就不容易读懂. 上面的嵌套也可以改写成 `#!python if score >= 90`、`#!python elif score >= 60`、`#!python else` 三个分支，效果相同，层次少了一层，读起来更清楚.

### 条件表达式

如果分支只是为了在两个值中选一个，可以用条件表达式（conditional expression）：

```python
值 1 if 条件 else 值 2
```

条件为真时，它的值是值 1；否则是值 2.

```python
>>> n = 7
>>> "偶数" if n % 2 == 0 else "奇数"
'奇数'
>>> a, b = 3, 8
>>> a if a > b else b
8
```

第 1 节说过，表达式能算出一个值，语句则不产生值. 条件表达式正是一个表达式，所以它可以写在赋值语句的右边、函数的参数里，或者 f-字符串的花括号中，比如 `#!python big = a if a > b else b`. `#!python if` 语句则是语句，它本身没有值，不能这样用. 条件表达式适合简单的二选一；判断复杂、需要嵌套时，还是用 `#!python if` 语句更清楚.

!!! note "`#!python match` 语句"

    Python 3.10 加入了 `#!python match` 语句，它把一个值依次与各个 `#!python case` 后面的模式比较，执行第一个匹配的分支：

    ```python
    cmd = input()
    match cmd:
        case "start":
            print("开始")
        case "stop" | "quit":
            print("结束")
        case _:
            print("未知命令")
    ```

    输入 `quit` 时输出 `结束`，输入 `hello` 时输出 `未知命令`. `|` 表示“或”，`_` 可以匹配任何值，相当于 `#!python else`. 这个程序用 `#!python if`-`#!python elif`-`#!python else` 同样能写出来. 课件中只是顺带提到 `#!python match`，Python 3.10 之前的版本也不支持它. 有兴趣的读者可以阅读官方教程的 [`match` 语句](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#match-statements)一节.

## 4. 循环语句

循环语句让一段代码重复执行. Python 有两种循环：`#!python while` 循环在条件成立时不断重复，适合事先不知道要重复多少次的情况；`#!python for` 循环从一组元素中每次取出一个，取完为止，重复的次数由元素的个数决定.

### `#!python while` 循环

`#!python while` 循环的格式和 `#!python if` 很像：

```python
while 条件:
    循环体
```

区别在于，`#!python if` 的语句块最多执行一次；`#!python while` 的循环体执行完之后，会**回到开头再判断一次条件**，条件仍然成立，就再执行一遍. 直到某一次判断时条件不成立，循环才结束，程序接着执行循环后面的语句. 下面的程序从 3 开始倒数：

```python
n = 3
while n > 0:
    print(n)
    n -= 1
print("发射！")
```

运行后的输出是：

```
3
2
1
发射！
```

把每一次判断条件时的情况列出来，执行过程就很清楚了：

| 第几次判断 | `n` 的值 | `#!python n > 0` | 接下来做什么                          |
| ---------- | -------- | ---------------- | ------------------------------------- |
| 1          | 3        | `#!python True`  | 输出 `3`，`n` 变为 2                  |
| 2          | 2        | `#!python True`  | 输出 `2`，`n` 变为 1                  |
| 3          | 1        | `#!python True`  | 输出 `1`，`n` 变为 0                  |
| 4          | 0        | `#!python False` | 结束循环，执行后面的 `#!python print` |

读不懂一段循环时，可以像这样列一张表，或者在循环体里临时加一行 `#!python print()`，把每一轮的变量值打印出来.

循环体中**必须有语句改变条件中用到的值**，这里是 `#!python n -= 1`. 如果漏掉这一句，`n` 永远是 3，条件永远成立，程序会不停地输出 `3`，永远不会结束，这称为死循环. 在 Thonny 中遇到死循环，可以点击工具栏上红色的停止按钮，或者按 Ctrl+C 中断程序. 提交到 PTA 的程序陷入死循环，会因为超过时间限制而得到“运行超时”的结果.

`#!python while` 循环最适合“不知道要重复几次，只知道什么时候停”的问题. 比如，一张厚 0.1 毫米的纸，对折多少次以后，厚度会超过珠穆朗玛峰的高度？

```python
thickness = 0.1  # 纸的厚度，单位是毫米
count = 0        # 已经对折的次数
while thickness < 8848860:  # 珠峰高 8848.86 米，即 8848860 毫米
    thickness *= 2
    count += 1
print(f"对折 {count} 次，厚度约为 {thickness / 1000:.2f} 米")
```

运行后的输出是：

```
对折 27 次，厚度约为 13421.77 米
```

注意 `thickness` 和 `count` 都要在循环开始**之前**给出初始值：循环第一次判断条件时就要用到 `thickness`，`#!python count += 1` 也要在 `count` 原来的值上加 1.

### `#!python for` 循环

`#!python for` 循环不看条件，而是从一组元素中每次取出一个，直到取完为止. 对字符串来说，元素就是其中的每一个字符：

```python
>>> for ch in "Hi!":
...     print(ch)
...
H
i
!
```

每一轮，`#!python for` 从 `#!python "Hi!"` 中取出下一个字符，绑定到名字 `ch` 上，然后执行一遍循环体；三个字符都取完，循环就结束了. 一般地，`#!python for` 循环写成：

```python
for 变量 in 可迭代对象:
    循环体
```

`#!python in` 后面的对象要能逐个取出元素，这样的对象称为可迭代对象（iterable），比如字符串、后面要讲的 `#!python range()`，以及列表等.

结合前面的分支语句，可以对每个字符分别做判断. 下面的程序统计一个单词中有几个元音字母：

```python
s = input()
count = 0
for ch in s:
    if ch in "aeiou":
        count += 1
print(count)
```

输入 `education` 时，输出为：

```
5
```

`count` 是一个计数器：循环开始之前设为 0，每遇到一个元音字母就加 1，循环结束时它就是元音字母的个数.

### `#!python range()`

要让循环重复固定的次数，或者遍历一串整数，就用 `#!python range()`. `#!python range()` 本身不会直接列出其中的数，用 `#!python list()` 把它转换成列表，就能看到其中有哪些数：

```python
>>> range(5)
range(0, 5)
>>> list(range(5))
[0, 1, 2, 3, 4]
>>> list(range(1, 6))
[1, 2, 3, 4, 5]
>>> list(range(0, 10, 3))
[0, 3, 6, 9]
>>> list(range(5, 0, -1))
[5, 4, 3, 2, 1]
>>> list(range(5, 1))
[]
```

`#!python range()` 有三种写法：

- `#!python range(stop)`：从 0 开始，到 `stop` 为止，**不包括** `stop`；
- `#!python range(start, stop)`：从 `start` 开始，到 `stop` 为止，同样不包括 `stop`；
- `#!python range(start, stop, step)`：每次增加 `step`，`step` 称为步长；步长为负数时从大往小数.

和字符串的切片一样，`#!python range()` 也是**左闭右开**的：`#!python range(1, 6)` 是 1 到 5. 所以 `#!python range(n)` 正好有 `n` 个数，`#!python for i in range(n):` 的循环体正好执行 `n` 次. `#!python range(5, 1)` 的步长是默认的 1，起点 5 却不小于终点 1，所以一个数也取不到，用它写的 `#!python for` 循环一次也不会执行.

`#!python range()` 的参数必须是整数：

```python
n = float(input())
for i in range(n):
    print(i)
```

输入 `3` 时，输出为：

```
Traceback (most recent call last):
  File "main.py", line 2, in <module>
    for i in range(n):
             ~~~~~^^^
TypeError: 'float' object cannot be interpreted as an integer
```

`#!python float()` 转换出来的 `#!python 3.0` 是浮点数，不能用作 `#!python range()` 的参数. 表示次数、个数的数据应该用 `#!python int()` 读入. `#!python range` 的详细说明见官方文档的 [range 对象](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#ranges).

下面的程序计算 $1 + 2 + \cdots + n$：

```python
n = int(input())
total = 0
for i in range(1, n + 1):
    total += i
print(total)
```

输入 `100` 时，输出为：

```
5050
```

要包括 `n` 本身，`#!python range()` 的终点要写 `#!python n + 1`. `total` 用来累加，和前面的计数器一样，要在循环之前设为 0. 如果把 `#!python total = 0` 写进循环体里：

```python
n = int(input())
for i in range(1, n + 1):
    total = 0
    total += i
print(total)
```

输入 `100` 时，输出为：

```
100
```

现在每一轮都先把 `total` 清零，再加上这一轮的 `i`，之前累加的结果全部丢掉了，最后剩下的只是最后一轮的 `#!python 0 + 100`. **只需要设置一次初始值的变量，要放在循环之前**.

### 遍历字符串的索引

`#!python for ch in s` 只能拿到字符本身. 如果还需要知道字符的位置，比如要把一个字符和它后面的字符比较，就要遍历索引. `#!python range(len(s))` 正好是字符串 `s` 的所有索引：

```python
>>> s = "abc"
>>> len(s)
3
>>> list(range(len(s)))
[0, 1, 2]
```

下面的程序统计一个字符串中有几处“相邻的两个字符相同”：

```python
s = input()
count = 0
for i in range(len(s) - 1):
    if s[i] == s[i + 1]:
        count += 1
print(count)
```

输入 `aabccc` 时，输出为：

```
3
```

相同的相邻字符出现在索引 0 和 1、3 和 4、4 和 5 三处. 循环的范围是 `#!python range(len(s) - 1)`，因为最后一个字符后面已经没有字符了. 如果写成 `#!python range(len(s))`：

```python
s = input()
count = 0
for i in range(len(s)):
    if s[i] == s[i + 1]:
        count += 1
print(count)
```

输入 `aabccc` 时，输出为：

```
Traceback (most recent call last):
  File "main.py", line 4, in <module>
    if s[i] == s[i + 1]:
               ~^^^^^^^
IndexError: string index out of range
```

最后一轮 `i` 是 5，`#!python s[i + 1]` 就是 `#!python s[6]`. 用[输入、输出与字符串](io-string.md)中的尺子来想，刻度 6 已经在尺子的最右端，后面没有格子了，所以报 `#!python IndexError`. 用索引写循环时，要检查第一轮和最后一轮的索引是否在范围之内.

!!! note "`#!python enumerate()`"

    同时需要索引和字符时，还可以用内置函数 [`#!python enumerate()`](https://docs.python.org/zh-cn/3/builtins/functions.html#enumerate)，写成 `#!python for i, ch in enumerate(s):`，每一轮 `i` 是索引，`ch` 是对应的字符. 课上没有讲到它，用 `#!python range(len(s))` 完全可以.

### 选择 `#!python while` 还是 `#!python for`

事先知道要处理哪些元素、或者要重复几次的，用 `#!python for`：遍历一个字符串，对 1 到 `n` 求和，读入 `n` 行数据. 什么时候停止要在循环过程中才能知道的，用 `#!python while`：纸要对折到多厚，读到某个结束标志为止. 同一个问题往往两种循环都能写，选更自然的那一种即可.

### 嵌套循环

循环体里还可以再写循环，称为嵌套循环. 下面的程序找出两个骰子点数之和为 7 的所有情况：

```python
for a in range(1, 7):
    for b in range(1, 7):
        if a + b == 7:
            print(a, b)
```

运行后的输出是：

```
1 6
2 5
3 4
4 3
5 2
6 1
```

外层循环每取一个 `a`，里层循环都要把 `b` 从 1 到 6 完整地走一遍，然后外层才取下一个 `a`. 外层 6 次，每次里层 6 次，`#!python if` 一共判断了 36 次.

### `#!python break` 与 `#!python continue`

`#!python break` 语句**立即结束**它所在的循环，循环体中剩下的语句不再执行，直接跳到循环后面的语句. 它常和 `#!python while True` 一起使用：条件永远为真，什么时候结束由循环体中的 `#!python break` 决定. 下面的程序不断读入整数，读到 0 为止，输出读到的数之和：

```python
total = 0
while True:
    x = int(input())
    if x == 0:
        break
    total += x
print(total)
```

依次输入：

```
3
5
-2
0
```

输出为：

```
6
```

这种写法的结构是：

```python
while True:
    读入或计算的语句  # 每一轮先做什么？
    if 结束条件:      # 什么时候应该停下？
        break
    处理的语句        # 还没有停下的话，还要做什么？
```

对初学者来说，先想清楚这三个问题，再把答案填进去，往往比直接写出 `#!python while` 的条件更容易.

`#!python continue` 语句**跳过本轮**循环体中剩下的语句，直接进入下一轮：`#!python while` 循环回去判断条件，`#!python for` 循环取下一个元素. 下面的程序读入一行整数，只把其中的非负数加起来：

```python
total = 0
for s in input().split():
    x = int(s)
    if x < 0:
        continue
    total += x
print(total)
```

输入 `3 -1 4 -1 5` 时，输出为：

```
12
```

`#!python input().split()` 拆出一个列表，`#!python for` 循环依次取出其中的每一段. 遇到负数时，`#!python continue` 跳过了后面的 `#!python total += x`. 这个例子也可以不用 `#!python continue`，写成 `#!python if x >= 0: total += x`. 不过后面要做的处理很多时，先用 `#!python continue` 把不需要处理的情况排除掉，后面的语句就不必都缩进在 `#!python if` 里面.

在嵌套循环中，`#!python break` 和 `#!python continue` 只作用于它们所在的**那一层**循环：

```python
for a in range(1, 4):
    for b in range(1, 4):
        if b == 2:
            break
        print(a, b)
```

运行后的输出是：

```
1 1
2 1
3 1
```

`#!python break` 只结束了里层循环，外层循环照常取下一个 `a`.

### 循环的 `#!python else` 子句

再看一个问题：找出一个单词中的第一个元音字母；如果没有，就输出“没有元音字母”. 找到第一个就可以用 `#!python break` 停下. 难点在于循环结束以后，怎么区分是找到了才停下，还是找遍了也没有. 一种办法是用一个名字记下来：

```python
s = input()
found = False
for ch in s:
    if ch in "aeiou":
        print("第一个元音字母是", ch)
        found = True
        break
if not found:
    print("没有元音字母")
```

输入 `python` 时，输出为：

```
第一个元音字母是 o
```

输入 `rhythm` 时，输出为：

```
没有元音字母
```

Python 为这种情况准备了循环的 `#!python else` 子句，不需要额外的名字：

```python
s = input()
for ch in s:
    if ch in "aeiou":
        print("第一个元音字母是", ch)
        break
else:
    print("没有元音字母")
```

两个程序的效果完全相同. 循环**正常结束**时，也就是 `#!python for` 循环取完了所有元素，或者 `#!python while` 循环的条件变为假，会执行一次 `#!python else` 下面的语句块；循环如果是被 `#!python break` 结束的，`#!python else` 就会被跳过. 不妨把这里的 `#!python else` 读作“没有被 `#!python break` 打断”.

要特别注意 `#!python else` 的缩进：它和 `#!python for` 对齐，属于 `#!python for` 循环. 如果把它缩进到和 `#!python if` 对齐：

```python
s = input()
for ch in s:
    if ch in "aeiou":
        print("第一个元音字母是", ch)
        break
    else:
        print("没有元音字母")
```

输入 `python` 时，输出变成了：

```
没有元音字母
没有元音字母
没有元音字母
没有元音字母
第一个元音字母是 o
```

现在 `#!python else` 属于循环体中的 `#!python if`，每遇到一个不是元音的字母就执行一次. 循环的 `#!python else` 子句的详细说明见官方教程的[循环的 `else` 子句](https://docs.python.org/zh-cn/3/tutorial/controlflow.html#else-clauses-on-loops).

## 5. 在 PTA 上用循环处理输入输出 { #pta }

[输入、输出与字符串](io-string.md#input-lines)中说过，一个 `#!python input()` 只读一行，程序中 `#!python input()` 的个数必须和输入的行数对应. 有了循环，`#!python input()` 就可以写在循环体中，读多少行由数据本身决定.

**先读个数，再循环读入.** 很多题目的第一行给出数据的个数 `n`，后面跟着 `n` 行数据. 先读入 `n`，再用 `#!python for i in range(n):` 循环 `n` 次，每次读一行：

```python
n = int(input())
total = 0
for i in range(n):
    x = float(input())
    total += x
print(f"{total / n:.2f}")
```

依次输入：

```
3
90
85.5
77
```

输出为：

```
84.17
```

**读到结束标志为止.** 有的题目不给出个数，而是用一个特殊的值（比如 0 或 `end`）表示输入结束. 这时用前面 `#!python break` 一节中 `#!python while True` 加 `#!python break` 的写法.

**一行中有多个值.** 个数确定时，像[输入、输出与字符串](io-string.md)中那样，用 `#!python a, b = input().split()` 解包；个数不确定时，用 `#!python for s in input().split():` 逐个处理，见前面 `#!python continue` 一节的例子.

**不要写提示语.** 循环中的 `#!python input("请输入：")` 每执行一次，提示语就输出一次，输出就和标准答案不一致了.

**在同一行输出多个值.** 循环中用 `#!python print(i, end=" ")` 输出，每个值后面都会跟一个空格，最后一个值也不例外，而行末的空格在屏幕上看不出来. [输入、输出与字符串](io-string.md)中说过，Shell 显示字符串就像打开了 Word 的“显示编辑标记”，引号标出了字符串的结尾. 把这些值拼接成一个字符串，就能在 Shell 中看到末尾的空格：

```python
>>> line = ""
>>> for i in range(1, 6):
...     line += str(i) + " "
...
>>> line
'1 2 3 4 5 '
>>> line.rstrip()
'1 2 3 4 5'
```

很多题目要求“行末不得有多余空格”，多出的这个空格会让这一行和标准答案不一致. 在 PTA 的测试界面中，空格显示为 `·`，可以借此检查（见 [PTA 测试方法](index.md#pta-test)）. 解决的办法有两种：像上面那样先拼接成字符串，再用 `#!python rstrip()` 去掉末尾的空格后输出；或者在**除第一个值以外**的每个值**前面**输出空格：

```python
n = int(input())
for i in range(1, n + 1):
    if i > 1:
        print(" ", end="")
    print(i, end="")
print()
```

输入 `5` 时，输出为 `1 2 3 4 5`，行末没有空格. 最后的 `#!python print()` 输出换行，结束这一行.

!!! note "用 `*` 一次输出"

    把 `*` 写在 `#!python print()` 的参数前面，会把其中的元素逐个取出，作为多个参数传给 `#!python print()`，元素之间用 `sep` 隔开，行末不会有多余的空格：

    ```python
    >>> print(*range(1, 6))
    1 2 3 4 5
    >>> print(*range(1, 6), sep=",")
    1,2,3,4,5
    ```

    这种写法实际上利用了解包，可以思考一下.

**避免死循环.** 程序陷入死循环时，PTA 会给出“运行超时”. 检查 `#!python while` 循环体中是否有改变条件的语句，以及 `#!python while True` 循环是否一定能遇到 `#!python break`.

下面用一道例题把这一页的内容串起来.

!!! quote "例题"

    第一行输入天数 `n`（`n` 至少为 1），接下来 `n` 行，每行是一天的最低气温（可以是小数，也可以是负数）. 输出这些气温中的最高值（保留 1 位小数），以及气温低于 0 度的天数，格式见样例.

    输入样例：

    ```
    5
    -3.5
    2
    -1
    0.5
    -7
    ```

    输出样例：

    ```
    最高气温 2.0 度，有 3 天低于 0 度
    ```

数据的个数在第一行给出，所以先读入 `n`，再用 `#!python for` 循环读 `n` 行. 低于 0 度的天数用计数器 `count` 统计. 最高值则用一个名字 `highest` 记录“到目前为止见过的最高气温”，每读到一个比它高的气温就更新它. 问题在于 `highest` 的初始值. 照着计数器的习惯，把它设为 0：

```python
n = int(input())
highest = 0
count = 0
for i in range(n):
    t = float(input())
    if t > highest:
        highest = t
    if t < 0:
        count += 1
print(f"最高气温 {highest:.1f} 度，有 {count} 天低于 0 度")
```

样例能够通过，但气温全是负数时就错了. 依次输入：

```
3
-5
-2
-8
```

输出为：

```
最高气温 0.0 度，有 3 天低于 0 度
```

三个气温都比 0 低，`highest` 一次也没有被更新，输出的 0.0 并不是读到的数据. 正确的做法是用**第一个**读到的气温作为初始值，再和后面的气温比较：

```python
n = int(input())
count = 0
for i in range(n):
    t = float(input())
    if i == 0 or t > highest:
        highest = t
    if t < 0:
        count += 1
print(f"最高气温 {highest:.1f} 度，有 {count} 天低于 0 度")
```

用同样的输入运行，输出为：

```
最高气温 -2.0 度，有 3 天低于 0 度
```

第一轮 `i` 为 0，`#!python i == 0` 为真，`#!python or` 的结果已经确定，后面的 `#!python t > highest` 不会被计算，这时 `highest` 还没有绑定任何对象也不要紧. 这里用到的正是前面讲的[短路](#short-circuit)，两个条件的顺序不能颠倒：写成 `#!python t > highest or i == 0`，第一轮就要先计算 `#!python t > highest`，程序会报 `#!python NameError: name 'highest' is not defined`.

读者也许会问：前面不是说，只需要设置一次初始值的变量要放在循环之前吗？区别在于，`count` 的初始值 0 在读入数据之前就知道；`highest` 的初始值是第一个气温，要读入之后才知道. `#!python i == 0` 只在第一轮成立，保证了 `highest` 的初始值恰好设置一次，之后每一轮只做比较和更新.

另外，循环体中的两个 `#!python if` 不能合并成 `#!python if`-`#!python elif`. 同一天的气温可能既是目前的最高值、又低于 0 度，比如第一天的 `-5`，两件事都要做. 如果第二个 `#!python if` 写成 `#!python elif`，用上面的输入运行，结果只数出 1 天.

!!! note "用负无穷作初始值"

    `highest` 的初始值也可以在循环之前设为负无穷 `#!python float("-inf")`，它比任何有限的数都小. 这样第一轮的 `#!python t > highest` 一定成立，条件里就不再需要 `#!python i == 0`.
