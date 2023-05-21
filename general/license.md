# 开源协议学习

## 1. 什么是开源协议

![](https://cdn.vansin.top/picgo/20230521170026.png)



### 版权

#### 什么文件中需标注版权信息

* [ ] Tests 必须
* [ ] Tools 必须
* [ ] Demo 必须
* [ ] Code 必须

#### 根据具体情况如何标注版权信息

1. 文件中的所有代码均由 OpenMMLab 内部同学或社区同学实现

需添加 OpenMMLab 版权声明

```
# Copyright (c) OpenMMLab. All rights reserved.
```

2. 文件中的所有代码均拷自第三方库且无明显修改

需添加第三方库版权声明。注意：无明显修改包括但不限于没有重构代码结构、没有优化代码性能、没有修复代码漏洞。例如，只做了少量的变量名重命名不属于明显修改。

* 如果第三库有提供版权信息，可直接复制

```
# Copyright (c) Facebook, Inc. and its affiliates. All Rights Reserved
# Copied from
```

* 如果没有提供版权信息，可以使用以下模板

```
# Copyright (c) Github URL
# Copied from xxx
```

3. 文件中的所有代码均拷自第三方库且对第三方库有明显修改

添加 OpenMMLab 版权声明，并注明 Modified from

```
// Copyright (c) OpenMMLab. All rights reserved.
// Modified from xxx
```

4. 文件中包含 OpenMMLab 内部同学或社区同学实现的代码和第三方库的代码并无明显修改第三方库代码

Copyright 可以叠加，第三方库的写法参照第 2 点

```
# Copyright (c) OpenMMLab. All rights reserved.
# Copyright (c) Facebook, Inc. and its affiliates. All Rights Reserved

...

# 可以在离第三库最近的地方注明 Copied from xxx
# Copied from xxx
```

5. 文件中包含 OpenMMLab 内部同学或社区同学实现的代码和第三方库的代码且对第三方库代码做了明显修改

有两种情况，第三方库有版权要求、第三方库无版权要求

* 第三方库有版权要求

```
# Copyright (c) OpenMMLab. All rights reserved.
# Copyright (c) Facebook, Inc. and its affiliates. All Rights Reserved

# 可以在离第三库最近的地方注明 Modified from xxx
# Modified from xxx
```

* 第三方库无版权要求

添加 OpenMMLab 版权声明，并在离第三方库最近的地方注明 Modified from xxx

```
# Copyright (c) OpenMMLab. All rights reserved.

# 可以在离第三库最近的地方注明 Modified from xxx
# Modified from xxx
```

* ### 协议

软件的许可方式大致可分为[专属软件](https://zh.wikipedia.org/wiki/%E4%B8%93%E5%B1%9E%E8%BD%AF%E4%BB%B6)（闭源）与[自由开源软件](https://zh.wikipedia.org/wiki/%E8%87%AA%E7%94%B1%E5%BC%80%E6%BA%90%E8%BD%AF%E4%BB%B6)（开源）。其主要区别在授予用户的[权利](https://zh.wikipedia.org/wiki/%E6%AC%8A%E5%88%A9)有所不同。（开源 ≠ 免费≠ 毫无限制地使用）开源授权是电脑软件和其他产品的一种授权类型，此种授权允许其源码、蓝图、或设计基于明文规定或明示条件下，被所有人使用、修改或分享。这允许最终用户和商业公司审查和修改源代码、蓝图或设计，以满足他们自己的定制、好奇心或故障排除需求。开源协议是开源软件生态系统的基础，可以促进软件的协同开发。license有其两面性，既授予权利，同时要求履行义务：声明、开源。

![](https://cdn.vansin.top/picgo/20230521170241.png)

#### ICLA & CCLA

CLA / ICLA 是 Contributor License Agreement 的缩写，是对 license 的法律性质补充，由法务制定，用以明确法律义务；一般分为公司级（CCLA）和个人级别的 ICLA，CCLA 即某公司代表签署 CLA 后可代表该公司所有员工都签署了该 CLA，而个人级别 CLA 只代表个人认可该 CLA,公司签署 CCLA 后，个人贡献仍需签署ICLA。CLA 可根据具体行情况进行适当调节，公司或组织可自行定义：比如阿里做了相应的本土化：[Alibaba Open Source Individual CLA](https://github.com/aliyun/cla)





## 2. 如何选择开源协议

* ### 开源协议种类

目前，国际公认的开源许可证共有[80多种](https://opensource.org/licenses/alphabetical)。它们的共同特征是，都允许用户免费地使用、修改、共享源码，但是都有各自的使用条件。但如果一种开源许可证没有任何使用条件，连保留作者信息都不需要，那么就等同于放弃版权了。这时，软件可以直接声明进入"公共领域"（public domain）。

* ### 选择恰当的协议

一图解释：

![](https://cdn.vansin.top/picgo/20230521170329.png)

[MIT](https://choosealicense.com/licenses/mit/) 最自由，任何人都可以售卖我的软件，甚至可以用我的名字促销。 [BSD](https://choosealicense.com/licenses/bsd-3-clause-clear/) 和 [Apache](https://choosealicense.com/licenses/apache-2.0/) 协议也很自由，跟 MIT 的区别分别是不允许用作者本人名义促销和保护作者版权。[GPL](https://choosealicense.com/licenses/gpl-3.0/) 族最严格，对代码的修改部分也必须是 GPL 的，同时基于 GPL 代码而开发的代码也必须按照 GPL 发布，而 MPL ，也就是 [Mozilla](https://choosealicense.com/licenses/mpl-2.0/) Public License 就温和一些，如果后续开发的代码中添加了新文件，同时新文件中也没有用到原来的代码，那么新文件可以不必继续沿用 MPL 。

总结：严格程度：[GPL](https://choosealicense.com/licenses/gpl-3.0/) v3.0> [Mozilla](https://choosealicense.com/licenses/mpl-2.0/) 2.0> [LGPL ](https://choosealicense.com/licenses/lgpl-3.0/)v3.0> [Apache](https://choosealicense.com/licenses/apache-2.0/) 2.0> [BSD](https://choosealicense.com/licenses/bsd-3-clause-clear/) 3> [MIT](https://choosealicense.com/licenses/mit/) （点击可查看license具体详情）

*   ### license的兼容性



![](https://cdn.vansin.top/picgo/20230521170407.png)

1. 兼容有方向性，宽松协议兼容于严格协议。
2. 典型不兼容场景：Apache 2.0 与 GPL v2不兼容；GPL v2与GPL v3不兼容。具体操作示例



## 3. 如何使用开源协议

### 具体操作示例

1. 在 GitHub.com 上，导航到存储库的主页。
2. 在文件列表上方，使用添加文件下拉菜单，单击创建新文件。

![](https://cdn.vansin.top/picgo/20230521170443.png)

3. 在文件名字段中，输入 _LICENSE_。
4. 在文件名字段的右侧，单击选择许可证模板。

![](https://cdn.vansin.top/picgo/20230521170501.png)

5. 在页面左侧的“向您的项目添加许可证”下，查看可用的许可证，然后从列表中选择许可证：Apache 2.0 。

![](https://cdn.vansin.top/picgo/20230521170515.png)

6. 单击审核并提交。

![](https://cdn.vansin.top/picgo/20230521170534.png)

7. 在页面底部，输入简短、有意义的提交消息，描述您对文件所做的更改。您可以在提交消息中将提交归因于多个作者。有关更多信息，请参阅“[创建具有多个共同作者的提交”](https://docs.github.com/en/articles/creating-a-commit-with-multiple-authors)。

![](https://cdn.vansin.top/picgo/20230521170551.png)

8. 在提交消息字段下方，决定是将提交添加到当前分支还是新分支。如果您当前的分支是默认分支，您应该选择为您的提交创建一个新分支，然后创建一个拉取请求。有关更多信息，请参阅“[创建新的拉取请求”](https://docs.github.com/en/articles/creating-a-pull-request)。

![](https://cdn.vansin.top/picgo/20230521170611.png)

9. 如果您在 GitHub.com 上有多个电子邮件地址与您的帐户相关联，请单击电子邮件地址下拉菜单并选择要用作 Git 作者电子邮件地址的电子邮件地址。此下拉菜单中仅显示经过验证的电子邮件地址。如果您启用了电子邮件地址隐私，则`<username>@users.noreply.github.com`是默认提交作者电子邮件地址。有关更多信息，请参阅“[设置您的提交电子邮件地址”](https://docs.github.com/en/articles/setting-your-commit-email-address)。

![](https://cdn.vansin.top/picgo/20230521170626.png)

10. 单击提交新文件。

![](https://cdn.vansin.top/picgo/20230521170641.png)

ps：如果您正在为现有项目做出贡献或对其进行扩展，那么继续使用该项目的许可证几乎总是最容易的。要查找其许可证，请查找名为`LICENSE`or的文件`COPYING`，然后浏览项目的`README`. 如果找不到许可证，请询问maintainer。

* ### 整段copy

可以在代码目录下放置原始codebase的license文件大多数人将他们的许可证文本放在存储库根目录中名为`LICENSE.txt`（或`LICENSE.md`或`LICENSE.rst`）的文件中；[这是 Hubot 的一个例子](https://github.com/github/hubot/blob/master/LICENSE.md)。

![](https://cdn.vansin.top/picgo/20230521170716.png)

* ### 局部重用代码

可在代码最开始的地方加上license字段

![](https://cdn.vansin.top/picgo/20230521170736.png)



## 4. 日常Q\&A

Q1：基于 MMSegmentation 开发的 SegFormer 使用的 Liscense 是不允许商用的（[https://github.com/NVlabs/SegFormer#license](https://github.com/NVlabs/SegFormer#license)），但是 OpenMMLab 复现后写的是 apache 2.0 （可以商用），所以应该按照哪一种协议呢？

A1：apache2.0没有传染性，兼容有方向性，宽松协议兼容于严格协议，是允许其它项目基于我们的协议进行修改的；与此同时，在支持新模型的时候对开源协议要谨慎，第三方开发者基于OpenMMLab做的开源项目，是可以自由选择的，跟我们apache 2.0的协议不冲突，像nvidia基本上所有的算法开源repo都是禁止商用的，但如果我们参考了这样的开源项目的代码来支持某个算法的话，是不能直接用apache 2.0协议的，除非是我们完全重写了一遍，没有使用他们repo里的代码。



## 5. 参考材料

[https://choosealicense.com/\
https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository\
https://opensource.com/resources/what-open-source\
https://opensource.org/faq\
https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository\
https://dwheeler.com/essays/floss-license-slide.html\
https://www.apache.org/licenses/contributor-agreements.html#clas\
https://www.apache.org/licenses/cla-faq.html#cclas-not-required\
\
](https://choosealicense.com/https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repositoryhttps://opensource.com/resources/what-open-sourcehttps://opensource.org/faqhttps://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repositoryhttps://dwheeler.com/essays/floss-license-slide.htmlhttps://www.apache.org/licenses/contributor-agreements.html#clashttps://www.apache.org/licenses/cla-faq.html#cclas-not-required)

