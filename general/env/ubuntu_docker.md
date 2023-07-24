# ubuntu docker 环境配置
## 1.安装 Docker和 docker compose：
[官方文档](https://docs.docker.com/engine/install/ubuntu/)
1. 安装前准备
```bash
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg
```
Add Docker’s official GPG key:
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
```bash
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

1.安装 Docker Engine
```bash
sudo apt-get update
```
2.安装 Engine, containerd 和 Docker Compose.
```bash
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## 2.安装 NVIDIA Docker
```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt update && sudo apt install -y nvidia-docker2
sudo systemctl restart docker
```
## 3.克隆 mmdetection 仓库并进入该目录：
```bash
git clone https://github.com/open-mmlab/mmdetection.git
cd mmdetection
```

## 4.构建 Docker 镜像：
```bash
sudo docker build -t mmdetection:latest .
```
构建过程可能需要花费一些时间。

## 5.运行 Docker 容器:
运行docker容器有两种方法，一种是docker run 和docker compose up

### Docker Run
Docker Run 是用于启动和运行 Docker 容器的命令。它可以使用 Docker Hub 或本地镜像来创建和运行容器。以下是一些常见的 Docker Run 命令选项：

- -d：后台运行容器。
- -p：将容器的端口映射到主机的端口。
- -v：将主机的目录或文件夹挂载到容器中。
- --name：为容器指定一个名称。
- --shm-size: 用于设置容器的共享内存大小。

例如，以下命令将启动一个名为 mmdetection_container 的 mmdetection 容器，并将容器的端口映射到主机的端口：
```bash
sudo docker run --gpus all -it --shm-size=8g --name mmdetection_container -v /path/to/mmdetection:/mmdetection mmdetection:latest
```
### Docker compose
Docker Compose 是一个工具，用于在单个 Docker 主机上管理多个容器的应用程序。它使用一个 YAML 文件来定义和配置应用程序中的服务、网络和卷等。以下是一些常见的 Docker Compose 命令：

- up：创建并启动应用程序中的所有服务。
- down：停止并删除应用程序中的所有服务。
- build：构建 Docker 镜像。
- ps：列出应用程序中的所有容器。

例如，以下是一个使用 Docker Compose 部署 mmdetection 应用程序的示例 YAML 文件：
```yaml
version: "3"

services:
  mmdet:
    build: ./
    container_name: <kaggle balabala>
    restart: always
    working_dir: <docker内目录>
    volumes:
      - <本地目录>:<docker内目录>
    tty: true
    shm_size: 50gb
    deploy:
      resources:
        reservations:
          devices:
          - driver: nvidia
            device_ids: ['0']
            capabilities: [gpu]
```

旧版本的Dokcer Compose启动命令
```shell
docker-compose up -d
```
新版本的Docker compose 启动命令
```shell
docker compose up -d
```



