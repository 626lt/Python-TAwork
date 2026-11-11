# 列表与元组

到目前为止，我们用过的整数、浮点数、布尔值，一个对象只表示一个数据. 实际的问题处理的却常常是一批数据：一个班的成绩，一句话中的所有单词，一个月每天的气温. 如果每个数据都要起一个名字，5 个成绩需要 5 个名字，500 个成绩就没法写了. Python 提供了几种复合数据类型（compound data type），可以把一批数据放进一个对象，作为一个整体来保存和处理. 这样的对象也称为数据容器，其中的每一个数据称为元素.

本章讲其中的两种：列表（list）和元组（tuple）. 它们和早已用过的字符串一样，元素按顺序排成一列，统称序列. 列表创建之后可以随时增删、替换元素，是最常用的容器；元组创建之后不能修改，适合表示固定的一组值. 另外两种容器是字典和集合，我们将在下一章讲解.

我们在第 1 节说明容器的用处和分类，第 2 节讲字符串、列表、元组共用的一套操作，第 3、4 节分别讲列表和元组，最后讲在 PTA 上做题时怎样用列表读入、处理和输出一批数据.

建议读者打开 Thonny，把下面的例子亲手试一遍. 和前两章一样，以 `>>>` 开头的行是在 Shell 中输入的代码，没有 `>>>` 的行是 Python 给出的结果；写成脚本的例子，我们在代码后面写出运行时的输入和输出.

## 1. 数据容器

### 把一批数据当作一个整体

把 5 个成绩放进一个列表，绑定到一个名字 `scores` 上：

```python
>>> scores = [88, 76, 95, 60, 83]
>>> len(scores)
5
>>> sum(scores)
402
>>> max(scores)
95
>>> sum(scores) / len(scores)
80.4
```

有了列表，“一共几个”“总和是多少”“最高分是多少”都只需要一次函数调用，而且成绩从 5 个变成 500 个，这几行代码一个字也不用改. 这就是容器的用处：程序处理的是“这一批数据”，不必关心其中具体有几个.

### 两个问题给容器分类

Python 常用的容器有五种：字符串、列表、元组、字典（dict）和集合（set）. 它们的区别主要在两个问题上.

**第一个问题：元素有没有位置.** 字符串、列表、元组中的元素排成一列，每个元素有一个位置编号，也就是索引，可以写 `#!python s[0]` 取出第一个. 这样的容器是**有序**的. 字典和集合中的元素没有位置编号，不能用 `#!python [0]` 取“第一个”：字典靠“键”来找对应的值，集合只关心一个元素在不在其中. 课上把它们称为**无序**的容器.

**第二个问题：内容能不能修改.** 我们在[输入、输出与字符串](io-string.md)一章中讲过，字符串一旦创建，内容就不能改变，是不可变的（immutable）. 元组也是如此. 列表、字典和集合则是可变的（mutable）：创建之后，可以添加、删除、替换其中的元素，对象还是原来那一个.

| 类型               | 字面量的样子                         | 有序 | 可变 |
| ------------------ | ------------------------------------ | ---- | ---- |
| 字符串 `#!python str`   | `#!python 'hello'`                   | 是   | 否   |
| 列表 `#!python list`    | `#!python ['h', 'e', 'l', 'l', 'o']` | 是   | 是   |
| 元组 `#!python tuple`   | `#!python ('h', 'e', 'l', 'l', 'o')` | 是   | 否   |
| 字典 `#!python dict`    | `#!python {'h': 1, 'e': 2}`          | 否   | 是   |
| 集合 `#!python set`     | `#!python {'h', 'e', 'l', 'o'}`      | 否   | 是   |

不可变的容器比较安全，数据不会被程序的其他部分无意中改掉；可变的容器比较灵活，数据可以随时增减. 数据确定之后不再变化的，适合用不可变的类型；需要边处理边增删的，用可变的类型.

### 序列

元素有先后次序、可以用索引访问的容器，统称序列（sequence）. 序列不是一种具体的类型，而是字符串、列表、元组这一类类型的统称，Python 中并没有一个叫“序列”的类型可以直接创建. 这个统称有用，是因为三种序列共用同一套操作：在字符串上学过的索引、切片、`+`、`*`、`#!python in`、`#!python len()`，在列表和元组上的写法和含义完全相同. 我们将在下一节讲解这套操作.

## 2. 序列的通用操作

本节的操作对字符串、列表、元组都适用. 例子大多用列表来写，列表用方括号把元素括起来，元素之间用逗号隔开；换成元组或字符串，结果是一样的.

### 索引与切片

我们在[输入、输出与字符串](io-string.md)一章中，将字符串画在一把尺子上，来解释索引和切片. 列表也可以画在同一把尺子上，只是每一格里放的不再是一个字符，而是一个元素：

```
 +---+---+---+---+----+----+
 | 2 | 3 | 5 | 7 | 11 | 13 |
 +---+---+---+---+----+----+
 0   1   2   3   4    5    6
-6  -5  -4  -3  -2   -1
```

规则和字符串完全一样：索引从 0 开始，`#!python a[i]` 是从刻度 `i` 开始的那一格；负数索引从末尾倒着数；切片 `#!python a[i:j]` 取刻度 `i` 和 `j` 之间的部分，左闭右开；第三个数是步长，步长为负时从右往左取.

```python
>>> a = [2, 3, 5, 7, 11, 13]
>>> a[0]
2
>>> a[-1]
13
>>> a[1:3]
[3, 5]
>>> a[:3]
[2, 3, 5]
>>> a[-3:]
[7, 11, 13]
>>> a[::2]
[2, 5, 11]
>>> a[4:0:-1]
[11, 7, 5, 3]
>>> a[::-1]
[13, 11, 7, 5, 3, 2]
>>> a[1:100]
[3, 5, 7, 11, 13]
>>> a[6]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a[6]
    ~^^^
IndexError: list index out of range
```

`#!python a[4:0:-1]` 从索引 4 开始向左走，到索引 0 之前停下，所以取到索引 4、3、2、1 的元素，不包含 `#!python a[0]`. 最后两行再次说明索引和切片对越界的态度不同：切片 `#!python a[1:100]` 超出范围时只取实际存在的部分；索引 `#!python a[6]` 超出范围则报 `#!python IndexError`，报错信息中的 `string` 换成了 `list`. 步长不能为 0.

