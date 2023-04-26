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
```bash
sudo docker run --gpus all -it --shm-size=8g --name mmdetection_container -v /path/to/mmdetection:/mmdetection mmdetection:latest
```






