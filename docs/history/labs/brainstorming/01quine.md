# Quine!

!!! failure "警告"

    本实验未正式发布，为试行版本.

> A quine is a computer program that takes no input and produces a copy of its own source code as its only output. The standard terms for these programs in the computability theory and computer science literature are "self-replicating programs", "self-reproducing programs", and "self-copying programs". (Wikipedia)

什么是 quine？我们已经看到了，一个程序可以有输入，可以有输出，其中的代码则说明了从输入到输出经历的过程. 那么，是否可以有一个**输出自身**的程序呢？接下来的探索就是为了表明这一点.

当然，敏锐的读者会发现，一个空文件本身就是 quine. 它没有代码，没有输入，甚至没有输出，那么它当然是一个 quine，而且是平凡的. 那么，是否存在一些更加有意思的 quine 呢？实际上，通过我们已经学过的知识，已经可以构成非常多的 quine 了.

## 错误也是 quine？

下面这个 quine 是一个近乎脑筋急转弯的东西，创建一个名为 `exceptionQuine.py` 的文件，其中包含以下**格式**的“代码”，其中的路径换成这个文件的路径：

```python
Traceback (most recent call last):
  File "E:\python-labs\exceptionQuine.py", line 1
    Traceback (most recent call last):
               ^^^^^^^^^^^
SyntaxError: invalid syntax. Perhaps you forgot a comma?
```

于是，在 Thonny 中执行时，我们貌似得到了一个 quine！这是一个很奇怪，但是在表面上也符合我们给出的要求的程序. 它有着合理的输入，合理的输出. 但是，在命令行执行这段代码时，我们得到的结果却不尽人意：

```powershell
PS E:\python-labs> python exceptionQuine.py
  File "E:\python-labs\exceptionQuine.py", line 1
    Traceback (most recent call last):
               ^^^^^^^^^^^
SyntaxError: invalid syntax. Perhaps you forgot a comma?
```

这时，你会发现第一个 Traceback 奇妙地消失了. 这是因为 Thonny 在执行程序时，实际上与命令行直接执行有一些（不影响正常输出结果）的差别. 你所需要完成的第一个工作是，修改上面的代码以使得其在命令行执行时也能得到一个 quine，只要理解了上面的 quine 涉及的思路，知道 Python 如何生成错误信息以及错误信息中的每一行意味着什么，这应该并不困难.

!!! note "`stdout` 和 `stderr`"

    实际上，上面的代码在严格意义上讲并不是一个 quine. 这涉及一些对操作系统的了解，兹说明如下：

    当一个程序执行时，它的输入和输出是以流（stream）的形式存在的. 形象地，你可以将其理解成流觞曲水：对于输入流，用户将字符放在上游，程序在下游截停输入的字符；对于输出流，程序将字符放在上游，用户在下游截停输出的结果. 一般地，操作系统会默认给程序提供三个流，分别是标准输入流 `stdin`，标准输出流 `stdout` 和标准错误流 `stderr`.

    因此，对于我们学过的 `#!python input()` 和 `#!python print()` 函数，更好的理解应当是，`#!python input()` 函数会从 `stdin` 当中不断地截取字符，以 `\n`，即 ++enter++ 键所指示的换行符作为一段输入终止，随后则关上流的挡板，以后的输入等待下一次读取；而 `#!python print()` 函数会将输出结果放到 `stdout` 流当中，供用户查看.

    那么 `stderr` 是干嘛用的呢？