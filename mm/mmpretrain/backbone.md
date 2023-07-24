# 骨干网络贡献指南

本教程旨在为用户在 MMPreTrain 上支持骨干网络提供全流程支持, 骨干网络的支持不要求复现训练精度，对齐推理精度即可。

主要分为准备阶段，实现阶段，收尾阶段和最后的发起PR：

## 准备阶段

在该阶段重点完成 原始论文的精读，研究原始代码实现，以及对 MMPreTrain 框架的熟悉

### 1. 原始论文精读

在进行算法复现之前，我们推荐对原始论文进行精读，重点关注以下几部分内容：

- 了解模型结构的细节，我们推荐你带着如下问题去阅读论文
  - 模型整体结构是什么样的
  - 模型结构有哪些 Block 或者 Layer
  - 该模型是否使用了自定义的算子
- 了解模型所使用的的数据集
  - 该论文是否是在 ImageNet 数据集进行测试
  - 该论文是否使用了其他数据集
- 了解模型所用的评测指标是什么
  - 分类常用的指标为 Top-1/5 Accuracy, 目前 MMPreTrain 已支持该指标

### 2. 原始代码研读(如有)

当你已完成论文的研读，建议在对模型有基本了解的基础上，结合原始代码了解模型细节，重点关注一下两个部分

- 模型结构的代码实现
- 推理时的数据处理方式
- 了解模型推理阶段的数据处理策略
  - 推理时使用的图片尺寸，是224，还是256，还是其他自定义尺寸?
  - 使用什么样的数据处理方法（包括 Resize 和 Crop）
  - Rssize 时使用的怎样的差值算法，使用的后端是 opencv 还是 PIL?

### 3. 了解 MMPreTrain 算法库

接下来，请参考以下资料，熟悉 MMPreTrain 算法库

- 结合 MMPreTrain 文档，研究 MMPreTrain 的基本使用方法
- 尝试进行 model zoo 中某个模型的训练和推理

## 实现阶段

1. 开发流程

首先，将 MMPreTrain 代码库 **fork** 一份到自己的 github 账户下，**clone** 自己账户下的 MMPreTrain repo，并 **checkout** 出一个新的分支，在新分支下进行新算法代码的实现。

```
git clone https://github.com/your_github_name/mmpretrain.git
cd mmpretrain/
git checkout dev
git checkout -b add_new_net # 分支名可以任意取，但最高有辨识度
```

2. 定义模型

模型结构主要在 `mmpretrain/models` 下进行定义，其文件结构如下所示：

```{text}
mmpretrain/models/
        ├── __init__.py
        ├── backbones/   # 定义整体的模型结构
        ├── classifiers/ # 分类器高阶函数，组装 backbone, neck, head 在一起
        ├── heads/
        ├── losses/
        ├── necks/
        └── .......
```

假设我们要复现的模型为 NewNet ，一个典型的流程是：

- 在 `mmpretrain/models/backbone` 下新建一个 python 文件 newnet.py
- 在 newnet.py 中定义模型结构，我们推荐依据模型结构，参考对应的经典模型的实现，来进行定义网络，如：
  - resnet.py
  - vision_transformer.py
  - swin_transformer.py
- 在定义过程中，对于一些常见的 Layer 或者 Block，可以优先使用 MMPreTrain 定义的结构（在 `mmpretrain/models/utils/` 文件下）

```
mmpretrain/models/utils/
        ├── __init__.py
        ├── attention.py
        ├── augment/
        ├── channel_shuffle.py
        ├── embed.py
        ├── helpers.py
        ├── inverted_residual.py
        ├── make_divisible.py
        ├── position_encoding.py
        └── se_layer.py
```

