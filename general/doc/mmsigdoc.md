# MMSIG 文档贡献指南 (建设中)

给本 MMSIG 贡献者教程贡献的步骤如下：

* Fork 仓库
* 编辑 markdown 文档
* 创建 PR (Pull Request)
* OpenMMLab 社区管理员 Review, Merge PR

## 1. Fork Github 仓库

点击进入[https://github.com/open-mmlab/OpenMMLabCamp/tree/mmsig](https://github.com/open-mmlab/OpenMMLabCamp/tree/mmsig) 进入 OpenMMLabCamp 仓库 mmsig 分支，然后点击 Fork。

![image](https://user-images.githubusercontent.com/25839884/233363301-5f6ef5b7-e9c0-4d5a-87ff-7cb95f152658.png)

取消勾选 Copy the main branch only，并点击 Create fork。

![创建新 fork](https://user-images.githubusercontent.com/25839884/233364929-0a0f9037-37b3-426e-87d7-26ae713c4308.png)

这样就把 OpenMMLabCamp 的仓库 Fork 到自己账号下面了，可以看到本教程的演示账号 OpenMMLab-Assistant-004 下面多了一个 `OpenMMLabCamp` 的 仓库。

![image](https://user-images.githubusercontent.com/25839884/233366598-b0885041-400e-4339-9722-6080f99932a2.png)

## 2. 编辑 markdown 文档

我们点击自己账号中 OpenMMLabCamp 仓库，切换到 mmsig 分支。

![image](https://user-images.githubusercontent.com/25839884/233367888-83eb7276-6a09-4dc9-8957-784a5adbc060.png)

在 OpenMMLabCamp 的 mmsig 分支找到需要编辑的对应文档，我们点击进行编辑。

![image](https://user-images.githubusercontent.com/129494131/233515946-f1b854b6-5b6f-4169-b5e6-feb279af49d0.png)

在 **Edit file** 中能够编辑 markdown 格式的文档。

![image](https://user-images.githubusercontent.com/129494131/233516204-b68f2f96-ae3f-47ca-baea-62204f988ba7.png)

点击 **Preview** 能够预览 Markdown 渲染出来的文档。

![image](https://user-images.githubusercontent.com/129494131/233516250-5f56b596-2873-431a-a6dd-701f7b1f4f65.png)

## 3. 创建 PR （Pull Request）

在完成文档的编辑后，我们在底部的 **Commit changes** 中选择 **Create a new branch for this commit and start a pull request.**， 并给分支命名,这里命名为 update\_mmsig\_doc 分支，然后点击 **Propose changes**

![image](https://user-images.githubusercontent.com/129494131/233517211-abfe54a7-bb40-490e-88aa-d68557308bb0.png)

点击 **compare across forks**, 并选择刚刚选择 **OpenMMLab-Assistant-004/OpenMMLabCamp**中的 **update\_mmsig\_doc** 分支 创建 Pull Request 到 **open-mmlab/OpenMMLabCamp**中的 mmsig 分支，编辑好Pull Request 的标题和详细内容后，点击 Create pull request。

![img\_v2\_cef1434b-45f3-4e2b-ac30-421d81194b9g](https://user-images.githubusercontent.com/129494131/233518289-cace14ec-af7b-45f4-a222-37cc40a31020.jpg)

此时在浏览器地址栏能得到此 PR 的链接，可以在 PR 链接中追踪状态，并等待 OpenMMLab 社区管理员对 PR 进行 Review 和 Merge。

## 4. OpenMMLab社区管理员 Review, Merge PR

![img\_v2\_1db926da-0eec-422f-a81d-e906c1e95a5g](https://user-images.githubusercontent.com/129494131/233519021-f626b713-9be0-4d8c-a895-5e566f9427e5.jpg)

如果社区管理员没有直接 merge 而是给出了一下修改建议，请在上文中对应的 **OpenMMLab-Assistant-004/OpenMMLabCamp**中的 **update\_mmsig\_doc** 分支中对应的文档进行修改。

![image](https://user-images.githubusercontent.com/129494131/233520491-6053e294-d571-4490-9a53-a577410b2084.png)

修改 PR 中的内容，不需要重新创建新的分支，只需要在原来的 PR 对应的分支上修改内容，并在修改内容后填写 commit changes 的标题和内容，并选择 **Commit directly to the update\_mmsig\_doc branch.** 并点击 **Commit changes** 。此时在 PR 界面出现了 \[Fix] fix typo 的记录，我们点击 Resolve conversation。

![image](https://user-images.githubusercontent.com/129494131/233520990-5021d377-67f4-470c-8f95-099b1a2c6837.png)

经过和社区管理员的多轮 review 与修改后，PR 就 Merge 了。

![image](https://user-images.githubusercontent.com/129494131/233522209-364cfd95-d0a3-4352-b88b-7831bac627b8.png)
