# Win11 环境配置

视频教程请观看：[https://www.bilibili.com/video/BV1Bh41157gL](https://www.bilibili.com/video/BV1Bh41157gL)

开源社区中蕴藏着非常丰富的宝藏，要充分体验 OpenMMLab 等开源社区带来的便利，我们需要具备扎实的环境配置技能。掌握了这些技能，我们可以轻松地运行各种开源 demo，并顺利进行后续的断点调试、源码分析等过程，这将帮助我们在学术和职业发展道路上不断从开源社区汲取力量，实现更高的成就。

本文将分为四个部分：

1. Visual Studio Code 的安装
2. Miniconda 配置 Python 虚拟环境
3. PyTorch & MMCV 等依赖的安装
4. 演示 MMDetection 目标检测 demo

## 1. Visual Studio Code 安装

我们到 vscode 的官网 [**https://code.visualstudio.com/**](https://code.visualstudio.com/) **下载安装**

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE.png)

在安装过程中请勾选上以下所有选项，方便后续能够通过右键打开文件夹。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-1.png)

### 1.1 vscode 打开文件夹

#### 鼠标右键法

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-2.png)

#### 命令行 code 法

在 powershell 中 使用 `code directory_path` 命令打开文件夹。

```powershell
code .  # 使用 vscode 打开当前文件夹， . 是当前文件夹的意思
code ..  # 使用 vscode 打开上一级文件夹，.. 是上一级文件夹的意思
code .\openmmlab\ # 使用 vscode 打开当前文件下的 openmmlab 文件夹
code ~ # 打开windows用户主目录路径文件夹
```

根据上述的命令，我们就可以轻松的在 powershell 中轻松使用 `code` 命令打开指定路径的文件夹了。

### 1.2 推荐插件

#### Python 相关

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-3.png)

[https://marketplace.visualstudio.com/items?itemName=ms-python.python](https://marketplace.visualstudio.com/items?itemName=ms-python.python)

#### 远程调试相关

Remote-SSH/Explorer/Development

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-4.png)

## 2. Miniconda 配置 Python 虚拟环境

为什么需要安装Miniconda？原始的Python安装不行吗？

* Miniconda 下的 conda 命令可以创建不同版本的 Python 虚拟环境，可以满足开源社区不同项目对不同 Python 版本的需求。
* conda 命令不仅能够安装常见的 Python 包，而且能够在 Windows 端通过 `conda install git` 安装 `git` 等命令行工具，大大提高了效率。
* conda 是 PyTorch 官方推荐的安装方法。
* Miniconda 的安装包只有70多 MB，非常轻量，而 Anaconda 的安装包则达到了 700 多 MB。官方推荐在安装 Anaconda 时使用 Miniconda。

以上是我们需要安装 Miniconda 的原因。

您可以在 Miniconda 的网站 [https://docs.conda.io/en/latest/miniconda.html](https://docs.conda.io/en/latest/miniconda.html) 下载 exe 文件进行安装。

### 2.1 配置环境变量

:: callout 👉

温馨提示：接下来的演示将全部采用 PowerShell 命令行。

:::

如下图所示，尽管已经安装了 Miniconda，但是此时在系统打开的 PowerShell 和 CMD 终端中无法使用 `conda`命令。这是因为环境变量 PATH 中没有 `conda`命令所在的路径。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-5.png)

环境变量是用于存储操作系统和应用程序需要使用的信息的一种机制。它们包含的信息可以是许多不同的类型，包括路径、文件名、用户名、密码、网络地址、端口号等等。环境变量通常被用作配置信息的存储位置，以便应用程序可以使用这些信息来自定义其行为。

在 Windows 中，您可以使用环境变量来存储各种配置信息。例如，您可以使用环境变量来指定应用程序存储文件的默认路径，或者指定应用程序使用的默认字体。您还可以使用环境变量来配置您的系统路径，以便您可以轻松地访问常用的命令行工具。

要查看 Windows 中的环境变量，可以按下 Win + R 快捷键，打开运行对话框，然后输入“sysdm.cpl”并按 Enter 键。这将打开“系统属性”窗口。在该窗口中，单击“高级”选项卡，然后单击“环境变量”按钮。

在“环境变量”窗口中，您可以查看当前用户和系统的环境变量。您还可以使用“新建”、“编辑”和“删除”按钮来添加、编辑或删除环境变量。注意，更改系统环境变量可能需要管理员权限。

在 PowerShell 中，您可以使用以下命令来列出所有环境变量：

```powershell
Get-ChildItem Env:
```

![Untitled](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-6.png)

您还可以使用以下命令来获取特定环境变量的值：

```powershell
$Env:VariableName
```

例如，要获取 PATH 环境变量的值，可以使用以下命令：

```powershell
$Env:PATH
```

希望这些信息能够帮助您更好地理解 Windows 中的环境变量，并且方便您在环境配置过程中进行调整。

#### **使用 Powershell 设置环境变量**

要在 PowerShell 中向 **`PATH`** 环境变量中新增指定路径，可以使用以下命令：

