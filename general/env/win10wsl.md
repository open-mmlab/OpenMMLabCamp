# Win 10 WSL 环境配置
姓名：geoffreyfan
## 一、将window10升级到最新版本

1、window10系统一定要升级到最新版，否则是无法在linux子系统里加载出显卡驱动的，那么就无法进行深度学习！

2、子系统不需要配置显卡。因为它是调用window10下的显卡驱动进行深度学习的，这就意味着不需要在子系统下载安装显卡驱动！

有人会问：不是安装WSL2的驱动吗？为什么装Windows10上的？实际上，官网文档上标注了，带有WSL2的官方nvidia驱动是整个过程唯一要装的GPU驱动！后面关于linux上的cuda的安装就要求不勾选driver！

![image](https://user-images.githubusercontent.com/105597268/233543981-2ca68668-7db1-432e-a342-11ac0331daf9.png)

### 更新win系统

微软Microsoft官网下载：https://www.microsoft.com/zh-cn/software-download/windows10。

点击立即更新，会下载一个微软软件，按照提示更新即可，装完需要重启，网速快的话整个过程大约一个小时。（我电脑是需要翻墙才能更新的，不翻墙就无法加载）。PS：查询系统版本方法：Win+R输入winver回车。

![image](https://user-images.githubusercontent.com/105597268/233544719-94c8dafb-f29b-49bd-8378-4f1f2b31a0ab.png)

![image](https://user-images.githubusercontent.com/105597268/233544743-6576de41-5254-4de9-bac1-bea57f2b6822.png)

另一种方式更新windows

![image](https://user-images.githubusercontent.com/105597268/233544762-871cf8c6-32c9-4ea6-a731-b03719274466.png)

![image](https://user-images.githubusercontent.com/105597268/233544776-c3ed805c-1d37-4d84-875a-cb2a6bc1c807.png)

![image](https://user-images.githubusercontent.com/105597268/233544794-95ef3c72-a3de-47cd-b9ed-b1e3928f8ca4.png)

### win下安装英伟达对linux子系统的显卡驱动

下载驱动（下载GEFORCE的那个）：http://www.nvidia.com/Download/index.aspx

注意，该是安装Windows驱动，而不是安装Linux驱动，在Windows下安装驱动后，会自动将驱动以libcuda.so的形式集成至WSL2中，因此切勿在WSL Linux中重复安装驱动。


![image](https://user-images.githubusercontent.com/105597268/233545426-3f13f4d3-cab5-40f0-a353-a070de5c2c9b.png)

![image](https://user-images.githubusercontent.com/105597268/233545477-fe21aece-1d40-40d8-a6c0-0f9f78fb0c2b.png)

![image](https://user-images.githubusercontent.com/105597268/233545693-c071b1cd-3de3-4987-98ca-4724c8f82cdc.png)

![image](https://user-images.githubusercontent.com/105597268/233545659-28f634eb-4b41-49d6-aa9c-72a63e7fd13d.png)

![image](https://user-images.githubusercontent.com/105597268/233545719-5ba40012-5d30-42e5-839f-a94eab7cdcb2.png)

![image](https://user-images.githubusercontent.com/105597268/233545847-afb912ce-65d7-47ca-bc73-505f7b2d289f.png)

## WSL2下载安装






