# Github 云环境(建设中)



* Windows Pre-commit 在本地老是过不了。

Github 中新增了 codespaces 的功能，每个月有 120 小时的免费时长，OpenMMLab 的社区贡献者们可以使用这个免费算力参与 OpenMMLab 的贡献。 [https://github.com/codespaces](https://mmsig.vansin.top/00env/github)

## 1. Github codespaces 介绍

访问 https://github.com/codespaces ，选择 Github 提供的 Blank 模板

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/4ffabfa1-912a-4e01-8bae-006372cebe81)

### 安装插件

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

下载将需要几秒钟或更长时间，这取决于你的网络环境。完成后，你会在当前文件夹中发现两个文件 rtmdet_tiny_8xb32-300e_coco.py 和 rtmdet_tiny_8xb32-300e_coco_20220902_112414-78e30dcc.pth。

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

我们通过在Linux系统中的`~/.bashrc` 或者 `~/.zshrc` 文件中添加以下命令

```shell
alias pyd='python -m debugpy --wait-for-client --listen 5678'
```

```shell
pyd demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu

# 原始命令是 python，只需要将 python 换成 pyd 就能在 vscode 中进行 debug 了。
# python demo/image_demo.py demo/demo.jpg rtmdet-ins_tiny_8xb32-300e_coco.py --weights rtmdet-ins_tiny_8xb32-300e_coco_20221130_151727-ec670f7e.pth --device cpu
```