索引和切片得到的东西不一样：

```python
>>> t = (2, 3, 5, 7)
>>> t[1:3]
(3, 5)
>>> "hello"[1:3]
'el'
>>> a = [2, 3, 5, 7]
>>> a[1]
3
>>> a[1:2]
[3]
```

**切片得到的是一个和原序列类型相同的新序列**：列表的切片是列表，元组的切片是元组，字符串的切片是字符串，原序列不受影响. 索引得到的则是元素本身. 所以 `#!python a[1]` 是整数 `#!python 3`，而 `#!python a[1:2]` 是只有一个元素的列表 `#!python [3]`，两者不是一回事.

身份证号、日期、商品条码这类数据，每一段的宽度固定，中间又没有分隔符，`#!python split()` 无从下手，用切片正好. 比如从 8 位的日期中取出年、月、日：

```python
>>> date = "20261107"
>>> year = date[:4]
>>> month = date[4:6]
>>> day = date[6:]
>>> year, month, day
('2026', '11', '07')
>>> int(month)
11
```

切出来的仍然是字符串，要参与计算，还要用 `#!python int()` 转换.

### 拼接与重复

`+` 把两个序列首尾相接，`*` 把序列重复若干次，得到的都是新的序列：

```python
>>> [1, 2] + [3, 4]
[1, 2, 3, 4]
>>> (1, 2) + (3, 4)
(1, 2, 3, 4)
>>> [0] * 5
[0, 0, 0, 0, 0]
>>> ("a", "b") * 2
('a', 'b', 'a', 'b')
```

`#!python [0] * 5` 是得到“5 个 0”的简便写法，常用来准备一个固定长度的列表，之后再逐个填入数据.

`+` 两边必须是同一种序列. 列表不能和元组相加，也不能和一个单独的数相加：

```python
>>> [1, 2] + (3, 4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    [1, 2] + (3, 4)
    ~~~~~~~^~~~~~~~
TypeError: can only concatenate list (not "tuple") to list
>>> [1, 2] + 3
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    [1, 2] + 3
    ~~~~~~~^~~
TypeError: can only concatenate list (not "int") to list
>>> [1, 2] + [3]
[1, 2, 3]
```

这和字符串不能加数字是同一个道理. 要用 `+` 往列表末尾加一个数，得先把这个数写成只有一个元素的列表 `#!python [3]`.

### 成员判断

`#!python in` 判断一个值是不是序列中的元素，`#!python not in` 判断它不是：

```python
>>> 3 in [1, 2, 3]
True
>>> "cat" not in ("dog", "bird")
True
>>> "1" in [1, 2, 3]
False
```

列表 `#!python [1, 2, 3]` 中的元素是整数，字符串 `#!python "1"` 不等于其中任何一个，所以第三行是 `#!python False`. 从 `#!python input()` 读来的数据忘记转换类型时，容易在这里出错.

字符串的 `#!python in` 有一点特殊：

```python
>>> "ab" in "abc"
True
>>> [1, 2] in [1, 2, 3]
False
```

对字符串，`#!python in` 判断的是左边是不是右边的**子串**，可以是连续的好几个字符. 对列表和元组，`#!python in` 只拿左边的值和每一个**元素**逐个比较：`#!python [1, 2, 3]` 的三个元素是 `#!python 1`、`#!python 2`、`#!python 3`，没有哪一个元素等于列表 `#!python [1, 2]`.

### 查找与计数

三种序列都有 `#!python count()` 和 `#!python index()` 两个方法. `#!python s.count(x)` 返回 `x` 在序列中出现的次数；`#!python s.index(x)` 返回 `x` **第一次**出现的索引，`#!python s.index(x, i)` 则从索引 `i` 开始找：

```python
>>> fruits = ["apple", "banana", "cherry", "apple"]
>>> fruits.count("apple")
2
>>> fruits.count("pear")
0
>>> fruits.index("apple")
0
>>> fruits.index("apple", 1)
3
>>> fruits.index("pear")
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    fruits.index("pear")
    ~~~~~~~~~~~~^^^^^^^^
ValueError: list.index(x): x not in list
```

要找的值不存在时，两个方法的表现不同：`#!python count()` 返回 0，`#!python index()` 则报 `#!python ValueError`. 所以不确定元素在不在序列中时，先用 `#!python in` 判断，再用 `#!python index()` 定位：

```python
fruits = ["apple", "banana", "cherry", "apple"]
name = "pear"
if name in fruits:
    print(fruits.index(name))
else:
    print("没有找到")
```

运行后的输出是：

```
没有找到
```

### 常用内置函数

`#!python len()` 返回元素个数，`#!python max()`、`#!python min()` 返回最大、最小的元素，`#!python sum()` 返回所有元素的和，`#!python sorted()` 返回排好序的结果：

```python
>>> nums = [21, 3, 55, -7, 11]
>>> len(nums)
5
>>> max(nums)
55
>>> min(nums)
-7
>>> sum(nums)
83
>>> sum(nums) / len(nums)
16.6
>>> sorted(nums)
[-7, 3, 11, 21, 55]
>>> sorted(nums, reverse=True)
[55, 21, 11, 3, -7]
>>> nums
[21, 3, 55, -7, 11]
```

`#!python sorted()` 默认从小到大排，加上 `#!python reverse=True` 则从大到小. 它**不改变原来的序列**，而是把排序的结果放进一个新列表返回：最后一行的 `nums` 还是原来的顺序.

这几个函数各有需要留意的地方：

```python
>>> sorted("world")
['d', 'l', 'o', 'r', 'w']
>>> sorted((3, 1, 2))
[1, 2, 3]
>>> max("hello")
'o'
>>> sum(["1", "2"])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    sum(["1", "2"])
    ~~~^^^^^^^^^^^^
TypeError: unsupported operand type(s) for +: 'int' and 'str'
>>> sum([])
0
>>> max([])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    max([])
    ~~~^^^^
ValueError: max() iterable argument is empty
```

