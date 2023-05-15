# Github CodeSpace 云环境(建设中)

如果小伙伴平时有以下的需求和困扰：

* 平时使用 Windows 进行开发，但是提 PR 时 Windows 的命令行 pre-commit 代码审查始终过不去，且不想安装 WSL。
* 又或者想有一个网络状况比较好的 Linux 环境。

可以尝试 Github 的 codespaces，Github 中新增了 codespaces 的功能，每个月有 120-core 时的免费时长，可以在这个 Linux 环境下熟悉 Linux 环境以及 Web 版 Vscode 的使用及 Debug。[https://github.com/codespaces](https://mmsig.vansin.top/00env/github)。

## 1. Github codespaces 介绍

访问 https://github.com/codespaces ，选择 Github 提供的 Blank 模板。我们可以在这个服务器上看源码，Debug。

![](https://github.com/open-mmlab/OpenMMLabCamp/assets/25839884/4ffabfa1-912a-4e01-8bae-006372cebe81)

有四种打开这个 codespaces 的方式

* Open in browser （使用浏览器打开）
* Open in Visual Studio Code（使用VSCODE打开）
* Open in JetBrains Gateway(使用 JetBrains Gateway 打开)
* Open in JupyterLab (使用 JupyterLab 打开)

本菜狗推荐使用 Open in browser 和 Open in Visual Studio Code

![](https://cdn.vansin.top/picgo/segment\_anything/20230514195323.png)

### 1.1 切换主题暗黑色

![切换主题](https://cdn.vansin.top/picgo/segment\_anything/20230515210023.png)

![选择深色(Visual Studio)](https://cdn.vansin.top/picgo/segment\_anything/20230515210245.png)



### 1.2 Codespaces 预装的环境

```bash
@OpenMMLab-Assistant-004 ➜ /workspaces/codespaces-blank $ pip list
Package                  Version
------------------------ ----------
aiofiles                 22.1.0
aiosqlite                0.19.0
anyio                    3.6.2
argon2-cffi              21.3.0
argon2-cffi-bindings     21.2.0
arrow                    1.2.3
asttokens                2.2.1
attrs                    23.1.0
Babel                    2.12.1
backcall                 0.2.0
beautifulsoup4           4.12.2
bleach                   6.0.0
certifi                  2022.12.7
cffi                     1.15.1
charset-normalizer       3.1.0
cmake                    3.26.3
colorama                 0.4.6
comm                     0.1.3
contourpy                1.0.7
cryptography             40.0.2
cycler                   0.11.0
debugpy                  1.6.7
decorator                5.1.1
defusedxml               0.7.1
executing                1.2.0
fastjsonschema           2.16.3
filelock                 3.12.0
fonttools                4.39.3
fqdn                     1.5.1
gitdb                    4.0.10
GitPython                3.1.31
idna                     3.4
ipykernel                6.22.0
ipython                  8.12.0
ipython-genutils         0.2.0
isoduration              20.11.0
jedi                     0.18.2
Jinja2                   3.1.2
joblib                   1.2.0
json5                    0.9.11
jsonpointer              2.3
jsonschema               4.17.3
jupyter_client           8.2.0
jupyter_core             5.3.0
jupyter-events           0.6.3
jupyter_server           2.5.0
jupyter_server_fileid    0.9.0
jupyter-server-mathjax   0.2.6
jupyter_server_terminals 0.4.4
jupyter_server_ydoc      0.8.0
jupyter-ydoc             0.2.4
jupyterlab               3.6.3
jupyterlab-git           0.41.0
jupyterlab-pygments      0.2.2
jupyterlab_server        2.22.1
kiwisolver               1.4.4
lit                      16.0.2
MarkupSafe               2.1.2
matplotlib               3.7.1
matplotlib-inline        0.1.6
mistune                  2.0.5
mpmath                   1.3.0
nbclassic                0.5.5
nbclient                 0.7.4
nbconvert                7.3.1
nbdime                   3.2.0
nbformat                 5.8.0
nest-asyncio             1.5.6
networkx                 3.1
notebook                 6.5.4
notebook_shim            0.2.3
numpy                    1.24.3
nvidia-cublas-cu11       11.10.3.66
nvidia-cuda-cupti-cu11   11.7.101
nvidia-cuda-nvrtc-cu11   11.7.99
nvidia-cuda-runtime-cu11 11.7.99
nvidia-cudnn-cu11        8.5.0.96
nvidia-cufft-cu11        10.9.0.58
nvidia-curand-cu11       10.2.10.91
nvidia-cusolver-cu11     11.4.0.1
nvidia-cusparse-cu11     11.7.4.91
nvidia-nccl-cu11         2.14.3
nvidia-nvtx-cu11         11.7.91
packaging                23.1
pandas                   2.0.1
pandocfilters            1.5.0
parso                    0.8.3
pexpect                  4.8.0
pickleshare              0.7.5
Pillow                   9.5.0
pip                      23.1.1
platformdirs             3.3.0
plotly                   5.14.1
prometheus-client        0.16.0
prompt-toolkit           3.0.38
psutil                   5.9.5
ptyprocess               0.7.0
pure-eval                0.2.2
pycparser                2.21
Pygments                 2.15.1
pyparsing                3.0.9
pyrsistent               0.19.3
python-dateutil          2.8.2
python-json-logger       2.0.7
pytz                     2023.3
PyYAML                   6.0
pyzmq                    25.0.2
requests                 2.28.2
rfc3339-validator        0.1.4
rfc3986-validator        0.1.1
scikit-learn             1.2.2
scipy                    1.10.1
seaborn                  0.12.2
Send2Trash               1.8.0
setuptools               67.7.2
six                      1.16.0
smmap                    5.0.0
sniffio                  1.3.0
soupsieve                2.4.1
stack-data               0.6.2
sympy                    1.11.1
tenacity                 8.2.2
terminado                0.17.1
threadpoolctl            3.1.0
tinycss2                 1.2.1
tomli                    2.0.1
torch                    2.0.0
tornado                  6.3.1
traitlets                5.9.0
triton                   2.0.0
typing_extensions        4.5.0
tzdata                   2023.3
uri-template             1.2.0
urllib3                  1.26.15
wcwidth                  0.2.6
webcolors                1.13
webencodings             0.5.1
websocket-client         1.5.1
wheel                    0.40.0
y-py                     0.5.9
ypy-websocket            0.8.2


```



### 1.3. 在浏览器端安装插件

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

## 4. pre-commit 运行代码审查

我们在 Windows 环境 powershell 上经常报错，
