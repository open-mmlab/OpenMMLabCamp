# 算法库文档贡献指南
本教程由社区 @Lum1104 贡献~

**给算法库文档贡献的步骤如下：**

* Fork 仓库
* 编辑 markdown 文档
* 创建 PR (Pull Request)
* OpenMMLab 社区管理员 Review, Merge PR

## 1. Fork Github 仓库
下面将以 MMDetection 为例，讲解如何为算法库文档做贡献：

1. 点击进入 [https://github.com/open-mmlab/mmdetection](https://github.com/open-mmlab/mmdetection) 进入 MMDetection 仓库，然后点击 Fork。
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/3ee6bcf8-f7c1-404f-9413-83e1b6ac025a)
2. 取消勾选 Copy the main branch only，并点击 Create fork。
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/2affc38c-b638-4ddf-bec4-1d87a8af3fb7)

这样就把 MMDetection 的仓库 Fork 到自己账号下面了，可以看到本教程的演示账号 Lum1104 下面多了一个 MMDetection 的 仓库。
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/20e1f79f-f03c-4bf2-b653-f8ca554c8016)

## 2. 编辑 markdown 文档
1. 首先将代码克隆到本地：
``` shell
git clone git@github.com:{username}/mmdetection.git
```
2. 添加原代码库为上游代码库
``` shell
git remote add upstream git@github.com:open-mmlab/mmdetection
```
3.  检查 remote 是否添加成功，在终端输入 `git remote -v`，下面是正常的输出：
``` shell
origin	git@github.com:{username}/mmdetection.git (fetch)
origin	git@github.com:{username}/mmdetection.git (push)
upstream	git@github.com:open-mmlab/mmdetection (fetch)
upstream	git@github.com:open-mmlab/mmdetection (push)
```
4.  创建开发分支
建议的分支命名规则为 username/pr_name。
``` shell
git checkout -b Lum1104/refactor_contributing_doc
```
5.  拉取领取任务中所提到的分支

我们点击自己账号中的 MMDetection 仓库，可以切换到 dev-3.x 分支。
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/30f5bf08-6da2-4c98-8d79-2ea251e4898f)
``` shell
git pull upstream dev-3.x
```
在后续的开发中，如果本地仓库的 dev-3.x 分支落后于 upstream 的 dev-3.x 分支，我们需要先拉取 upstream 的代码进行同步，再执行上面的命令。
6. 配置 pre-commit

在本地开发环境中，我们使用 pre-commit 来检查代码风格，以确保代码风格的统一。在提交代码，需要先安装 pre-commit（需要在 mmdetection 目录下执行）:
``` shell
pip install -U pre-commit
pre-commit install
```
检查 pre-commit 是否配置成功，并安装 .pre-commit-config.yaml 中的钩子：

**Windows 用户请不要运行对所有文件检查，可能会有问题**
``` shell
pre-commit run --all-files
```
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/5cd141cf-802e-4d22-a7a2-9c8fa29f46c6)
``` note
如果你是中国用户，由于网络原因，可能会出现安装失败的情况，这时可以使用国内源 pre-commit install -c .pre-commit-config-zh-cn.yaml pre-commit run –all-files -c .pre-commit-config-zh-cn.yaml
```

如果安装过程被中断，可以重复执行 `pre-commit run ...` 继续安装。

如果提交的代码不符合代码风格规范，pre-commit 会发出警告，并自动修复部分错误。

如果我们想临时绕开 pre-commit 的检查提交一次代码，可以在 git commit 时加上 --no-verify（需要保证最后推送至远程仓库的代码能够通过 pre-commit 检查）。
``` shell
git commit -m "xxx" --no-verify
```
7. 编辑需要修改的文档

根据具体需要，把文档进行对应修改：
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/e5f7653d-3c90-4e52-99aa-ea6ceb291ed3)

``` note
1. 上传图片到文档里的方法：在 GitHub 中新建一个 issue 然后将需要添加的图片直接拖入。
2. 直接在 GitHub 中编辑文档，然后将图片拖入，就可以自动上传啦~
```

8. 提交到远程仓库
``` shell
git add xxx
git commit -m "[Docs] what you have done."
git push origin Lum1104/refactor_contributing_doc
```
注意如果在 pre-commit 发现了错误他会尝试自动修复，这时候再执行一次上面的流程即可。
## 3. 创建 PR （Pull Request）
当完成 push 到远程仓库后，登录自己的 GitHub 账号就可以发现在 MMDetection 仓库中有自己的 commit 记录了，在 open-mmlab/mmdetection 页面发起 pull request 请求。
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/7eaae4aa-9958-4249-bc06-db1efe20a9fa)
接着按照下图的操作**选择正确的分支进行提交**：
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/d7f25978-aedf-42da-983a-2cd7a90411bb)
在 comment 处根据提示修改 PR 描述，以便于其他开发者更好地理解你的修改：
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/fad4b509-1f69-4fff-aec0-3f5894a53f14)
`PR 标题处可使用 [Docs] 字样标识此处主要修改了文档，[WIP] 字样表示仍在开发中 (work in progress)`
## 4. OpenMMLab社区管理员 Review, Merge PR
如果社区管理员没有直接 merge 而是给出了一下修改建议（Review），请回到自己的仓库中对应的文档进行修改（例如上面则是回到Lum1104/refactor_contributing_doc），直接 push 到自己用户下的远程仓库就会被自动同步到 PR 里面了。
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/5a9b8723-8812-4ace-a9c3-1190da6954c3)
完成以后点击 Resolve conversation 按钮。
经过与社区管理员的多轮 review 和修改后，PR 就被合入（merge）到仓库中了：
![image](https://github.com/Lum1104/OpenMMLabCamp/assets/87774050/78af9d0d-199b-4743-a2ec-9f5e17811d87)
至此本教程完结，恭喜你已经成为一个开源社区贡献者啦！
