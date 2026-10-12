# 输入、输出与字符串

课程第 1 章把程序概括为“输入—处理—输出”（IPO）：读入数据，加以处理，再把结果输出. 我们在 PTA 上做作业、考试时写的程序是这样，实际应用中的程序大体上也是这样.

PTA 上的编程题，输入和输出都很简单：程序从标准输入读取测试数据，处理之后把结果打印到标准输出. 做这两件事，只需要两个函数：`#!python input()` 负责读入，`#!python print()` 负责输出. 对做作业以及考试来说，掌握这两个函数就已足够.

输入和输出之间是处理. 处理到最后，往往要把手里的数据整理成题目要求的输出格式，这时常常要用到字符串的方法. 字符串方法再配合 f-字符串（格式化字符串），能更简单、清晰、快捷地把数据转化成题目要求的输出格式.

建议读者打开 Thonny，在 Shell 中把下面的例子逐个输入一遍. 以 `>>>` 开头的行是我们输入的代码，没有 `>>>` 的行是 Python 给出的结果，个别例外会在第一次出现时说明.

## 1. 字符串

### 字符串字面量

用一对引号括起来的文本就是一个字符串（string），类型是 `#!python str`. 单引号和双引号的效果完全相同. 像 `#!python "Hello"` 这样直接写在代码里的值称为字面量（literal），官方中文文档译作“字面值”，说的是同一个东西.

```python
>>> "Hello"
'Hello'
>>> '浙江大学'
'浙江大学'
>>> "2026"
'2026'
>>> type("2026")
<class 'str'>
>>> type(2026)
<class 'int'>
```

`#!python "2026"` 和 `#!python 2026` 看起来很像，却是不同类型的值：前者是由四个字符组成的文本，后者是一个整数：

```python
>>> print("1 + 2")
1 + 2
>>> print(1 + 2)
3
```

第一行的 `1 + 2` 在引号里，Python 把它当作普通文字原样显示；第二行没有引号，Python 先计算这个表达式，再显示结果.

如果文本本身含有引号，可以换用另一种引号把它括起来，也可以在引号前加反斜杠 `\`：

```python
>>> "It's Python"
"It's Python"
>>> 'He said "hi"'
'He said "hi"'
>>> 'It\'s Python'
"It's Python"
```

最后一个例子中，`\'` 表示一个单引号字符，而不是字符串的结束. Python 显示结果时自己选择了双引号，这只是显示方式不同，字符串的内容是一样的.

!!! warning "注意中英文符号"

    代码中的引号、括号、逗号、冒号都必须是英文（半角）符号. 在中文输入法下打出的 `“”`、`（）` 不是 Python 能识别的符号：

    ```python
    >>> print(“你好”)
      File "<stdin>", line 1
        print(“你好”)
              ^
    SyntaxError: invalid character '“' (U+201C)
    ```

    按照我们讲过的读报错方法，先看最后一行：`#!python SyntaxError` 表示语法错误，`invalid character '“'` 指出了是哪个字符出了问题. 字符串**内部**的文字不受这个限制，`#!python "你好，世界！"` 里的中文标点完全合法.

### 字符与编码

计算机内部只能存储数字，所以每个字符都对应一个编号，称为这个字符的编码. Python 使用的是 Unicode 编码，它给世界上几乎所有文字中的字符都分配了编号. 内置函数 `#!python ord()` 给出单个字符的编码，`#!python chr()` 反过来由编码得到字符：

```python
>>> ord("A")
65
>>> chr(97)
'a'
>>> ord("浙")
27993
>>> chr(27993)
'浙'
```

编码习惯上写成十六进制. 用内置函数 `#!python hex()` 把编码换成十六进制，排列的规律就清楚多了：

```python
>>> hex(ord("0"))
'0x30'
>>> hex(ord("7"))
'0x37'
>>> hex(ord("A"))
'0x41'
>>> hex(ord("a"))
'0x61'
>>> hex(ord("一"))
'0x4e00'
>>> hex(ord("浙"))
'0x6d59'
>>> chr(0x6d59)
'浙'
```

数字字符 `#!python '0'` 到 `#!python '9'` 的编码是 `#!python 0x30` 到 `#!python 0x39`，最后一位正好就是这个数字；大写字母 `#!python 'A'` 是 `#!python 0x41`，小写字母 `#!python 'a'` 是 `#!python 0x61`，大小写只差一位；Unicode 中的基本汉字从 `#!python 0x4e00` 开始排列，第一个字恰好是“一”. 换成十进制的 48、65、97，这些规律就看不出来了.

前面报错信息里的 `U+201C`，也是这种写法：`U+` 表示 Unicode 编码，后面的 `201C` 是十六进制数，对应的正是中文左引号：

```python
>>> hex(ord("“"))
'0x201c'
>>> chr(0x201c)
'“'
```

### 转义字符与 `#!python print()` 的显示

反斜杠 `\` 在字符串中有特殊作用：它和后面的字符一起表示一个不方便直接写出的字符，这种写法称为转义序列（escape sequence）. 常用的有：

| 转义序列   | 含义           |
| ---------- | -------------- |
| `\n`       | 换行           |
| `\t`       | 制表符（Tab）  |
| `\\`       | 反斜杠本身     |
| `\'`、`\"` | 单引号、双引号 |

```python
>>> s = "第一行\n第二行"
>>> s
'第一行\n第二行'
>>> print(s)
第一行
第二行
```

同一个字符串，在这里显示成了两种样子：直接输入 `s` 时，显示的结果带着引号，`\n` 也原样写出；`#!python print(s)` 显示的结果没有引号，`\n` 真的变成了换行. 为什么会这样？