- 无论参数是字符串、元组还是列表，`#!python sorted()` 返回的**总是列表**.
- 字符之间按编码比较大小，所以 `#!python max("hello")` 是编码最大的 `#!python 'o'`.
- `#!python sum()` 只能对数求和. 它从 0 开始把元素一个个加上去，遇到字符串就成了 `#!python 0 + "1"`，于是报 `#!python TypeError`. `#!python split()` 拆出来的是字符串，求和之前要先转换成数.
- 空列表的和是 0，但空列表没有最大值，`#!python max([])` 会报 `#!python ValueError`. 对可能为空的列表求最大、最小值或平均值之前，要先判断它是否为空. 我们在[流程控制](control-flow.md)一章“真值测试”一节中说过，空的容器在条件中算作假，所以可以直接写 `#!python if nums:`.

### 遍历

我们在[流程控制](control-flow.md)一章中，用 `#!python for` 循环逐个取出字符串中的字符. 序列都是可迭代对象，列表和元组可以用同样的方式遍历，每一轮取出一个元素：

```python
scores = [88, 76, 95]
for s in scores:
    print(s)
```

运行后的输出是：

```
88
76
95
```

只需要元素本身时，这是最直接的写法. 如果还需要元素的位置，可以像遍历字符串的索引那样，用 `#!python range(len(scores))`：

```python
scores = [88, 76, 95]
for i in range(len(scores)):
    print(f"第 {i} 号元素是 {scores[i]}")
```

运行后的输出是：

```
第 0 号元素是 88
第 1 号元素是 76
第 2 号元素是 95
```

