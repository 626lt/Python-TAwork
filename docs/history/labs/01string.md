# 实验 1：字符串转写指令

在这个实验中，我们会用到一系列课堂上学过的内容来完成一个任务。给定一个字符串 `input_str`，我们需要根据一个给定的指令来完成一个操作.

## 实验准备

- 按照第一个实验的要求完成 Python 环境的安装和配置；
- 了解关于字符串方法与字符串切片相关的知识.

## 任务要求

打开 `problem.py` 文件，你会看到这样的代码：

```python
def single_line_process(input_str, line):
    '''
    @param input_str: str   输入字符串
    @param line: str        简单语句
    @return: str            返回对 input_str 进行 line 指定的操作后的结果
    '''
    ##############################################################
    # 在这里实现你的代码
    ##############################################################

    return input_str # 将返回的东西替换为你的计算得到的值
```

这被称作一个函数. 在这里，我们不要求你了解关于函数的知识，只要在里面填写代码即可. 注意，所有你所填写的代码（称作函数体）的缩进应当在 `#!python def` 语句下面一级.

我们要求你书写的代码能够对 `A = B;` 这样的简单语句（即 `line`）中的内容完成处理. 我们保证这里的输入最后为一个分号，语句中间有且仅有一个 `=`，所有空格忽略不计. 这样的简单语句进行的操作就是如果在 `input_str` 这个字符串中出现了等号左边的字符串，就将它的**首次出现**替换为等号右边的字符串，并且在 `#!python return` 关键字后面填入完成计算之后的字符串.

## 计算示例

你可以直接执行 `problem.py` 来测试你的代码，测试数据在函数下方的语句块中进行设置. 下面是几个典型例子和边界条件：

最简单的情形：

```python
single_line_process("ABC", "A = B;") == "BBC"
single_line_process("AAC", "A = B;") == "BAC"
single_line_process("ABACAB", "AB = CDE;") == "CDEACAB"
```

而空字符串的情形比较特殊：

```python
single_line_process("ABC", "= B;") == "BABC"
single_line_process("ABC", "A = ;") == "BC"
```

当出现在等号左边时，我们默认每个字符串最开头就是应当被替换的空字符串；当出现在等号右边时，替换为空字符串相当于直接删除这个字符.

## 测试方法

直接执行 `test.py` 来完成测试. 如果你的代码正确，则会出现如下成功提示：

```
........
----------------------------------------------------------------------
Ran 8 tests in 0.001s

OK
.
----------------------------------------------------------------------
Ran 1 test in 0.004s

OK
```

通过测试后，请将 lab1 目录压缩为一个 ZIP 文件，并提交到学在浙大。

## 选做的拓展知识之一：代码混淆

在这个实验中，我们提供了 `baseline.py`. 它是通过代码混淆（code obfuscation）进行保护的. 所谓的代码混淆指的是将一段好理解的代码转换成更难理解的代码，这种转换的目的往往是保护知识产权——或者说保持代码的垄断地位，在实际的软件开发中时有应用. 我们使用的框架是 [obfupy](https://github.com/wqking/obfupy). 我们建议你借助人工智能理解它的文档，完成以下几个问题：

1. 使用 Pip 安装 obfupy，可以参考[环境配置指南中的对应章节](00env.md/#pip)；
2. 解释它进行混淆的四个步骤（rewrite, format, replace, codec），粗略解释它的执行过程；
3. 执行它的示例，并且对自己写的小程序进行混淆，观察结果；
4. 修改它的示例代码中的配置，并且进行测试，尝试理解一些选项的意义；
5. 阅读其 [Rewriter 部分的代码文件](https://github.com/wqking/obfupy/tree/master/obfupy/transformers/internal/rewriter)中的任意一个模块，并解释其执行过程.

## 选做的拓展知识之二：模块化测试

注意到，我们使用了 [unitest 模块](https://docs.python.org/3/library/unittest.html)来完成模块测试. 按照以下步骤，借助人工智能探索它以及其它测试方式：

1. 阅读 unitest 模块的文档，尝试自己在测试模块（`test.py`）中增加一些测试样例；
2. 探索样例生成：我们使用 [random 模块](https://docs.python.org/3/library/random.html)生成了一些测试样例，解释测试过程并且进行一些修改和尝试；
3. 尝试将部分测试改写成使用 [doctest 模块](https://docs.python.org/3/library/doctest.html)完成.

你可以尝试回答下面两个问题：

1. 思考 unitest 模块和 doctest 模块的区别，并尝试给出二者不同的使用场景；
2. 单元测试的意义是什么？如何设置测试样例能够保证自己的测试足够充分？给出一些实际的例子.

## 参考资料

- 建议阅读：[关于字符串方法的官方文档](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)
- [obfupy 的官方仓库](https://github.com/wqking/obfupy)
- [unitest 模块的官方文档](https://docs.python.org/3/library/unittest.html)
- [random 模块的官方文档](https://docs.python.org/3/library/random.html)
- [doctest 模块的官方文档](https://docs.python.org/3/library/doctest.html)

## 拓展阅读

- [Montford 关于代码混淆的介绍短文](appendum/Montford_2008_Obfuscated_Code.pdf)
- [Boaz Barak 关于代码混淆中的不可能性的短文：Can We Obfuscate Programs?](https://www.boazbarak.org/papers/obf_informal)
- [Boaz Barak et al. 对应的长篇论文](https://www.boazbarak.org/Papers/obfuscate.pdf)
- [Can LLMs Obfuscate Code? A Systematic Analysis of Large Language Models into Assembly Code Obfuscation](https://arxiv.org/abs/2412.16135)
