# Python 虚拟环境管理

当我们实际使用 Python 进行工作时，往往会碰到需要大量依赖包的情况. 而很多的依赖包之间会出现冲突和依赖的关系. 为了方便处理这种情况，我们会使用 venv 或者 conda 之类的工具来创建虚拟环境，在虚拟环境中安装依赖包，使得不同虚拟环境之间依赖包互不干扰.

## conda

conda 可以帮助你在同一台机子的同一个账户下创建和管理多个 Python 环境，各个环境相互独立，不会相互影响；而且每个环境封装在一个文件夹中，克隆、移除都很方便. 

一般说的安装 conda 可能是安装 Anaconda 或者 miniconda. 

- Anaconda 完全包含了 miniconda，预装了 Juputer Notebook 等组件，也提供图形化功能
- 相比之下 miniconda 比较轻量级，只提供 python 和 conda 功能

相较之下更推荐使用 miniconda，可以在 [Latest Miniconda installer links by Python version](https://docs.conda.io/projects/miniconda/en/latest/miniconda-other-installer-links.html) 下载安装最新版的 miniconda. 

在 Windows 上安装配置 conda 环境时可能会发生一些环境问题，需要大家各自搜索解决，也可以适当参考[这篇知乎文章](https://zhuanlan.zhihu.com/p/591091259). 另一种解决方案是使用 WSL，这样可以在 Windows 上启动一个 Linux 子系统，随后在这个子系统上面按照 Linux 的方式安装即可。如果需要从零开始使用 WSL，可以参考 ZhouTimeMachine 在另一门课程写的[教程](https://zhoutimemachine.github.io/2023_FPA/env/windows_lost/#wsl)进行安装. 

安装完成并配置终端自动初始化 conda 后，你启动终端时最左侧应当会出现一个 `(base)`，这是因为 conda 默认的全局环境为 base. 如果做什么都使用这个环境，很容易导致环境混乱. 

针对某种特定的用途应当新建一个专用的环境。例如希望新建一个专门用于本课程的环境，你可以先给它起一个名字，例如 sr. 然后用下面的命令可以创建一个名为 sr，python 版本为 3.10 的环境. 

```bash
conda create sr python=3.10
```

> 如果用别的名字，把上面命令中的 `sr` 修改成你自己想用的名字即可，下面也同理；也不一定就使用 3.10 版本的 python，可以根据自己的需要修改版本号.

完成创建后，需要激活新建的环境才能使用，即

```bash
conda activate sr
```

此时应当看到终端最左侧的 `(base)` 变成了 `(sr)`. 随后就可以使用 conda 或者 pip 在这个新环境里面安装所需要的包进行使用了. 要经常关注终端最左侧显示的当前 conda 环境，防止使用了错误的环境使程序无法运行，或者将包安装到错误的环境中.