```powershell
$env:Path += ";C:\ProgramData\miniconda3\"
$env:Path += ";C:\ProgramData\miniconda3\Library\bin"
$env:Path += ";C:\ProgramData\miniconda3\Scripts"
```

这将在当前 PowerShell 会话中临时添加这些路径到 **`PATH`** 环境变量。请注意，这些更改仅对当前会话有效，当您关闭 PowerShell 时，这些更改将丢失。

要永久添加这些路径到 **`PATH`** 环境变量，请使用以下命令：

```powershell
[Environment]::SetEnvironmentVariable("Path", [Environment]::GetEnvironmentVariable("Path", "User") + ";C:\ProgramData\miniconda3\;C:\ProgramData\miniconda3\Library\bin;C:\ProgramData\miniconda3\Scripts", "User")
```

这条命令将把指定的路径添加到用户级别的 **`PATH`** 环境变量中。这意味着更改仅对当前用户有效。要将这些路径添加到系统级别的 **`PATH`** 环境变量中（对所有用户有效），请将 "User" 参数替换为 "Machine"。请注意，修改系统级别的 **`PATH`** 变量可能需要管理员权限。

在更改环境变量后，您需要重新启动 PowerShell 或者其他应用程序，以使更改生效。

#### **可视化窗口配置环境变量**

所以，我们在windows 中需要设置一下 conda 的环境变量。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-7.png)

如上图所示，在系统环境变量中的 PATH 变量中，新增以下路径（请社区同学根据自己 miniconda 安装的实际位置进行调整）。

```bash
C:\ProgramData\miniconda3\
C:\ProgramData\miniconda3\Library\bin
C:\ProgramData\miniconda3\Scripts
```

原理解释：我们可以看到，在路径 `C:\\ProgramData\\miniconda3\\Scripts` 下可以找到可执行文件 `conda` 和 `pip`。当我们在命令行中执行 `conda` 命令时，系统会根据环境变量 `PATH` 里留下的路径 `C:\\ProgramData\\miniconda3\\Scripts` 找到 `conda` 可执行程序并执行它。因此，如果 `conda` 所在的文件夹没有出现在系统环境变量 `PATH` 中，系统就无法找到该程序并报错。这就是我们需要设置环境变量的原因。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-8.png)

完成设置后我们重新打卡 powershell 终端 输入 conda 命令，发现我们已经可以成功执行了，如下图所提示的，conda 命令包含 `create` 、`init` 、`info` 等命令。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-9.png)

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

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-10.png)

完成此操作后，您可以通过输入以下命令激活名为 `openmmlab` 的 Python 3.9 虚拟环境：

```powershell
conda activate openmmlab
```

要退出虚拟环境，请运行：

```powershell
conda deactivate
```

### 2.2 切换国内源

由于 conda、pypi 等服务器位于海外，使用默认源安装软件可能会因为网络延迟等原因而变得非常缓慢，甚至失败。因此，使用国内源可以更快地下载和安装所需的软件包，提高操作效率。同时，国内源还可能提供一些海外源没有的软件包，或者提供更加稳定的软件包。因此，为了方便国内用户使用，建议将 conda 和 pip 源切换到国内源。

#### conda 换源

conda 换源方法参考自: [https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/)

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

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-11.png)

`.condarc` 文件中的内容如下，如果不想通过命令行的方式换源，也可以在 `%USERPROFILE%` 路径下新建一个 `.condarc` 文件，然后将以下内容复制到 `.condarc` 文件中也可以实现换 conda 源的操作。

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

#### pypi 换源