!!! warning "Shell 里看到的，不一定是程序输出的"

    Python 有两种运行方式：

    - 交互式模式像“对话”：在 Shell 里输入一行，Python 立即回应；
    - 脚本模式像“写信”：在编辑区写好整个程序，再一次运行；PTA 运行我们提交的程序，用的就是这种方式.

    把上面的例子写成一个脚本：

    ```python
    s = "第一行\n第二行"
    s
    print(s)
    ```

    运行后的输出是：

    ```
    第一行
    第二行
    ```

    第二行的 `s` 在 Shell 里会显示 `#!python '第一行\n第二行'`，在脚本里却没有任何输出，脚本只输出了 `#!python print(s)` 的结果. 只有在 Shell 里，单独输入的表达式才会被自动显示出来. 这些自动显示的值就像草稿纸上的演算，方便我们自己检查；PTA 阅卷只看答题卡，也就是程序用 `#!python print()` 输出的内容.

    Shell 自动显示一个字符串时，显示的是**它在代码里怎么写**：带着引号，换行写成 `\n`. `#!python print()` 输出的则是字符串的**内容**本身. 用过 Word 或 WPS 的读者，可以联想“显示编辑标记”这个功能：打开它，文档里会出现 ¶ 这样的符号，标出每一处换行；关掉它，看到的才是打印出来的样子. Shell 显示字符串，就像打开了编辑标记：引号标出字符串从哪里开始、到哪里结束，`\n`、`\t` 标出看不见的换行和制表符. `#!python print()` 则像打印到纸上，只留下内容本身.

    所以，本页例子中 Shell 显示的引号不属于字符串的内容. 程序真正输出的、PTA 拿去比较的，都是 `#!python print()` 显示的样子.

    !!! note "两种显示背后的函数"

        Shell 自动显示一个值时，用的是内置函数 `#!python repr()` 的结果；`#!python print()` 则先像 `#!python str()` 那样把参数转换成字符串. 对字符串来说，两者差别很大：

        ```python
        >>> s = "第一行\n第二行"
        >>> print(repr(s))
        '第一行\n第二行'
        >>> print(str(s))
        第一行
        第二行
        ```

        对整数和浮点数，两者的结果相同，所以 `#!python 1 + 2` 无论在 Shell 里直接显示，还是用 `#!python print()` 输出，都是 `#!python 3`. 这两个函数分别调用对象的 `#!python __repr__()` 和 `#!python __str__()` 方法，详见官方文档：

        - `#!python repr()`：<https://docs.python.org/zh-cn/3/builtins/functions.html#repr>
        - `#!python str()`：<https://docs.python.org/zh-cn/3/builtins/stdtypes.html#str>

```python
>>> print("姓名\t成绩")
姓名	成绩
>>> print("反斜杠：\\")
反斜杠：\
```

制表符显示出来是一段空白，它会把后面的文字推到下一个对齐位置，所以空白的宽度不固定.

转义有时会带来意外，最常见的是 Windows 路径. 下面第一行里的 `\n` 和 `\t` 被当成了换行和制表符：

```python
>>> print("D:\new\table.txt")
D:
ew	able.txt
>>> print(r"D:\new\table.txt")
D:\new\table.txt
```

在引号前加 `r` 得到原始字符串（raw string），其中的反斜杠不再有特殊含义.

用三个引号 `"""` 或 `'''` 括起来的字符串可以跨越多行，其中的换行会成为字符串内容的一部分：

```python
>>> print("""第一行
... 第二行""")
第一行
第二行
```

在 Shell 中输入多行时，第二行开头的 `...` 是 Python 给出的提示，表示上一行还没有结束，它不属于字符串.

### 拼接、重复与长度

`+` 把两个字符串首尾相接，`*` 把字符串重复若干次，内置函数 `#!python len()` 返回字符串中的字符个数. 一个汉字也只算一个字符：

```python
>>> "Py" + "thon"
'Python'
>>> "=" * 10
'=========='
>>> s = "ab"
>>> s * 2 + "c"
'ababc'
>>> len("Python")
6
>>> len("浙江大学")
4
```

`#!python s * 2 + "c"` 和算术一样先算乘法：`#!python s * 2` 得到 `#!python 'abab'`，再与 `#!python 'c'` 拼接.

`+` 两边必须都是字符串，字符串和数字不能直接相加：

```python
>>> "年龄：" + 18
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    "年龄：" + 18
    ~~~~~~~~~^~~~
TypeError: can only concatenate str (not "int") to str
>>> "年龄：" + str(18)
'年龄：18'
```

