# 熟悉 GitHub/Git(建设中)

Git/GitHub 是开源社区贡献的必备技能，本文将详细介绍 OpenMMLab 社区中 GitHub 经常用到的几项功能，Github 是人类开源财富的聚集地，作为社区用户，我们需要充分熟悉 Github 的使用，汲取来自开源社区的能力。

本文将介绍Github和Git命令行的基本操作

* Github&#x20;
  * Code
  * Issues
  * Pull requests
  * Discussions
* Git 核心知识点
  * 分布式版本管理 Git 介绍
  * 在 Git 中配置账号/邮箱/密码
  * git clone/pull/add/commit/push/checkout 等命令介绍

## 1. Github

在 [mmdetection](https://github.com/open-mmlab/mmdetection) 的github页面有几个重要的功能

* Code
* Issues
* Pull requests
* Discussions

![](https://cdn.vansin.top/picgo/segment\_anything/20230516102953.png)

### 1.1 Code

在 Code 页面，左边为导航的目录栏，右边为点击文件后的详细内容，在 Github 的 Code 中就能比较方便的看源码。

![](https://cdn.vansin.top/picgo/segment\_anything/20230516105557.png)

### 1.2 Issues

如果想要在 OpenMMLab 社区有效的获得来自社区的帮助，我们可以通过提 Issues 的方式获得来自社区开发者和官方开发者的帮助，在 mmdetection 中，提供了以下四个 Issues 的模板。建议用户选择对应的模板新建 Issue 快速的从社区获得帮助。

* Error report(错误报告)
* Feature request(特性请求)
* General questions(通用问题)
* Reimplementation Questions(复现问题)

![](https://cdn.vansin.top/picgo/segment\_anything/20230516110002.png)

### 1.3 Pull requests

这也是想给开源社区贡献力量的同学必须关注的部分。



### 1.4 Discussions



## 2. Git 命令

git 是分布式的版本控制软件

### 2.1 配置账号和邮箱

设置账号和邮箱

```shell
git config --global user.name xxx
git config --global user.email xxx
```

长期存储密码

```shell
git config --global credential.helper store
```

### 2.2 git 概念讲解





