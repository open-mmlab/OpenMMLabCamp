# 熟悉 GitHub/Git(建设中)

GitHub是一个基于Git的代码托管平台，它允许用户存储和分享他们的代码库。Git是一种分布式版本控制系统，它可以帮助开发者跟踪他们的代码变化并协同工作。

在使用Git时，您可以创建一个仓库来存储您的代码，然后将代码推送到远程服务器上的GitHub仓库中。这样，其他人就可以访问您的代码，并且您可以轻松地与他们协作开发项目。

除了存储代码之外，GitHub还提供了许多其他功能，例如问题跟踪、合并请求和团队管理工具等。这些功能使得GitHub成为了许多软件开发团队的首选平台。

Git/GitHub 是开源社区贡献的必备技能，本文将详细介绍 OpenMMLab 社区中 GitHub 经常用到的几项功能，Github 是人类开源财富的聚集地，作为社区用户，我们需要充分熟悉 Github 的使用，汲取来自开源社区的能力。

本文将介绍Github和Git命令行的基本操作

* Github
  * Code
  * Issues
  * Pull requests
  * Discussions
* Git 核心知识点
  * 分布式版本管理 Git 介绍
  * 在 Git 中配置账号/邮箱/密码
  * git clone/pull/add/commit/push/checkout 等命令介绍

## 1. 熟悉 Github 页面

在 [mmdetection](https://github.com/open-mmlab/mmdetection) 的github页面有几个重要的功能。

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



## 2. 学习 Git

如本文第1部分所述，GitHub 是基于 Git 的版本库托管网站。那么 Git 究竟是什么，又如何使用呢？ 在这部分中，我们将介绍这一强大工具的基本概念和用法。

### 2.1 Git 是什么

简单地说，Git 是一个版本控制系统，用来记录、管理和恢复对源代码（或任意类应的文件）所做的各种修改。与其他版本管理工具相比，Git 具有以下特点：

* 快速：Git 的大部分操作都在本地进行，而不需要访问远程数据，因而非常快速。
* 安全：Git 中所有数据记录在存储前都会计算校验和，并用校验和来引用。并且，Git 通常的操作只会向数据库中添加数据，而不会删除数据。因此，使用 Git 可以完整地记录版本信息，避免传输过程中的信息缺失或文件损坏，并且通常不会执行不可逆的操作。
* 完全分布式：即各个客户端都保留完整的代码仓库历史记录，并可以指定和若干不同的远端代码仓库进行交互（如 GitHub 上的多个远程仓库）。
* 强力支持非线性开发： Git 中的分支（branch）管理功能，可以支持大量开发者并行开发同一个软件项目。





### 2.2 Git 中的一些基本概念



