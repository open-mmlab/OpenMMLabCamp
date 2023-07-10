# MMSIG任务领取指南

本文档详细介绍了如何领取和完成MMSIG任务。文章包括了以下内容：查看任务的方法、任务类型、任务状态、任务难度和任务系列的标签说明；领取任务的步骤和要求，包括添加微信号、领取确认和任务状态更新；完成任务的步骤，包括提交PR和回复PR链接；根据评审意见修改任务的流程；成功合并任务后的状态和奖励。

通过这篇文档可以清楚地了解如何参与MMSIG任务并获得相应的积分奖励。

## 目录
* [查看任务](#1-查看任务)
* [领取任务(添加喵喵微信)](#2-领取任务添加喵喵微信)
* [完成任务](#3-完成任务)
* [根据评审意见修改任务](#4-根据评审意见修改任务)
* [成功合并](#5-成功合并)

## 1 查看任务

点击进入[OpenMMLabCamp仓库的Discussion栏](https://github.com/open-mmlab/OpenMMLabCamp/discussions)下，即可以查看到所有的任务。

在左栏的`Categories`栏下可以看到，可以点击进入不同的算法库。

![Discussion页面](https://github.com/open-mmlab/OpenMMLabCamp/assets/62195058/5a3225e7-7c36-42e6-aeed-d96e69a25cfc)

每个任务都有对应的标签，标签的含义如下：

### 任务类型标签

任务类型标签用于描述不同任务的类型。

| 任务类型  | 标签内容                   |
|-------|------------------------|
| 文档类   | Documentation          |
| 优化类   | Enhancement            |
| 算法复现类 | Algorithm Reproduction |
| 修复类   | Fix                    |
| 其他类   | Other                  |

1. 文档类（Documentation）：指涉及编写或翻译文档、手册、教程等与文档相关的任务。

2. 优化类（Enhancement）：指改进或增强现有功能、性能或用户体验的任务。

3. 算法复现类（Algorithm Reproduction）：指复现或验证特定算法或模型的任务。

4. 修复类（Fix）：指修复现有功能、逻辑或代码中的错误或问题的任务。

5. 其他类（Other）：指无法归类到上述类别的其他任务。

### 任务状态标签

任务状态标签用于跟踪任务的进展和状态。

| 任务状态  | 标签内容                    | 
|-------|-------------------------|
| 未领取   | StageAwaitingAssignment |
| 已领取   | StageAssigned           |
| 已提交PR | StagePR                 |
| 已完成   | StageDone               | 
| 他人完成  | StageOtherDone          |
| 任务废弃  | StageCancelled          |

1. 未领取（StageAwaitingAssignment）：任务尚未被分配给任何人，可以被领取。
2. 已领取（StageAssigned）：任务已被分配给某个人或团队。
3. 已提交PR（StagePR）：任务的工作已经完成并作为Pull Request提交，等待审查和合并。
4. 已完成（StageDone）：任务已经通过审查并成功合并。
5. 他人完成（StageOtherDone）：任务由他人完成。
6. 任务废弃（StageCancelled）：任务被废弃或取消，不再需要进一步的处理。

✅提示：可以使用Label标签来筛选尚未被领取的任务

### 任务难度标签
任务难度标签用于描述任务的难度级别。

| 任务难度 | 标签内容   |
|------|--------|
| 简单   | Easy   |
| 中等   | Medium |
| 困难   | Hard   |

1. 简单（Easy）：指相对容易理解和完成的任务，适合新手或经验较少的开发者。
2. 中等（Medium）：指具有一定挑战性但仍然可行的任务，适合有一定经验的开发者。
3. 困难（Hard）：指具有较高难度或复杂性的任务，需要有深入经验和技能的开发者来完成。

### 任务系列标签
任务系列标签用于描述任务所属的系列。

| 任务系列       | 标签内容      |
|------------|-----------|
| MMSIG      | MMSIG任务   |
| MMCodeCamp | 超级视客营专属任务 |

1. MMSIG：指MMSIG的任务。
2. MMCodeCamp：指超级视客营专属的任务。

🆘注意：只能领取MMSIG的任务，不能领取MMCodeCamp的任务！

可以通过`Sort by`来对任务按指定字段排序，通过`Label`来筛选出自己想要的任务。

![image](https://github.com/open-mmlab/OpenMMLabCamp/assets/62195058/2e44e80d-1f79-454a-933b-f43dc481be9e)

## 2 领取任务(添加喵喵微信)

要领取任务，请点击所需任务的Discussion详情页，并在评论区回复`领取任务`。

![领取任务](https://github.com/open-mmlab/OpenMMLabCamp/assets/62195058/0a23735b-8bd9-4ddb-a274-25efe0472f5c)

请在回复后立即添加小助手喵喵微信（微信号：`openmmlabwx`）。社区管理员将尽快复核领取情况，一旦复核完成，任务状态将变更为`已领取`，同时Discussion正文区将更新领取人的Github ID和领取时间。

领取任务后，请尽量在预计耗时内及时完成任务。如果有任何其他情况，请及时与小助手喵喵沟通。如果没有及时沟通，任务可能会被收回。

## 3 完成任务

完成任务后，贡献者需要向算法库提交PR，并在Discussion的评论区回复PR链接。

具体如何完成任务和提交Pull Request（PR），可以参考[代码贡献指南](https://mmengine.readthedocs.io/zh_CN/latest/notes/contributing.html)，请注意符合[拉取请求规范](https://mmengine.readthedocs.io/zh_CN/latest/notes/contributing.html#id11)。

🆘注意：PR标题必须以[MMSIG]开头

![image](https://github.com/open-mmlab/mmpose/assets/62195058/7d44dfb6-bc81-4360-b5b7-3128f213c352)

社区管理员将对提交的PR进行复核，并将其提交给技术人员进行评审。一旦复核完成，任务状态将变为“已提交PR”。

![完成任务](https://github.com/open-mmlab/OpenMMLabCamp/assets/62195058/dd54bc6b-dc9c-4ed1-b277-293385dec162)

## 4 根据评审意见修改任务

技术人员将尽快对PR完成评审并给出评审意见，如果评审意见中有需要修改的部分，请在进行修改后再次提交PR。

## 5 成功合并

经过技术人员评审通过后，PR将被被允许合并到算法库中。合并成功后，任务状态将变为`已完成`，贡献者将获得相应的积分奖励。