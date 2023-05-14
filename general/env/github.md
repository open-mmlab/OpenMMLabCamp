# Github 云环境(建设中)





如果小伙伴平时有以下的需求和困扰

* 想用 Linux 系统，但不想在自己电脑上装 Linux 系统。
* 也不想在 Windows 上安装 WSL
* Windows Pre-commit 在本地老是过不了。

可以尝试 Github 的 codespaces，Github 中新增了 codespaces 的功能，每个月有 60 小时的免费时长，可以在这个 Linux 环境下熟悉 Linux 环境以及 Web 版 Vscode 的使用及 Debug。[https://github.com/codespaces](https://mmsig.vansin.top/00env/github)

## 1. Github codespaces 介绍

访问 https://github.com/codespaces ，选择 Github 提供的 Blank 模板。我们可以在这个服务器上看源码，Debug。



![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/4ffabfa1-912a-4e01-8bae-006372cebe81)

有四种打开这个 codespaces 的方式

* Open in browser （用浏览器打开）
* Open in Visual Studio Code
* Open in JetBrains Gateway
* Open in JupyterLab

本菜狗推荐使用 Open in browser 和 Open in Visual Studio Code

![](https://cdn.vansin.top/picgo/segment\_anything/20230514195323.png)

###

### 1.1. 安装插件

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/c9142f91-a7cb-49f0-b65e-575ca4f772dc)

## 2. 运行 MMDetection Demo

```shell
sudo apt update
sudo apt-get install libgl1-mesa-glx
```

```shell
pip install openmim
mim install "mmcv>=2.0.0"
git clone https://github.com/open-mmlab/mmdetection.git
# git clone https://gitee.com/open-mmlab/mmdetection.git
cd mmdetection
git checkout tags/v3.0.0
pip install -v -e .
code .
```

```shell
cd path/to/mmdetection
mim download mmdet --config rtmdet-ins_tiny_8xb32-300e_coco --dest .
```

下载将需要几秒钟或更长时间，这取决于你的网络环境。完成后，你会在当前文件夹中发现两个文件 rtmdet\_tiny\_8xb32-300e\_coco.py 和 rtmdet\_tiny\_8xb32-300e\_coco\_20220902\_112414-78e30dcc.pth。

```shell
python demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu
```

## 3. 在 Github Codespaces 中 Debug

```shell
pip install debugpy
```

![image](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/feb3a916-4be0-43b0-b097-71ddd265cf2f)

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

我们通过在Linux系统中的`~/.bashrc` 或者 `~/.zshrc` 文件中添加以下命令。

```shell
alias pyd='python -m debugpy --wait-for-client --listen 5678'
```

```shell
pyd demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu

# 原始命令是 python，只需要将 python 换成 pyd 就能在 vscode 中进行 debug 了。
# python demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu
```
