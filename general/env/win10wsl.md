# Windows WSL 环境配置

本教程由社区同学 @geoffreyfan 贡献\~

## 1. Win10 升级最新版本和 Win10 安装英伟达对 Linux 子系统的显卡驱动

1、Win10 系统一定要升级到最新版，否则是无法在 Linux 子系统里加载出显卡驱动的，那么就无法进行深度学习！

2、Linux 子系统不需要配置显卡。因为它是调用 Win10 系统下的显卡驱动进行深度学习的，这就意味着不需要在 Linux 子系统下载安装显卡驱动！

大家可能会有这样的疑问：不是安装 WSL2 的显卡驱动吗？为什么会装 Win10 系统上的显卡驱动？其实，官网文档上说明了，带有 WSL2 的官方 NVIDIA 驱动是整个过程唯一要装的 GPU 驱动！所以后面关于 Linux 上的 cuda toolkit 的配置就要求不勾选 Driver！

![](https://user-images.githubusercontent.com/105597268/233543981-2ca68668-7db1-432e-a342-11ac0331daf9.png)

### 更新 Win10 系统

微软 Microsoft 官网下载：https://www.microsoft.com/zh-cn/software-download/windows10。

点击立即更新，会下载一个微软软件，按照提示更新即可，装完需要重启，网速快的话整个过程大约一个小时。（请确保您的网络能够正常访问）。

![](https://user-images.githubusercontent.com/105597268/233544719-94c8dafb-f29b-49bd-8378-4f1f2b31a0ab.png)

![](https://user-images.githubusercontent.com/105597268/233544743-6576de41-5254-4de9-bac1-bea57f2b6822.png)

另一种方式更新 Win10 系统：

![](https://user-images.githubusercontent.com/105597268/233544762-871cf8c6-32c9-4ea6-a731-b03719274466.png)

![](https://user-images.githubusercontent.com/105597268/233544776-c3ed805c-1d37-4d84-875a-cb2a6bc1c807.png)

![](https://user-images.githubusercontent.com/105597268/233544794-95ef3c72-a3de-47cd-b9ed-b1e3928f8ca4.png)

PS：查询系统版本方法：Win+R 输入 winver 回车：

![](https://user-images.githubusercontent.com/105597268/233577393-ac7da003-9320-4dd5-9d71-34b03a02e148.png)

### Win10 系统下安装英伟达对 Linux 子系统的显卡驱动

下载驱动（下载GEFORCE的那个）：http://www.nvidia.com/Download/index.aspx

注意，该是安装 Win10 驱动，而不是安装 Linux 驱动，在 Win10 下安装驱动后，会自动将驱动以 libcuda.so 的形式集成至 WSL2 中，因此切勿在 WSL Linux 中重复安装驱动。

根据自己显卡的型号选择驱动，Notebooks 代表笔记本，我是台式电脑 3080 显卡，所以选择 GeForce RX 30 Series 下的 GeForce RX 3080。

![](https://user-images.githubusercontent.com/105597268/233551395-537e1988-2894-476e-a3f8-c60e4ee5c853.png)

![](https://user-images.githubusercontent.com/105597268/233550992-457c493f-c194-4deb-b65f-155ddb8dff58.png)

![](https://user-images.githubusercontent.com/105597268/233545477-fe21aece-1d40-40d8-a6c0-0f9f78fb0c2b.png)

![](https://user-images.githubusercontent.com/105597268/233545659-28f634eb-4b41-49d6-aa9c-72a63e7fd13d.png)

![](https://user-images.githubusercontent.com/105597268/233545719-5ba40012-5d30-42e5-839f-a94eab7cdcb2.png)

![](https://user-images.githubusercontent.com/105597268/233545847-afb912ce-65d7-47ca-bc73-505f7b2d289f.png)

在 Ubuntu-22.04.exe 程序中输入以下指令，验证驱动是否安装成功：

```sh
nvidia-smi -pm 1
lspci | grep NVIDIA
nvidia-smi
```

![](https://user-images.githubusercontent.com/105597268/233559790-1fae3280-77d7-4079-8711-fe9fc58222e9.png)

## 2. WSL2 和 Ubuntu-22.04 Linux 子系统的下载安装

### 下载并安装 WSL2 的 Linux 内核包

下载链接：https://wslstorestorage.blob.core.windows.net/wslblob/wsl\_update\_x64.msi

下载完点击安装即可，安装中不停地点击next就可以。

![](https://user-images.githubusercontent.com/105597268/233546108-e97fcec8-0848-48f1-a1b1-76327a5101aa.png)

打开 powershell：

![](https://user-images.githubusercontent.com/105597268/233546130-c846904d-f372-40bd-a7a5-7018bbd7cc2c.png)

启动 WSL2 ，在 powershell 中输入下面指令：

```sh
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

启用“虚拟机平台”，在 powershell 中输入下面指令：

```sh
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

将 WSL2 设置为默认版本，在 powershell 中输入下面指令：

```sh
wsl --set-default-version 2
```

### Win10 下安装 Linux 子系统

网站地址：https://www.microsoft.com/zh-cn/

搜索 ubuntu，选择 22.04 版本安装：

![](https://user-images.githubusercontent.com/105597268/233546452-184fad0d-b31d-4bd8-b3e2-18b1555bd2aa.png)

![](https://user-images.githubusercontent.com/105597268/233546466-9e391780-70f0-41d5-8836-0f1ca1db015f.png)

进入控制面板的 “程序和功能” 里边的设置，勾选 “ Hyper-V ” 和 “适用于 Linux 的 Windows 子系统” ：

![](https://user-images.githubusercontent.com/105597268/233546795-db997990-28aa-4870-96a5-ea0d9418afdb.png)

![](https://user-images.githubusercontent.com/105597268/233546804-0086f867-a564-4ad0-81f9-9a4ddc020045.png)

下载完打开 Ubuntu-22.04 Linux 子系统：

![](https://user-images.githubusercontent.com/105597268/233546859-45c1e1c6-aa4e-427c-a375-e2170c60363e.png)

![](https://user-images.githubusercontent.com/105597268/233546867-db689603-384a-432b-82ae-308840e52f8a.png)

### WSL2 的位置迁移

在 PowerShell 中运行下面命令显示所有发行版的详细信息：

```sh
wsl --list --verbose
```

![](https://user-images.githubusercontent.com/105597268/233547562-c5e7ae73-d0a2-45c1-85c9-4ca421d029da.png)

终止正在运行的 wsl：

```sh
wsl --shutdown
```

将需要迁移的 Ubuntu-22.04 Linux 子系统，进行导出：

```sh
wsl --export Ubuntu-22.04 E:/export.tar
```

导出完成之后，就需要将原有的进行卸载：

```sh
wsl --unregister Ubuntu-22.04
```

注销完后：

![](https://user-images.githubusercontent.com/105597268/233547746-f532ffe2-bdd7-416b-82dc-03029c5f1caa.png)

然后将导出的文件放到需要保存的地方，进行导入即可：

```sh
wsl --import Ubuntu-22.04 E:\export\ E:\export.tar --version 2
```

![](https://user-images.githubusercontent.com/105597268/233589225-937d4217-959c-4728-a69f-1f091563478d.png)

![](https://user-images.githubusercontent.com/105597268/233589320-f5234c4d-21d9-4999-b12a-ad3d0f796adf.png)

如果后面想要重装 Ubuntu-22.04 Linux 子系统，删除之前的版本，在执行了 wsl --shutdown 指令后，直接删除 ext4.vhdx 文件即可。

### 进行 WSL2 的 root 和普通用户的切换：

在 cmd 里面输入下面指令，切换到根目录：

```sh
ubuntu2204 config --default-user root
```

![](https://user-images.githubusercontent.com/105597268/233590514-561c671a-22e0-4afa-928c-1ced30a3497c.png)

打开 Ubuntu-22.04.exe 程序，此时就进入根目录了，输入 passwd 用户名，并设置密码：

![](https://user-images.githubusercontent.com/105597268/233548076-34145ecb-8b46-49d4-ae87-8295829ae8ce.png)

在 cmd 里面输入下面指令，切换到普通用户：

```
ubuntu2204 config --default-user geoffreyfan（这里填写你自己的用户名字） 
```

![](https://user-images.githubusercontent.com/105597268/233590812-cee2d3a7-fb20-469e-89f5-ddce74645ac1.png)

打开 Ubuntu-22.04.exe 程序，此时就进入普通用户界面了：

![](https://user-images.githubusercontent.com/105597268/233548107-857c6f92-a1e2-450a-b77d-4cc6e3345f4e.png)

## 3. VScode 远程连接 Ubuntu-22.04 Linux 子系统 :

下载 WSL 远程连接 Ubuntu-22.04 Linux 子系统的插件：

![](https://user-images.githubusercontent.com/105597268/233548620-fc6867e6-5552-469b-a205-936553fa8a16.png)

快捷指令远程连接，用 VScode 打开 Ubuntu-22.04 Linux 子系统：

```
code .
```

![](https://user-images.githubusercontent.com/105597268/233664035-e18937c6-8aa3-4dfb-863a-1887b92dd56b.png)

手动远程连接，用 VScode 打开 Ubuntu-22.04 Linux 子系统：

![](https://user-images.githubusercontent.com/105597268/233548642-e12a5de7-556d-4bf4-98cf-4b701a7711ff.png)

![](https://user-images.githubusercontent.com/105597268/233548674-26a8edba-09f7-447f-bbee-410ed0fc9126.png)

关闭远程连接， Powershell 输入下面指令（重装系统卸载 Ubuntu-22.04 Linux 子系统也需要先输入下面指令）：

```
wsl --shutdown
```

![](https://user-images.githubusercontent.com/105597268/233548708-de0c32f7-3ff2-4088-b37a-ac2aa319478a.png)

点击打开 Ubuntu-22.04.exe 程序：

![](https://user-images.githubusercontent.com/105597268/233548951-ce31856b-0b0a-4ab8-a89b-d01260f99f6b.png)

重新进行远程连接：

![](https://user-images.githubusercontent.com/105597268/233548975-3d24ce7e-f604-4344-b966-047c64a2e5c2.png)

打开 Ubuntu-22.04 Linux 子系统里面的文件：open Folder , 点击 OK ：

![](https://user-images.githubusercontent.com/105597268/233549008-ef26bd5e-aa3b-44f6-9948-570237b166ca.png)

![](https://user-images.githubusercontent.com/105597268/233549034-0bb03630-8c8b-459c-9e88-05a97bd9ab1d.png)

Ubuntu-22.04 Linux 子系统里面的 /home/geoffreyfan (这里是你自己的用户名字) / 路径下的文档就被打开了:

![](https://user-images.githubusercontent.com/105597268/233549085-53076917-cddd-4b3f-820e-43b1cfeb7814.png)

可以通过拖拉 Win10 里面的文件，方便地上传文件到 Ubuntu-22.04 Linux 子系统的这个路径下面：

![](https://user-images.githubusercontent.com/105597268/233627037-0bb11b45-25d6-4ee6-863f-2fbb0125919f.png)

## 4. WSL2 的 Miniconda 配置

安装 Miconda 地址: https://docs.conda.io/en/latest/miniconda.html

![](https://user-images.githubusercontent.com/105597268/233582670-1464caf7-a3d7-48f4-849d-f459b45dd6cc.png)

![](https://user-images.githubusercontent.com/105597268/233583155-f8037347-48ca-4756-80df-28b6948b9c90.png)

浏览器下载下完后，拖入到 Ubuntu-22.04 Linux 子系统里面：

![](https://user-images.githubusercontent.com/105597268/233628794-950bfcab-645c-44cf-be0b-cd8d488fc9e9.png)

输入指令：bash Miniconda3-latest-Linux-x86\_64.sh 安装：

![](https://user-images.githubusercontent.com/105597268/233629009-bc48cc05-705d-4956-8d4a-5d37ae670280.png)

安装过程中，刚开始要按回车键很多次（大概一直按住5秒这样），之后两三个地方会让你输入 yes/no ，全输入 yes 。有提示输入回车的，就按回车。

安装完成之后，在 .bashrc 文件中最后一行加上：PATH=/home/user/miniconda3/bin:$PATH ，并保存：

![](https://user-images.githubusercontent.com/105597268/233629221-53e51d1b-7b40-45e3-94b2-3e99baff1f53.png)

创建一个 project.py 文件，配置 miniconda 路径下的 python 解释器，并在终端执行该 python 文件，如果前面有 （base） 这个标志，那么说明 miniconda 配置成功：

![](https://user-images.githubusercontent.com/105597268/233629718-519b87c7-cc65-4dc9-99fb-f5dba91bafe2.png)

![](https://user-images.githubusercontent.com/105597268/233629827-418c2569-93ef-4946-a3b5-5bf2ebd15b7a.png)

用 miniconda 创建虚拟环境, 并激活该虚拟环境（根据自己的需求来，这里我们选择配置 3.9 版本的 python 环境）：

```
conda create -n pytorch python=3.9 -y
conda activate pytorch
```

![](https://user-images.githubusercontent.com/105597268/233629974-89ad74d5-6a09-4e4b-82dc-add0a8cf0339.png)

配置好虚拟环境后，我们配置 miniconda 下我们创建的虚拟环境下的解释器：

![](https://user-images.githubusercontent.com/105597268/233630372-da60128f-c77f-4aab-8024-9e6f8f313127.png)

配置好后，我们在终端运行我们创建的python文件，会发现 （base） 标志变成了我们的虚拟环境的名称 （pytorch） :

![](https://user-images.githubusercontent.com/105597268/233630630-dc8d754a-7928-4193-bda5-20224cdba5f7.png)

## 5. 下载 torch==1.9.1+cu111 和 torchvision==0.10.1+cu111

如果用以下指令下载很慢：

```
pip install torch==1.9.1+cu110 torchvision==0.10.1+cu110 -f https://download.pytorch.org/whl/torch_stable.html
```

直接手动去官网下载.whl文件，然后拖到 Ubuntu-22.04 Linux 子系统里面，官网地址：https://download.pytorch.org/whl/torch\_stable.html

![](https://user-images.githubusercontent.com/105597268/233557377-e01c271a-ed83-4e8c-a694-d71e0e6ada99.png)

蓝色小横线消失了，就上传完了：

![](https://user-images.githubusercontent.com/105597268/233556601-ff402f11-185c-4953-8de0-21e26601c3cd.png)

上传完之后，Ubuntu-22.04 Linux 子系统的 /home/geoffreyfan (这里是你自己的用户名字) / 路径下的文件中会有两个相应的安装包：

![](https://user-images.githubusercontent.com/105597268/233630823-6e83678f-583b-4161-929c-7bac50eb7ae4.png)

创建一个 project.py 文件，输入下面的代码，并配置之前我们创建的 miniconda 的虚拟环境的 python 的解释器：

```
import torch
import torchvision
device = torch.device('cuda')
print(torch.cuda.is_available())
print(torch.__version__)
print(torchvision.__version__)
```

![](https://user-images.githubusercontent.com/105597268/233631119-89a5b439-76ce-4b34-9b34-5e6deaaa58e4.png)

在终端执行以下指令：

```
pip install torch-1.9.1+cu111-cp39-cp39-linux_x86_64.whl
pip install torchvision-0.10.1+cu111-cp39-cp39-linux_x86_64.whl
```

![](https://user-images.githubusercontent.com/105597268/233561233-d4b035e7-aa26-46cc-9b53-305a8f981ce0.png)

然后运行我们刚才创建的 project.py 文件：

![](https://user-images.githubusercontent.com/105597268/233628149-ed5e3714-53a0-4c97-9ea1-1f9116b93540.png)

![](https://user-images.githubusercontent.com/105597268/233628271-7cdc43b8-a7a2-48ee-a35f-efb941ba0f00.png)

## 6. WSL2 的 cuda toolkit 配置（说明：如果需要使用到 nvcc，即要使用到源码编译，执行 WSL2 的 cuda toolkit 配置，如果只需要进行预编译，请忽略该步骤）

注意：不要使用 sudo apt install nvidia-cuda-toolkit 进行配置，会默认配置一个低版本的 cuda toolkit 。

没有配置 cuda toolkit 之前：

![](https://user-images.githubusercontent.com/105597268/233554064-b9d08cef-d16b-450a-b936-58d2f55dd232.png)

去英伟达的官网：https://developer.nvidia.com/cuda-downloads

下载往期版本：

![](https://user-images.githubusercontent.com/105597268/233554120-fdb2ba64-60b5-4e38-abf5-3b673a3a0a19.png)

下载 cu111 版本（注意， Win10 系统上的显卡驱动的版本可以不用与这里的 cuda 版本一致，比如我下载的 Win10 系统上的显卡驱动版本为 12.1 ，显卡驱动是向下兼容的）：

![](https://user-images.githubusercontent.com/105597268/233554161-b8e2258c-6238-4164-9efb-31fa17fe5dc3.png)

进行选择，下方会出现相应的指令(可以勾选 “runfile(local)” ，操作步骤同理)：

![](https://user-images.githubusercontent.com/105597268/233555068-3f38e1a0-166c-4f54-a351-fa5b725c4413.png)

![](https://user-images.githubusercontent.com/105597268/233769208-ec1d76d0-bbf0-469d-8b7d-9bcb082157ef.png)

打开 Ubuntu-22.04.exe ：输入下面指令：

```
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda-repo-wsl-ubuntu-11-1-local_11.1.0-1_amd64.deb
sudo dpkg -i cuda-repo-wsl-ubuntu-11-1-local_11.1.0-1_amd64.deb
sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub （这条指令可能会随着时间被官网更新，请每次都到官网获取新的指令）
sudo apt-get update
sudo apt-get -y install cuda
```

小知识：在执行了 wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local\_installers/cuda-repo-wsl-ubuntu-11-1-local\_11.1.0-1\_amd64.deb 指令后，Ubuntu-22.04 Linux 子系统的 /home/geoffreyfan (这里是你自己的用户名字) / 路径下会出现相应的安装包，后面重新安装，直接执行 sudo dpkg -i cuda-repo-wsl-ubuntu-11-1-local\_11.1.0-1\_amd64.deb 就可以了，不用再反复下载安装包：

![](https://user-images.githubusercontent.com/105597268/233631320-94aa49d5-1505-47aa-bcc1-41bd711a5542.png)

注意：在 sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub 指令之后，安装的过程中不要勾选 Driver 否则会下载失败：

![](https://user-images.githubusercontent.com/105597268/233576435-74dc4412-daab-40e1-b8d1-0a05501b2c74.png)

在 .bashrc 文件中添加下面两条代码指令，并保存：

```
export LD_LIBRARY_PATH=/usr/local/cuda/lib
export PATH=$PATH:/usr/local/cuda/bin
```

![](https://user-images.githubusercontent.com/105597268/233631575-f1b1fce2-b8ae-4988-a331-10ab88bf9455.png)

安装完了后，输入下面指令进行验证：

```
nvcc -V
```

![](https://user-images.githubusercontent.com/105597268/233561757-ff3fad4f-5b2c-4e8c-9fe9-529dd9c6ca1d.png)

## 7. 有关一些问题的解决思路

### 7.1 解决：nvcc: command not found （首先看自己有没有输入错误，比如将 nvcc 打错成 ncvv 等）

nvcc 是 The main wrapper for the NVIDIA CUDA Compiler suite. Used to compile and link both host and gpu code. ( NVIDIA CUDA 编译器套件的主要包装器,用于编译和链接主机和 gpu 代码)。一般使用 nvcc -V 查看CUDA版本。

输入下面指令，查看 cuda toolkit 的 bin 目录下是否有 nvcc：

```
cd /usr/local/cuda/bin
```

如果存在(如果不存在，请参考WSL2的cuda配置,进行重新配置)：

![](https://user-images.githubusercontent.com/105597268/233568001-f1668e4a-3ccb-43b2-9e39-59a83211244e.png)

直接将 cuda toolkit 的路径加入系统路径（即将下面指令加入 .bashrc 文件）即可：

```
export LD_LIBRARY_PATH=/usr/local/cuda/lib
export PATH=$PATH:/usr/local/cuda/bin
```

![](https://user-images.githubusercontent.com/105597268/233568948-58904e1c-57fc-4cbd-9d0c-3501a8ac92ed.png)

![](https://user-images.githubusercontent.com/105597268/233568987-36b56b0e-d0f0-4d87-9a15-3af83f2b263e.png)

保存：

![](https://user-images.githubusercontent.com/105597268/233569042-fb98c59e-7f87-4fc7-ba22-139bed57145f.png)

退出 Ubuntu-22.04.exe 程序，然后再开启，再次执行 nvcc -V 就可以看到相应 cuda 版本了：

![](https://user-images.githubusercontent.com/105597268/233569202-2c7939eb-f276-4e1e-b9f7-4d28667c1761.png)

解释说明：

Ubuntu-22.04 Linux 里面的 Cuda toolkit api 使用 nvcc -V 显示

Win10 系统里面的 Driver api， 使用 nvidia-smi 显示

如果报错的命令是 RuntimeError ，那就使用 nvcc -V 命令查看是否是版本不匹配。

### 7.2 解决：查看显卡使用情况 nvidia-smi 报错：command not found（首先看自己有没有输入错误，比如将 nvidia-smi 打错成 nvidia-sim 等）

gpu 重启以后，是默认关闭的，在 Ubuntu-22.04.exe 下执行：

```
nvidia-smi -pm 1
lspci | grep NVIDIA
nvidia-smi
```

![](https://user-images.githubusercontent.com/105597268/233572066-987b373a-8448-43d6-bd34-4d1d7e5fb90d.png)

### 7.3 解决：下载完 anaconda 之后仍然报错：conda: command not found

将以下添加到 Ubuntu-22.04 Linux 子系统的 /home/geoffreyfan (这里是你自己的用户名字) / 路径下的 .bashrc 文件里面:

```
PATH=/home/user/anaconda3/bin:$PATH
```

![](https://user-images.githubusercontent.com/105597268/233572324-d52abedb-eda0-4bab-a3c3-789f9c018711.png)

在终端运行 project.py 文件，前面有 (base) 这个标示符就表示成功了：

![](https://user-images.githubusercontent.com/105597268/233631968-16276ceb-6958-4084-af55-6312f4162453.png)

### 7.4 解决：su: Authentication failure

su 命令不能切换 root ，提示 su: Authentication failure，只要你 sudo passwd root 过一次之后，下次再 su 的时候只要输入密码就可以成功登录了。

![](https://user-images.githubusercontent.com/105597268/233572500-53b8151b-a197-49b9-9fa9-eaa0542420a4.png)

### 7.5 解决：系统找不到指定的文件。

![](https://user-images.githubusercontent.com/105597268/233660551-e739fc38-fca8-492c-8980-c43e7b9edc35.png)

系统卸载没有卸载干净，导致重新安装后出现该问题。

第一步，查询当前已安装的系统：

```
wsl.exe --list --all
```

![](https://user-images.githubusercontent.com/105597268/233660852-20fb9a43-19b4-4c10-b108-a4116bd1c9e4.png)

第二步，注销当前注册的系统：

```
wsl.exe --unregister Ubuntu-22.04 (第一步查询出来需要注销的系统名称)
```

第三步,重新启动 unbuntu 系统，系统会重新初始化，效果如下：

![](https://user-images.githubusercontent.com/105597268/233662679-98e8271c-920f-4656-9fb6-7df5eba43370.png)
