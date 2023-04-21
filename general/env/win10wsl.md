# Win 10 WSL 环境配置
姓名：geoffreyfan
## 一、Win10升级最新版本和Win10安装英伟达对linux子系统的显卡驱动

1、Win10系统一定要升级到最新版，否则是无法在Linux子系统里加载出显卡驱动的，那么就无法进行深度学习！

2、Linux子系统不需要配置显卡。因为它是调用Win10系统下的显卡驱动进行深度学习的，这就意味着不需要在Linux子系统下载安装显卡驱动！

大家可能会有这样的疑问：不是安装WSL2的显卡驱动吗？为什么会装Win10系统上的显卡驱动？其实，官网文档上说明了，带有WSL2的官方NVIDIA驱动是整个过程唯一要装的GPU驱动！所以后面关于Linux上的cuda的安装就要求不勾选Driver！

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233543981-2ca68668-7db1-432e-a342-11ac0331daf9.png"/>
</div>

### 更新Win10系统

微软Microsoft官网下载：https://www.microsoft.com/zh-cn/software-download/windows10。

点击立即更新，会下载一个微软软件，按照提示更新即可，装完需要重启，网速快的话整个过程大约一个小时。（我电脑是需要翻墙才能更新的，不翻墙就无法加载）。

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233544719-94c8dafb-f29b-49bd-8378-4f1f2b31a0ab.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233544743-6576de41-5254-4de9-bac1-bea57f2b6822.png"/>
</div>

另一种方式更新Win10系统：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233544762-871cf8c6-32c9-4ea6-a731-b03719274466.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233544776-c3ed805c-1d37-4d84-875a-cb2a6bc1c807.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233544794-95ef3c72-a3de-47cd-b9ed-b1e3928f8ca4.png"/>
</div>

PS：查询系统版本方法：Win+R输入winver回车：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233577393-ac7da003-9320-4dd5-9d71-34b03a02e148.png"/>
</div>

### Win10系统下安装英伟达对linux子系统的显卡驱动

下载驱动（下载GEFORCE的那个）：http://www.nvidia.com/Download/index.aspx

注意，该是安装Windows驱动，而不是安装Linux驱动，在Windows下安装驱动后，会自动将驱动以libcuda.so的形式集成至WSL2中，因此切勿在WSL Linux中重复安装驱动。

根据自己显卡的型号选择驱动，notebooks代表笔记本，我是台式电脑3080显卡，所以选择GeForce RX 30 Series下的GeForce RX 3080。

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233551395-537e1988-2894-476e-a3f8-c60e4ee5c853.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233550992-457c493f-c194-4deb-b65f-155ddb8dff58.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233545426-3f13f4d3-cab5-40f0-a353-a070de5c2c9b.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233545477-fe21aece-1d40-40d8-a6c0-0f9f78fb0c2b.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233577393-ac7da003-9320-4dd5-9d71-34b03a02e148.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233545659-28f634eb-4b41-49d6-aa9c-72a63e7fd13d.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233545719-5ba40012-5d30-42e5-839f-a94eab7cdcb2.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233545847-afb912ce-65d7-47ca-bc73-505f7b2d289f.png"/>
</div>

在ubuntu exe程序中输入以下指令，验证驱动是否安装成功：

```
nvidia-smi -pm 1

lspci | grep NVIDIA

nvidia-smi
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233559790-1fae3280-77d7-4079-8711-fe9fc58222e9.png"/>
</div>

## 二、WSL2和Ubuntu22.04的下载安装

### 下载并安装WSL2的Linux内核包

下载链接：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

下载完点击安装即可，安装中不停地next就可以。

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546108-e97fcec8-0848-48f1-a1b1-76327a5101aa.png"/>
</div>

打开powershell：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546130-c846904d-f372-40bd-a7a5-7018bbd7cc2c.png"/>
</div>

启动WSL2，在powershell中输入下面指令：

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

启用“虚拟机平台”，在powershell中输入下面指令：

```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

将WSL2设置为默认版本，在powershell中输入下面指令：

```
wsl --set-default-version 2
```

### Win10下安装linux子系统

网站地址：https://www.microsoft.com/zh-cn/

