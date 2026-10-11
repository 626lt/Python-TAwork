# 基于 Monte Carlo 算法实现积分计算及其可视化

!!! warning

    此实验为以前的版本，现在放出供计算机科学基础课程作为拓展阅读，并非 Python 课程的正式实验，Python 课程的班级不需要完成这个实验.

## 实验目的

1. 理解 random 库的用法以及 Monte Carlo 算法的基本思路。
2. 使用 Matplotlib 完成初步的可视化。
3. \* 对随机和伪随机的概念有初步的理解，

## 实验环境

- Python 3.x 环境
- MatplotLib 库

## 实验步骤

### 环境配置和安装

参考[环境配置指南](../00env.md)中的指导安装 Matplotlib 库.

### 利用圆面积估计 $\pi$ 值

我们都已经熟知，积分本质上就是面积的计算. 我们首先考虑计算简单图形的面积：圆面积. 一个圆是由 $x^2 + y^2 <= 1$ 所界定的一块面积. 而根据高中学过的关于几何概型的知识，一个图形的面积与总面积的比值就是在总面积上投点落入图形当中的概率. 也就是说，我们可以考虑以下图形：

<figure markdown="span">
![](../../../assets/history/bonus/monte-carlo-1.svg){ width=200px }
</figure>

上面的图形中，只要取在阴影部分，我们就假定它在圆内. 因此，采用一个随机数生成器的手段，根据点落在圆内的频率估计总的概率，就能给出圆面积的估计. 为了方便起见，我们考虑第一象限的圆，即上图中粉色方框给出的部分.

首先，我们需要引入随机数库并完成基本参数的设置：

```python
import random

inside_circle = 0
num_samples = 10000000
```

其中 `inside_circle` 为在圆圈内的取样点的个数，`num_samples` 为总的取样个数，下面我们将取样操作进行 `num_samples` 次，这使用 `#!python for` 循环来实现：

```python
for _ in range(num_samples):
    x = random.random()
    y = random.random()
```

!!! note

    其中的 `_` 指的是循环变量可以被抛弃，这是一个通用的“垃圾桶”. 也就是说，你甚至可以写出这样的解包操作：


    ```python
    a, _, b = [1, 2, 3]
    ```

    这样就令 `a` 为 `1`，`b` 为 `3`，而中间的值则被永久抛弃了.

`random` 库中的 `random` 函数在 `(0, 1]` 区间内取到一个随机浮点数. 接下来判断它是否在圆内，对于每一次循环，都需要进行一次下面的判断：

```python
if x ** 2 + y ** 2 <= 1:
    inside_circle += 1
```

最后我们就能根据面积进行对 $\pi$ 的估算：

```python
pi_estimate = 4 * (inside_circle / num_samples)
```

思考，如何用这个方法计算更多的积分？你可以尝试计算一些简单的值，例如 $y = x^2$ 之类的函数在 `(0, 1)` 区间中曲线下的面积，这如我们所知就是其在它在 `(0, 1)` 上的积分.

### 利用函数改写现在的代码

现在的代码如下：

```python
import random

inside_circle = 0
num_samples = 10000000
for _ in range(num_samples):
    x = random.random()
    y = random.random()

    if x ** 2 + y ** 2 <= 1:
        inside_circle += 1

pi_estimate = 4 * (inside_circle / num_samples)
```

思考，这样的过程是一个怎样的流程？首先，我们初始化了一堆变量；然后，我们频繁进行采样，判断采样的点是否满足要求；最后，根据采样得到的频率给出计算结果. 为了让这样的程序更加清晰，我们定义几个函数：

```python
def sampling(i):
    """
    @param       i: 第 i 次采样；
    @return (x, y): 返回采样的结果.
    """
    x = random.random()
    y = random.random()

    return (x, y)
```

!!! note

    一般地，我们用三引号来表示一个函数的文档. 读者可以尝试使用

    ```python
    sampling.__doc__
    ```

    来访问函数 `sampling` 的文档，即会作为一个字符串得到下面的注释.

    实际上，三引号的意味是一个保持内部格式的字符串. 这样的字符串在输出的时候能够保持代码中的回车等格式，读者可以阅读官方说明，进行更多的尝试.


这个函数主要做的事情是完成对结果的采样. 我们在后面会发现，这样的采样并非是真正的随机，而是具备自己的算法内核的. 这也就是为什么 Python 官方将 `random` 库描述为**伪**随机数生成器而非真正意义上的随机数生成器. 而第二个函数主要做的是返回点是否满足要求：

```python
def check(x, y):
    """
    @param (x, y): 采样所得的点
    @return     b: True 或者 False，是否满足要求
    """
    return x ** 2 + y ** 2 <= 1
```