也可以用内置函数 [`#!python enumerate()`](https://docs.python.org/zh-cn/3/builtins/functions.html#enumerate). 它把每个元素和它的索引配成一对，用 `#!python list()` 可以看到这些“对”：

```python
>>> list(enumerate(["a", "b", "c"]))
[(0, 'a'), (1, 'b'), (2, 'c')]
```

每一对是一个元组，前一个是索引，后一个是元素. 在 `#!python for` 后面写两个名字，就能把每一对解包，同时拿到索引和元素：

```python
scores = [88, 76, 95]
for i, s in enumerate(scores):
    print(f"第 {i} 号元素是 {s}")
```

输出和上一个程序完全相同. 这种写法不必再写 `#!python scores[i]`，也不会把索引写错，同时需要索引和元素时推荐使用.

序列通用操作的完整列表见官方文档的[通用序列操作](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#common-sequence-operations).

## 3. 列表

列表是最常用的容器. 它有序，可以用上一节的全部操作；它可变，元素可以随时增加、删除、替换；它对元素的类型没有限制，一个列表里可以同时放整数、字符串，甚至另一个列表.

### 已经用过的列表

列表并不陌生，前两章处理输入输出时已经多次用到它：

```python
>>> parts = "2026 11 07".split()
>>> parts
['2026', '11', '07']
>>> type(parts)
<class 'list'>
>>> len(parts)
3
>>> "-".join(parts)
'2026-11-07'
>>> y, m, d = parts
>>> m
'11'
>>> first, *rest = parts
>>> rest
['11', '07']
>>> list(range(3))
[0, 1, 2]
```

- [输入、输出与字符串](io-string.md)中，`#!python split()` 拆出来的结果是列表，`#!python join()` 连接的是列表中的字符串.
- `#!python a, b = input().split()` 是对列表解包：把 `#!python split()` 得到的列表中的元素依次分给左边的名字.
- [流程控制](control-flow.md)中，带星号的名字收集到的“剩下的元素”是一个列表；`#!python for s in input().split():` 遍历的是一个列表；为了看到 `#!python range()` 中有哪些数，用 `#!python list()` 把它转换成了列表.

当时只需要知道列表是“按顺序排好的一组值”. 我们将在本节讲解列表的完整用法：怎样创建，怎样修改其中的元素，怎样增删和排序.

### 创建列表

直接用方括号写出元素，是列表的字面量；内置函数 `#!python list()` 则把其他可迭代对象中的元素逐个取出，放进一个新列表：

```python
>>> []
[]
>>> [1, "two", 3.0, True]
[1, 'two', 3.0, True]
>>> list("hello")
['h', 'e', 'l', 'l', 'o']
>>> list(range(1, 10, 2))
[1, 3, 5, 7, 9]
>>> list((1, 2, 3))
[1, 2, 3]
>>> list()
[]
>>> list(123)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    list(123)
    ~~~~^^^^^
TypeError: 'int' object is not iterable
```

`#!python []` 和 `#!python list()` 都是空列表，一个元素也没有. `#!python list()` 的参数必须能逐个取出元素：字符串可以，取出的是一个个字符；整数 `#!python 123` 不行，报错的最后一行说 `#!python int` 对象不是可迭代的（not iterable）.

### 修改元素

列表是可变的，可以给某个位置赋一个新的值：

```python
>>> a = [1, 3, 5, 7, 11]
>>> a[0] = 2
>>> a
[2, 3, 5, 7, 11]
>>> a[-1] = 13
>>> a
[2, 3, 5, 7, 13]
>>> a[5] = 17
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a[5] = 17
    ~^^^
IndexError: list assignment index out of range
```

同样的写法用在字符串上会报 `#!python TypeError`，因为字符串不可变. 赋值的位置必须已经存在：`a` 只有 5 个元素，没有索引 5，所以 `#!python a[5] = 17` 报 `#!python IndexError`. **给索引赋值只能替换已有的元素，不能添加新元素**；添加元素要用后面的 `#!python append()`.

在循环中修改元素，要通过索引来改. 下面的程序想给每个成绩加 5 分：

```python
scores = [88, 76, 95]
for s in scores:
    s = s + 5
print(scores)
```

运行后的输出是：

```
[88, 76, 95]
```

列表没有变. 每一轮，`s` 这张标签贴在列表的一个元素上；`#!python s = s + 5` 算出一个新的整数，把标签 `s` 揭下来贴到新整数上，列表中的那个位置仍然指着原来的整数. 要改变列表，得给列表的位置赋值：

```python
scores = [88, 76, 95]
for i in range(len(scores)):
    scores[i] = scores[i] + 5
print(scores)
```

运行后的输出是：

```
[93, 81, 100]
```

给切片赋值，可以一次替换一段元素. 等号右边的元素个数不必和被替换的那一段相同：

```python
>>> nums = [10, 20, 30, 40, 50]
>>> nums[1:4] = [25, 35]
>>> nums
[10, 25, 35, 50]
>>> nums[1:1] = [100, 200]
>>> nums
[10, 100, 200, 25, 35, 50]
>>> nums[3:5] = []
>>> nums
[10, 100, 200, 50]
```

第一次把索引 1 到 3 的三个元素换成了两个. `#!python nums[1:1]` 是刻度 1 处一段长度为 0 的切片，给它赋值相当于在刻度 1 处插入. 把一段切片换成空列表，相当于删除这一段.

### 添加元素

| 方法                         | 作用                                   |
| ---------------------------- | -------------------------------------- |
| `#!python a.append(x)`       | 在末尾添加一个元素 `x`                 |
| `#!python a.extend(t)`       | 把可迭代对象 `t` 中的元素逐个添加到末尾 |
| `#!python a.insert(i, x)`    | 在索引 `i` 的位置插入 `x`，原来这个位置及其后的元素依次后移 |

```python
>>> a = [6, 3, 5]
>>> a.append(13)
>>> a
[6, 3, 5, 13]
>>> a.extend([7, 1])
>>> a
[6, 3, 5, 13, 7, 1]
>>> a.extend("ab")
>>> a
[6, 3, 5, 13, 7, 1, 'a', 'b']
>>> a.insert(0, 99)
>>> a
[99, 6, 3, 5, 13, 7, 1, 'a', 'b']
```

这三个方法都直接修改列表 `a` 本身，这种修改方式称为**原地**修改. 调用之后 Shell 什么也没有显示，要再输入 `a` 才能看到修改后的列表. 我们将在下面“原地修改的方法返回 `#!python None`”一节解释其中的原因.

`#!python append()` 和 `#!python extend()` 的区别，在参数是列表时最明显：

```python
>>> a = [1, 2]
>>> a.append([3, 4])
>>> a
[1, 2, [3, 4]]
>>> len(a)
3
>>> b = [1, 2]
>>> b.extend([3, 4])
>>> b
[1, 2, 3, 4]
>>> len(b)
4
>>> b.extend(5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    b.extend(5)
    ~~~~~~~~^^^
TypeError: 'int' object is not iterable
```

`#!python append()` 把参数**整个**当作一个元素放到末尾，所以 `a` 的第三个元素是列表 `#!python [3, 4]`，长度只增加 1. `#!python extend()` 把参数中的元素**逐个**取出来添加，所以它的参数必须是可迭代对象，`#!python b.extend(5)` 会报错.

`#!python append()` 最常见的用法，是从一个空列表开始，在循环中把数据一个个加进去. 下面的程序先读入个数 `n`，再读 `n` 行成绩，存进列表：

```python
n = int(input())
scores = []
for i in range(n):
    scores.append(int(input()))
print(scores)
print(max(scores) - min(scores))
```

依次输入：

```
4
90
85
77
68
```

输出为：

```
[90, 85, 77, 68]
22
```

数据存进列表之后，就可以反复使用：这里先后求了最大值和最小值. [流程控制](control-flow.md)中边读边处理的写法，每个数据读过就丢掉了，只能处理一遍.

### 原地修改的方法返回 `#!python None`

一种常见的错误写法，是把 `#!python append()` 的结果再赋值给列表：

```python
>>> a = [1, 2]
>>> a = a.append(3)
>>> a
>>> print(a)
None
>>> a.append(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a.append(4)
    ^^^^^^^^
AttributeError: 'NoneType' object has no attribute 'append'
```

`#!python a.append(3)` 确实把 3 加进了列表，但它的返回值是 `#!python None`. `#!python None` 是 Python 中表示“什么也没有”的特殊值，Shell 不显示它，所以前面调用 `#!python append()` 之后什么也没有显示. `#!python a = a.append(3)` 把名字 `a` 绑定到了这个 `#!python None` 上，原来的列表就找不到了；再调用 `#!python a.append(4)`，报错说 `#!python NoneType` 的对象没有 `#!python append` 这个属性.

这和字符串方法的习惯正好相反. 字符串不可变，`#!python s.upper()` 只能返回一个新字符串，要写 `#!python s = s.upper()` 才能留下结果. 列表可变，`#!python append()`、`#!python extend()`、`#!python insert()`、`#!python remove()`、`#!python clear()`、`#!python sort()`、`#!python reverse()` 都直接改列表本身，返回 `#!python None`. **调用这些方法时单独写一行，不要把结果赋值给任何名字.**

### 删除元素

| 方法                    | 作用                                                         |
| ----------------------- | ------------------------------------------------------------ |
| `#!python a.pop()`      | 删除**最后一个**元素，并返回它                               |
| `#!python a.pop(i)`     | 删除索引 `i` 处的元素，并返回它                              |
| `#!python a.remove(x)`  | 删除**第一个**值等于 `x` 的元素                              |
| `#!python a.clear()`    | 删除所有元素                                                 |

```python
>>> a = [2, 3, 5, 7, 11]
>>> a.pop()
11
>>> a.pop(2)
5
>>> a
[2, 3, 7]
>>> a.remove(3)
>>> a
[2, 7]
>>> a.remove(100)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a.remove(100)
    ~~~~~~~~^^^^^
ValueError: list.remove(x): x not in list
>>> a.clear()
>>> a
[]
>>> a.pop()
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    a.pop()
    ~~~~~^^
IndexError: pop from empty list
```

`#!python pop()` 按**位置**删除，`#!python remove()` 按**值**删除. `#!python pop()` 是原地修改的方法中的例外：它有返回值，就是被删掉的那个元素，可以接着使用. 要删的值不在列表中时，`#!python remove()` 报 `#!python ValueError`；列表已经空了，`#!python pop()` 报 `#!python IndexError`.

!!! note "`#!python del` 语句"

    按位置删除还可以用 `#!python del` 语句. 它不返回被删除的元素，但可以一次删除一段切片：

    ```python
    >>> a = [2, 3, 5, 7, 11]
    >>> del a[0]
    >>> a
    [3, 5, 7, 11]
    >>> del a[1:3]
    >>> a
    [3, 11]
    ```

`#!python remove()` 一次只删除一个元素. 要删除列表中所有的 5，容易想到在遍历时逐个删除：

```python
a = [2, 5, 5, 7, 5, 11]
for x in a:
    if x == 5:
        a.remove(x)
print(a)
```

运行后的输出是：

```
[2, 7, 5, 11]
```

还剩下一个 5. `#!python for` 循环是按位置依次取元素的：第一轮取索引 0，第二轮取索引 1，依此类推. 第二轮取到索引 1 处的 5 并把它删除，后面的元素都往前移了一格，原来在索引 2 的那个 5 移到了索引 1；而第三轮取的是索引 2，这个 5 就被跳过了. **不要在遍历一个列表的同时增删它的元素.**

正确的写法之一，是不遍历，只要列表中还有 5 就继续删：

```python
a = [2, 5, 5, 7, 5, 11]
while 5 in a:
    a.remove(5)
print(a)
```

运行后的输出是：

```
[2, 7, 11]
```

另一种写法是把要保留的元素放进一个新列表，我们将在后面“列表推导式”一节介绍.

### 排序与反转

`#!python a.sort()` 把列表中的元素从小到大排序，加上 `#!python reverse=True` 则从大到小；`#!python a.reverse()` 把元素的顺序颠倒过来. 两者都是原地修改，返回 `#!python None`：

```python
>>> a = [8, 2, 10, 5, 3, 11]
>>> a.sort()
>>> a
[2, 3, 5, 8, 10, 11]
>>> a.sort(reverse=True)
>>> a
[11, 10, 8, 5, 3, 2]
>>> a.reverse()
>>> a
[2, 3, 5, 8, 10, 11]
```

注意 `#!python reverse()` 只是颠倒顺序，并不排序.

排序有两种写法，区别在于改不改原列表：

|              | `#!python a.sort()`           | `#!python sorted(a)`               |
| ------------ | ----------------------------- | ---------------------------------- |
| 是什么       | 列表的方法                    | 内置函数                           |
| 适用于       | 只有列表                      | 任何可迭代对象                     |
| 原来的对象   | 被修改                        | 不变                               |
| 返回值       | `#!python None`               | 排好序的新列表                     |

```python
>>> b = [3, 1, 2]
>>> c = sorted(b)
>>> c
[1, 2, 3]
>>> b
[3, 1, 2]
>>> b = b.sort()
>>> print(b)
None
```

`#!python b = b.sort()` 和前面的 `#!python a = a.append(3)` 是同一种错误. 还要用到原来的顺序时用 `#!python sorted()`，不再需要时用 `#!python sort()`.

排序依靠的是元素之间的比较，所以元素的类型决定了排序的结果：

```python
>>> sorted(["10", "9", "100"])
['10', '100', '9']
>>> sorted([10, 9, 100])
[9, 10, 100]
>>> sorted(["banana", "Cherry", "apple"])
['Cherry', 'apple', 'banana']
>>> sorted([1, "2", 3])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    sorted([1, "2", 3])
    ~~~~~~^^^^^^^^^^^^^
TypeError: '<' not supported between instances of 'str' and 'int'
```

字符串按字符的编码逐个比较，所以 `#!python "10"` 排在 `#!python "9"` 前面，大写字母开头的 `#!python "Cherry"` 排在小写字母前面. **对 `#!python split()` 拆出来的数排序，要先转换成整数或浮点数.** 整数和字符串之间不能比较大小，混在一起排序会报 `#!python TypeError`.

排序还可以指定按什么标准来比较，见官方文档的[排序的技术](https://docs.python.org/zh-cn/3/howto/sorting.html#sortinghowto).

### 赋值不会复制列表

把一个列表赋值给另一个名字，再通过新名字修改：

```python
>>> a = [1, 2, 3]
>>> b = a
>>> b.append(4)
>>> a
[1, 2, 3, 4]
>>> a is b
True
```

我们只对 `b` 调用了 `#!python append()`，`a` 却也变了. 赋值语句做的事情是绑定名字：`#!python b = a` 没有创建新的列表，只是把标签 `b` 贴到了 `a` 所贴的那个列表上. 两张标签贴在同一个对象上，`#!python a is b` 为 `#!python True`，这正是我们在[流程控制](control-flow.md)一章中讲解身份运算 `#!python is` 时见过的情形；通过哪一张标签修改这个列表，从另一张标签看到的都是修改后的结果.

以前没有遇到这种情况，是因为整数、字符串都不可变. 两个名字贴在同一个字符串上，谁也改不了它；`#!python s = s.upper()` 只是把标签 `s` 改贴到一个新字符串上，另一个名字不受影响. 列表可变，“同一个对象上有几个名字”才开始有影响.

要得到一个内容相同、彼此独立的新列表，需要复制. `#!python a.copy()`、切片 `#!python a[:]` 和 `#!python list(a)` 都能做到：

```python
>>> a = [1, 2, 3]
>>> c = a.copy()
>>> c.append(4)
>>> c
[1, 2, 3, 4]
>>> a
[1, 2, 3]
>>> d = a[:]
>>> d == a
True
>>> d is a
False
```

`d` 和 `a` 的值相等，却是两个不同的对象，修改其中一个不会影响另一个.

!!! note "`#!python a += [3]` 和 `#!python a = a + [3]`"

    我们在[流程控制](control-flow.md)一章中说过，`#!python j += 3` 相当于 `#!python j = j + 3`. 对列表，两者有一点区别：

    ```python
    >>> a = [1, 2]
    >>> b = a
    >>> a += [3]
    >>> b
    [1, 2, 3]
    >>> a = a + [4]
    >>> a
    [1, 2, 3, 4]
    >>> b
    [1, 2, 3]
    ```

    `#!python a + [4]` 拼接出一个**新**列表，再把 `a` 改贴到新列表上，`b` 仍然贴在原来的列表上. 而根据[语言参考手册](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#augmented-assignment-statements)，增强赋值会尽可能原地修改对象：对列表，`#!python a += [3]` 相当于 `#!python a.extend([3])`，改的是原来的列表，所以 `b` 也看到了变化.

### 列表推导式

根据已有的数据生成一个新列表，按前面学过的写法需要三步：准备一个空列表，写一个循环，在循环体中 `#!python append()`. 比如生成 1 到 5 的平方：

```python
squares = []
for x in range(1, 6):
    squares.append(x ** 2)
print(squares)
```

运行后的输出是：

```
[1, 4, 9, 16, 25]
```

列表推导式（list comprehension）把这三步写成一个表达式：

```python
>>> [x ** 2 for x in range(1, 6)]
[1, 4, 9, 16, 25]
>>> [len(s) for s in ["apple", "kiwi", "banana"]]
[5, 4, 6]
>>> [int(s) for s in "10 20 5".split()]
[10, 20, 5]
```

推导式的形式是 `[表达式 for 变量 in 可迭代对象]`. 读的时候先看后半部分的 `#!python for`：它和普通的 `#!python for` 循环一样，每一轮取出一个元素绑定到变量上；再看最前面的表达式：每一轮对它求值一次，求出的值依次成为新列表的元素. 第三行把 `#!python split()` 拆出来的每一个字符串都转换成整数，是 PTA 上读入一行数据时很常用的写法.

在后面再加上 `#!python if 条件`，就只有条件为真的那些轮次才会产生元素，起到筛选的作用：

```python
>>> [n for n in range(1, 8) if n % 2 == 1]
[1, 3, 5, 7]
>>> words = ["Apple", "kiwi", "Banana", "fig", "Cherry"]
>>> [w.lower() for w in words if len(w) >= 5]
['apple', 'banana', 'cherry']
>>> a = [2, 5, 5, 7, 5, 11]
>>> [x for x in a if x != 5]
[2, 7, 11]
```

第一个推导式相当于下面的循环：

```python
odds = []
for n in range(1, 8):
    if n % 2 == 1:
        odds.append(n)
print(odds)
```

第二个推导式先筛选、再转换：只保留长度不小于 5 的单词，并把留下的单词转换成小写. 第三个推导式就是前面“删除所有的 5”的另一种写法：不去删除，而是把不等于 5 的元素留在一个新列表里，原列表 `a` 没有改变.

推导式中的 `#!python if` 可能出现在两个位置，含义不同：

```python
>>> [n if n % 2 == 1 else -n for n in range(1, 8)]
[1, -2, 3, -4, 5, -6, 7]
>>> [n for n in range(1, 8) if n % 2 == 1 else -n]
  File "<stdin>", line 1
    [n for n in range(1, 8) if n % 2 == 1 else -n]
                                          ^^^^
SyntaxError: invalid syntax
```

写在 `#!python for` **前面**的 `#!python n if n % 2 == 1 else -n` 是[流程控制](control-flow.md)中讲过的条件表达式，它是推导式开头那个“表达式”的一部分：每个 `n` 都会产生一个元素，奇数保持不变，偶数取相反数，所以结果仍有 7 个元素. 写在 `#!python for` **后面**的 `#!python if` 用来筛选，决定这一轮要不要产生元素，它不能带 `#!python else`，第二行因此报语法错误.

推导式适合“对每个元素做同一种转换，或者挑出一部分元素”这样的简单情形. 逻辑复杂到一行写不清楚时，还是写成普通的循环. 更多例子见 Python 教程的[列表推导式](https://docs.python.org/zh-cn/3/tutorial/datastructures.html#list-comprehensions)一节，以及本站的 [map 函数与列表推导式](../faq/map.md).

### 嵌套列表

列表的元素可以是任何对象，也可以是列表. 元素都是列表的列表，可以用来表示表格、矩阵这样有行有列的数据，每个内层列表是一行：

```python
>>> m = [[1, 2, 3], [4, 5, 6]]
>>> len(m)
2
>>> m[0]
[1, 2, 3]
>>> m[0][1]
2
>>> m[1][-1]
6
>>> len(m[0])
3
```

`m` 只有 2 个元素，每个元素是一个有 3 个整数的列表，所以 `#!python len(m)` 是行数，`#!python len(m[0])` 是列数. `#!python m[0][1]` 从左往右分两步求值：`#!python m[0]` 取出第一行 `#!python [1, 2, 3]`，再对这一行取 `#!python [1]`，得到 `#!python 2`. 一般地，`#!python m[i][j]` 是第 `i` 行、第 `j` 列的元素，行和列都从 0 开始数.

遍历嵌套列表，可以逐行处理，也可以用嵌套循环访问每一个元素：

```python
m = [[1, 2, 3], [4, 5, 6]]
for row in m:
    print(sum(row))
for i in range(len(m)):
    for j in range(len(m[i])):
        print(m[i][j], end=" ")
    print()
```

运行后的输出是：

```
6
15
1 2 3 
4 5 6 
```

前面用 `#!python [0] * 5` 得到过 5 个 0. 照这个办法准备一个 2 行 3 列、元素全为 0 的表格，却会出问题：

```python
>>> g = [[0] * 3] * 2
>>> g
[[0, 0, 0], [0, 0, 0]]
>>> g[0][0] = 1
>>> g
[[1, 0, 0], [1, 0, 0]]
>>> g[0] is g[1]
True
```

只改了第一行的第一个元素，第二行却跟着变了. `*` 重复的是列表中的元素，而这里的元素是**同一个**内层列表 `#!python [0, 0, 0]`：重复 2 次，得到的是指向同一个列表的两个位置，并没有复制出第二个列表. `#!python g[0] is g[1]` 为 `#!python True`，说明两行是同一个对象. 这和 `#!python b = a` 是同一个道理.

每一行都要是一个独立的列表，就得让 `#!python [0] * 3` 执行多次，每次创建一个新列表. 用列表推导式正合适：

```python
>>> g = [[0] * 3 for i in range(2)]
>>> g[0][0] = 1
>>> g
[[1, 0, 0], [0, 0, 0]]
```

`#!python [0] * 5` 没有这个问题：整数不可变，`#!python a[0] = 1` 是把这个位置换成另一个整数，不会影响其他位置.

嵌套列表的复制也有同样的问题. `#!python copy()` 和 `#!python a[:]` 只复制最外面一层，称为浅拷贝（shallow copy）：新列表是独立的，但其中的元素还是原来那些内层列表.

```python
>>> a = [[1, 2], [3, 4]]
>>> b = a.copy()
>>> b.append([5, 6])
>>> b[0].append(99)
>>> b
[[1, 2, 99], [3, 4], [5, 6]]
>>> a
[[1, 2, 99], [3, 4]]
>>> a[0] is b[0]
True
```

给 `b` 添加一行，`a` 不受影响，因为外层列表是两个不同的对象. 但 `#!python b[0]` 和 `#!python a[0]` 是同一个内层列表，通过 `b` 修改它，`a` 也看得到. 要把每一层都复制一份，用标准库 `copy` 模块中的 [`#!python deepcopy()`](https://docs.python.org/zh-cn/3/library/copy.html#copy.deepcopy)，称为深拷贝（deep copy）：

```python
>>> import copy
>>> a = [[1, 2], [3, 4]]
>>> c = copy.deepcopy(a)
>>> c[0].append(99)
>>> c
[[1, 2, 99], [3, 4]]
>>> a
[[1, 2], [3, 4]]
```

三种情况对比如下：

| 写法                          | 外层列表   | 内层列表   |
| ----------------------------- | ---------- | ---------- |
| `#!python b = a`              | 同一个     | 同一个     |
| `#!python b = a.copy()`、`#!python b = a[:]` | 新的 | 同一个 |
| `#!python b = copy.deepcopy(a)` | 新的     | 新的       |

元素都是整数、字符串这类不可变对象的列表，浅拷贝就够了.

列表的全部方法见官方文档的[列表](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#lists)和[可变序列类型](https://docs.python.org/zh-cn/3/builtins/stdtypes.html#mutable-sequence-types).

## 4. 元组

元组可以看作不可变的列表：它有序，元素可以是任何类型，第 2 节的操作全部适用；但创建之后不能添加、删除或替换元素.

### 创建元组

元组的字面量用圆括号括起来，元素之间用逗号隔开. 内置函数 `#!python tuple()` 把其他可迭代对象转换成元组：

```python
>>> point = (100, 200)
>>> point
(100, 200)
>>> rgb = 255, 128, 0
>>> rgb
(255, 128, 0)
>>> type(rgb)
<class 'tuple'>
>>> tuple("abc")
('a', 'b', 'c')
>>> tuple([1, 2, 3])
(1, 2, 3)
>>> ()
()
```

第二个例子没有写括号，得到的也是元组. **决定元组的是逗号，不是圆括号**：用逗号隔开的几个值就构成一个元组，括号只是为了看得清楚，Shell 显示元组时总是带着括号. [流程控制](control-flow.md)中交换两个值的 `#!python a, b = b, a`，右边的 `#!python b, a` 就是这样一个元组. 唯一的例外是空元组，写成一对空的圆括号 `#!python ()`.

只有一个元素的元组因此需要特别注意：

```python
>>> single = (42,)
>>> single
(42,)
>>> type(single)
<class 'tuple'>
>>> len(single)
1
>>> type((42))
<class 'int'>
>>> 42,
(42,)
```

`#!python (42)` 中的圆括号是算术里改变运算顺序的那种括号，和 `#!python (1 + 2) * 3` 中的一样，所以 `#!python (42)` 就是整数 `#!python 42`. 只有一个元素的元组，**元素后面必须跟一个逗号**，写成 `#!python (42,)`.

### 元组不可变

```python
>>> t = (1, 2, 3)
>>> t[0]
1
>>> t[1:]
(2, 3)
>>> t[0] = 9
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    t[0] = 9
    ~^^^
TypeError: 'tuple' object does not support item assignment
>>> t.append(4)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    t.append(4)
    ^^^^^^^^
AttributeError: 'tuple' object has no attribute 'append'. Did you mean to use a 'list' object?
>>> t + (4,)
(1, 2, 3, 4)
>>> t
(1, 2, 3)
```

索引和切片照常可用，但给某个位置赋值会报 `#!python TypeError`，和字符串一样. 元组也没有 `#!python append()`、`#!python remove()`、`#!python sort()` 这些修改内容的方法，它的方法只有第 2 节的 `#!python count()` 和 `#!python index()` 两个. 报错信息最后还提示：是不是想用列表？

和字符串一样，要得到不同的内容，只能创建一个**新**元组：`#!python t + (4,)` 拼接出一个新元组，`t` 本身没有变. 要对元组排序，用 `#!python sorted(t)`，得到的是一个列表.

“不可变”指的是元组的每个位置指向哪个对象不能改变. 如果某个位置上的对象本身是可变的，比如一个列表，这个列表的内容仍然可以修改：

```python
>>> t = ([255, 0, 0], [0, 255, 255])
>>> t[0][0] = 0
>>> t
([0, 0, 0], [0, 255, 255])
>>> t[0] = [1, 1, 1]
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    t[0] = [1, 1, 1]
    ~^^^
TypeError: 'tuple' object does not support item assignment
```

`#!python t[0][0] = 0` 改的是 `#!python t[0]` 这个列表的内容，元组的第一个位置仍然指着同一个列表，所以可以. `#!python t[0] = [1, 1, 1]` 要让第一个位置指向另一个列表，这才是修改元组，所以报错.

### 元组的用途

**把几个相关的值组成一个整体.** 平面上的一个点 `#!python (3, 4)`，一种颜色 `#!python (255, 128, 0)`，一个日期 `#!python (2026, 11, 7)`，都是固定的几个值，每个位置有固定的含义，不会增加也不会减少，适合用元组表示. 若干个这样的整体，可以再放进一个列表：

```python
>>> points = [(1, 2), (3, 4), (5, 6)]
>>> points[1]
(3, 4)
>>> points[1][0]
3
>>> for x, y in points:
...     print(x + y)
...
3
7
11
```

`#!python for x, y in points` 每一轮取出一个元组，并把它解包给 `x` 和 `y`. 前面的 `#!python for i, s in enumerate(scores)` 也是这样：`#!python enumerate()` 产生的每一对就是一个元组.

**一次得到多个结果.** 内置函数 [`#!python divmod()`](https://docs.python.org/zh-cn/3/builtins/functions.html#divmod) 做一次除法，同时给出商和余数，看起来像是返回了两个值：

```python
>>> result = divmod(17, 5)
>>> result
(3, 2)
>>> type(result)
<class 'tuple'>
>>> result[0]
3
>>> q, r = divmod(17, 5)
>>> q
3
>>> r
2
>>> q, r, x = divmod(17, 5)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    q, r, x = divmod(17, 5)
    ^^^^^^^
ValueError: not enough values to unpack (expected 3, got 2)
```

一次函数调用求值得到的只有**一个**对象. `#!python divmod(17, 5)` 返回的是一个元组 `#!python (3, 2)`，商和余数是它的两个元素，可以绑定到一个名字 `result` 上，再用索引取出. 所谓“返回多个值”，是两件事合在一起：函数把几个结果装进一个元组返回；调用的一方用[流程控制](control-flow.md)中的解包，把元组中的元素依次分给几个名字. `#!python q, r = divmod(17, 5)` 和 `#!python a, b = "34"` 是同一种赋值，所以左边名字的个数必须和元组的元素个数相同，最后一行多写了一个名字，报的正是解包时见过的 `#!python ValueError`.

`#!python a, b = b, a` 也是同样的两步：右边的 `#!python b, a` 先打包成一个元组，再解包给左边的两个名字. 后面学到自己定义函数时，让函数返回几个结果，用的还是这个办法.

**保护数据不被修改.** 一批数据确定之后不应该再变，比如一周七天的名称，用元组保存，程序中任何试图修改它的代码都会报错，而不是悄悄改掉数据. 另外，字典的键和集合的元素要求是不可变的对象，元组可以，列表不行，我们将在讲解字典和集合时用到这一点.

列表还是元组，看数据是否需要修改：个数会变、元素要增删或排序的，用列表；固定的几个值组成一个整体、不应被修改的，用元组. 元组的更多说明见 Python 教程的[元组和序列](https://docs.python.org/zh-cn/3/tutorial/datastructures.html#tuples-and-sequences)一节.

## 5. 在 PTA 上用列表处理数据 { #pta }

### 读入一批数据

**一行中有若干个数.** `#!python input().split()` 得到的是字符串的列表，直接计算会出错；用列表推导式把每个元素转换成整数，或者用 `#!python map()` 再转换成列表：

```python
>>> nums = "3 1 2".split()
>>> nums
['3', '1', '2']
>>> sum(nums)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    sum(nums)
    ~~~^^^^^^
TypeError: unsupported operand type(s) for +: 'int' and 'str'
>>> nums = [int(x) for x in "3 1 2".split()]
>>> nums
[3, 1, 2]
>>> sum(nums)
6
>>> list(map(int, "3 1 2".split()))
[3, 1, 2]
```

在程序中，把 `#!python "3 1 2"` 换成 `#!python input()`，写成 `#!python nums = [int(x) for x in input().split()]`. 这种写法不需要事先知道一行中有几个数.

[输入、输出与字符串](io-string.md)中的 `#!python a, b, c = map(float, input().split())` 没有写 `#!python list()`，是因为解包只要求右边能逐个取出元素. `#!python map()` 的结果可以逐个取出元素，但它本身不是列表，不能用索引，也不能求 `#!python len()`；要把这些数留下来反复使用，就得像上面那样用 `#!python list()` 转换，或者改用列表推导式. 详见 [map 函数与列表推导式](../faq/map.md).

**每行一个数.** 先读个数 `n`，再循环 `n` 次，每次把读到的数 `#!python append()` 到列表中，见前面“添加元素”一节的例子.

### 输出一个列表

直接 `#!python print()` 一个列表，输出的是带方括号和逗号的样子，题目要求的通常是用空格隔开的一行：

```python
>>> a = [3, 1, 2]
>>> print(a)
[3, 1, 2]
>>> " ".join(a)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
    " ".join(a)
    ~~~~~~~~^^^
TypeError: sequence item 0: expected str instance, int found
>>> " ".join([str(x) for x in a])
'3 1 2'
>>> print(*a)
3 1 2
>>> print(*a, sep=", ")
3, 1, 2
```

我们在[输入、输出与字符串](io-string.md)一章中讲过，`#!python join()` 要求列表中的元素都是字符串. `a` 中是整数，所以报错说第 0 个元素应该是 `#!python str`，实际却是 `#!python int`；先用推导式把每个元素转换成字符串，再 `#!python join()` 就可以了. 也可以在列表前面加一个星号，写成 `#!python print(*a)`：星号把列表中的元素拆开，作为一个个单独的参数传给 `#!python print()`，相当于 `#!python print(3, 1, 2)`，元素之间的分隔符仍然由 `sep` 决定.

这两种写法输出的行末都没有多余的空格. 我们在[流程控制](control-flow.md#pta)一章中用循环加 `#!python end=" "` 输出时，最后一个值后面会多出一个空格，需要专门处理；有了列表，把要输出的值先收集起来，最后一次输出，就没有这个问题.

### 一个完整的例子

第一行给出人数 `n`，第二行给出 `n` 个整数成绩，用空格隔开. 要求第一行输出平均分，保留两位小数；第二行输出高于平均分的人数；第三行从高到低输出这些高于平均分的成绩，用空格隔开.

平均分要等所有成绩都读完才能算出来，算出来之后还要回头把每个成绩和它比较，所以成绩必须先存起来：

```python
n = int(input())
scores = [int(x) for x in input().split()]
avg = sum(scores) / n
high = sorted([s for s in scores if s > avg], reverse=True)
print(f"{avg:.2f}")
print(len(high))
print(" ".join([str(s) for s in high]))
```

依次输入：

```
6
88 76 95 60 83 91
```

输出为：

```
82.17
4
95 91 88 83
```

第 2 行读入并转换，得到整数的列表. 第 4 行先用带 `#!python if` 的推导式挑出高于平均分的成绩，再交给 `#!python sorted()` 从大到小排序. 最后一行把整数转换成字符串，用空格连接起来输出.

写完之后，想一想边界情况. 所有人成绩相同时，没有人高于平均分，`high` 是空列表：

```
3
80 80 80
```

输出为：

```
80.00
0

```

空列表 `#!python join()` 的结果是空字符串，第三行输出了一个空行，程序没有报错. 如果这里要对 `high` 求 `#!python max()`，就会遇到前面说的 `#!python ValueError`. 至于没有符合条件的数据时究竟应该输出什么，要看题目的要求.