搜索ubuntu，选择22.04版本安装：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546452-184fad0d-b31d-4bd8-b3e2-18b1555bd2aa.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546466-9e391780-70f0-41d5-8836-0f1ca1db015f.png"/>
</div>

选控制面板的“程序和功能”里边的设置，勾选“Hyper-V”和“适用于Linux的Windows子系统”：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546795-db997990-28aa-4870-96a5-ea0d9418afdb.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546804-0086f867-a564-4ad0-81f9-9a4ddc020045.png"/>
</div>

下载完打开Ubuntu2204：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546859-45c1e1c6-aa4e-427c-a375-e2170c60363e.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233546867-db689603-384a-432b-82ae-308840e52f8a.png"/>
</div>

### WSL2的位置迁移

在 PowerShell 中运行下面命令显示所有发行版的详细信息：

```
wsl --list --verbose
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233547562-c5e7ae73-d0a2-45c1-85c9-4ca421d029da.png"/>
</div>

终止正在运行的wsl：

```
wsl --shutdown
```

将需要迁移的Linux，进行导出：

```
wsl --export Ubuntu-22.04 E:/export.tar
```

导出完成之后，就需要将原有的进行卸载：

```
wsl --unregister Ubuntu-22.04
```

注销完后：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233547746-f532ffe2-bdd7-416b-82dc-03029c5f1caa.png"/>
</div>

然后将导出的文件放到需要保存的地方，进行导入即可：

```
wsl --import Ubuntu-22.04 E:\export\ E:\export.tar --version 2
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233589225-937d4217-959c-4728-a69f-1f091563478d.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233589320-f5234c4d-21d9-4999-b12a-ad3d0f796adf.png"/>
</div>

如果后面想要重装Ubuntu系统，删除之前的版本，在执行了 wsl --shutdown 指令后，直接删除 ext4.vhdx 文件即可。

### 在WSL2的root和普通用户的切换：

在cmd里面输入下面指令，切换到根目录：

```
ubuntu2204 config --default-user root
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233590514-561c671a-22e0-4afa-928c-1ced30a3497c.png"/>
</div>

打开ubuntu exe程序，此时就进入根目录了，输入 passwd 用户名，并设置密码：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548076-34145ecb-8b46-49d4-ae87-8295829ae8ce.png"/>
</div>

在cmd里面输入下面指令，切换到普通用户：

```
ubuntu2204 config --default-user geoffreyfan（这里填写你自己的用户名字） 
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233590812-cee2d3a7-fb20-469e-89f5-ddce74645ac1.png"/>
</div>

打开ubuntu exe程序，此时就进入普通用户界面了：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548107-857c6f92-a1e2-450a-b77d-4cc6e3345f4e.png"/>
</div>

## 三、VScode远程连接Ubuntu2204:

下载Remote-SSH插件：


<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548620-fc6867e6-5552-469b-a205-936553fa8a16.png"/>
</div>

远程连接：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548642-e12a5de7-556d-4bf4-98cf-4b701a7711ff.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548674-26a8edba-09f7-447f-bbee-410ed0fc9126.png"/>
</div>

关闭远程连接，Powershell输入下面指令（重装系统卸载Ubuntu也需要先输入下面指令）：