复盘整理一下，现在的主程序是

```python
inside_circle = 0
num_samples = 10000000
for i in range(num_samples):
    x, y = sampling(i)

    if check(x, y):
        inside_circle += 1

pi_estimate = 4 * (inside_circle / num_samples)
```

函数的定义放在主函数的前面以使得其可以更好地被利用. 这使得我们：
1. 可以更清楚地看到主程序执行的过程；
2. 可以更容易地修改需要修改的部分，例如，只要修改 `check` 函数就能计算其它区域的积分（当然，最后计算的结果需要稍微做出一点修改）.

这就是应用函数来厘清控制流的一个例证.

### 利用 matplotlib 可视化采样的过程

!!! note

    对于更加习惯 C 式的程序的读者，可以将主程序也包裹起来，下面是对上面的程序进行修改的结果：

    ```python
    import random

    inside_circle = 0
    num_samples = 10000000

    def sampling(i):
        """
        @param       i: 第 i 次采样；
        @return (x, y): 返回采样的结果.
        """
        x = random.random()
        y = random.random()

        return (x, y)

    def check(x, y):
        """
        @param (x, y): 采样所得的点
        @return     b: True 或者 False，是否满足要求
        """
        return x ** 2 + y ** 2 <= 1

    if __name__ == "__main__":
        for i in range(num_samples):
            x, y = sampling(i)

            if check(x, y):
                inside_circle += 1

        pi_estimate = 4 * (inside_circle / num_samples)

        print(pi_estimate)
    ```

    下面因为程序太长的时候要保持清晰性，就延续这样的代码进行修改. 其中 `__name__` 实际上是一个内置变量，用于记录当前函数名，而 `__main__` 则是 Python 默认给主程序设置的名字. 其中，全局变量被放在了最外侧，这与 C 式的风格是一致的.

我们先在程序之前引入 matplotlib 库，如下所示：

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
```

下面我们需要初始化更新步长. 即，每一步需要增加多少个采样点. 在最上面，我们补充：

```python
update_step = 100
```

随后，在主程序中，我们增加代码：

```python
fig, ax = plt.subplots()
ax.set_xlim(0, 1)
ax.set_ylim(0, 1)
ax.set_aspect("equal")
plt.title("Estimating pi using Monte Carlo method: 0")
plt.xlabel("x")
plt.ylabel("y")
```

其中 `fig` 是最终的图像，`ax` 则是我们需要操作的图像. 上面三个调用 `ax` 的方法分别是设置 `x` 轴和 `y` 轴的范围，并且设置 `x` 和 `y` 具备相同的度量，即相同单位具备相同间隔. 下面三个 `plt` 的函数则分别设置了图像的标题、`x` 和 `y` 轴的文字.

在进行动图生成的过程中，最关键的一步是定义更新函数. 代码如下：

```python
def update(frame):
    global inside_circle
    x_inside, y_inside = [], []
    x_outside, y_outside = [], []

    for _ in range(update_step):
        x, y = sampling(_)

        if check(x, y):
            inside_circle += 1
            x_inside.append(x)
            y_inside.append(y)
        else:
            x_outside.append(x)
            y_outside.append(y)

    ax.scatter(x_inside, y_inside, color="red", s=0.03)
    ax.scatter(x_outside, y_outside, color="blue", s=0.03)

    x_inside.clear()
    y_inside.clear()
    x_outside.clear()
    y_outside.clear()

    pi_estimate = 4 * (inside_circle / (frame * update_step)) if frame != 0 else 0
    plt.title(f"Estimating pi using Monte Carlo method: {pi_estimate:.4f}")
