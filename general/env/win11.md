# Win11 环境配置（建设中）

vscode安装

miniconda 虚拟环境配置（conda 和 pip 换国内源）

安装pytorch

安装mmcv

安装mmdetection

运行一个 mmdetection rtmdet 的demo

YvesXu 编写中，预计周末开始更新\~@[YvesXu (github.com)](https://github.com/YvesXu)


win10 系统也可以参照本教程进行基本的开发环境的配置。

场景：今天学妹问我怎么在windows上配置环境，刚刚接触深度学习和Python，如何优雅的在windows上优雅的配置开发环境呢？

# 1. Visual Studio Code 安装

从官网下载 [https://code.visualstudio.com/](https://code.visualstudio.com/) exe 进行安装，此处就不演示了。

必须安装的插件：Python 插件

# 2. Miniconda 安装

您可以在 Miniconda 的网站 [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html) 下载 exe 文件进行安装。我们选择安装 Miniconda 而不是 Anaconda 是因为其包大小较小，安装速度更快，而且基本上它的功能就能够满足我们的需求。

## 设置环境变量

PowerShell 是一个跨平台的命令行 shell 和脚本语言，它与 cmd.exe（命令提示符）不同，它可以更好地处理文本和对象，以及更好地支持脚本和自定义函数。它是 Windows 7 和 Windows Server 2008 R2 中的默认 shell，也可以在 Windows 8、Windows 10、Windows Server 2012、Windows Server 2016 和其他操作系统中使用。

cmd.exe 是一个命令行解释器，它启动一个命令行界面，在该界面中，用户可以键入并执行命令。它是 Windows 操作系统的一部分，用于执行命令和批处理脚本，它与 PowerShell 不同，因为它不支持对象和脚本的处理，而且其功能非常有限。

::: callout 👉

注意：后续我们都是用 PowerShell 命令行进行演示。

:::

如下图所示，尽管已经安装了 Miniconda，但是此时在系统打开的 PowerShell 和 CMD 终端中无法使用 conda 命令。这是因为系统环境变量 PATH 中没有 conda 命令所在的路径。

![notion_win11](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%2039c1bcbaa93f4c69b9f089752b1717aa.png)

notion_win11

所以，我们在windows 中需要设置一下 conda 的环境变量。

![notion_env](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%2039c1bcbaa93f4c69b9f089752b1717aa-1.png)


如上图所示，在系统环境变量中的 PATH 变量中，新增以下路径（请社区同学根据自己 miniconda 安装的实际位置进行调整）。

```bash
C:\ProgramData\miniconda3\
C:\ProgramData\miniconda3\Library\bin
C:\ProgramData\miniconda3\Scripts
```

原理解释：我们可以看到，在路径 `C:\\ProgramData\\miniconda3\\Scripts` 下可以找到可执行文件 `conda` 和 `pip`。当我们在命令行中执行 `conda` 命令时，系统会根据环境变量 `PATH` 里留下的路径 `C:\\ProgramData\\miniconda3\\Scripts` 找到 `conda` 可执行程序并执行它。因此，如果 `conda` 所在的文件夹没有出现在系统环境变量 `PATH` 中，系统就无法找到该程序并报错。这就是我们需要设置环境变量的原因。

![notion_conda](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%2039c1bcbaa93f4c69b9f089752b1717aa-2.png)


完成设置后我们重新打卡 powershell 终端 输入 conda 命令，发现我们已经可以成功执行了，如下图所提示的，conda 命令包含 `create` 、`init` 、`info` 等命令。

![notion_20230430_powershell](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%2039c1bcbaa93f4c69b9f089752b1717aa-3.png)

notion_20230430_powershell

以管理员模式打开 PowerShell，然后输入以下命令：

```powershell
Set-ExecutionPolicy RemoteSigned
```

在 PowerShell 终端中，输入以下命令 `conda init`初始化，可以让打开cmd或者powershell时进入conda的base环境。

```powershell
conda init
```

这将添加 Conda 初始化的脚本到您的 PowerShell 配置文件中，以便在以后启动 PowerShell 时自动激活 Conda 的 base 环境。

在 PowerShell 终端中，输入以下命令创建名为 `openmmlab` 的 Python 3.9 虚拟环境：

```powershell
conda create -n openmmlab python=3.9
```

![notion/1](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%2039c1bcbaa93f4c69b9f089752b1717aa-4.png)

notion/1

完成此操作后，您可以通过输入以下命令激活名为 `openmmlab` 的 Python 3.9 虚拟环境：

```powershell
conda activate openmmlab
```

要退出虚拟环境，请运行：

```powershell
conda deactivate
```

## 换源

由于conda、pypi等服务器位于海外，如果想要顺畅地安装PyTorch等软件，需要将 conda 和 pip 源切换到国内源。

```powershell

conda config --add channels defaults
conda config --set show_channel_urls true

# 设置默认channels
conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
conda config --add default_channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2

# 设置 custom channels
conda config --set custom_channels.conda-forge https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
conda config --set custom_channels.msys2 https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
conda config --set custom_channels.bioconda https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
conda config --set custom_channels.menpo https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
conda config --set custom_channels.pytorch https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
conda config --set custom_channels.pytorch-lts https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
conda config --set custom_channels.simpleitk https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

在 powershell 中运行一句一句执行上述命令，可以发现在 `C:\Users\{UserName}` 路径下生成了`.condarc` 文件，此时就实现了换源的操作，此时`{UserName}` 为 `msnod` ，用户需要根据自己用户名修改。

![notion/2](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE%2039c1bcbaa93f4c69b9f089752b1717aa-5.png)

notion/2

`.condarc` 文件中的内容如下，如果不想通过命令行的方式换源，也可以在 `%USERPROFILE%` 路径下新建一个 `.condarc` 文件，然后将以下内容复制到 `.condarc`  文件中也可以实现换 conda 源的操作。

```yaml
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

## 安装 PyTorch

在安装 PyTorch 之前，使用 `conda create` 命令创建一个名为 openmmlab 的 Python 3.9 虚拟环境。

```powershell
conda create -n openmmlab python=3.9
```

使用以下命令进入 `openmmlab` 环境： 

```powershell
conda activate openmmlab
```

使用如下命令安装 Pytorch 的 cpu 版本。

```powershell
conda install pytorch torchvision torchaudio cpuonly -c pytorch
```

`-c  pytorch`的意思是使用  `pytorch: [https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud](https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud)` 这个 channel 进行下载并安装预编译包。

## 3. 安装 MMEngine+MMCV 预编译包

```powershell
pip install openmim
mim install "mmcv>=2.0.0"
```

## 4. 安装 MMDetection 并运行 Demo