```
wsl --shutdown
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548708-de0c32f7-3ff2-4088-b37a-ac2aa319478a.png"/>
</div>

点击打开ubuntu2204.exe程序：


<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548951-ce31856b-0b0a-4ab8-a89b-d01260f99f6b.png"/>
</div>

重新进行远程连接：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233548975-3d24ce7e-f604-4344-b966-047c64a2e5c2.png"/>
</div>

打开ubuntu里面的文件：open Folder, 点击ok：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233549008-ef26bd5e-aa3b-44f6-9948-570237b166ca.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233549034-0bb03630-8c8b-459c-9e88-05a97bd9ab1d.png"/>
</div>

ubuntu里面/home/geoffreyfan/ 路径下的文档就被打开了:

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233549085-53076917-cddd-4b3f-820e-43b1cfeb7814.png"/>
</div>

可以通过拖拉windows里面的文件，放到这个路径下面：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233627037-0bb11b45-25d6-4ee6-863f-2fbb0125919f.png"/>
</div>

## 四、WSL2的Miniconda配置

安装Miconda地址: https://docs.conda.io/en/latest/miniconda.html

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233582670-1464caf7-a3d7-48f4-849d-f459b45dd6cc.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233583155-f8037347-48ca-4756-80df-28b6948b9c90.png"/>
</div>


浏览器下载下完后，拖入到unbuntu里面：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233628794-950bfcab-645c-44cf-be0b-cd8d488fc9e9.png"/>
</div>

输入指令：bash Miniconda3-latest-Linux-x86_64.sh安装：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233629009-bc48cc05-705d-4956-8d4a-5d37ae670280.png"/>
</div>

安装过程中，刚开始要按回车键很多次（大概一直按住5秒这样），之后两三个地方会让你输入yes/no，全输入yes。有提示输入回车的，就按回车。

安装完成之后，在 .bashrc 文件中最后一行加上：PATH=/home/user/miniconda3/bin:$PATH ，并保存：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233629221-53e51d1b-7b40-45e3-94b2-3e99baff1f53.png"/>
</div>

创建一个project.py文件，配置miniconda路径下的python解释器，并在终端执行该python文件，如果前面有（base）这个标志，那么说明miniconda配置成功：

<img src="https://user-images.githubusercontent.com/105597268/233629718-519b87c7-cc65-4dc9-99fb-f5dba91bafe2.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233629827-418c2569-93ef-4946-a3b5-5bf2ebd15b7a.png"/>
</div>

用miniconda创建虚拟环境, 并激活该虚拟环境（根据自己的需求来，这里我们选择配置3.9版本的python环境）：

```
conda create -n pytorch python=3.9 -y
conda activate pytorch
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233629974-89ad74d5-6a09-4e4b-82dc-add0a8cf0339.png"/>
</div>

配置好虚拟环境后，我们配置miniconda下我们创建的虚拟环境下的解释器：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233630372-da60128f-c77f-4aab-8024-9e6f8f313127.png"/>
</div>

配置好后，我们在终端运行我们创建的python文件，会发现（base）标志变成了我们的虚拟环境的名称（pytorch）:

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233630630-dc8d754a-7928-4193-bda5-20224cdba5f7.png"/>
</div>

## 五、下载torch==1.9.1+cu111 和 torchvision==0.10.1+cu111

如果用以下指令下载很慢：

```
pip install torch==1.9.1+cu110 torchvision==0.10.1+cu110 -f https://download.pytorch.org/whl/torch_stable.html
```

直接手动去官网下载.whl文件，然后拖到ubuntu里面，官网地址：https://download.pytorch.org/whl/torch_stable.html

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233557377-e01c271a-ed83-4e8c-a694-d71e0e6ada99.png"/>
</div>

蓝色小横线消失了，就上传完了：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233556601-ff402f11-185c-4953-8de0-21e26601c3cd.png"/>
</div>

上传完之后，Ubuntu文件中会有两个相应的安装包：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233630823-6e83678f-583b-4161-929c-7bac50eb7ae4.png"/>
</div>

创建一个project.py文件，输入下面的代码，并配置之前我们创建的miniconda的虚拟环境的python的解释器：

```
import torch

import torchvision

device = torch.device('cuda')

print(torch.cuda.is_available())

print(torch.__version__)

print(torchvision.__version__)
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233631119-89a5b439-76ce-4b34-9b34-5e6deaaa58e4.png"/>
</div>

在终端执行以下指令：

```
pip install torch-1.9.1+cu111-cp39-cp39-linux_x86_64.whl

pip install torchvision-0.10.1+cu111-cp39-cp39-linux_x86_64.whl
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233561233-d4b035e7-aa26-46cc-9b53-305a8f981ce0.png"/>
</div>

然后运行我们刚才建立的python文件：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233628149-ed5e3714-53a0-4c97-9ea1-1f9116b93540.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233628271-7cdc43b8-a7a2-48ee-a35f-efb941ba0f00.png"/>
</div>

## 六、WSL2的cuda toolkit配置（说明：如果需要使用到nvcc，即要使用到源码编译，执行WSL2的cuda toolkit配置，如果只需要进行预编译，请忽略该步骤）