当你完成模型的定义后，请在 [`/mmpretrain/models/backbones/__init__.py`](https://github.com/open-mmlab/mmpretrain/blob/main/mmpretrain/models/__init__.py) 中 import 定义的 NewNet，以完成模型的注册。

```python
........
from .newnet import NewNet
__all__ = [
    ....
    'NewNet'
]
```

3. 配置文件

如果对MMPreTrain的配置文件不熟悉，可以阅读[相关文档](https://mmpretrain.readthedocs.io/en/latest/user_guides/config.html)学习 OpenMMLab 的 Config 机制。

**添加新的配置文件时，建议基于已有的算法配置，直接替换model的设置，让其可以运行起来。**

为了能够进行模型的推理，我们需要在 `configs/` 中加入新模型的config文件夹。

```{text}
#当前configs的文件目录如下:
configs/
├── _base_/
│   ├── datasets/            # 数据集相关，找一个与支持模型相近的使用， 常用的有 resnet, ViT, Swin, ConvNext。
│   ├── default_runtime.py   # 默认运行设置，支持推理时不用管  
│   └── schedules/           # 训练优化设置， 支持推理时不用管
├── resnet/
│   ├── README.md
│   ├── resnet50_8xb32_in1k.py
│   └── ......
├── convnext/
│   ├── README.md
│   ├── convnext-base_32xb128_in1k-384px.py
│   └── ............
├── ......
└── ......
```

在 configs 目录下下新建一个新模型的文件夹，在其中定义各个具体尺寸或配置的模型，如 [`configs/riformer/`](https://github.com/open-mmlab/mmpretrain/tree/main/configs/riformer) 所示：

```
configs/convnext/
    ├── README.md             # 模型的整体介绍，模型的可用模型的整理,
    ├── riformer-m36_8xb128_in1k.py 
    ├── riformer-m36_8xb64_in1k-384px.py
    ├── riformer-m48_8xb64_in1k-384px.py
    ├── riformer-m48_8xb64_in1k.py
    ├── c.......
    └── metafile.yml          # 该模型的各个配置的模型信息汇总，方便统一管理
```

我们以 [`configs/riformer/riformer-m48_8xb64_in1k.py`](https://github.com/open-mmlab/mmpretrain/blob/main/configs/riformer/riformer-m48_8xb64_in1k.py) 为例解释其内容：

```python
# _base_ 中直接导入 configs/_base_ 里已经写好的结构。细心的同学会发现，这里是直接复制 poolformer 的内容
_base_ = [
    '../_base_/datasets/imagenet_bs128_poolformer_medium_224.py',
    '../_base_/schedules/imagenet_bs1024_adamw_swin.py',
    '../_base_/default_runtime.py',
]                 
# Model settings 这里定义了推理模型的结构
model = dict(
    type='ImageClassifier',
    backbone=dict(
        type='RIFormer',
        arch='m48',
        drop_path_rate=0.1,
        init_cfg=[
            dict(
                type='TruncNormal',
                layer=['Conv2d', 'Linear'],
                std=.02,
                bias=0.),
            dict(type='Constant', layer=['GroupNorm'], val=1., bias=0.),
        ]),
    neck=dict(type='GlobalAveragePooling'),
    head=dict(
        type='LinearClsHead',
        num_classes=1000,
        in_channels=768,
        loss=dict(type='CrossEntropyLoss', loss_weight=1.0),
    ))
# 下面是一些训练时的配置，在对齐推理精度时可以忽略
# schedule settings
optim_wrapper = dict(
    optimizer=dict(lr=4e-3),
    clip_grad=dict(max_norm=5.0),
)
# NOTE: `auto_scale_lr` is for automatically scaling LR
# based on the actual training batch size.
# base_batch_size = (32 GPUs) x (128 samples per GPU)
auto_scale_lr = dict(base_batch_size=4096)
```

它仿照 PoolFormer 的结构，仅仅只修改了 `model`， 基于已经有的算法配置，将 `model` 替换成新的主干网络是最简便的方式。

4. 权重转换

当你完成模型的配置文件构建时，距离模型的推理对齐，又近了一步。此时，为了获得 MMPreTrain 可以使用的模型，你需要完成以下几步：

- 下载官方提供的推理模型
- 参考 [`tools/convert_models`](https://github.com/open-mmlab/mmpretrain/tree/main/tools/model_converters) 中写xxx_to_mmpretrain.py，实现待复现模型的权重转换脚本，在这个过程中，你需要先使用前面实现的配置文件和模型结构，来启动一次推理任务，进而获得模型参数在 mmpretrain 中的name和shape，方便对官方模型进行转换。
- 在对齐过程中，可以使用 [`.dev_scripts/ckpt_tree.py`](https://github.com/open-mmlab/mmpretrain/blob/main/.dev_scripts/ckpt_tree.py) 帮助你查看模型权重中的键名。

```{text}
tools/convert_models
    ├── efficientnet_to_mmcls.py  # mmpretrain 以前叫 mmcls
    ├── mlpmixer_to_mmcls.py
    ├── mobilenetv2_to_mmcls.py
    ├── publish_model.py
    ├── reparameterize_model.py
    ├── reparameterize_repvgg.py
    ├── repvgg_to_mmcls.py
    ├── shufflenetv2_to_mmcls.py
    ├── torchvision_to_mmcls.py
    ├── twins2mmcls.py
    ├── van2mmcls.py
    └── vgg_to_mmcls.py
```

5. 对齐推理精度

如果你要复现的模型使用的是标准 ImageNet 数据集，常用的推理阶段数据流水线如下：

```
test_pipeline = [
    dict(type='LoadImageFromFile'),
    dict(type='ResizeEdge', scale=256, edge='short'),  # 缩放短边至某一长度
    dict(type='CenterCrop', crop_size=224),  # 从图像中心裁剪某一尺寸图像
    dict(type='PackClsInputs'),
]
```

需要注意几点：

- 缩放短边时的短边尺寸
- CenterCrop 时的图像尺寸
- 缩放时所使用的具体方法，比如官方代码使用 torchvision 的 transform 进行数据处理，则会使用 pillow 进行缩放，此时应当设置 ResizeEdge 参数 backend='pillow'；如果官方代码缩放时指定了缩放算法， 如 bicubic，则还应当设置  ResizeEdge 参数 interpolation='bicubic'。

在完成模型的定义，配置文件构建以及权重转换之后，即可使用tools中的工具进行模型的推理，对齐模型精度

## 收尾阶段

这部分选做，做完上面部分可以发起 PR, Reviewer 会及时反馈。

1. Metafile构建

Metafile 是为 MMPreTrain 统一管理支持的算法，因此，我们要求在复现模型的最好需要为模型撰写metafile，可以查看 [ConvNeXt](https://github.com/open-mmlab/mmpretrain/blob/main/configs/convnext/metafile.yml) 的例子：

主要由两部分构成：

- Collections：一般一篇论文相关的模型会放在一个合集里

  - Name：合集名称，一般为对应算法简称
  - Metadata（可选）：所有模型共用的一些具体信息，在合集中主要包括
    - Training Data：训练所使用的数据集
    - Architecture：模型包含的结构（只列举主要的一些结构即可，名称可以参考 https://paperswithcode.com/methods）
  - Paper：论文信息
    - URL：论文摘要页，一般为 Arxiv 链接
    - Title：论文标题
  - README：与该 metafile 相关联的算法 readme 文件路径
  - Code：
    - Version：代码实现所在的版本，一般为下一次发版的版本
    - URL：主干网络代码链接

- Models：主要包括各个模型信息，一般一个 checkpoint 对应一个模型

  - Name：模型名称，命名规则参见 配置文件命名规则
  - Metadata：模型的一些具体信息，主要包括
    - FLOPs：模型 FLOPs 指标, 开始可空着，后面有工具批量计算
    - Parameters：模型参数数量， 开始可空着，后面有工具批量计算
    - Training Data：如果该模型使用的训练数据集与 Collection 中指定的不一致，则可以在这里覆写
  - In Collection：模型所在合集
  - Results：模型指标信息
    - Dataset：评测该模型使用的数据集
    - Metrics：评测结果，一般包括 Top 1 Accuracy 和 Top 5 Accuracy
    - Task：评测任务，一般为 Image Classification
  - Weights：权重文件下载链接（将权重文件或者权重转换脚本发给 maintainer 即可）
  - Config：模型对应的配置文件路径
  - Converted From（可选）：如果该模型是通过官方模型转权重获得的，需要填写这部分
    - Weight：官方权重下载路径
    - Code：官方代码路径

2. 加入 `model-index.yml` 中

最后，需要将该 metafile 文件的路径添加至根目录下的 [model-index.yml](https://github.com/open-mmlab/mmpretrain/blob/main/model-index.yml) 中

3. 模型数据分析

为了获得所复现算法的各个尺寸模型的FLOPs和 Parameters，我们提供了如下工具：

如果你已经编写了上述 metafile 并加入了 model-index.yml，可以使用 [.dev_scripts/benchmark_regression/1-benchmark_valid.py](https://github.com/open-mmlab/mmpretrain/blob/main/.dev_scripts/benchmark_regression/1-benchmark_valid.py) 批量计算

`python .dev_scripts/benchmark_regression/1-benchmark_valid.py --models xxx --flops`

如果没有加入 metafile，可以使用 [tools/analysis_tools/get_flops.py](https://github.com/open-mmlab/mmpretrain/blob/main/tools/analysis_tools/get_flops.py) 计算某个配置文件的参数：

`python tools/analysis_tools/get_flops.py ${配置文件}`

4. 格式检测单元测试

在完成模型推理和metafile构建之后，请对所有的代码和文档进行格式检测，我们使用pre-commit hooks进行格式检查，可使用如下步骤：

```
# 格式检查
pip install pre-commit 
pre-commit install 
pre-commit run --all-files
```

5. 单元测试

添加相关模型的单元测试, 如果已经配置了相关 metafile，可以直接指定模型名进行测试，直接在 [test_list](https://github.com/open-mmlab/mmpretrain/blob/e80418a424aaefb81c95df458216bb3e9af246c4/tests/test_models/test_models.py#L22) 中添加相关链接。

## 发起PR

恭喜你，你已经完成了所有步骤，最后就是发起PR了！！！

此时将对代码的修改进行commit，并提起pull request(PR)，同时指定相关同学进行code review，根据review意见修改代码。

当 reviewer 同意你的 PR 后，就会合并进 `dev`, 等待后续的新版本发布，一般每一个月都会发布一个版本。

