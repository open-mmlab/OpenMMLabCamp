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

#### 2.2.1 工作区、暂存区和本地仓库

* 工作区（Working Directory）

当我们在本地创建一个 Git 项目，或者从 GitHub 上 clone 代码到本地后，项目所在的这个目录就是“工作区”。顾名思义，这里就是我们对项目文件进行编辑和使用的地方。如图6所示：

![图6 MMPose 工作区和仓库区](https://cdn.vansin.top/picgo/segment\_anything/20230518201953.png)

* 仓库区 / 本地仓库（Repository）

在项目目录中，会发现一个`.git`隐藏目录，它不是软件代码的一部分，也不属于工作区，而是 Git 的版本仓库。简单来说，这个仓库区才是一个 Git 项目的“本体”，其中包含了所有历史版本的完整信息。而工作区正是对某个特定版本提取出来的内容。我们通常所说的仓库区，除了指`.git`目录这个物理位置，也指一种逻辑状态，即某个修改“记录进 Git 版本仓库”。

* 暂存区（Stage/Index）

暂存区是 Git 中独有的一个概念，它其实是位于`.git`目录中的一个索引文件，记录了下一次提交（commit）时将要存入仓库区的文件列表信息。当我们在工作区进行了一些修改后，并不直接将其提交进版本仓库，而是先通过`git add`指令将改动放入暂存区（即记录将要提交的文件索引）。在若干次`git add`后，再通过`git commit`指令将暂存区的修改提交到仓库区。这样的设计，让使用者可以更灵活地选择每次提交的内容。这里涉及到的 git 指令，将在2.4部分详细介绍。

**2.2.2 文件状态**

结合2.2.1中工作区、暂存区和仓库区的概念，我们接下来介绍 Git 中的3种文件状态：

* 已跟踪（tracked）：通常，Git 会自动跟踪工作区中文件，发现哪些文件经过了编辑。当新增文件时，可以使用`git add`指令将文件设置为被跟踪的状态。
  * 对于一些不希望 Git 自动跟踪的文件（如本地的数据文件或临时文件等），可将其路径或匹配符加入`.gitignore`文件中，Git 便不会自动跟踪这些文件。可以参考 3.2.1 中的内容。
* 已修改（modified）：表示该工作区中的文件被修改，但还没有添加到暂存区。这里的修改包括内容编辑、重命名、移动位置、删除文件等。
* 已暂存（staged）：表示该文件的修改已经提交到暂存区。当使用`git commit`提交修改后，随着工作区版本更新，这些文件的状态也会变回“已跟踪”。

图7概括了文件状态的变化周期。

![图7 Git 文件状态变化周期 （图片来源：Git - Git 是什么?）](https://cdn.vansin.top/picgo/segment\_anything/20230518202043.png)

**2.2.3 分支**

分支（branch）是 Git 用来支持并行开发的机制。一个分支可以简单理解为一系列提交组成的版本历史。通常一个项目会有一个主分支 master（或main）。开发者从 master 分支创建新的分支，进行功能的开发，并在开发完成后将该分支合并回 master 分支。通过这种方式，开发者们可以在多个分支上并行开发，而不会互相干扰。Git 因其独特的数据保存机制和分支模型（详见：[Git - 分支简介](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%AE%80%E4%BB%8B)）可以实现非常快速的分支创建和切换，因而也鼓励在开发流程中频繁使用分支与合并。关于分支的新建、合并与解决冲突新建和合并分支是基于 Git 的开发流程中很常用的操作。例如，当我们要在项目中开发一个功能时，通常会从当前的 master 分支新建一个分支用于开发（关于这里用到的 Git 指令，可以在 [2.3](https://openmmlab.feishu.cn/docs/doccnm6MfPOPfZgnZ569EhA1DIf#cBWtEI) 部分中介绍）：

```sh
# 从远程仓库 origin 拉取最新的 master
git fetch origin
# 从 master 分支新建 dev 分支用于开发
git checkout master
git checkout -b dev
```

在开发过程中，所有的修改都会被提交到 dev 分支。当开发完成后，我们会将 dev 分支合并进 master 分支：

```
# 切换回 master 分支
$ git checkout master

# 将 dev 分支合并到当前（master）分支
$ git merge dev
```

很多时候，在合并分支时会遇到“冲突”，即待合并的两个分支中对同一文件的同一处做了不同修改，使得 Git 无法自动合并这些修改。这时，Git 会暂停分支合并，将包含冲突的文件标识为未合并（unmerged）状态，等待手动解决冲突。此时可以用`git status`指令查看存在冲突的文件：

```
# 例如，在 run.py 文件中存在冲突
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

```

3. Unmerged paths:
4. (use "git add \<file>..." to mark resolution)

```

    both modified:      run.py

no changes added to commit (use "git add" and/or "git commit -a")
```

Git 会在存在冲突的文件中对应位置加入标准的冲突解决标记，方便手动解决冲突，看起来如下：

```
<<<<<<< HEAD:run.py
print('OpenMMLab is an open-source project')
=======
print('OpenMMLab is AWESOME!')
>>>>>>> dev:run.py
```

这里`HEAD`表示当前分支（即 master）所在位置，`=======`上面的部分就是当前分支中的内容，下面的部分则是 dev 分支中的内容。我们可以手动编辑冲突的内容，并删除冲突解决标记 （含有`<<<<<<<`，`>>>>>>>`和`=======`的行）。例如将例子中的这部分代码改为：

```
print('OpenMMLab is an awesome open-source project')
```

手动解决冲突后，需要将修改过的文件添加到暂存区，然后继续分支合并即可：

```
# 添加解决冲突时的修改
$ git add run.py
# 继续分支合并
$ git merge --continue
```

如果在分支合并过程中想要中断合并，并回到合并前的状态，可以使用`git merge --abort`指令。成功合并分支后，会生成一个新的提交（Commit），并将当前分支的指针 HEAD 指向该提交。

### 2.3 Git 基础指令

在这部分，我们介绍一些常用的 Git 指令。Git 的指令通常有着强大而丰富的功能，可以灵活地设置众多参数。这里只给出每个指令最基础和常见的用法，更多用法可以在 Git 官方教程（[Git - 设置与配置](https://git-scm.com/book/zh/v2/%E9%99%84%E5%BD%95-C%3A-Git-%E5%91%BD%E4%BB%A4-%E8%AE%BE%E7%BD%AE%E4%B8%8E%E9%85%8D%E7%BD%AE)）中学习，或者在命令行中查看对应指令的文档，如：

```bash
# 查看 git checkout 指令文档
$ man git-checkout

# 查看 git checkout 指令参数说明
$ git status -h
```

**2.3.1 Git 配置**

* **git config**

```bash
# 设置用户信息
git config --global user.name "John Doe"
git config --global user.email johndoe@example.com

# 设置 Git 自动记录密码（token），从而无需在每次 pull 或 push 时输入
# 注意：信息会以明文存储在本地，需考虑安全性
git config --global credential.helper store
```
