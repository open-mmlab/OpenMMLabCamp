# Win 10 WSL 环境配置
姓名：geoffreyfan
## 一、Win10升级最新版本和Win10安装英伟达对linux子系统的显卡驱动

1、window10系统一定要升级到最新版，否则是无法在linux子系统里加载出显卡驱动的，那么就无法进行深度学习！

2、子系统不需要配置显卡。因为它是调用Window10下的显卡驱动进行深度学习的，这就意味着不需要在子系统下载安装显卡驱动！

有人会问：不是安装WSL2的驱动吗？为什么装Windows10上的？实际上，官网文档上标注了，带有WSL2的官方nvidia驱动是整个过程唯一要装的GPU驱动！后面关于linux上的cuda的安装就要求不勾选driver！

![image](https://user-images.githubusercontent.com/105597268/233543981-2ca68668-7db1-432e-a342-11ac0331daf9.png#pic_center)

### 更新Win10系统

微软Microsoft官网下载：https://www.microsoft.com/zh-cn/software-download/windows10。

点击立即更新，会下载一个微软软件，按照提示更新即可，装完需要重启，网速快的话整个过程大约一个小时。（我电脑是需要翻墙才能更新的，不翻墙就无法加载）。PS：查询系统版本方法：Win+R输入winver回车。

![image](https://user-images.githubusercontent.com/105597268/233544719-94c8dafb-f29b-49bd-8378-4f1f2b31a0ab.png)

![image](https://user-images.githubusercontent.com/105597268/233544743-6576de41-5254-4de9-bac1-bea57f2b6822.png)

另一种方式更新windows

![image](https://user-images.githubusercontent.com/105597268/233544762-871cf8c6-32c9-4ea6-a731-b03719274466.png)

![image](https://user-images.githubusercontent.com/105597268/233544776-c3ed805c-1d37-4d84-875a-cb2a6bc1c807.png)

![image](https://user-images.githubusercontent.com/105597268/233544794-95ef3c72-a3de-47cd-b9ed-b1e3928f8ca4.png)

### Win10下安装英伟达对linux子系统的显卡驱动

下载驱动（下载GEFORCE的那个）：http://www.nvidia.com/Download/index.aspx

注意，该是安装Windows驱动，而不是安装Linux驱动，在Windows下安装驱动后，会自动将驱动以libcuda.so的形式集成至WSL2中，因此切勿在WSL Linux中重复安装驱动。

根据自己显卡的型号选择驱动，notebooks代表笔记本，我是台式电脑3080显卡，所以选择GeForce RX 30 Series下的GeForce RX 3080。

![image](https://user-images.githubusercontent.com/105597268/233551395-537e1988-2894-476e-a3f8-c60e4ee5c853.png)

![image](https://user-images.githubusercontent.com/105597268/233550992-457c493f-c194-4deb-b65f-155ddb8dff58.png)

![image](https://user-images.githubusercontent.com/105597268/233545426-3f13f4d3-cab5-40f0-a353-a070de5c2c9b.png)

![image](https://user-images.githubusercontent.com/105597268/233545477-fe21aece-1d40-40d8-a6c0-0f9f78fb0c2b.png)

![image](https://user-images.githubusercontent.com/105597268/233545693-c071b1cd-3de3-4987-98ca-4724c8f82cdc.png)

![image](https://user-images.githubusercontent.com/105597268/233545659-28f634eb-4b41-49d6-aa9c-72a63e7fd13d.png)

![image](https://user-images.githubusercontent.com/105597268/233545719-5ba40012-5d30-42e5-839f-a94eab7cdcb2.png)

![image](https://user-images.githubusercontent.com/105597268/233545847-afb912ce-65d7-47ca-bc73-505f7b2d289f.png)

## 二、WSL2和Ubuntu22.04下载安装

### 下载并安装WSL2 Linux内核包

下载链接：https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi

下载完点安装即可，安装中，不停地next就可以。

![image](https://user-images.githubusercontent.com/105597268/233546108-e97fcec8-0848-48f1-a1b1-76327a5101aa.png)

打开powershell

![image](https://user-images.githubusercontent.com/105597268/233546130-c846904d-f372-40bd-a7a5-7018bbd7cc2c.png)

启动WSL2，在powershell中输入下面指令：

dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart

启用“虚拟机平台”，在powershell中输入下面指令：

dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart

将WSL2设置为默认版本，在powershell中输入下面指令：

wsl --set-default-version 2

### Win10下安装linux子系统

网站地址：https://www.microsoft.com/zh-cn/

搜索ubuntu，选择22.04版本安装

![image](https://user-images.githubusercontent.com/105597268/233546452-184fad0d-b31d-4bd8-b3e2-18b1555bd2aa.png)

![image](https://user-images.githubusercontent.com/105597268/233546466-9e391780-70f0-41d5-8836-0f1ca1db015f.png)

选控制面板的“程序和功能”里边的设置，勾选“Hyper-V”和“适用于Linux的Windows子系统”

![image](https://user-images.githubusercontent.com/105597268/233546795-db997990-28aa-4870-96a5-ea0d9418afdb.png)

![image](https://user-images.githubusercontent.com/105597268/233546804-0086f867-a564-4ad0-81f9-9a4ddc020045.png)

下载完打开Ubuntu2204：

![image](https://user-images.githubusercontent.com/105597268/233546859-45c1e1c6-aa4e-427c-a375-e2170c60363e.png)

![image](https://user-images.githubusercontent.com/105597268/233546867-db689603-384a-432b-82ae-308840e52f8a.png)

### WSL2位置迁移

在 PowerShell 中运行下面命令显示所有发行版的详细信息：

wsl --list --verbose

![image](https://user-images.githubusercontent.com/105597268/233547562-c5e7ae73-d0a2-45c1-85c9-4ca421d029da.png)

终止正在运行的wsl：

wsl --shutdown

将需要迁移的Linux，进行导出：

wsl --export Ubuntu-22.04 E:/export.tar

导出完成之后，就需要将原有的分发进行卸载：

wsl --unregister Ubuntu-22.04

注销完后：

![image](https://user-images.githubusercontent.com/105597268/233547746-f532ffe2-bdd7-416b-82dc-03029c5f1caa.png)

然后将导出的文件放到需要保存的地方，进行导入即可：

wsl --import Ubuntu-22.04 E:\export\ E:\export.tar --version 2

### 在WSL2 root 和普通用户的切换：

在cmd里面输入下面指令，切换到根目录：

ubuntu2204 config --default-user root

输入 passwd 用户名，并设置密码：

![image](https://user-images.githubusercontent.com/105597268/233548076-34145ecb-8b46-49d4-ae87-8295829ae8ce.png)

ubuntu2204 config --default-user geoffreyfan 切换到普通用户

![image](https://user-images.githubusercontent.com/105597268/233548107-857c6f92-a1e2-450a-b77d-4cc6e3345f4e.png)

## 三、VScode远程连接Ubuntu2204:

下载Remote-SSH插件：

![image](https://user-images.githubusercontent.com/105597268/233548620-fc6867e6-5552-469b-a205-936553fa8a16.png)

远程连接：

![image](https://user-images.githubusercontent.com/105597268/233548642-e12a5de7-556d-4bf4-98cf-4b701a7711ff.png)

![image](https://user-images.githubusercontent.com/105597268/233548674-26a8edba-09f7-447f-bbee-410ed0fc9126.png)

关闭远程连接，Powershell输入下面指令（重装系统卸载Ubuntu也需要先输入下面指令）：

wsl --shutdown

![image](https://user-images.githubusercontent.com/105597268/233548708-de0c32f7-3ff2-4088-b37a-ac2aa319478a.png)

点击打开ubuntu2204.exe程序：

![image](https://user-images.githubusercontent.com/105597268/233548951-ce31856b-0b0a-4ab8-a89b-d01260f99f6b.png)

重新进行远程连接：

![image](https://user-images.githubusercontent.com/105597268/233548975-3d24ce7e-f604-4344-b966-047c64a2e5c2.png)

打开ubuntu里面的文件：open Folder, 点击ok：

![image](https://user-images.githubusercontent.com/105597268/233549008-ef26bd5e-aa3b-44f6-9948-570237b166ca.png)

![image](https://user-images.githubusercontent.com/105597268/233549034-0bb03630-8c8b-459c-9e88-05a97bd9ab1d.png)

路径为/home/geoffreyfan/   路径下的文档就被打开了:

![image](https://user-images.githubusercontent.com/105597268/233549085-53076917-cddd-4b3f-820e-43b1cfeb7814.png)

可以通过拖拉windows里面的文件，放到这个路径下面

![image](https://user-images.githubusercontent.com/105597268/233549251-ed9733d4-62bd-40c0-994b-a145b93b7c27.png)

## 四、WSL2的Anaconda 配置

安装anaconda地址： https://www.anaconda.com/

![image](https://user-images.githubusercontent.com/105597268/233549351-576d586e-c210-4713-b515-ed0024d09033.png)

![image](https://user-images.githubusercontent.com/105597268/233549365-c6ed8c3f-bdcd-4970-9ee7-6d3774c3e671.png)

![image](https://user-images.githubusercontent.com/105597268/233549378-f8e88873-b7a4-4bd7-95af-46eba360e837.png)

浏览器下载下完后，拖入到unbuntu里面：

![image](https://user-images.githubusercontent.com/105597268/233549401-22e38e9b-5681-4d22-a544-33b4e126f851.png)

![image](https://user-images.githubusercontent.com/105597268/233549429-2f791c85-1bd3-49ea-b63f-252357ec9a39.png)

输入指令：bash Anaconda3-2023.03-Linux-x86_64.sh安装：

![image](https://user-images.githubusercontent.com/105597268/233549487-95c2061f-df66-4c3c-a48b-8b3a2bc4f39a.png)

安装过程中，刚开始要按回车键很多次（大概一直按住5秒这样），之后两三个地方会让你输入yes/no，全输入yes。有提示输入回车的，就按回车，然后等待1分钟左右即可安装完毕。

安装完成之后，在 .bashrc文件中最后一行加上：PATH=/home/user/anaconda3/bin:$PATH ，并保存

![image](https://user-images.githubusercontent.com/105597268/233549656-4316a952-7954-465b-91d4-8f3b2d240ad8.png)

创建一个python文件，配置anaconda路径下的python解释器，并在终端执行该python文件，如果前面有（base）这个标志，那么说明anaconda配置成功：

![image](https://user-images.githubusercontent.com/105597268/233550247-91b4f05f-2490-4cc0-9b03-8ddf2b4f165f.png)

![image](https://user-images.githubusercontent.com/105597268/233550587-76fdf0a8-5906-4369-83ec-a821be0adef8.png)

用anaconda创建虚拟环境, 并激活该虚拟环境（根据自己的需求来，这里我们配置3.9版本的python环境）：

conda create -n pytorch python=3.9 -y
conda activate pytorch

![image](https://user-images.githubusercontent.com/105597268/233552441-ac0b60af-799f-4811-aad6-5918f7cba70c.png)

配置好虚拟环境后，我们配置anaconda下我们创建的虚拟环境下的解释器：

![image](https://user-images.githubusercontent.com/105597268/233552962-7a322355-9a95-419a-8513-39f7ccaf97df.png)

![image](https://user-images.githubusercontent.com/105597268/233553023-fb83cf7e-d438-40a8-ba5d-691604d0d48b.png)

配置好后，我们在终端运行我们创建的python文件，会发现（base）标志变成了我们的虚拟环境的名称（pytorch）:

![image](https://user-images.githubusercontent.com/105597268/233553518-57757d98-9579-4ce1-88c4-1d4f8a1c7112.png)

## 五、WSL2的cuda配置





















