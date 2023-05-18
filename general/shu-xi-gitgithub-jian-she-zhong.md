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

上面指令中的 `--global` 指定了配置的层级。Git 中支持3个层级的配置，分别是 system（系统）、global（全局）和 local（本地），靠后的层级会覆盖掉靠前层级的配置。`git config`指令会将配置写入特定的文件中，因而我们也可以通过手动编辑配置文件的方式对 Git 进行配置。在 Linux 系统中，这文件的路径通常是：

* system：/etc/gitconfig（需要 sudo 权限，不推荐）git
* global： \~/.gitconfig (或者 \~/.config/git/config）
* local：正在操作的仓库的 .git 路径下的配置文件, 即 \[REPO\_PATH]/.git/config

**2.3.2 建立仓库**

* **git init**

`git init`指令用于在一个本地的路径下创建和初始化 Git 仓库。该指令会在项目目录下创建`.git`目录，但不会自动关联远程仓库（如 GitHub 上的远程仓库）。

```bash
# 创建本地代码仓库
$ cd ~/my_project/
$ git init
```

* **git clone**

`git clone`指令用将远程代码仓库下载到本地，以创建本地仓库。这是实际开发中更常用的一种创建本地仓库的方式。

```bash
# 从远程仓库创建本地仓库
$ git clone https://github.com/open-mmlab/mmpose.git
```

`git clone`中使用的远程代码库 URL，可以在其 GitHub 首页看到（如图8），其他平台如 GitLab 也类似。

![图8 GitHub 上远程仓库的URL](https://cdn.vansin.top/picgo/segment\_anything/20230518202413.png)

**2.3.3 状态查询**

* **git status**

`git status`指令用于查看当前本地仓库的状态，包括所在的分支、文件状态等。

```bash
# 查看本地仓库状态
$ git status

On branch v2.0_dataset
Your branch is up to date with 'origin/v2.0_dataset'.

Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.md

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   README_CN.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)

        new_file.py
```

在上面的例子中，可以看到返回的信息包括：

* 本地处于 v2.0\_dataset 分支
* 有一个新增文件`new_file.py`处于未被追踪的状态（关于文件状态，可回顾 [2.2.2 文件状态](https://openmmlab.feishu.cn/docs/doccnm6MfPOPfZgnZ569EhA1DIf#tJRr85)）
* **git diff**



`git diff`指令用来比较仓库中的文件在修改先后的差异。常见的用法有

```
# 查看工作区与暂存区的差异（基本用法）
$ git diff

# 查看暂存区与最近一次提交的差异
$ git diff --staged

# 比较两个提交（或分支）的差异
$ git diff <branch-a> <branch-b>
# 例如：
$ git diff master dev

# 比较分支B的最近提交与分支A、B最近的公共提交之间的差异
# 即查看分支B在分支A的基础上加入了哪些内容
$ git diff <branch-a>...<branch-b>
# 例如
$ git diff master...dev
```

**2.3.4 文件操作**

* **git add**

`git add`指令用来将工作区的修改添加到暂存区。可参考本文 2.2 中关于工作区、暂存区和文件状态的介绍。

```
# 添加指定文件的修改到暂存区
$ git add <file-a> <file-b>

# 添加当前目录下的所有修改到暂存区
$ git add .
```

* **git commit**

`git commit`指令用来将所有已添加到暂存区的修改生成一个提交对象，保存进仓库区，并将当前分支的指针（HEAD）移动到该次提交上。

```
# 提交暂存区修改
$ git commit

# 提交暂存区修改，并直接输入commit message
$ git commit -m "Some commit messages"

# 重做上一次提交，通常用于修改 commit message
$ git commit --amend

# 跳过git add，直接将工作区的修改提交
$ git commit -a
```

**2.3.5 分支管理**

* **git branch**

`git branch`指令用来进行分支管理操作，如列出有的分支、创建新分支、删除分支及重命名分支等。

```
# 列出所有的本地分支
$ git branch

# 列出所有的本地和远程分支
$ git branch -a

# 在当前的提交对象上创建一个新分支（但并不自动切换到新分支）
$ git branch <branch-name>

# 重命名分支,若未指定old-branch，则默认重命名当前分支
$ git branch -m [<old-branch>] <new-branch>

# 删除分支
$ git branch -D <branch-name>
```

* **git checkout**

`git checkout`指令主要用于切换分支。

```
# 切换到已有分支
$ git checkout <branch-name>

# 从当前提交创建并切换到新分支
$ git checkout -b <branch-name>
```

此外，`git checkout`指令还用于丢弃工作区中的修改（未提交到暂存区），或从其他提交对象（如其他分支，或特定commit等）中提取文件到当前工作区。

```
# 撤销指定文件的修改
$ git checkout -- <filename>

# 批量撤销(.py文件中的）修改
$ git checkout -- '*.py'

# 从指定分支中提取文件，添加到工作区
$ git checkout <branch-name> <filename>
```

由于`git checkout`的功能较多且分散，在较新版本的 Git 中引入了`git switch`指令，来代替`git checkout`的分支切换功能，可以参考 [Git - git-switch Documentation](https://git-scm.com/docs/git-switch).

* **git merge**

`git merge`指令用来合并分支。关于分支合并的说明，可以参考 [2.2.3](https://openmmlab.feishu.cn/docs/doccnm6MfPOPfZgnZ569EhA1DIf#DexPUT) 部分。

```
# 合并指定分支到当前分支
$ git merge <branch-name>.

# 在合并过程中继续/中止
$ git merge --continue
$ git merge --abort
```



**2.3.6 远程仓库管理与同步**

* **git remote**

`git remote`指令用来管理本地仓库与远程仓库的关联。

```
# 查看关联的远程仓库
$ git remote -v

origin  https://github.com/ly015/mmpose.git (fetch)
origin  https://github.com/ly015/mmpose.git (push)
upstream        https://github.com/open-mmlab/mmpose.git (fetch)
upstream        https://github.com/open-mmlab/mmpose.git (push)
```

在上面的例子中，可以看到已经关联的远程仓库有2个，命名分别是 origin（URL：https://github.com/ly015/mmpose.git）和 upstream（URL：https://github.com/open-mmlab/mmpose.git）。`git remote`除了用于查看关联仓库，还用于进行关联的管理：

```
# 添加关联的远程仓库
# git remote add <name> <url>
$ git remote add zhang3 https://github.com/zhang3/mmpose.git

# 重命名远程仓库
# git remote rename <old> <new>
$ git remote rename zhang3 li4

# 设置远程仓库URL
# git remote set-url <name> <url>
$ git remote set-url li4 https://github.com/li4/mmpose.git

# 删除关联远程仓库
# git remote remove <name>
$ git remote remove li4

```

* **git fetch**

`git fetch`指令用于与一个远程的仓库交互，并且将远程仓库中有而本地仓库中没有的所有信息拉取下来，然后存储在本地仓库中。这个指令不会修改本地工作区、暂存区的内容，也不会修改本地分支的信息。例如，从远程仓库 origin 拉取的 master 分支会在本地存储为 origin/master, 而不会与原有的本地分支 master 相混淆。

```
# 从远程仓库（默认远程仓库为 origin）拉取信息
$ git fetch [<remote-name>]
```

* **git push**

`git push`指令用于将本地仓库的分支推送到远程仓库，即比较本地分支与远程仓库中对应关联的分支，并将差异同步到远程仓库。该指令需要有远程仓库的写权限，因此这通常是需要验证的。

```
# 推送当前分支到默认远程仓库
$ git push

# 推送本地分支到指定远程仓库的指定分支。若 <local-branch-name> 和 <remote-branch-name>
# 之一未指定，则默认两者相同；若两者均未指定，则默认将当前本地分支推送到同名远程分支
$ git push <remote-name> <local-branch-name>:<remote-branch-name>

# 在指定远程仓库创建与本地分支关联的远程分支，并推送。
# 通常用于将本地创建的分支第一次推送到远程仓库
$ git push --set-upstream <branch-name>
```

* **git pull**

`git pull`指令用于拉取远程分支到本地，即比较本地分支与远程仓库中对应关联的分支，并将差异合入本地分支。通常`git pull`指令可以看作是`git fetch`+`git merge`。如同一般的分支合并，如果远程关联分支和本地仓库中的提交历史存在冲突，则需要解决冲突后才能继续合并。

```
# 拉取当前本地分支关联的远程分支
$ git pull

# 拉取指定远程仓库的指定分支到当前本地分支
$ git pull <remote-name> <branch-name>
```

### 2.4 Git 进阶指令

* **git mv**

`git mv`指令用于重命名或移动文件、目录或链接。与`mv`不同的是，`git mv`会自动将这一修改添加到暂存区。

```
# 移动文件或目录
$ git mv <source> <destination>

# 将文件或目录移动到指定路径下
$ git mv <source> ... <destination-directory>
```

* **git rm**

`git rm`指令用于从工作区或暂存区移除文件。`git rm`会自动将这一修改添加到暂存区（即将该文件从已追踪的文件清单中移除），类似`git mv`。如果我们只想把文件从暂存区移除（即让 Git 不再跟踪该文件），但仍保留在工作区，则可以使用`--cached`选项。

```
# 从工作区和暂存区移除文件
$ git rm <filename>
# 仅从暂存区移除文件
$ git rm --cached <filename>
```

* **git stash**

`git stash`指令用来临时保存一些还没有提交的工作，以便在分支上不需要提交未完成工作就可以清理工作目录。例如，我们正在dev分支编辑`run.py`文件，临时需要切换到master分支跑一个测试任务。在切换分支前，需要清理工作区和暂存区，但我们并不希望将未完成的修改提交到仓库，此时可以用`git stash`指令临时贮藏当前的工作，并在完成临时任务后回复这些工作：

```
# 临时贮藏 dev 分支当前的工作
$ git add run.py
$ git stash # or "git stash push"
# 此时，工作区和暂存区会被清理，恢复到最近一次提交的状态

# 切换到 master 分支，完成临时任务
$ git checkout master
$ python run.py

# 切换回 dev 分支，回复之前贮存的工作
$ git checkout dev
$ git stash pop
```

`git stash`会将贮藏的修改存放在一个栈上，因此可以多次使用`git stash`贮藏修改。栈的状态可以用`git stash list`查看，例如。

```
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051 Revert "added file_size"
stash@{2}: WIP on master: 21d80a5 added number to log
```

`git stash`的常见用法汇总如下

```
# 贮藏当前分支的修改
$ git stash [push]

# 显示贮藏栈情况
$ git stash list

# 应用一个的贮藏的工作，即将其恢复到工作区，但不删除栈中的内容
$ git stash apply  # 应用最近一个贮藏
$ git stash apply stash@{2}  # 应用指定的贮藏

# 从栈中弹出最近一个贮藏，并应用
$ git stash pop

```

* **git reset**

`git reset`指令用来执行撤销操作。例如，它可以将当前分支的 HEAD 指针移动到指定的提交上，从而实现撤销到该提交之后的所有提交。根据指令参数，这种撤销可以是只移动 HEAD 指针，而不改变工作区内容（被撤销的提交内容会保留在工作区，变成已修改，未提交的状态）；也可以直接修改工作区的内容。此外，`git reset`还可以用来撤销提交到暂存区的内容。一些例子如下：

```
# 基本用法
$ git reset [<commit>]  # 将 HEAD 指针移动到指定的提交（默认为最近一次提交），并重置暂存区
$ git reset [--mixed]  # 重置 HEAD 指针及暂存区
$ git reset --soft  # 仅重置 HEAD 指针
$ git reset --hard  # 重置 HEAD 指针、暂存区及工作区

# 撤销提交的例子
$ git reset HEAD~2  # 撤销当前分支最近的2次提交，但不改变工作区内容
$ git reset HEAD~2 --hard  # 撤销当前分支最近的2次提交，并将工作区内容回复到撤销后的状态
```

由于`git reset --hard`指令会直接修改工作区，有可能会导致数据丢失（Git 会真正销毁数据的仅有的几个操作之一），所以在这样使用前一定要确保自己知道其后果。

* **git cherry-pick**

`git cherry-pick`指令用来获得在单个提交中引入的变更，然后尝试将作为一c个新的提交引入到你当前分支上。 这通常用来从另一个分支合并单个或几个提交，而不是整个分支。例如，当前有 main 和 dev 两个分支，提交历史如下：

![图9a git cherry-pick 示意（操作前）](https://cdn.vansin.top/picgo/segment\_anything/20230518202604.png)

如果我们只希望将 dev 分支中的 C3 提交合并到 main 分支中，可以使用以下方式：

```
$ git checkout main
$ git cherry-pick C3
```

此时，提交历史会变为：

![图9b git cherry-pick 示意（操作后）](https://cdn.vansin.top/picgo/segment\_anything/20230518202630.png)

`git cherry-pick`指令的常见用法如下：

```
# 合并单个提交到当前分支
$ git cherry-pick <commit>
# 合并两个提交到当前分支
$ git cherry-pick <commit1> <commit2>

# 当合并遇到冲突时，可能需要手动解决，与 merge 中类似
$ git cherry-pick --continue  # 继续 cherry-pick
$ git cherry-pick --abort  # 中止 cherry-pick
```

* git revert

`git revert`指令用来撤销之前的提交。与`git reset`指令移动`HEAD`指针的方式，`git revert`会创建一个与之前提交的变更完全相反的新提交（本质上是一个特殊的 cherry-pick），从而实现撤销：

```
# 撤销单个或多个提交
$ git revert <commit1> [<commit2> ...]

# 同样，当遇到冲突时，解决的方式与 merge 中类似
$ git revert (--continue | --abort)
```

下图中可以更直观地对比`git reset`和`git revert`的区别：

![图10 git revert 与 git reset 对比   左：初始状态    中：执行 git reset C2   右：执行git revert C3](https://cdn.vansin.top/picgo/segment\_anything/20230518202724.png)

* **git rebase**

`git rebase`指令采用“变基”的方式整合不同分支的修改。回顾之前介绍的`git merge`指令，其工作方式是在当前分支创建一个新提交，在其中合并目标分支和当前分支的修改。与之不同，`git rebase`会首先寻找当前分支和目标分支的“分叉点”，并尝试将当前分支在“分叉点“之后的部分”转移“到目标分支的末尾。下图中对比了使用`git rebase`和`git merge`合并分支的不同：

![图11 git rebase 与 git merge 对比：初始状态](https://cdn.vansin.top/picgo/segment\_anything/20230518202757.png)

![图11b git rebase 与 git merge 对比：在 main 分支（当前分支）上合并 dev 分支（目标分支）](https://cdn.vansin.top/picgo/segment\_anything/20230518202815.png)

`$ git checkout main && git merge dev`

![](https://cdn.vansin.top/picgo/segment\_anything/20230518202833.png)

图11c git rebase 与 git merge 对比：将 dev 分支（当前分支）rebase 到 main 分支（目标分支）`$ git checkout dev && git rebase main`在上图中可以看到，`git rebase`和`git merge`都可以实现分支的整合，但`git rebase`可以避免提交历史中出现分叉，保持较为清晰的提交历史。因此在实际项目开发中，通常会在将开发分支合入主分支前，先将其 rebase 到最新的主分支（在 [3.1](https://openmmlab.feishu.cn/docs/doccnm6MfPOPfZgnZ569EhA1DIf#f1RrHh) 部分中可以看到这一步）。`git rebase`指令的常见用法如下：

```
# 将当前分支 rebase 到目标分支 upstream-branch
$ git rebase <upstream-branch>

# 将指定分支 rebase 到目标分支 upstream-branch
$ git rebase <upstream-branch> <branch-name>
$ git rebase --onto <upstream-branch> <branch-name>~1  # 另一种方式

# 使用 -i 标志可以交互式的方式进行 rebase，使操作者可以手动选择对每个 commit 的处理方式。
# 一个常见用法是用这种方式合并当前分支的最近若干个 commit，例如：
$ git rebase -i HEAD~3
```

与 merge 类似，在 rebase 的过程中也可能会遇到冲突，需要手动解决。解决冲突的过程也与 merge 中相似（使用`--continue`，`--abort`等标志）。需要注意的是，由于 rebase 的工作方式是先切换到目标分支，然后依次添加当前分支中的提交，因此在处理冲突时，`HEAD`指向的是目标分支，这一点与 merge 稍有不同（但在实际开发中，通常 merge 时把主分支作为当前分支，而 rebase 时把主分支作为目标分支，所以 `HEAD` 都会指向主分支 master（或 main）分支，所以不至于混淆）。