注意：不要使用 sudo apt install nvidia-cuda-toolkit 进行配置，会默认配置为一个低版本的 cuda toolkit

没有配置cuda toolkit之前：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233554064-b9d08cef-d16b-450a-b936-58d2f55dd232.png"/>
</div>

去英伟达的官网：https://developer.nvidia.com/cuda-downloads

下载往期版本：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233554120-fdb2ba64-60b5-4e38-abf5-3b673a3a0a19.png"/>
</div>

下载cu111版本（注意，Win10系统上的显卡驱动的版本不用与这里的cuda版本一致，比如我下载的Win10系统上的显卡驱动版本为12.1，显卡驱动是向下兼容的）：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233554161-b8e2258c-6238-4164-9efb-31fa17fe5dc3.png"/>
</div>

进行选择，下方会出现相应的指令：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233555068-3f38e1a0-166c-4f54-a351-fa5b725c4413.png"/>
</div>

打开ubuntu exe：输入下面指令：

```
wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin

sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600

wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda-repo-wsl-ubuntu-11-1-local_11.1.0-1_amd64.deb

sudo dpkg -i cuda-repo-wsl-ubuntu-11-1-local_11.1.0-1_amd64.deb

sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub （这条指令是会变化的，请每次都到官网获取新的指令）

sudo apt-get update

sudo apt-get -y install cuda
```

小知识：在执行了wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda-repo-wsl-ubuntu-11-1-local_11.1.0-1_amd64.deb 指令后，ubuntu里面会出现相应的安装包，后面重新安装，直接sudo dpkg -i cuda-repo-wsl-ubuntu-11-1-local_11.1.0-1_amd64.deb就可以了，不用再反复下载安装包：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233631320-94aa49d5-1505-47aa-bcc1-41bd711a5542.png"/>
</div>

注意：在sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub指令之后，安装的过程中不要勾选driver否则会下载失败：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233576435-74dc4412-daab-40e1-b8d1-0a05501b2c74.png"/>
</div>

在 .bashrc 文件中添加下面两条代码指令，并保存：

```
export LD_LIBRARY_PATH=/usr/local/cuda/lib
export PATH=$PATH:/usr/local/cuda/bin
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233631575-f1b1fce2-b8ae-4988-a331-10ab88bf9455.png"/>
</div>

安装完了后，输入下面指令进行验证：

```
nvcc -V
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233561757-ff3fad4f-5b2c-4e8c-9fe9-529dd9c6ca1d.png"/>
</div>

### 如何删除WSL2的cuda toolkit

首先打开ubuntu 的local路径下的文件：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233632731-bc9da4af-091d-4560-8060-9a6bf21d712b.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233633108-ad2b63e7-cff0-4651-b1fb-f2c2be2a278c.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233633233-055ff0a4-7684-49c2-bdb7-c5b07f2679b7.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233633379-65f78251-a982-477b-9390-e9e6096cb646.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233633471-ec626178-1691-4165-acc3-46ac947bb63a.png"/>
</div>



## 七、有关一些问题的解决思路

### 解决：nvcc: command not found

nvcc 是The main wrapper for the NVIDIA CUDA Compiler suite. Used to compile and link both host and gpu code.(NVIDIA CUDA 编译器套件的主要包装器,用于编译和链接主机和 gpu 代码)。一般使用 nvcc -V 查看CUDA版本。

输入下面指令，查看cuda的bin目录下是否有nvcc：

```
cd /usr/local/cuda/bin
```

如果存在(如果不存在，请参考WSL2的cuda配置,进行重新配置)：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233568001-f1668e4a-3ccb-43b2-9e39-59a83211244e.png"/>
</div>

直接将cuda路径加入系统路径（即将下面指令加入.bashrc文件）即可：

