# 标准库

!!! note "关于标准库"

    [标准库](https://docs.python.org/zh-cn/3/library/index.html)指的是 Python 自带的一些可用的函数库. 读者可以自行阅读文档中关于它的叙述. 在这里，我们会首先介绍文档书写和阅读的规范，然后再介绍一些重要的模块. 在计算机相关的学习中，文档阅读能力往往是相当重要的，标准库文档也是大量可用的 Python 库文档的参照，对其进行阅读，并了解其中使用的术语和惯例将会有助于读者更进一步的学习. 另外，鉴于其它的文档可能没有中译，在分析过程中，我们将会引用英文的文档，再进行解释，读者也可自行打开中文文档对照阅读.

    一般地，在标准库文档中的术语的定义如未说明，都会出现在[语言参考手册](https://docs.python.org/zh-cn/3/reference/index.html)中，但是这种文档的叙述往往会更加技术性，也会有更多（或者说，过多）掉书袋式的专业内容. 我们将会在[补充材料](../history/interesting-addendum/lang_ref.md)中讨论它的解读方式，供有兴趣的读者参考.

## 标准库的阅读方式

让我们以 [`#!python str` 类型的文档](https://docs.python.org/3/library/stdtypes.html#text-sequence-type-str)为例. 我们已经在[之前的学习](complex_types.md)中了解了它的基本概念，接下来，我们将会一起阅读理解标准库中对其的一些叙述，借此来理解标准库文档中的术语和书写惯例.

在标准库的介绍当中，标题之下首先出现了一些引言：

??? info "引言与概述性信息 - 点击展开"

    Textual data in Python is handled with [`#!python str`](https://docs.python.org/3/library/stdtypes.html#str) objects, or *strings*. Strings are immutable [sequences](https://docs.python.org/3/library/stdtypes.html#typesseq) of Unicode code points. String literals are written in a variety of ways:

    - Single quotes: `#!python 'allows embedded "double" quotes'`
    - Double quotes: `#!python "allows embedded 'single' quotes"`
    - Triple quoted: `#!python '''Three single quotes''', """Three double quotes"""`

    Triple quoted strings may span multiple lines - all associated whitespace will be included in the string literal.

    String literals that are part of a single expression and have only whitespace between them will be implicitly converted to a single string literal. That is, `#!python ("spam " "eggs") == "spam eggs"`.

    See [String and Bytes literals](https://docs.python.org/3/reference/lexical_analysis.html#strings) for more about the various forms of string literal, including supported escape sequences, and the `r` (“raw”) prefix that disables most escape sequence processing.

    Strings may also be created from other objects using the [`str`](https://docs.python.org/3/library/stdtypes.html#str) constructor.

    Since there is no separate “character” type, indexing a string produces strings of length 1. That is, for a non-empty string *s*, `#!python s[0] == s[0:1]`.

    There is also no mutable string type, but [`str.join()`](https://docs.python.org/3/library/stdtypes.html#str.join) or [`io.StringIO`](https://docs.python.org/3/library/io.html#io.StringIO) can be used to efficiently construct strings from multiple fragments.

    > *Changed in version 3.3*: For backwards compatibility with the Python 2 series, the `u` prefix is once again permitted on string literals. It has no effect on the meaning of string literals and cannot be combined with the `r` prefix.

在阅读这些内容之前，需要补充一些背景知识. 首先，在书写标准库文档时，作者通常字斟句酌. 也就是说，每一句叙述或者是功能性的描述，或者是设计者设计时思路的体现. 对于一些不太细致的文档而言，这一点不一定成立，但是 Python 标准库文档并非如此. 其次，标准库文档的目标是让编程人员快速入门这种类型/模块的使用方式，所以对于熟练的编程人员（希望读者现在已经配得上这个称呼），在阅读标准库的时候，应当配合编程实践完成——你可以不断地去尝试其中描述的内容，来对这些内容获得更具体的理解.

接下来，让我们回到文档. 前面一部分开门见山，介绍了字符串类型的语法（syntax，即它“长什么样”）和语义（semantics，即它“表达什么含义”）. 第一句话就告诉你，字符串表示的是文本型数据（textual data），即它存储的是文本；第二句话告诉你，字符串是**不可变的**（immutable）的 Unicode 码点（code point）组成的序列（sequence）；第三句话及后续的列表、以及紧接着的一段告诉你字符串的语法长什么样，以及它的各种写法有什么作用.

!!! note "Unicode 编码和码点"

    Unicode 编码是一个非常复杂的编码模式. 关于它的最详尽的描述可见于其[标准](https://www.unicode.org/versions/Unicode16.0.0/UnicodeStandard-16.0.pdf)的叙述. 同样，这也相当复杂，关于码点的省流介绍如下（引自 2.4 节开头的位置）：

    > On a computer, abstract characters are encoded internally as numbers. To create a complete character encoding, it is necessary to define the list of all characters to be encoded and to establish systematic rules for how the numbers represent the characters.
    >
    > The range of integers used to code the abstract characters is called the codespace. A particular integer in this set is called a code point. When an abstract character is mapped or assigned to a particular code point in the codespace, it is then referred to as an encoded character.

    简而言之，码点指的是用来编码字符的一个整数. 在通常的用法中，我们会称其为一个“字符”，但这样的叙述是不精确的，因此标准库的作者使用了 Unicode 标准的术语，将其称作码点.

在这一小段话中，包含了非常大的信息量. 首先，第一句话实际上是对 *string* 这个术语的定义. 注意文档中的斜体标识，往往这意味着强调或者术语，也用于单独出现的变量名. 这里说 `#!python str` objects or strings 的意思就是我们用字符串 strings 来称呼一个 `#!python str` 类型的对象. 第二句话的关键信息之一是不可变的（immutable）这个词，它告诉了你这个类型的属性；序列（sequence）这个词初看不起眼，但是它作为一个链接的身份暴露了它：这意味着 `#!python str` 类型具备所有序列应该具备的操作，点击链接，你会看到一个巨大的表格，其中覆盖了所有可以在字符串上（以及其它序列类型上）执行的操作和运算符. 同样，在下面介绍标准库时，所有说到用**序列**作为参数的地方也都可以传入字符串，当然，要求可变序列时不行. 第三句话和下面的内容中，则介绍了它的构造方法以及其它属性. 注意，其中的例子往往也说明了一些东西，其例子用代码块包裹，意思是这是有意义的代码，其中介绍说，单引号中字符串可以包含双引号，双引号字符串中可以包含单引号，这也是一个可以关注的点. 所谓的字面量（literal）可以被粗略地理解成所见即所得的东西：它会被直接拿进去当成一个这一类型的对象处理.

在列表下边有一段简短的叙述，关于三引号字符串. 注意其中的空白符（whitespace）这个词. 作为一个术语，空白符不止意味着空格，还包含制表符（tab），回车（enter）等等格式性的字符. 这意味着我们可以写出下边的代码：

```python
print("""*   *
*   *
*****
*   *
*   *""")
```

并且期望它输出：

```
*   *
*   *
*****
*   *
*   *
```

这在有些时候是一个非常有用的小技巧. 注意，这里的连接符意味着后边的话是对前面的话的解释说明：如果一个三引号字符串跨越了很多行，它意味着什么呢？意味着其中的空白符（包括回车）也会被直接算入到字符串当中去.

接下来阅读下面的信息. 它指出，那些**作为单一表达式的部分**（part of a single expression）且其中只有空白符间隔的字符串字面量会被当成同一个字面量处理，并且给出了例子. 注意我们加粗的这一部分，它强调的是作为单一表达式的部分，作为一个反例，请看下面的代码：

```python
p = "hello"
"world"
print(p)
```

简单的尝试会告诉你，它的输出是 `hello` 而不是 `helloworld`，这貌似和之前关于空白符包含回车的说明矛盾. 但是，单一表达式这个条件提醒你，在换行之后，这就是两个表达式：一个赋值表达式、一个什么都不干的单个字面量表达式. 如果要深入了解其细节，需要参照其语言参考对于表达式的定义，但粗略地理解就是，这是两个表达式，不符合“作为单一表达式的部分”的条件.

接下来，它还告诉你，可以参照另一部分文档理解更多字符串字面量的形式. 当然，这一部分文档来自语言参考中的词法分析（lexical analysis）章节，其中使用 BNF（Backus-Naur Form）来表述了字符串字面量的语法细节. 这里需要涉及更多的技术性细节，在此暂且略过，我们会在补充文档中介绍一点相关的内容. 当然，其中关于转义序列（escape sequence）的叙述告诉你你也可以在那里找到所有支持的转义序列，并且告诉你可以用 `r` 前缀来取消**大部分**（most）转义序列的处理，如果你（作为一个更专业的技术人员）阅读了这段叙述和对应的 BNF 公式，你就会知道我们可以这样写：

```python
print(r"hello\nworld")
```

它会直接输出 `hello\nworld`. 而如果没有 `r` 前缀，它就会在中间将 `\n` 转义成换行. 这里的“大部分”这个词也会迫使你去阅读语言参考手册，其中有更详尽的叙述，但和我们此处讨论的内容无关.

接下来，它告诉你如何去创建（create）一个字符串，它使用 `#!python str` 构造函数（constructor）来完成. 点击这个 `#!python str` 的链接将会跳转到类型构造函数，我们在后面也会分析它的文档. 下面一段告诉你，取下标也会直接产生一个字符串，其原因是没有单个字符（character）类型. 它提出的是它和序列（sequence）在取下标时语义的细小差异（例如，对列表取下标，取出的是其中的元素而不是单元素的列表，与切片不同）及其原因. 最后一句正文，它告诉你如何从多个字符串片段构造一个完整的字符串，并且指出没有可变的（mutable）字符串类型，点击涉及的函数也会让你能够跳转到对应的文档.

最后是一段非正文性的部分. 其中标出了 *Changes in version 3.3*，即在 Python 3.3 版本中更新的部分，其目标是为了和 Python 2 系列向后兼容（backwards compatibility），并且陈述了 `u` 前缀的语义，这对于现代的程序员来说用处不大，仅仅陈述了版本兼容性的问题. 往往，我们在使用一些更古老的环境（例如某些地方陈旧的生产环境或者维护老项目）时需要注意这些叙述，确保自己使用的功能在这个版本已经被支持或者兼容.

接下来我们进入到其关于构造函数的叙述当中：

??? info "构造函数的介绍 - 点击展开"

    `#!python class str(object='')`<br>
    `#!python class str(object=b'', encoding='utf-8', errors='strict')`

    > Return a string version of object. If object is not provided, returns the empty string. Otherwise, the behavior of `#!python str()` depends on whether *encoding* or *errors* is given, as follows.
    >
    > If neither *encoding* nor *errors* is given, `#!python str(object)` returns [`#!python type(object).__str__(object)`](https://docs.python.org/3/reference/datamodel.html#object.__str__), which is the “informal” or nicely printable string representation of *object*. For string objects, this is the string itself. If object does not have a [`#!python __str__()`](https://docs.python.org/3/reference/datamodel.html#object.__str__) method, then [`#!python str()`](https://docs.python.org/3/library/stdtypes.html#str) falls back to returning [`repr(object)`](https://docs.python.org/3/library/functions.html#repr).
    >
    > If at least one of *encoding* or *errors* is given, *object* should be a [bytes-like object](https://docs.python.org/3/glossary.html#term-bytes-like-object) (e.g. [`#!python bytes`](https://docs.python.org/3/library/stdtypes.html#bytes) or [`#!python bytearray`](https://docs.python.org/3/library/stdtypes.html#bytearray)). In this case, if object is a [`#!python bytes`](https://docs.python.org/3/library/stdtypes.html#bytes) (or [`#!python bytearray`](https://docs.python.org/3/library/stdtypes.html#bytearray)) object, then `#!python str(bytes, encoding, errors)` is equivalent to [`#!python bytes.decode(encoding, errors)`](https://docs.python.org/3/library/stdtypes.html#bytes.decode). Otherwise, the bytes object underlying the buffer object is obtained before calling [`#!python bytes.decode()`](https://docs.python.org/3/library/stdtypes.html#bytes.decode). See [Binary Sequence Types — bytes, bytearray, memoryview](https://docs.python.org/3/library/stdtypes.html#binaryseq) and [Buffer Protocol](https://docs.python.org/3/c-api/buffer.html#bufferobjects) for information on buffer objects.
    >
    > Passing a [`#!python bytes`](https://docs.python.org/3/library/stdtypes.html#bytes) object to [`#!python str()`](https://docs.python.org/3/library/stdtypes.html#str) without the *encoding* or *errors* arguments falls under the first case of returning the informal string representation (see also the [`-b`](https://docs.python.org/3/using/cmdline.html#cmdoption-b) command-line option to Python). For example:
    >
    > ```python
    > >>> str(b'Zoot!')
    > "b'Zoot!'"
    > ```
    >
    > For more information on the `#!python str` class and its methods, see [Text Sequence Type — str](https://docs.python.org/3/library/stdtypes.html#textseq) and the [String Methods](https://docs.python.org/3/library/stdtypes.html#string-methods) section below. To output formatted strings, see the [f-strings](https://docs.python.org/3/reference/lexical_analysis.html#f-strings) and [Format String Syntax](https://docs.python.org/3/library/string.html#formatstrings) sections. In addition, see the [Text Processing Services](https://docs.python.org/3/library/text.html#stringservices) section.

!!! warning "未完成"

## `#!python random` 库和随机数生成

对于伪随机数生成器（pseudo-random number generator, PRNG）库的功能的详尽，参见[官方文档](https://docs.python.org/3/library/random.html). 在此，我们将首先介绍伪随机数生成的一些前置知识，以便读者在阅读官方文档时能够更加轻松，然后展示几个常用的函数的用法，作为官方文档的一个缩略的参考.

首先，读者可能会考虑的第一个问题是，为什么叫“伪随机数”而不是“随机数”？这是一个非常好的问题，也直指核心. 为了回答这个问题，我们首先需要考虑我们如何看待“随机”. 通常地，我们把抛硬币时硬币正面朝上还是反面朝上称作一个随机事件. 但是，如果知道了抛出硬币的一切数据，我们也可以通过物理法则来预测它落地时是正面朝上的还是反面朝上的. 因此，我们理解随机事件实际上出于一种统计意义上：在无数次抛硬币的过程当中，我们得到的正反面朝上的次数服从某个统计分布. 而对于计算机来说，通常它就出于一种“知道一切”的窘境当中：我们知道内存中每个位置都放着一个确定的东西，也知道在 CPU 中执行一条指令会产生什么. 因此，并非出于统计意义的随机数生成，即真正的，因为未知全貌而产生的“真随机”是不可能达到的目标. 所以我们退而求其次，要求它产生的数据服从某个统计规律，虽然它实际上可以预测. 这种随机数就被称作伪随机数（pseudo-random number）.

理解了伪随机数的概念，读者也就不难理解标准库文档给出的安全性说明. 实际上，`#!python random` 库应用的是梅森旋转素数（Mersenne Twister）算法来实现的伪随机数生成，它并不是密码学强（cryptographically strong）的，因此将其用作一些密码学用途是有安全隐患的. 读者可以搜索一系列关于 PRNG 攻击的资料来理解并佐证这一点.

!!! notes "安全随机数和系统的熵池（entropy pool）"

    为了生成密码学强的随机数，实际上我们需要一些专有的硬件资源. 在 [`#!python secrets` 库](https://docs.python.org/3/library/secrets.html#module-secrets)当中，Python 提供了一些对应的函数. 粗略地讲，这个硬件资源当中“充满噪声”，其中存储了一堆不知为何物的信息，在需要随机数的时候，系统会从中取一点信息. 并且，系统可以通过充放电重置里边的信息，以保证随机数是“足够随机的”. 这样的硬件资源就被称作熵池. 当然，在长期使用中，会出现熵池混乱度下降的情况，也有可能出现可预测的随机数，近年来也有一系列关于此种攻击的研究.

基于此，我们就知道了 `#!python random` 库的一些功能的意义了. 其中一部分的函数是用来设置伪随机数生成器的状态的，另一部分函数是用来使得伪随机数生成器可以生成一个随机数的. 前一部分被官方文档称作簿记函数（book-keeping function），注意，这里的 book 更好的理解方式是账本（作为一个英语笑话，有兴趣的读者可以自己查一下 cook the book 的意思，并非焚书），它相当于是记下了随机数生成器每次生成的过程.

最常见的簿记函数是 `#!python random.seed(a=None, version=2)`. 我们已经说过，在阅读标准库的时候，里边的函数写法就是函数定义的写法. 也就是说，这个函数在定义时就是有两个可选参数的. 其中 *version* 参数并不重要，仅仅是版本号，而参数 *a* 则是设置的随机数种子，也就是说，我们从哪个地方开始生成随机数，它会决定一系列随机数的生成. 如果它的值是默认值 `None`，那么它会以系统时间，或者在有随机源（即熵池）的情况下，以随机源生成的随机数为种子. 从相同的种子出发，用相同的方法总能生成一系列相同的随机数.

此外，簿记函数还有 `#!python random.getstate()` 和 `#!python random.setstate(state)`，不太常用，有兴趣的读者可以参考标准库的叙述.

生成随机数的过程通常有两类，一类是生成浮点数，一类是生成整数. 前者的代表性方法是 `random.random()`，它会生成一个 $[0.0, 1.0)$ 内的**均匀分布的**浮点数. 而进一步的生成 `random.uniform(a, b)` 则相当于生成一个 `a + (b - a) * random()` 的随机数. 对概率论比较熟悉的读者应当知道，它就是生成一个在 $[a, b)$ 之间的均匀分布的浮点数. 当然，在计算机中，因为进位的误差，它有可能取到 *b* 的值.

另外，还有一些在不同的分布下（例如正态分布、指数分布等等）生成浮点数的方式，可以参照标准库的叙述.

对于整数的生成，最具代表性的方法是

`random.randrange(stop)`<br>`random.randrange(start, stop[, step])`

还记得我们已经说过，在文档中方括号的含义是“可选”. 因此，这就意味着它有下面三种形式：

- `random.randrange(stop)`
- `random.randrange(start, stop)`
- `random.randrange(start, stop, step)`

其中 *start*，*stop* 和 *step* 三个参数的值就相当于 `#!python range` 类的三个参数. 它会从这个范围内机会均等地取出一个整数（依然是左闭右开）. 作为一个简写，我们也有 `random.randint(a, b)`，它相当于 `random.randrange(a, b + 1)`.

!!! 整数抽取的均等性问题

    一个比较有意思的点是，虽然看起来整数抽取相当简单，但是要保证均等性并不容易. 一个很直接的想法是，直接使用 `int(random() * n)`，借助浮点数的抽取来完成. 但是，这实际上会引入一种被称作模数偏差（modulo bias）的东西. 如果要做到真正意义上均等的抽取，那么必须引入更加精巧的算法.

与随机生成整数类似，我们也可以随机采用序列中的一个元素. `random.choice(seq)` 会机会均等地返回**非空**序列 *seq* 当中的一个元素. 如果想要为序列中的元素赋予不同的权重，可以使用 `random.choices` 函数，更详细的介绍参见标准库文档.

我们也可以随机打乱一个序列. `random.shuffle(x)` 会使得**可变**序列 `x` 被修改成一个随机打乱的序列. 这实际上通过康托展开（Cantor expansion）来完成，在 `#!python itertools` 库中我们会对其进行更详尽的介绍. 较为重要的一点是，它只能正确处理比较短的序列. 梅森旋转素数的处理极限是长为 2080 的序列.

!!! warning "处理极限"

    注意，在面对超出处理极限的序列时，`random.shuffle` 并不会报错，而是将不能保证各种打乱方式之间的机会均等性，有一部分打乱方式将永远不可能被产生. 因此，在实际工程中使用这类函数时，需要额外注意.

    为了处理这种问题，人们实际上做了非常多的尝试. 这一问题事实上是因为随机数生成器能生成的随机数范围还是太小. 有一个经典的问题被称为蓄水池采样（reservior sampling）问题，就是专门为了研究这种在较小的随机数范围内完成大范围采样的任务而生的. 当然，到目前为止，处理这个问题的算法效率依旧不高，这可能也是为什么 Python 没有内置它的实现的原因.

最后一个比较重要的函数是 `random.sample(population, k, *, counts=None)`. 这个函数将会**返回**（注意它与 `random.shuffle(x)` 中**修改**不同）一个从序列 *population*（不一定要可变）中随机抽样出来的长度为 *k* 的列表. 注意，这里的抽样是不放回的抽样，也就是说一个样本不会被抽到多次. 如果要使得它能被抽到指定次，可以设置 *counts* 参数；如果想要使得一个样本可以被抽到无限次，可以使用 `random.choices` 函数. 这个函数不会改变 *population* 中的内容，所以它当然不要求 *population* 可变.