```

对于每一帧，我们进行 100 个采样的循环，并且估计 `pi` 值. 其中比较难以理解的可能是 `ax` 的 `scatter` 方法. 它在这个图像上**增加**了一系列散点，其 `x` 和 `y` 的坐标分别是前面两个数组给出的对应数值. `color` 参数表征画点的颜色，而 `s` 表征画点的轮廓粗细. 然后，我们生成更新过程：

```python
ani = animation.FuncAnimation(fig, update, frames=num_samples // update_step, repeat=False)
```

这个函数使用 `update` 作为一个回调函数（callback）来传入. 我们传入参数 `repeat=False`，其目的是使得帧达到最大值时不再反复执行并使得传入 `update` 参数的 `frame` 清零. 请读者思考，如果不做如此处理，会发生什么问题？另一种方式是传入 `init_func` 函数来清空画面. 有兴趣的读者也可以基于此进行修改，使得初始化过程更加清晰，主程序的流程更加简短.

!!! notes "回调函数"

    在[类型与值](../../../faq/typing.md)一部分中，我们业已提及，在 Python 中万物皆类型. 实际上，函数也是一个特殊的类型. 如果执行

    ```python
    print(type(update))
    ```

    得到的输出结果是 `<class 'function'>`，也就是函数类. 它是一个可调用（callable）的类型，即可以在它后面跟上括号，表示用括号中的参数调用这个函数. 实际上，它有一个名为 `__call__` 的方法，在调用是会使用这个方法来进行处理. 在后面学习了关于类的知识之后，读者会对这种性质有更多的理解.

    作为一个例子，可以考虑下面的简单代码：

    ```python
    def add(x, y):
        return x + y

    def mul(x, y):
        return x * y

    def apply(f, x, y):
        return f(x, y)

    x, y = 3, 4
    print(apply(add, x, y))
    print(apply(mul, x, y))
    ```

    它的执行结果就是

    ```
    7
    12
    ```

可以通过

```python
plt.show()
```

来展示画图的结果，或者使用

```python
ani.save("monte_carlo_estimation.gif", writer="pillow", fps=10)
```

来储存动图. 注意，这个计算过程可能会有点久，读者可以修改 `num_samples` 来改变运行的步数以缩短执行的时间.

最终，代码如下：

```python
import random
import matplotlib.pyplot as plt
import matplotlib.animation as animation

inside_circle = 0
num_samples = 10000
update_step = 100

def sampling(i):
    """
    @param       i: 第 i 次采样；
    @return (x, y): 返回采样的结果.
    """
    x = random.random()
    y = random.random()

    return (x, y)

def check(x, y):
    """
    @param (x, y): 采样所得的点
    @return     b: True 或者 False，是否满足要求
    """
    return x ** 2 + y ** 2 <= 1

def update(frame):
    global inside_circle
    x_inside, y_inside = [], []
    x_outside, y_outside = [], []

    for _ in range(update_step):
        x, y = sampling(_)

        if check(x, y):
            inside_circle += 1
            x_inside.append(x)
            y_inside.append(y)
        else:
            x_outside.append(x)
            y_outside.append(y)

    ax.scatter(x_inside, y_inside, color="red", s=0.03)
    ax.scatter(x_outside, y_outside, color="blue", s=0.03)

    x_inside.clear()
    y_inside.clear()
    x_outside.clear()
    y_outside.clear()

    pi_estimate = 4 * (inside_circle / (frame * update_step)) if frame != 0 else 0
    plt.title(f"Estimating pi using Monte Carlo method: {pi_estimate:.4f}")

if __name__ == "__main__":
    fig, ax = plt.subplots()
    ax.set_xlim(0, 1)
    ax.set_ylim(0, 1)
    ax.set_aspect("equal")
    plt.title("Estimating pi using Monte Carlo method: 0")
    plt.xlabel("x")
    plt.ylabel("y")
    ani = animation.FuncAnimation(fig, update, frames=num_samples // update_step, repeat=False)
    # ani.save("monte_carlo_estimation.gif", writer="pillow", fps=10)
    plt.show()
```

### \* 修改随机数生成器：一个额外练习

!!! warning

    这一部分是一个额外的练习，不计入总分，供有兴趣的读者完成.

接下来，我们考虑采样的过程. 下面的代码修改基于第三步的结果，读者也可以在第四步的基础上完成修改. 现在的随机数生成器生成的结果（在表面上）是不可复现的，这是因为它的随机种子（seed）是与时间相关的. 而如果我们在初始化的时候新增一个种子的设置，那么采样的结果就可以复现. 在初始化的部分中加入：

```python
random.seed(0)
```

然后再执行几次程序观察结果. 不难发现，现在的执行结果就完全一致了. 也就是说，拿到了随机数生成种子，就能拿到一切可能的随机数. 这在一些密码学算法中会导致致命的漏洞.

!!! note

    可以替换上面的种子为其它整数，观察执行的结果.

下面我们考虑自己实现一个形如此的结构. 首先，我们考虑实现一种完全均匀的采样方式，称作 Halton 序列. 读者可以搜索 Wikipedia 或参见下面的参考资料来理解其原理，下面给出一个算法描述，其输入为采样次数 $k$ 和基数 $b$：

1. 对于第 $k$ 次采样，将 $k$ 转化为 $b$ 进制数 $d_m d_{m - 1} \cdots d_2 d_1$；
2. 将这个数倒过来，作为 $b$ 进制小数 $0.d_1 d_2 \cdots d_{m - 1} d_m$；
3. 将这个 $b$ 进制小数转化为一个 $(0, 1)$ 之间的小数，这就是这个取样值.

我们可以将这个算法实现成一个函数：

```python
def halton_sequence(k, b):
    """
    @param  k: 采样的次数
    @param  b: 转化过程中引入的基数
    @return x: 采样得到的结果
    """
    #######################
    # 这里是你需要实现的代码 #
    #######################
    return x
```

用于我们的目的，需要对 $x$ 和 $y$ 进行分别的采样. 出于无关性的目标，我们会考虑两者取样的基数互素，即没有除 $1$ 之外的公因数. 也就是说，修改之后的采样函数为：

```python
def sampling(k, b1, b2):
    """
    @param       k: 采样的次数
    @param      b1: 采样 x 时用到的基数
    @param      b2: 采样 y 时用到的基数
    @return (x, y): 返回采样的结果
    """
    x = halton_sequence(k, b1)
    y = halton_sequence(k, b2)

    return (x, y)
```

现在，整个程序形如：

```python
def halton_sequence(k, b):
    """
    @param  k: 采样的次数
    @param  b: 转化过程中引入的基数
    @return x: 采样得到的结果
    """
    #######################
    # 这里是你需要实现的代码 #
    #######################
    return x

def sampling(k, b1, b2):
    """
    @param       k: 采样的次数
    @param      b1: 采样 x 时用到的基数
    @param      b2: 采样 y 时用到的基数
    @return (x, y): 返回采样的结果
    """
    x = halton_sequence(k, b1)
    y = halton_sequence(k, b2)

    return (x, y)

def check(x, y):
    """
    @param (x, y): 采样所得的点
    @return     b: True 或者 False，是否满足要求
    """
    return x ** 2 + y ** 2 <= 1

inside_circle = 0
num_samples = 10000000
factor1 = 17
factor2 = 23
for i in range(num_samples):
    x, y = sampling(i, factor1, factor2)

    if check(x, y):
        inside_circle += 1

pi_estimate = 4 * (inside_circle / num_samples)
```

接下来，我们引入随机数种子，我们取一个大整数，然后修改主程序如下：

```python
inside_circle = 0
num_samples = 10000000
factor1 = 17
factor2 = 23
seed = 19260817
for i in range(num_samples):
    x, y = sampling(i * seed, factor1, factor2)

    if check(x, y):
        inside_circle += 1

pi_estimate = 4 * (inside_circle / num_samples)
```

当然，这个大整数如果要做出和原来的 `random` 一样的结果，可以通过 `time` 库中的 `time_ns` 函数来完成取样：

```python
seed = time.time_ns()
```

注意需要引入 `time` 模块. 此函数的行为可以参见官方说明，见参考资料.

接下来，请读者自行完成实现，并且进行额外的尝试. 例如，可以使用 matplotlib 画出 Halton 序列取样的位置，或者使用置换之类的方式来完成随机数种子的设计，亦参见参考资料. 一个提示是，可以使用 `itertools` 库中的 `permutations` 来生成一系列置换. 同样，你也可以使用这种采样方式计算更多可能的面积，不妨自行尝试.

## 提交方式

我们希望你提交以下内容：

1. 实验代码，`py` 类型的文件，可以有多个.
2. 实验结果，即运行之后的图像，以 `gif` 类型提交.
3. 实验报告，包含对自己的代码的解释、结果分析，以及**过程中遇上的报错和任何遇到的好玩的问题，以及在查资料的时候发现的有趣的知识**，以 `pdf` 类型文件提交.

一般地，只需要提交一个 `zip` 文件即可. 你也可以尝试：

1. 修改代码中保存文件的路径，进而整理文件结构，代码、运行结果和文档分开.
2. 使用 Jupyter Notebook，提交整合的 `ipynb` 类型文件.
3. 你也可以将你的尝试写成博客，发布到网上，然后提交可公开访问的链接.

作为一门编程语言类的课程，我们鼓励同学进行多种多样的尝试，因此，基于原始代码和功能之上的任何改动都是允许且被鼓励的. 但是，为了避免无意义的内卷，只需要完成基础功能、认真写完报告就能获得最高的分数，因此也不必勉强自己，尽情享受完成代码、修改代码的过程吧！

## 参考资料

- [Python `random` 库的官方文档](https://docs.python.org/3/library/random.html)
- [Matplotlib 的官网](https://matplotlib.org/)
- [Physically Based Rendering 一书中关于 Halton 序列的叙述](https://www.pbr-book.org/3ed-2018/Sampling_and_Reconstruction/The_Halton_Sampler)
- [Python `time` 库的官方文档](https://docs.python.org/3/library/time.html)