pypi 换源方法参考自：[https://mirrors.tuna.tsinghua.edu.cn/help/pypi/](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/)

```
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

上述代码将 pip 配置文件中的源地址修改为清华源。

原理解释：pip 是 Python 包管理工具，使用时也需要从 Internet 上下载很多东西，包括安装包、依赖包等等，同样需要换成国内的镜像源。清华大学开源软件镜像站也提供了 Python 的镜像源，只需要将 pip 的源地址替换成清华源即可。

除了使用 `pip config` 命令修改 pip 源之外，您还可以手动编辑 `pip.conf` 文件。该文件通常位于 `~/.pip/` 或 `%APPDATA%\\pip\\` 目录下。

在 `pip.conf` 文件中添加以下内容，即可将 pip 源修改为清华源：

```
[global]
index-url = <https://pypi.tuna.tsinghua.edu.cn/simple>
```

原理解释：`pip.conf` 是 pip 的配置文件，其中包含了 pip 的配置信息，包括 pip 源的地址。通过手动编辑 `pip.conf` 文件，您可以将 pip 源修改为清华源，使 pip 下载和安装 Python 包时更加快速和稳定。

### 2.3 安装 Git

windows 是没有预装 Git 命令行软件的，所有我们在 PowerShell 运行 git 命名，是会报错的。

![Untitled](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-12.png)

我们可以使用 conda 安装 git

```powershell
 conda install git
```

安装完毕后我们能在 `C:\ProgramData\miniconda3\Library\bin` 路径下找到刚刚安装的 `git` 可执行命令。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-13.png)

## 3. PyTorch & MMCV 等依赖安装

```powershell
# ~  代表 C:\Users\{UserName} 用户主目录
cd ~

# 在 ~ 用户主目录中创建 openmmlab 文件夹
mkdir openmmlab

cd openmmlab

# 使用 vscode 打开 openmmlab 文件夹
code .
```

通过上述命令使用 vscode 打开 openmmlab 文件夹后，新建一个 `hello_world.py` 的 python文件。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-14.png)

然后在 VSCODE 中安装 Python 插件

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-15.png)

### 3.1 创建虚拟环境

![Untitled](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-16.png)

在 Explore 工作区，点击鼠标右键，然后点击 Open in Integrated Terminal （在集成终端中打开），在 vscode 的工作区下面就出现了命令行终端。

![Untitled](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-17.png)

我们在上述的命令行终端中运行

```powershell
conda create -n openmmlab python=3.9
```

使用以下命令进入 `openmmlab` 环境：

```powershell
conda activate openmmlab
```

### 3.2 Visual Studio Code 选择虚拟环境

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-18.png)

我们先点击 hello\_world.py 这个 python 文件，再点击右下角的虚拟环境选择器，最后在 vscode 上方选择刚刚创建的 python 虚拟环境。

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-19.png)

此时 在 Explore 工作区，点击鼠标右键，然后点击 Open in Integrated Terminal （在集成终端中打开）openmmlab 的 conda 虚拟环境会被自动激活。

### 3.3 安装 PyTorch cpu 版本

![](https://cdn.vansin.top/picgo/Windows%2011%20%E7%8E%AF%E5%A2%83%E9%85%8D%E7%BD%AE-20.png)

我们先在 vscode 中新建一个 powershell 终端，然后使用如下命令安装 Pytorch 的 cpu 版本。

```powershell
conda install pytorch torchvision torchaudio cpuonly -c pytorch
```

`-c pytorch`的意思是使用 `pytorch: [https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud](https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud)` 这个 channel 进行下载并安装预编译包。

### 3.4 安装 MMCV

使用以下命令在 vscode 的 powershell 终端的 openmmlab 环境中 通过 mim 安装 mmcv。

```powershell
pip install openmim
mim install "mmcv>=2.0.0"
```

到这里 openmmlab 依赖的 pytorch mmcv 的软件包已经安装完毕。

## 4. 安装 MMDetection 并运行 Demo

现在我们来运行 MMDetection 的 Demo。

```bash
conda install pycocotools -c conda-forge

pip install openmim
mim install "mmcv>=2.0.0"
git clone https://github.com/open-mmlab/mmdetection.git
# git clone https://gitee.com/open-mmlab/mmdetection.git
cd mmdetection
git checkout tags/v3.0.0

pip install -v -e . # 源码安装 mmdet

code .  # 打开 mmdetection 工作区
```

### 4.1 验证安装

步骤 1. 我们需要下载配置文件和模型权重文件。

```shell
cd path/to/mmdetection
mim download mmdet --config rtmdet-ins_tiny_8xb32-300e_coco --dest .
```

下载将需要几秒钟或更长时间，这取决于你的网络环境。完成后，你会在当前文件夹中发现两个文件 `rtmdet_tiny_8xb32-300e_coco.py` 和 `rtmdet_tiny_8xb32-300e_coco_20220902_112414-78e30dcc.pth`。

**步骤 2.** 推理验证。

```shell
python demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu
```

你会在当前文件夹中的 `outputs/vis` 文件夹中看到一个新的图像 `demo.jpg`，图像中包含有网络预测的检测框。

![image](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/561cbe54-3bac-4d37-bf07-1b5ad6cf855d)

### 4.2 对 image\_demo.py 进行简单的 debug。

```shell
pip install debugpy
```

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/7e399eeb-2345-44a0-a1aa-2b0647d9f2be)

将 launch.json 文件替换为以下配置。

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Python: Remote Attach",
            "type": "python",
            "request": "attach",
            "connect": {
                "host": "localhost",
                "port": 5678
            },
            "justMyCode": false
        }
    ]
}
```

```shell
python -m debugpy --listen 5678 --wait-for-client demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu
```

通过以下三步能对上文运行的程序进行 debug。

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/ef34f94e-6ae5-46cd-b886-7b6e5517ac85)

但是此种 debug 方式需要将原来的 `python` 替换为 `python -m debugpy --listen 5678 --wait-for-client` 命令输入起来也比较累，所以下文将采取`别名`的方式简化命令。

### 4.3 设置 `python -m debugpy --listen 5678 --wait-for-client` 别名为 `pyd`

在 powershell 中执行 `code $PROFILE`, 使用 vscode 打开 powershell 的 `$PROFILE` 文件

并在 $PROFILE 中插入以下内容设置别名

```shell
function pyd {
    python -m debugpy --wait-for-client --listen 5678 $args
}
```

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/79349287-a02c-4f58-9869-9a9aa953fc39)

此时 debug 命令就简化为

```shell
pyd demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu

# 原始命令是 python，只需要将 python 换成 pyd 就能在 vscode 中进行 debug 了。
# python demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu
```

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/aae3ad97-56ef-4f59-ab8b-c01f351d2900)
