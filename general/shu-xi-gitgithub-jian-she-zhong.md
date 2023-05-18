# 熟悉 GitHub/Git(建设中)

GitHub是一个基于Git的代码托管平台，它允许用户存储和分享他们的代码库。Git是一种分布式版本控制系统，它可以帮助开发者跟踪他们的代码变化并协同工作。

在使用Git时，您可以创建一个仓库来存储您的代码，然后将代码推送到远程服务器上的GitHub仓库中。这样，其他人就可以访问您的代码，并且您可以轻松地与他们协作开发项目。

除了存储代码之外，GitHub还提供了许多其他功能，例如问题跟踪、合并请求和团队管理工具等。这些功能使得GitHub成为了许多软件开发团队的首选平台。

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