```
export LD_LIBRARY_PATH=/usr/local/cuda/lib

export PATH=$PATH:/usr/local/cuda/bin
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233568948-58904e1c-57fc-4cbd-9d0c-3501a8ac92ed.png"/>
</div>

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233568987-36b56b0e-d0f0-4d87-9a15-3af83f2b263e.png"/>
</div>

保存：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233569042-fb98c59e-7f87-4fc7-ba22-139bed57145f.png"/>
</div>

退出ubuntu exe程序，然后再开启，再次执行nvcc -V 就可以看到相应cuda版本了：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233569202-2c7939eb-f276-4e1e-b9f7-4d28667c1761.png"/>
</div>

解释说明：

CUDA有两个CUDA版本

runtime api 使用nvcc -V显示

driver api， 使用nvidia-smi显示

如果报错的命令是RuntimeError，那就使用nvcc -V命令查看是否是版本不匹配。

### 解决：查看显卡使用情况nvidia-smi报错：command not found

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233571801-24f910d0-3978-4678-b32f-c4ab421ea057.png"/>
</div>

gpu重启以后，是默认关闭的，在root下执行：

```
nvidia-smi -pm 1
lspci | grep NVIDIA
nvidia-msi
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233572066-987b373a-8448-43d6-bd34-4d1d7e5fb90d.png"/>
</div>

### 解决：下载完anaconda之后仍然报错：conda: command not found

将以下添加到.bashrc文件里面:

```
PATH=/home/user/anaconda3/bin:$PATH
```

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233572324-d52abedb-eda0-4bab-a3c3-789f9c018711.png"/>
</div>

在终端运行一个python文件，前面有(base)这个标示符就表示成功了：

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233631968-16276ceb-6958-4084-af55-6312f4162453.png"/>
</div>

### 解决：su: Authentication failure

su命令不能切换root，提示su: Authentication failure，只要你sudo passwd root过一次之后，下次再su的时候只要输入密码就可以成功登录了。

<div align=center>
<img src="https://user-images.githubusercontent.com/105597268/233572500-53b8151b-a197-49b9-9fa9-eaa0542420a4.png"/>
</div>

### 解决：系统找不到指定的文件。

![image](https://user-images.githubusercontent.com/105597268/233660551-e739fc38-fca8-492c-8980-c43e7b9edc35.png)

系统卸载没有卸载干净，导致重新安装后出现该问题。

第一步，查询当前已安装的系统：

wsl.exe --list --all

![image](https://user-images.githubusercontent.com/105597268/233660852-20fb9a43-19b4-4c10-b108-a4116bd1c9e4.png)

 第二步，注销当前注册的系统：

wsl.exe --unregister Ubuntu-22.04 (第一步查询出来需要注销的系统名称)

 第三步,重新启动unbuntu系统，系统会重新初始化，效果如下：
 
 ![image](https://user-images.githubusercontent.com/105597268/233662679-98e8271c-920f-4656-9fb6-7df5eba43370.png)

### 解决：执行sudo apt-key add /var/cuda-repo-wsl-ubuntu-11-1-local/7fa2af80.pub 和 sudo apt-get update 分别报错

Warning: apt-key is deprecated. Manage keyring files in trusted.gpg.d instead (see apt-key(8))

W: file:/var/cuda-repo-wsl-ubuntu-11-1-local/Release.gpg: Key is stored in legacy trusted.gpg keyring (/etc/apt/trusted.gpg), see the DEPRECATION section in apt-key(8) for details.

为了最好地确保 RPM 和 Debian 软件包存储库的安全性和可靠性， NVIDIA 从 2022 年 4 月 27 日开始更新并轮换apt、dnf/yum和zypper软件包管理器使用的签名密钥。

如果不更新存储库签名密钥，则在尝试从 CUDA 存储库访问或安装软件包时，可能会出现软件包管理错误。

要确保继续访问最新的 NVIDIA 软件，请完成以下步骤。

删除过期的签名密钥：

```
sudo apt-key del 7fa2af80

sudo rpm --erase gpg-pubkey-7fa2af80*
```

安装新的 cuda 钥匙圈组件

```
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/3bf863cc.pub

sudo dpkg -i cuda-keyring_1.0-1_all.deb
```





找出/var/ 目录下的 cuda-repo-wsl-ubuntu-11-1-local 文件，并将其删除

在根目录下，采用以下指令将其删除：

```
rm -rf cuda-repo-wsl-ubuntu-11-1-local
```

![image](https://user-images.githubusercontent.com/105597268/233643647-e5c7d1f2-13d2-44d3-be91-f5a7f96ac02a.png)













