报错的最后一行说，只能把 `#!python str`（而不是 `#!python int`）拼接到 `#!python str` 上. 用 `#!python str()` 把数字转换成字符串之后就可以拼接了. 不过这种文字和数字混排的输出，用后面介绍的 [f-字符串](#f-string) 会方便得多.

### 索引与切片

字符串中的每个字符都有一个编号，称为索引（index），用方括号可以取出对应位置的字符. 和尺子上的刻度一样，索引从 0 开始. 不妨把字符串 `#!python "string"` 画在一把尺子上：

```
 +---+---+---+---+---+---+
 | s | t | r | i | n | g |
 +---+---+---+---+---+---+
 0   1   2   3   4   5   6
-6  -5  -4  -3  -2  -1
```

尺子上的第 1 厘米，是从刻度 0 到刻度 1 的那一格. 同样，`#!python word[i]` 取的是从刻度 `i` **开始**的那一格里的字符：

```python
>>> word = "string"
>>> word[0]
's'
>>> word[5]
'g'
```

换句话说，索引就是“这个字符前面有几个字符”：`#!python 's'` 前面没有字符，索引是 0；`#!python 'g'` 前面有 5 个字符，索引是 5.

尺子下面还有一行负数刻度. 负数索引表示“倒数第几个”，`#!python word[-1]` 就是倒数第 1 个字符. 根据[官方文档](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#common-sequence-operations)，Python 遇到负数索引时，会先把它加上字符串的长度，换算成正数索引再去取：

```python
>>> word = "string"
>>> len(word)
6
>>> word[-1]
'g'
>>> word[len(word) - 1]
'g'
>>> word[-6]
's'
```

`#!python word[-1]` 就是 `#!python word[6 - 1]`，也就是 `#!python word[5]`；`#!python word[-6]` 换算成 `#!python word[0]`，是第一个字符. 因为 `#!python -0` 就是 `#!python 0`，倒数只能从 `#!python -1` 数起.

刻度 6 在尺子的最右端，它后面已经没有格子了，所以 `#!python word[6]` 取不到任何字符：

```python
>>> word = "string"
>>> word[6]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    word[6]
    ~~~~^^^
IndexError: string index out of range
```

`word` 有 6 个字符，合法的索引是 0 到 5，或者 -6 到 -1. 超出这个范围，Python 就报告 `#!python IndexError`.

!!! note "Python 怎样找到第 i 个字符"

    在 CPython（官方的 Python 解释器）中，一个字符串的所有字符一个挨一个地存放在内存里，同一个字符串中每个字符占的空间一样大. 要找 `#!python word[i]`，不必从头一个个数，只要从开头跳过 `i` 个字符的位置就到了. 索引就是要跳过的字符个数，所以第一个字符的索引是 0；取长字符串中的字符，也和取短字符串一样快. 这是 CPython 的实现细节，详见 [PEP 393](https://peps.python.org/pep-0393/).

索引一次只能取出一个字符. 要取出连续的一段，就要用切片（slice）. 还是用上面那把尺子，`#!python word[i:j]` 取出的是刻度 `i` 和刻度 `j` 之间的部分：

```python
>>> word = "string"
>>> word[1:4]
'tri'
>>> word[:3]
'str'
>>> word[3:]
'ing'
>>> word[3:6]
'ing'
>>> word[-3:]
'ing'
>>> word[:3] + word[3:]
'string'
```

`#!python word[1:4]` 是刻度 1 到刻度 4 之间的三格，即 `#!python 'tri'`. 换成索引来说，切片包含索引 `i` 的字符，但**不包含**索引 `j` 的字符，也就是**“左闭右开”**.

省略开头表示从最左端的刻度 0 开始，省略结尾表示一直切到最右端（这里是刻度 6），所以 `#!python word[3:]` 和 `#!python word[3:6]` 的结果相同. 刻度 6 不能用作索引，却可以作为切片的终点.

因为是左闭右开，`#!python word[:3]` 和 `#!python word[3:]` 在刻度 3 处切开，既不重复也不遗漏，拼起来正好是原字符串.

这也是索引从 0 开始的一个好处：刻度 `k` 的左边正好有 `k` 个字符，所以 `#!python word[:k]` 就是前 `k` 个字符，`#!python word[k:]` 就是剩下的部分，不需要再加 1 或减 1. 比如要把字符串从中间分成两半，只要用 `//` 把长度整除 2，得到中间的刻度：

```python
>>> word = "string"
>>> mid = len(word) // 2
>>> mid
3
>>> word[:mid]
'str'
>>> word[mid:]
'ing'
```

长度为奇数时，整除会舍去余数，前一半比后一半少一个字符；这时 `#!python t[mid]` 恰好是正中间的那个字符：

```python
>>> t = "abcde"
>>> mid = len(t) // 2
>>> mid
2
>>> t[:mid]
'ab'
>>> t[mid:]
'cde'
>>> t[mid]
'c'
```

切片的范围超出字符串时不会报错，只取实际存在的部分；完全超出时得到空字符串 `#!python ''`：

```python
>>> word = "string"
>>> word[1:100]
'tring'
>>> word[10:]
''
```

这一点和索引不同：前面的 `#!python word[6]` 超出范围，直接报了 `#!python IndexError`.

切片还可以写第三个数 `#!python word[i:j:k]`，称为步长（step），表示每隔几个位置取一个字符. 步长为负数时，切片从右往左取：

```python
>>> word = "string"
>>> word[::2]
'srn'
>>> word[::-1]
'gnirts'
>>> word[::-2]
'git'
>>> word[4:1:-1]
'nir'
```

`#!python word[::2]` 从头开始每两个取一个，即索引 0、2、4 的字符.

步长为负时方向反了过来：省略的开头表示从**最后一个**字符开始，省略的结尾表示一直取到**第一个**字符为止. 所以 `#!python word[::-1]` 把整个字符串倒了过来，`#!python word[::-2]` 取的是索引 5、3、1（-1、-3、-5）. `#!python word[4:1:-1]` 从索引 4 开始向左走，到索引 1 之前停下，取到索引 4、3、2.

无论步长正负，开头的位置总是包含在内，结尾的位置总是不包含.

### 字符串的不可变性

既然能用索引取出某个位置的字符，能不能给这个位置赋一个新字符呢？不能. **字符串一旦创建，内容就不能改变**，即字符串是不可变的（immutable）. 给某个位置赋值会报错：

```python
>>> word = "string"
>>> word[0] = "S"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    word[0] = "S"
    ~~~~^^^
TypeError: 'str' object does not support item assignment
>>> "S" + word[1:]
'String'
```

要得到不同的字符串，只能用已有的部分拼出一个**新**字符串. 下一节的字符串方法也遵循这个规则.

## 2. 常用字符串方法

除了运算符，字符串还自带许多处理文本的工具，称为方法（method）. 方法是属于某种类型的函数，调用时写成 **“值.方法名(参数)”**，例如 `#!python s.upper()`. 它和 `#!python len(s)` 这样的普通函数调用写法不同：`#!python upper()` 是字符串的方法，只能写成 `#!python s.upper()`，不能写成 `#!python upper(s)`. 严格来说，也可以在前面加上类型名，写成 `#!python str.upper(s)`，但这就说来话长了，这里不展开.

上一节说过字符串不可修改，所以**字符串方法从不改变原字符串，而是返回一个新字符串**：

```python
>>> s = "hello world"
>>> s.upper()
'HELLO WORLD'
>>> s.title()
'Hello World'
>>> s
'hello world'
>>> s = s.title()
>>> s
'Hello World'
```

调用 `#!python s.upper()` 和 `#!python s.title()` 之后，`s` 仍然是 `#!python 'hello world'`. 想保留结果，就要把返回的新字符串重新赋给一个名字，比如 `#!python s = s.title()`.

课程中常用的方法如下，后面逐组说明：

| 方法                                                         | 作用                                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------------------ |
| `#!python upper()`、`#!python lower()`、`#!python title()`   | 全部变大写、全部变小写、每个单词首字母大写                               |
| `#!python find(sub)`、`#!python count(sub)`                  | 查找子串第一次出现的位置（找不到返回 `#!python -1`）、统计子串出现的次数 |
| `#!python replace(old, new)`                                 | 把子串 `old` 替换成 `new`                                                |
| `#!python translate(table)`                                  | 按对照表逐个替换字符，对照表用 `#!python str.maketrans()` 生成           |
| `#!python strip()`、`#!python lstrip()`、`#!python rstrip()` | 去掉两端、左端、右端的空白字符                                           |
| `#!python split(sep)`、`#!python join(...)`                  | 按分隔符拆成列表、把列表中的字符串连接起来                               |
| `#!python startswith(p)`、`#!python endswith(p)`             | 是否以 `p` 开头、结尾                                                    |
| `#!python isdigit()`、`#!python isalpha()`                   | 是否全部由数字、字母组成                                                 |

字符串的方法远不止这些. 完整的列表见官方文档的[字符串的方法](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#string-methods)；也可以像实验 2 那样，在 Shell 中用 `#!python dir(str)` 列出所有方法，用 `#!python help(str.find)` 查看某个方法的说明.

### 大小写

`#!python upper()` 把所有字母变成大写，`#!python lower()` 变成小写，`#!python title()` 只把每个单词的首字母变成大写、其余字母变成小写. 比较两个字符串时如果不想区分大小写，可以先把两边都转成小写：

```python
>>> "Python".lower() == "PYTHON".lower()
True
```

### 判断、查找与统计子串

最简单的问题是：一个字符串是否出现在另一个字符串中. 这用运算符 `#!python in` 来判断，结果是 `#!python True` 或 `#!python False`：

```python
>>> word = "string"
>>> "ing" in word
True
>>> "abc" in word
False
```

**`#!python in` 是运算符**而不是方法，和 `+` 一样写在两个值中间. 它只回答“在不在”；想知道子串在哪里、出现了几次，就要用下面的方法.

`#!python find(sub)` 返回子串 `sub` **第一次**出现的位置；第二个参数可以指定从哪个位置开始找；找不到时返回 `#!python -1`. `#!python count(sub)` 返回子串出现的次数：

```python
>>> s = "banana"
>>> s.find("an")
1
>>> s.find("an", 2)
3
>>> s.find("x")
-1
>>> s.count("an")
2
>>> s.count("a")
3
>>> "aaaa".count("aa")
2
```

`#!python "aaaa"` 中的 `#!python "aa"` 只算 2 次，因为 `#!python count()` 统计的是**互不重叠**的出现次数.

使用 `#!python find()` 的结果之前，要先判断它是不是 `#!python -1`. 对非空字符串来说，`#!python -1` 本身就是一个合法的索引（最后一个字符），直接拿去取字符不会报错，只会悄悄地得到错误的结果. 如果只想知道子串在不在，用前面的 `#!python in` 更直接.

字符串还有一个 `#!python index()` 方法，找到时和 `#!python find()` 一样返回位置，找不到时则直接报错：

```python
>>> s = "banana"
>>> s.index("x")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    s.index("x")
    ~~~~~~~^^^^^
ValueError: substring not found
```

### 替换

`#!python replace(old, new)` 把所有的 `old` 都替换成 `new`；第三个参数可以限制最多替换几处. **替换是区分大小写的**：

```python
>>> s = "cats like cats"
>>> s.replace("cats", "dogs")
'dogs like dogs'
>>> s.replace("cats", "dogs", 1)
'dogs like cats'
>>> s
'cats like cats'
>>> "Python".replace("python", "Java")
'Python'
```

最后一个例子中，`#!python "Python"` 里没有小写的 `#!python "python"`，所以什么也没有替换，原样返回.

`#!python replace()` 一次只处理一种替换. 如果想让几个字符各自换成别的字符，连着调用 `#!python replace()` 可能出问题. 比如把“左”和“右”互换：

```python
>>> s = "先左转再右转"
>>> t = s.replace("左", "右")
>>> t
'先右转再右转'
>>> t.replace("右", "左")
'先左转再左转'
>>> s.replace("左", "右").replace("右", "左")
'先左转再左转'
```

最后一行把两步写在了一起. `#!python replace()` 返回的是一个新字符串，所以可以紧接着在这个新字符串上再调用方法，这种写法常被称为链式调用. 它从左往右执行：先算 `#!python s.replace("左", "右")`，得到 `#!python '先右转再右转'`，也就是上面的 `t`；再在这个结果上调用 `#!python .replace("右", "左")`. 所以它和分两步写的结果完全相同，只是没有给中间结果起名字.

无论哪种写法，第一次 `#!python replace()` 都把“左”换成了“右”，这时字符串里已经分不清哪个“右”是原来的、哪个是刚换过来的；第二次又把所有的“右”都换成了“左”.

这时可以用实验 2 中的 `#!python translate()`. 它按一张对照表替换字符，对照表用 `#!python str.maketrans()` 生成：

```python
>>> table = str.maketrans("左右", "右左")
>>> table
{24038: 21491, 21491: 24038}
>>> s = "先左转再右转"
>>> s.translate(table)
'先右转再左转'
```

`#!python str.maketrans()` 的两个参数必须一样长，按位置一一对应：第一个字符串中的第 1 个字符，对应第二个字符串中的第 1 个字符，以此类推. 所以这张表里“左”对应“右”，“右”对应“左”.

直接查看 `table`，显示的是 `#!python {24038: 21491, 21491: 24038}`. 其中 24038 和 21491 正是“左”和“右”的编码，可以用前面讲过的 `#!python ord()` 验证：对照表里记的不是字符本身，而是编码之间的对应. 这种用花括号写成的值叫作字典，后面的章节会讲到.

`#!python str.maketrans()` 只负责生成对照表，不处理某个具体的字符串，所以写成 `#!python str.maketrans(...)`，通过类型名 `#!python str` 来调用.

`#!python translate()` 拿着这张表，逐个查看原字符串中的每个字符：表里有的，换成对应的字符；表里没有的，原样保留. 每个字符只查一次表，换过的字符不会再被换第二次，所以“左”和“右”能正确地互换. 两个方法的详细说明见官方文档：

- `#!python str.translate()`：<https://docs.python.org/zh-cn/3/builtins/stdtypes.html#str.translate>
- `#!python str.maketrans()`：<https://docs.python.org/zh-cn/3/builtins/stdtypes.html#str.maketrans>

!!! note "`#!python str.maketrans()` 的第三个参数"

    `#!python str.maketrans()` 还可以传第三个参数. 它也是一个字符串，其中的每个字符都会被删除：

    ```python
    >>> table = str.maketrans("", "", "-")
    >>> table
    {45: None}
    >>> "138-0000-1234".translate(table)
    '13800001234'
    ```

    这里前两个参数都是空字符串，表示不做任何替换. 对照表里，`-` 的编码 45 对应 `#!python None`，意思是“换成空，也就是删掉”.

### 去除空白

`#!python strip()` 去掉字符串**两端**的空白字符，包括空格、`\n`、`\t` 等；`#!python lstrip()` 只去左端，`#!python rstrip()` 只去右端. 字符串中间的空白不受影响：

```python
>>> s = "  hello  \n"
>>> s.strip()
'hello'
>>> s.lstrip()
'hello  \n'
>>> s.rstrip()
'  hello'
```

给 `#!python strip()` 传入一个字符串作为参数时，它去掉的就不再是空白，而是两端出现在这个参数中的字符：

```python
>>> "**重要**".strip("*")
'重要'
```

如果字符串两端既有空白、又有要去掉的字符，就要小心了：

```python
>>> s = "  **重要**  "
>>> s.strip("*")
'  **重要**  '
>>> s.strip("* ")
'重要'
>>> s.strip().strip("*")
'重要'
>>> "*重*要*".strip("*")
'重*要'
```

`#!python s.strip("*")` 什么也没有去掉. 传入参数之后，`#!python strip()` 只去掉参数里的字符：它**从两端往中间看**，遇到第一个不在参数里的字符就停下. `s` 的两端都是空格，不在 `#!python "*"` 里，所以一开始就停下了.

要连空格一起去掉，可以把空格也写进参数：`#!python "* "` 表示空格和星号都要去掉. 参数只说明要去掉哪些字符，与字符的顺序无关，写成 `#!python " *"` 效果相同（见[官方文档](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#str.strip)）. 也可以用前面讲过的链式调用，先用 `#!python strip()` 去掉空白，再用 `#!python strip("*")` 去掉星号. 最后一个例子说明，和不带参数时一样，夹在中间的字符不受影响.

### 拆分与合并

`#!python split()` 把字符串拆成若干段，得到一个列表（list）. 列表在后面的章节会详细讲，现在只需要知道它是按顺序排好的一组值，写成 `#!python ['2026', '10', '11']` 的样子，并且可以用 `#!python len()` 数出其中有几个元素.

```python
>>> "2026-10-11".split("-")
['2026', '10', '11']
>>> parts = "2026-10-11".split("-")
>>> len(parts)
3
```

`#!python split(sep)` 在**每一个** `sep` 处切开. 如果两个分隔符连在一起，中间就会切出一个空字符串 `#!python ''`：

```python
>>> "1,2,,3".split(",")
['1', '2', '', '3']
>>> "a  b   c".split(" ")    # 仔细查一下有几个空格
['a', '', 'b', '', '', 'c']
```

开头和结尾也是一样. 分隔符出现在开头或结尾时，它外侧也会切出一个空字符串；两端的空白也不会被忽略，而是原样留在切出来的段里：

```python
>>> ",1,2,".split(",")
['', '1', '2', '']
>>> " 1, 2 ".split(",")
[' 1', ' 2 ']
```

也就是说，`#!python split(sep)` 只认 `sep`，其他字符，包括空白，一律当作普通内容. 这一点和下面不带参数的 `#!python split()` 正好相反.

不带参数的 `#!python split()` 则不同：它把连续的任意空白（空格、`\n`、`\t`）当作**一个**分隔符，并且忽略开头和结尾的空白：

```python
>>> "a  b   c".split()
['a', 'b', 'c']
>>> "  12   34  ".split()
['12', '34']
```

所以，当输入中的数据用空格隔开、但空格个数不确定时，应该用不带参数的 `#!python split()`.

`#!python join()` 做相反的事情：把一个列表中的字符串用指定的分隔符连接起来. 注意写法是“分隔符.join(列表)”，并且列表中的元素必须都是字符串：

```python
>>> parts = "2026-10-11".split("-")
>>> parts
['2026', '10', '11']
>>> "/".join(parts)
'2026/10/11'
>>> "".join(["P", "y"])
'Py'
>>> "-".join([1, 2])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    "-".join([1, 2])
    ~~~~~~~~^^^^^^^^
TypeError: sequence item 0: expected str instance, int found
```

### 判断开头、结尾与内容

`#!python startswith()` 和 `#!python endswith()` 判断字符串是否以某个子串开头或结尾：

```python
>>> "report.pdf".endswith(".pdf")
True
>>> "Python".startswith("Py")
True
>>> "Python".startswith("py")
False
>>> "Python".lower().startswith("py")
True
>>> "REPORT.PDF".lower().endswith(".pdf")
True
```

和 `#!python replace()` 一样，这两个方法是区分大小写的，所以 `#!python "Python"` 不以 `#!python "py"` 开头. 它们也没有“不区分大小写”的参数：可选的第二、三个参数指定的是检查的范围，与大小写无关（见[官方文档](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#str.startswith)）. 需要忽略大小写时，可以像前面“大小写”一节那样，先用 `#!python lower()` 把字符串变成小写，再接着调用 `#!python startswith()` 或 `#!python endswith()`. 这样写时，参数也要写成小写.

`#!python isdigit()` 判断字符串是否非空且全部由数字组成：

```python
>>> "2026".isdigit()
True
>>> "-5".isdigit()
False
>>> "3.14".isdigit()
False
>>> "".isdigit()
False
```

它只看每个字符是不是数字，所以负号和小数点都会让它返回 `#!python False`. 也就是说，它不能用来判断一个字符串是不是负数或小数.

!!! note "`#!python isdigit()` 为真，`#!python int()` 也未必能转换"

    `#!python isdigit()` 认的“数字”比 0 到 9 多，例如上标 ² 也算. 但 `#!python int()` 并不认它：

    ```python
    >>> "²".isdigit()
    True
    >>> int("²")
    Traceback (most recent call last):
      File "<stdin>", line 1, in <module>
        int("²")
        ~~~^^^^^
    ValueError: invalid literal for int() with base 10: '²'
    >>> "²".isdecimal()
    False
    >>> "2026".isdecimal()
    True
    ```

    字符串还有一个 `#!python isdecimal()` 方法，它认的数字字符和 `#!python int()` 一致. 平时遇到的都是 0 到 9，两者没有区别；真要先判断再用 `#!python int()` 转换，用 `#!python isdecimal()` 更稳妥.

顺手一提，怎样判断一个字符串是不是合法的负数或小数呢？字符串没有现成的方法能做到. 自己拼凑规则，比如“去掉开头的负号，只允许出现一个小数点”，很难考虑周全：`1.2.3` 这样的章节编号、`1e5` 这样的科学计数法、`--5` 这样多余的负号，都要一一单独处理. 最可靠的标准是 `#!python float()` 本身：能被 `#!python float()` 转换的就是合法的数，不能转换的会报 `#!python ValueError`：

```python
>>> float("-3.14")
-3.14
>>> float("1e5")
100000.0
>>> float("1.2.3")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    float("1.2.3")
    ~~~~~^^^^^^^^^
ValueError: could not convert string to float: '1.2.3'
```

在 PTA 上做题时，题目会给出输入格式，一般不需要自己判断，直接尊重格式进行转换即可. 只有确实需要“先判断，不合法也不让程序停下”时，才要用到下面方框中的 `#!python try` 语句.

!!! note "用 `#!python try` 判断能否转换"

    `#!python try` 语句属于后面异常处理的内容，这里只给出写法：

    ```python
    s = input()
    try:
        x = float(s)
        print(x, "是合法的数")
    except ValueError:
        print(s, "不是合法的数")
    ```

    Python 先执行 `#!python try` 下面的代码. 如果其中的 `#!python float(s)` 报了 `#!python ValueError`，程序不会停止，而是转去执行 `#!python except ValueError` 下面的代码. 输入 `-3.14` 时，输出为：

    ```
    -3.14 是合法的数
    ```

    输入 `1.2.3` 时，输出为：

    ```
    1.2.3 不是合法的数
    ```

`#!python isalpha()` 判断字符串是否非空且全部由字母组成. 这里的“字母”按 Unicode 的定义，汉字也算在内：

```python
>>> "abc".isalpha()
True
>>> "浙大".isalpha()
True
>>> "abc123".isalpha()
False
```

方法名中的 `alpha` 是 alphabet（字母表）的缩写.

## 3. 输出：`#!python print()`

`#!python print()` 把参数显示在屏幕上，更准确地说，是写到标准输出（standard output/stdout）.

### 多个参数与分隔符 `sep`

`#!python print()` 可以一次接收多个参数，显示时它们之间默认用**一个空格**隔开. 参数不必是字符串，数字等其他类型的值会先像 `#!python str()` 那样转换成文字. 用 `sep` 可以指定别的分隔符：

```python
>>> print("总价：", 25, "元")
总价： 25 元
>>> print("总价：", 25, "元", sep="")
总价：25元
>>> print(2026, 10, 11, sep="-")
2026-10-11
>>> print("abc", 3.5, True)
abc 3.5 True
```

第一行输出中“：”和“25”之间、“25”和“元”之间各有一个空格，它们是 `#!python print()` 自动加上的，而不是我们写在字符串里的. 题目要求的输出格式里如果没有这些空格，就要用 `#!python sep=""`，或者改用后面的 [f-字符串](#f-string).

### 结尾 `end`

每次调用 `#!python print()`，输出的最后默认会加一个换行，所以下一次 `#!python print()` 从新的一行开始. 用 `end` 可以改变结尾. 下面是一个脚本和它的输出：

```python
print("第一句", end="")
print("第二句")
print()
print("第三句", end="!\n")
```

```
第一句第二句

第三句!
```

第一行的 `#!python end=""` 去掉了换行，所以“第二句”紧接在“第一句”后面. 不带任何参数的 `#!python print()` 只输出一个换行，得到一个空行.

**`sep` 和 `end` 必须写成 `sep=...`、`end=...` 的形式**，不能省略 `sep=` 或 `end=`，这种写法称为关键字参数（keyword argument）.

## 4. 输入：`#!python input()`

### `#!python input()` 返回字符串

`#!python input()` 从标准输入（standard input/stdin）读取**一整行**，把它作为字符串返回，**末尾的换行不包含在内**. 自己运行程序时，标准输入就是键盘；在 PTA 上，标准输入是题目的测试数据. 即使输入的是数字，得到的也是字符串：

```python
>>> s = input()  # 注意下面的 42 是我们在键盘上输入的，或者是读取题目输入
42
>>> s
'42'
>>> type(s)
<class 'str'>
>>> s + 1
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    s + 1
    ~~^~~
TypeError: can only concatenate str (not "int") to str
>>> n = int(s)
>>> n + 1
43
```

`s` 的值是字符串 `#!python '42'`，不能直接和数字相加，要先用 `#!python int()` 转换成整数.

`#!python input()` 可以带一个参数作为提示语，它会先被显示出来，然后再等待输入：

```python
>>> name = input("你的名字：")
你的名字：Alice
>>> name
'Alice'
```

提示语是**输出**的一部分. 自己运行程序时它很友好，但在 PTA 上做题时它会混进程序的输出里，请格外注意！

### 转换成数字

最常见的写法是把读入和转换写在一起：`#!python n = int(input())` 读入一个整数，`#!python x = float(input())` 读入一个小数. 选用哪一个，要看数据可能是什么样子：

```python
>>> int("3.14")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    int("3.14")
    ~~~^^^^^^^^
ValueError: invalid literal for int() with base 10: '3.14'
>>> float("3.14")
3.14
```

`#!python int()` 只能转换表示整数的字符串，`#!python '3.14'` 不是，于是报 `#!python ValueError`. 题目说输入“可以是小数”时，就应该用 `#!python float()`.

还有两个细节. 字符串两端的空白不影响转换；`#!python int()` 也可以作用于浮点数（注意这里面参数是浮点数而非浮点数的字符串），这时它直接去掉小数部分：

```python
>>> int(" 42 ")
42
>>> int(3.99)
3
>>> int(-3.99)
-3
```

这种去掉小数部分的做法称为**向零截断**，它既不是四舍五入，也不是向下取整. 如果是向下取整，`#!python -3.99` 应当得到 `#!python -4`，而这里得到的是 `#!python -3`.

关于类型转换的更多讨论，可以参阅[类型与值](../faq/typing.md).

### 一行中有多个值

每次调用 `#!python input()` 读取一整行. 如果一行中有多个值，就先用 `#!python split()` 把这一行拆开，再把拆出来的各段分别赋给几个名字：

```python
>>> line = input()
3 4
>>> line
'3 4'
>>> line.split()
['3', '4']
>>> a, b = input().split()
3 4
>>> a
'3'
>>> b
'4'
>>> a + b
'34'
>>> int(a) + int(b)
7
>>> a, b = input().split()
1 2 3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a, b = input().split()
    ^^^^
ValueError: too many values to unpack (expected 2, got 3)
```

`#!python a, b = input().split()` 一次完成了三件事：读入一行，按空白拆成列表，再把列表中的两个元素依次赋给 `a` 和 `b`（解包）. 拆出来的仍然是**字符串**，所以 `#!python a + b` 得到的是 `#!python '34'` 而不是 `#!python 7`，计算之前要先转换.

等号左边名字的个数，必须和拆出来的段数一致. 最后一次输入的 `1 2 3` 拆出了三段，左边却只有两个名字，报错的最后一行说的正是这件事：要分给名字的值太多了，只需要 2 个（expected 2），实际有 3 个（got 3）. 这样的语法实际上是**解包**，课上与我们后面都会有讲解.

如果数据之间用逗号等其他符号隔开，就把这个符号传给 `#!python split()`：

```python
>>> a, b = input().split(",")
3,4
>>> a, b
('3', '4')
>>> x, y = input().split(",")
3, 4
>>> x, y
('3', ' 4')
>>> int(y)
4
```

输入 `3, 4` 时逗号后面有空格，拆出来的 `y` 是 `#!python ' 4'`：前面讲过，`#!python split(sep)` 只认分隔符，不会去掉空白. 好在 `#!python int()` 会忽略两端的空白，转换结果不受影响.

!!! tip "简便写法"

    拆出来的每一段都要转换时，可以用 `#!python map()` 一次完成：

    ```python
    >>> a, b, c = map(float, input().split())
    1.5 2 3
    >>> a + b + c
    6.5
    ```

    `#!python map(float, ...)` 对拆出来的每一段都调用 `#!python float()`. 可以把它想象成数学里的映射：`#!python float` 是对应法则，`#!python split()` 拆出来的列表中的每一个元素，都按这同一个法则，调用 `#!python float()` 变成一个浮点数. 这也是 `#!python map` 这个名字的由来，map 在英文里就有“映射”的意思. 它的详细用法见 [map 函数与列表推导式](../faq/map.md).

### 读多行，还是读一行 { #input-lines }

一个 `#!python input()` 只读一行，所以程序中 `#!python input()` 的个数和写法，必须和输入数据的行数、每行的内容对应起来. 假设输入只有一行 `3 4`，下面这个程序却按两行来读：

```python
a = int(input())
b = int(input())
print(a + b)
```

```
Traceback (most recent call last):
  File "main.py", line 1, in <module>
    a = int(input())
ValueError: invalid literal for int() with base 10: '3 4'
```

第一个 `#!python input()` 读到的是整行 `#!python '3 4'`，它不是一个整数，`#!python int()` 当场报错. 如果读入时没有立即转换，第一行能顺利读完，错误就出现在第二个 `#!python input()` 上：

```python
a = input()
b = input()
print(a, b)
```

```
Traceback (most recent call last):
  File "main.py", line 2, in <module>
    b = input()
EOFError: EOF when reading a line
```

EOF 是 end of file 的缩写，意思是输入已经读完了，第二个 `#!python input()` 没有更多的行可读. 这两种报错都说明程序读取输入的方式和数据的格式对不上.

## 5. 格式化输出

### f-字符串 { #f-string }

用 `+` 拼接文字和数字时，每个数字都要先用 `#!python str()` 转换；用 `#!python print()` 的多个参数时，又会多出空格. 格式化字符串字面值（formatted string literal），简称 f-字符串（f-string），可以同时解决这两个问题：在字符串的引号前加上 `f`，字符串中用花括号 `{}` 括起来的部分会被替换成其中表达式的值.

```python
>>> name = "Alice"
>>> score = 92.456
>>> "成绩：" + str(score) + " 分"
'成绩：92.456 分'
>>> f"{name} 的成绩是 {score} 分"
'Alice 的成绩是 92.456 分'
>>> f"1 + 2 = {1 + 2}"
'1 + 2 = 3'
>>> f"{name.upper()} 有 {len(name)} 个字母"
'ALICE 有 5 个字母'
>>> f"{{name}} 会显示成 {name}"
'{name} 会显示成 Alice'
```

花括号中可以是名字，也可以是任意表达式，包括计算和方法调用. 花括号外的部分原样保留，`1 + 2 = ` 这几个字符就是普通文字. 如果需要在结果中显示花括号本身，就写成两个：`{{` 和 `}}`.

### 格式说明符

在花括号中的**表达式后面加一个冒号**，冒号后面的格式说明符（format specifier）可以控制这个值显示成什么样子. 下面按用途介绍最常用的几种.

**保留小数**：`.nf` 表示以小数形式显示，保留 `n` 位小数. 其中的 `f` 取自 fixed-point，即定点表示法.

```python
>>> x = 3.14159
>>> f"{x:.2f}"
'3.14'
>>> f"{x:.0f}"
'3'
>>> f"{2:.2f}"
'2.00'
>>> f"{x}"
'3.14159'
>>> f"{x:f}"
'3.141590'
```

`.2f` 对整数同样适用，`#!python 2` 显示成了 `2.00`. 精度为 0 时，连小数点也不显示. 只写 `f` 不写精度，则默认保留 6 位小数.

**宽度与对齐**：冒号后的一个整数表示最少占多少个字符的宽度，不够时用空格补齐. **字符串默认靠左，数字默认靠右**.

`<`、`>`、`^` 分别指定靠左、靠右、居中；对齐符号前面还可以写一个用来填充的字符. 下面的例子在结果两边加了方括号，只是为了看清空格的位置：

```python
>>> f"[{'hi':6}]"
'[hi    ]'
>>> f"[{42:6}]"
'[    42]'
>>> f"[{'hi':<6}]"
'[hi    ]'
>>> f"[{'hi':>6}]"
'[    hi]'
>>> f"[{'hi':^6}]"
'[  hi  ]'
>>> f"[{'hi':*^6}]"
'[**hi**]'
>>> f"[{3.14159:8.2f}]"
'[    3.14]'
>>> f"[{3.14159:^8.2f}]"
'[  3.14  ]'
>>> f"[{3.14159:*^8.2f}]"
'[**3.14**]'
>>> f"{7:02d}:{5:02d}"
'07:05'
>>> f"[{'hello':3}]"
'[hello]'
```

`8.2f` 同时规定了宽度和小数位数：保留两位小数，整体占 8 个字符. `02d` 表示按十进制整数显示（`d` 的含义是 decimal，十进制），宽度为 2、不足时在前面补 0，常用于显示时间. 宽度只是**最少**的字符数，内容更长时不会被截断，最后一个例子中的 `#!python 'hello'` 依然完整显示.

**千位分隔符与百分比**：逗号 `,` 表示每三位插入一个千位分隔符；`%` 先把数乘以 100，再加上百分号. 下面的 `,.2f` 和 `.1%` 还同时指定了保留的小数位数：

```python
>>> f"{1234567.891:,.2f}"
'1,234,567.89'
>>> f"{0.856:.1%}"
'85.6%'
```

**进制**：`b`、`o`、`x` 分别以二进制（binary）、八进制（octal）、十六进制（hexadecimal）显示整数，前面加上 `#` 会带上 `0b`、`0o`、`0x` 前缀：

```python
>>> f"{255:b} {255:o} {255:x}"
'11111111 377 ff'
>>> f"{255:#b} {255:#o} {255:#x}"
'0b11111111 0o377 0xff'
```

**组合使用**：上面这些说明符可以写在一起. 下面的 `*>12,.2f` 依次规定了填充字符、对齐方式、宽度、千位分隔符和小数位数，这个顺序不能随意调换：

```python
>>> f"{1234.5:*>12,.2f}"
'****1,234.50'
```

**显示表达式**：在表达式后面写一个等号 `=`，结果会同时显示表达式本身和它的值，检查程序时很方便：

```python
>>> age = 19
>>> f"{age=}"
'age=19'
>>> f"{age + 1=}"
'age + 1=20'
```

下表汇总了这些说明符：

| 写法            | 含义                     | 例子                             | 结果                         |
| --------------- | ------------------------ | -------------------------------- | ---------------------------- |
| `.nf`           | 保留 `n` 位小数          | `#!python f"{3.14159:.2f}"`      | `3.14`                       |
| `w`             | 最小宽度为 `w`           | `#!python f"{42:6}"`             | 前面补 4 个空格，共 6 个字符 |
| `<`、`>`、`^`   | 靠左、靠右、居中         | `#!python f"{'hi':^6}"`          | 两边各补 2 个空格            |
| 填充字符 + 对齐 | 用指定字符补齐           | `#!python f"{'hi':*^6}"`         | `**hi**`                     |
| `0wd`           | 整数宽度为 `w`，前面补 0 | `#!python f"{7:02d}"`            | `07`                         |
| `,`             | 千位分隔符               | `#!python f"{1234567.891:,.2f}"` | `1,234,567.89`               |
| `.n%`           | 百分比，保留 `n` 位小数  | `#!python f"{0.856:.1%}"`        | `85.6%`                      |
| `b`、`o`、`x`   | 二进制、八进制、十六进制 | `#!python f"{255:x}"`            | `ff`                         |
| `=`             | 显示表达式和它的值       | `#!python f"{age=}"`             | `age=19`                     |

完整的规则见官方文档的[格式规格迷你语言](https://docs.python.org/zh-cn/3/library/string.html#formatspec).

### 常见错误：忘记转换类型

`.2f` 这样的说明符只能用于数字. `#!python input()` 读到的是字符串，忘记转换就直接格式化，会得到下面的错误：

```python
>>> price = input()
9.5
>>> f"{price:.2f}"
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    f"{price:.2f}"
      ^^^^^^^^^^^
ValueError: Unknown format code 'f' for object of type 'str'
>>> f"{float(price):.2f}"
'9.50'
```

报错的意思是：格式代码 `f` 不能用于 `#!python str` 类型的对象. 先用 `#!python float()` 转换就好了.

### 保留小数用格式化，不用 `#!python round()`

内置函数 `#!python round(x, n)` 把 `x` **舍入**到 `n` 位小数，但它返回的是一个**数**，而不是一段文字. 数在显示时不会保留末尾的 0：

```python
>>> round(3.14159, 2)
3.14
>>> round(2.50, 2)
2.5
>>> f"{2.5:.2f}"
'2.50'
>>> 0.1 + 0.2
0.30000000000000004
>>> f"{0.1 + 0.2:.2f}"
'0.30'
```

题目要求“保留两位小数”时，输出应当是 `2.50` 而不是 `2.5`. `#!python round()` 做不到这一点，要用格式说明符 `.2f`. 另外，大多数小数在计算机中只能近似表示，`#!python 0.1 + 0.2` 的结果并不恰好是 `#!python 0.3`；用格式说明符控制位数，也能避免把这样的误差显示出来.

还要强调一点：**`#!python round()` 和格式说明符都不是数学课上的“四舍五入”**，它们严格按照[官方文档](https://docs.python.org/zh-cn/3/builtins/functions.html#round)规定的规则舍入. 最明显的区别是，遇到正好一半的情况时会舍入到偶数，`#!python round(2.5)` 得到的是 `#!python 2`. 下面的方框给出了更多细节.

!!! note "舍入的细节"

    根据[官方文档](https://docs.python.org/zh-cn/3/library/string.html#formatspec)，格式说明符对浮点数的舍入方式与 `#!python round()` 相同. 首先，遇到正好一半的情况时，`#!python round()` 会舍入到偶数：

    ```python
    >>> round(0.5)
    0
    >>> round(1.5)
    2
    >>> round(2.5)
    2
    ```

    其次，舍入的对象是浮点数在计算机中实际存储的值，而不是我们写下的十进制数：

    ```python
    >>> round(2.675, 2)
    2.67
    >>> f"{2.675:.2f}"
    '2.67'
    >>> f"{2.675:.20f}"
    '2.67499999999999982236'
    ```

    `#!python 2.675` 实际存储的值比 2.675 略小一点，所以两种方式都舍入成了 `2.67`. 关于浮点数为什么不能精确表示，可以阅读官方教程的[浮点算术：问题和限制](https://docs.python.org/zh-cn/3/tutorial/floatingpoint.html).

!!! tip "f-字符串中的引号"

    在 Python 3.12 之前，f-字符串的花括号内不能使用和外层相同的引号. 3.12 取消了这个限制，所以下面的写法在 3.15 中是合法的：

    ```python
    >>> f"{"hi".upper()}"
    'HI'
    ```

    但如果 Thonny 或 PTA 使用的是较早的 Python，同样的代码会报语法错误. 为了保险，**花括号内的字符串请使用和外层不同的引号**，像前面的 `#!python f"{'hi':*^6}"` 那样.

!!! note "关于 `#!python str.format()`"

    在一些较早的资料中，常常能看到字符串的 `#!python format()` 方法. 它同样用 `{}` 标记要替换的位置，但要替换进去的值作为参数传给 `#!python format()`，按顺序填入；也可以在花括号中写上编号，指定填入第几个参数：

    ```python
    >>> "{} 的成绩是 {:.1f}".format("Alice", 92.456)
    'Alice 的成绩是 92.5'
    >>> "{0} 和 {1}，{1} 和 {0}".format("甲", "乙")
    '甲 和 乙，乙 和 甲'
    ```

    冒号后面的格式说明符和 f-字符串完全相同. 新写的代码用 f-字符串即可.

## 6. 在 PTA 上做题时的输入输出 { #pta }

在 PTA 上提交程序后，测试数据会作为程序的输入：程序中的 `#!python input()` 读到的就是这些数据. 程序用 `#!python print()` 输出的全部内容，会与题目的标准答案进行比较. 因此，做题时的输入输出要注意以下几点.

**不要写提示语.** 前面说过，`#!python input()` 的提示语也是输出. 以下面的程序为例：

```python
n = int(input("请输入一个整数："))
print(n * 2)
```

测试数据为 `21` 时，它实际输出的是：

```
请输入一个整数：42
```

而标准答案只有 `42`. 提示语混进了输出，答案就不对了. 做题时一律写成 `#!python input()`.

**按输入格式读取.** 先看清题目的“输入格式”和“输入样例”：数据分几行，每行有几个值，用什么隔开. 每一行对应一个 `#!python input()`；同一行的多个值用 `#!python split()` 拆开. 读取方式和数据格式对不上，就会出现[前面](#input-lines)那样的 `#!python ValueError` 或 `#!python EOFError`.

**按题意转换类型.** 题目说“可以是小数”的数据要用 `#!python float()`，用 `#!python int()` 会在遇到小数时报错. 只有确定是整数的数据才用 `#!python int()`.

**输出与样例逐字符一致.** 文字、空格、标点都要和“输出样例”完全相同，中文冒号 `：` 和英文冒号 `:` 是不同的字符. 要特别留意 `#!python print()` 在多个参数之间自动加上的空格. 一个稳妥的做法是从题目的输出样例中复制文字，再把其中会变化的部分换成 f-字符串的 `{}`.

**保留小数用格式说明符.** 要求保留几位小数，就用 `.nf`，不要用 `#!python round()`.

下面用一道例题把这一页的内容串起来.

!!! quote "例题"

    输入一行，依次是姓名、身高（米）和体重（千克），用空格隔开. 计算 BMI（体重除以身高的平方），按样例格式输出，保留 1 位小数.

    输入样例：

    ```
    Alice 1.65 55
    ```

    输出样例：

    ```
    Alice 的 BMI 为 20.2
    ```

三个值在同一行，所以用一个 `#!python input()` 读入，再用 `#!python split()` 拆成三段. 身高和体重都可能是小数，要转换成 `#!python float`；姓名本来就是文字，保持字符串即可. 输出时从样例中复制文字，再用 f-字符串填入姓名和保留 1 位小数的 BMI：

```python
name, height, weight = input().split()
height = float(height)
weight = float(weight)
bmi = weight / height ** 2
print(f"{name} 的 BMI 为 {bmi:.1f}")
```

用样例输入运行，输出为：

```
Alice 的 BMI 为 20.2
```
