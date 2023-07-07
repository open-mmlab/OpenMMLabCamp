# Config适配文档

## 适配指南
教程: https://mmengine--1071.org.readthedocs.build/zh_CN/1071/advanced_tutorials/config.html#python
1. 找一个自己感兴趣的算法，把老 config 转换成新 config，并通过验证脚本的校验。反馈转换过程中遇到的问题
2. 基于新的 config 跑通训练、测试、和 demo，并观察 dump 下来的配置文件是否合理
3. 观察新 config 的报错信息是否直观，并反馈不合理的报错信息

## 环境配置
### 创建环境
创建一个新的 python 环境，以 conda 为例
```bash
conda create -n new_config python=3.10 # 3.7 以上其他版本也可以
```
按照官网教程安装 PyTorch
### 安装指定分支的 MMEngine
```bash
git clone -b new_config_type https://github.com/HAOCHENYE/mmengine.git mmengine_new_config
cd mmengine_new_config
# 如果是新环境，没有安装 MMEngine 相关的依赖，需执行
pip install -e .
```
### 源码编译 MMCV
```bash
git clone https://github.com/open-mmlab/mmcv.git
cd mmcv
MMCV_WITH_OPS=1 pip install -e .
```
### 算法库适配
基于 new_config 分支，源码编译算法库
```bash
# mmdet
https://github.com/open-mmlab/mmdetection/tree/new_config

# mmyolo
https://github.com/open-mmlab/mmyolo/tree/new_config

# mmpretrain
https://github.com/open-mmlab/mmpretrain/tree/new_config

# mmpose
https://github.com/open-mmlab/mmpose

# mmrazor
https://github.com/open-mmlab/mmrazor/tree/new_config
```

打样 PR：
https://github.com/open-mmlab/mmdetection/pull/10366
https://github.com/open-mmlab/mmyolo/pull/787
https://github.com/open-mmlab/mmrazor/pull/539
https://github.com/open-mmlab/mmpose/pull/2390
https://github.com/open-mmlab/mmpretrain/pull/1567

## 验证脚本
https://gist.github.com/HAOCHENYE/079fb5125d9fa8ce113bf3da81ae5d95

对比两个 config：

--new: 转换后的 config

--old：老 config

--scope：算法库的 scope

> 验证脚本不一定适用于所有 config，有时候报错信息需要具体分析，可以在群里交流

1. 校验新老两个配置文件
```bash
python config --new ${new_config} --old ${old_config} --scope ${scope}
```
2. 校验转换后所有的配置文件
要求老的配置文件在 configs 目录下，新的配置文件的 {scope}/configs 目录下
```bash
python config --scope ${scope}
```

## read_base 语法更新
全局替换即可

![image](https://github.com/open-mmlab/OpenMMLabCamp/assets/62195058/07d0a047-a43b-49d8-9d88-4c12b0e78387)

## 反馈收集：


## 交流群二维码：

![image](https://github.com/open-mmlab/OpenMMLabCamp/assets/62195058/039f6a5b-a9a0-438f-b401-499b52f658d1)
