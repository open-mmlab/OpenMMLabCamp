# 如何为 MMDeploy 提供模型支持
在 MMDeploy 中添加新模型支持主要包括两个部分：模型转换支持和模型推理支持。本文将重点介绍如何为 MMDeploy 提供模型支持，并以 MMPose 中的 YoloX-Pose 作为示例进行讲解。

提供模型支持的一般流程如下图所示，分别是模型转换支持，模型推理支持以及结果验证。模型转换支持指的是将模型正确的转换为后端格式，主要包括部署配置文件的编写以及函数重写；模型推理支持指的是在推理后端运行转换得到的模型，主要包括模型调用以及结果后处理；结果验证的目的是确保转换得到的模型能够正确运行，这部分包括了推理结果的可视化对比以及精度测试。下面我们将详细介绍上述流程。

![flowchart](https://github.com/open-mmlab/OpenMMLabCamp/assets/110151316/591a5643-7e25-4480-b794-cca74041fc41)


## 模型转换支持
模型转换是将 PyTorch格式模型转换为目标推理后端格式的过程。不同深度学习框架采用不同的格式对神经网络模型进行表示，例如 ONNXRuntime 使用 `onnx` 格式， TensorRT 使用一种引擎文件格式。在 mmdeploy 中，我们提供了`tools/deploy.py`脚本用于进行快速、便捷的模型转换。以 MMPretrain 中的 ResNet 为例，其转换为 TensorRT 模型的命令如下所示。关于该转换脚本的更多参数请查看[这里](https://mmdeploy.readthedocs.io/zh_CN/latest/02-how-to-run/convert_model.html)。
```bash
python tools/deploy.py \
configs/mmpretrain/classification_tensorrt_dynamic-224x224-224x224.py \  # 模型部署配置文件
../mmpretrain/configs/resnet/resnet18_8xb32_in1k.py \   # 模型文件
../mmpretrain/checkpoints/resnet18_8xb32_in1k_20210831-fbbb1da6.pth \  # PyTorch模型路径
../mmpretrain/demo/demo.JPEG \  # 用于转换的模型输入图片
--device cuda:0 \  # 运行的设备
--work-dir ./work_dir/mmpretrain/resnet \  # 转换结果和日志的保存路径
--test-img ../mmpretrain/demo/dog.jpg # 用于测试的图片
--dump-info  # 是否输出 SDK 推理所需的信息
```
如果转换成功，你可以在对应的 `work-dir` 目录中找到模型转换后的输出结果，如转换后的模型文件、测试图片可视化结果等。

实际部署过程中，需要注意并非所有模型都能被 mmdeploy 完美支持。以 MMPose 中的 YoloX-Pose 为例，直接执行上述命令会报错，原因在于该模型同时涵盖了目标检测与关键点检测两个子任务，当前版本的 mmdeploy 并未完全支持这种类型的模型转换和推理。以下将以 YoloX-Pose 模型为例，分别从部署配置文件、函数重写、后端模型支持以及添加单元测试等方面阐述为 mmdeploy 提供模型支持的一般流程。

### 配置文件
openmmlab系列算法库采用模块化、可继承的配置文件管理模型定义和训练所需的所有参数，方便用户构建对比实验。MMDeploy 也采用部署配置文件来管理模型部署相关的各种参数设置，并为各算法库提供一系列预定义的[部署配置文件](https://github.com/open-mmlab/mmdeploy/tree/main/configs)。用户在支持新模型的时候，通常可以使用已有的部署配置文件，对于特殊的模型可添加新的配置文件。在本教程中，由于 YOLOX-Pose 模型的输出和后处理不同于已支持的关键点检测模型，因而需要添加新的配置文件。以ONNXRuntime 后端的部署为例, 我们创建部署配置文件`configs/mmpose/pose-detection_yolox-pose_onnxruntime_dynamic.py`，设定父级配置文件`pose-detection_static.py`和`../_base_/backends/onnxruntime.py`。YOLOX-Pose模型共有两个输出（检测框和关键点），我们在`onnx_config`配置中设定其`output_names`字段。另外，为了支持动态 batch 的推理，我们还需要设置其`dynamic_axes`字段。
```python
_base_ = ['./pose-detection_static.py', '../_base_/backends/onnxruntime.py']

onnx_config = dict(
    # 设置输出名，对应检测框和关键点
    output_names=['dets', 'keypoints'],
    # 设置动态维度以支持动态batch推理
    dynamic_axes={
        'input': {
            0: 'batch',
        },
        'dets': {
            0: 'batch',
        },
        'keypoints': {
            0: 'batch'
        }
    })
```
YOLOX-Pose模型中需要设定检测后处理参数，我们在代码库配置`codebase_config`中添加`post_processing`字段，设定`Non-Maximum Suppression(NMS)`相关参数。
```python
codebase_config = dict(
    # 检测后处理参数
    post_processing=dict(
        score_threshold=0.05,
        iou_threshold=0.5,
        max_output_boxes_per_class=200,
        pre_top_k=5000,
        keep_top_k=100,
        background_label_id=-1,
    ))
```
至此，我们完成了 YoloX-Pose 在 ONNXRuntime 后端的部署配置文件，部署配置文件中更多参数设置可参考[如何编写部署配置文件](https://mmdeploy.readthedocs.io/zh_CN/latest/02-how-to-run/write_config.html#)。我们可以通过运行如下脚本查看部署配置文件完整内容：
```python
import mmengine

cfg_path = "configs/mmpose/pose-detection_yolox-pose_onnxruntime_dynamic.py"
cfg = mmengine.Config.fromfile(cfg_path)
print(cfg.pretty_text)

#输出配置文件内容
"""
onnx_config = dict(
    type='onnx',
    export_params=True,
    keep_initializers_as_inputs=False,
    opset_version=11,
    save_file='end2end.onnx',
    input_names=['input'],
    output_names=['dets', 'keypoints'],
    input_shape=None,
    optimize=True,
    dynamic_axes=dict(
        input=dict({0: 'batch'}),
        dets=dict({0: 'batch'}),
        keypoints=dict({0: 'batch'})))
codebase_config = dict(
    type='mmpose',
    task='PoseDetection',
    post_processing=dict(
        score_threshold=0.05,
        iou_threshold=0.5,
        max_output_boxes_per_class=200,
        pre_top_k=5000,
        keep_top_k=100,
        background_label_id=-1))
backend_config = dict(type='onnxruntime')
"""
```

### 函数重写
在进行`torch2onnx`的模型转换时，可能会由于自定义算子不支持、模型`forward`中包含切断计算图trace操作等原因导致无法正常运行。为此，mmdeploy设计了重写机制，将训练代码和部署代码解耦，在不侵入算法库代码的情况下实现后端感知的 ONNX 模型转换。有了重写机制，我们只需找到模型在forward时无法导出的函数，然后对其进行重写。以 YoloX-Pose 为例，直接执行模型转换脚本`tools/deploy.py`，则会遇到以下报错：
```shell
RuntimeError: ONNX export failed: Couldn't export Python operator NMSop
Defined at:
/anaconda3/envs/torch/lib/python3.7/site-packages/mmcv/ops/nms.py(128): nms
    ...
/workspace/mmpose/projects/yolox-pose/models/yolox_pose_head.py(260): predict_by_feat
```
通过观察报错信息，我们可以发现问题出在 MMPose 中 `YOLOXPoseHead` 的 `predict_by_feat` 方法，其中 `mmcv.ops.batched_nms` 无法正常导出至 onnx。另外，当前 `predict_by_feat` 的返回值类型为 MMEngine 中的 `InstanceData` 数据类型，而不是导出 onnx 所支持的 `Tensor` 或 `Tuple[Tensor]` 类型。针对这两点问题，我们需要在 mmdeploy 中对 `YOLOXPoseHead.predict_by_feat` 方法进行重写。为了方便控制重写代码生效范围和代码查阅，mmdeploy将重写代码在`mmdeploy/codebase`目录下进行统一管理。我们可以在 `mmdeploy/codebase/mmpose/models/heads` 目录下新建 `yolox_pose_head.py` 模块保存重写代码。此外，为了使重写代码生效，我们还需在该目录下的 `__init__.py` 文件中导入该模块。

MMDeploy 支持使用装饰器注册重写函数，我们使用函数重写装饰器`FUNCTION_REWRITER`对 `YOLOXPoseHead.predict_by_feat` 进行重写。在函数重写装饰器中，`func_name` 表示需要重写的完整函数名，`backend` 可指定该重写所生效的推理后端。通过为不同`backend`添加重写函数，就可实现前文提到的*后端感知的 ONNX 模型转换*。重写函数的参数应与原函数保持一致，特殊情况下也可增删参数。针对前述的第一个问题，即 ONNX 不支持导出 `mmcv.ops.batched_nms`，MMDeploy 提供了相应的函数 `multiclass_nms`，并在内部为不同后端添加了对应重写函数。针对第二个问题，即函数返回值是非 `Tensor` 或 `Tuple[Tensor]` 类型，我们在重写函数中遵循原函数的处理逻辑，但全部操作都在`Tensor`数据类型进行。通常我们会返回带batch维度的`Tensor`以保证在后端可进行多batch推理功能。对`YOLOXPoseHead.predict_by_feat`的重写函数关键代码如下所示，完整的重写代码可以参考[PR#2184](https://github.com/open-mmlab/mmdeploy/pull/2184)。
```python
@FUNCTION_REWRITER.register_rewriter(func_name='models.yolox_pose_head.'
                                     'YOLOXPoseHead.predict_by_feat'，backend='default')
def yolox_pose_head__predict_by_feat(
    self,
    cls_scores: List[Tensor],
    bbox_preds: List[Tensor],
    objectnesses: Optional[List[Tensor]] = None,
    kpt_preds: Optional[List[Tensor]] = None,
    vis_preds: Optional[List[Tensor]] = None,
    batch_img_metas: Optional[List[dict]] = None,
    cfg: Optional[ConfigDict] = None,
    rescale: bool = True,
    with_nms: bool = True) -> Tuple[Tensor]:
    
    ...
    
    # 调用 mmdeploy 提供的 mmdeploy.mmcv.ops.nms.multiclass_nms 以正常导出onnx模型
    _, _, nms_indices = multiclass_nms(
        bboxes,
        scores,
        max_output_boxes_per_class,
        iou_threshold,
        score_threshold,
        pre_top_k=pre_top_k,
        keep_top_k=keep_top_k,
        output_index=True)
    
    # 根据 nms_indices 获取对应的检测结果
    batch_inds = torch.arange(num_imgs, device=scores.device).view(-1, 1)
    dets = torch.cat([bboxes, scores], dim=2)
    dets = dets[batch_inds, nms_indices, ...]
    pred_kpts = pred_kpts[batch_inds, nms_indices, ...]
        
    # 注意此处返回列表中的变量要与上面提到的 onnx_config 中的 output_names 字段对应
    return dets, pred_kpts
```
添加重写函数之后，我们可以完成模型转换，运行`tools/deploy.py`转换脚本可得到`end2end.onnx`模型，但在模型推理时脚本仍然会报错。这是因为我们还没有为后端模型推理提供支持。接下来，我们将介绍如何提供后端推理支持。
```bash
python tools/deploy.py \
configs/mmpose/pose-detection_yolox-pose_onnxruntime_dynamic.py \
../mmpose/projects/yolox-pose/configs/yolox-pose_s_8xb32-300e_coco.py \
../mmdeploy_checkpoints/mmpose/yolox-pose_s_8xb32-300e_coco-9f5e3924_20230321.pth \
../mmpose/tests/data/coco/000000000785.jpg \
--work-dir work_dir
```

## 模型推理支持
### 推理支持
为了验证转换后模型的正确性，mmdeploy 为不同的任务和后端添加了一系列用于模型推理的`End2EndModel`类。`End2EndModel`的目的是确保已转换的模型能够在后端正常进行推理和测试。目前，mmdeploy 支持多种后端，如 ONNXRuntime、TensorRT、NCNN 和 OpenVINO 等。同时，mmdeploy 还提供支持使用多种 SDK（例如 C++、Java等）进行推理。

为了支持后端模型，我们需要相应的后端模型类来调用后端模型以及进行模型输出的后处理，并将结果进行包装。以 MMPose 为例，在 `mmdeploy/codebase/mmpose/deploy/pose_detection_model.py` 文件中定义了其对应的后端模型类。对于 YoloX-Pose 模型，由于它执行的任务与一般的 MMPose 模型有所不同（模型输出和后处理方式均有差异），因此我们可以通过在原本的 MMPose 模型中添加一个新的用于 YoloX-Pose 模型的后处理的函数 `pack_yolox_pose_result`。该函数负责将后端模型的推理结果进行后处理和封装并返回给可视化组件以展示推理结果。对于 YoloX-Pose 来说，首先，由于在某些后端上的 NMS 实现会导致最后的结果中存在小于0的预测分数，所以我们需要在后处理阶段将这些结果剔除。其次，由于图像在预处理阶段经历过 resize 操作，因此我们需要对后端模型的推理结果进行 rescale 操作来使预测结果在正确的位置上，具体而言，我们可以遍历后端模型的推理结果，对其进行相应的后处理并封装成可视化组件所需的 `InstanceData` 类型：
```python
def pack_yolox_pose_result(self, preds: List[torch.Tensor],
                           data_samples: List[BaseDataElement]):
    assert preds[0].shape[0] == len(data_samples)
    batched_dets, batched_kpts = preds
    # 遍历推理结果
    for data_sample_idx, data_sample in enumerate(data_samples):
        bboxes = batched_dets[data_sample_idx, :, :4]
        bbox_scores = batched_dets[data_sample_idx, :, 4]
        keypoints = batched_kpts[data_sample_idx, :, :, :2]
        keypoint_scores = batched_kpts[data_sample_idx, :, :, 2]
        
        # 将预测分数小于等于0的结果剔除
        inds = bbox_scores > 0.0
        bboxes = bboxes[inds, :]
        bbox_scores = bbox_scores[inds]
        keypoints = keypoints[inds, :]
        keypoint_scores = keypoint_scores[inds]

        pred_instances = InstanceData()
        # rescale 操作
        scale_factor = data_sample.scale_factor
        scale_factor = keypoints.new_tensor(scale_factor)
        keypoints /= keypoints.new_tensor(scale_factor).reshape(1, 1, 2)
        bboxes /= keypoints.new_tensor(scale_factor).repeat(1, 2)
        pred_instances.bboxes = bboxes.cpu().numpy()
        pred_instances.bbox_scores = bbox_scores
        # 精度测试时需要keypoints为numpy数组类型
        pred_instances.keypoints = keypoints.cpu().numpy()
        pred_instances.keypoint_scores = keypoint_scores
        pred_instances.lebels = torch.zeros(bboxes.shape[0])

        data_sample.pred_instances = pred_instances
    return data_samples
```
有了上面的后处理函数，我们可以在模型的 `forward` 函数中根据是否是 YoloX-Pose 模型调用后处理函数并返回结果：
```python
def forward(self,
            inputs: torch.Tensor,
            data_samples: List[BaseDataElement],
            mode: str = 'predict',
            **kwargs):
    assert mode == 'predict', \
    'Backend model only support mode==predict,' f' but get {mode}'
    # 获取后端模型输出
    inputs = inputs.contiguous().to(self.device)
    batch_outputs = self.wrapper({self.input_name: inputs})
    batch_outputs = self.wrapper.output_to_list(batch_outputs)
    # 如果是YoloX-Pose模型，则调用相应的后处理函数并返回结果
    if self.model_cfg.model.type == 'YOLODetector':
        return self.pack_yolox_pose_result(batch_outputs, data_samples)
```
至此，我们完成了对 YoloX-Pose的后端模型支持，可以成功运行上面的模型转换命令以得到 onnx 模型以及 PyTorch模型和后端模型的推理可视化结果。如下图所示，通过比较转换得到的 ONNX 模型输出和原始 PyTorch模型的输出，可以发现模型成功转换并在后端进行推理。

<table><tr>
<td align="center"><img src="https://github.com/open-mmlab/mmdeploy/assets/110151316/d7b0dd25-09bc-44f8-849e-88eb058afe93" border="0"/><p>onnx 模型输出</p></td>
<td align="center"><img src="https://github.com/open-mmlab/mmdeploy/assets/110151316/067d11cd-dde0-40a4-b159-ea9eb93f1d9f" border="0"/><p>PyTorch 模型输出</p></td>
</tr></table>

在验证可视化结果一致后，还需要进行精度测试，进一步验证我们转换得到的模型的正确性。

### 精度测试
可视化结果一致并不能说明我们转换得到的模型一定正确，我们还需要在更大的数据集上进行精度测试，并与原始的 PyTorch模型进行精度对齐。MMDeploy 提供了 `tools/test.py` 这个脚本来进行精度测试，对于上面转换得到的 YoloX-Pose 的 ONNX 模型来说，我们可以通过如下命令启动测试：
```bash
python tools/test.py \
configs/mmpose/pose-detection_yolox-pose_onnxruntime_dynamic.py \  # 模型部署配置文件
../mmpose/projects/yolox-pose/configs/yolox-pose_s_8xb32-300e_coco.py \ # 模型配置文件
--model work_dir/end2end.onnx \ # 后端模型文件路径
--device cuda:0 # 运行设备
--batch-size 8 # 测试多batch推理
```
在测试完成后，我们可以得到精度测试的结果，并与原始的PyTorch模型的指标进行对比，上述命令的测试结果如下表所示。可以看出ONNX Runtime模型的精度与原始PyTorch模型一致，至此，我们完成了模型转换之后的模型精度对齐工作。

|    模型/指标    |  AP   | AP50  | AP75  |  AR   | AR50  | AR75  |
|:-----------:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
|   PyTorch   | 0.632 | 0.875 | 0.692 | 0.676 | 0.907 | 0.734 |
| ONNXRuntime | 0.632 | 0.875 | 0.692 | 0.676 | 0.907 | 0.734 |


## 单元测试编写
为确保所支持的模型在后续版本迭代中保持稳定且正确运行，我们可在 mmdeploy 中为其添加相应的单元测试。以 YoloX-Pose 为例，我们对该模型的 `yolox_pose_head` 模块进行了重写，因此可以针对这次重写添加相应的测试用例。MMDeploy 的单元测试存放在`tests/`目录下，我们可以在 `tests/test_codebase/test_mmpose/test_mmpose_models.py` 中添加如下测试用例：
```python
@pytest.mark.parametrize('backend_type', [Backend.ONNXRUNTIME])
def test_yolox_pose_head(backend_type: Backend):
    #导入包
        ...
    
    # 构建PyTorch模型
    head = YOLOXPoseHead(
    head_module=dict(
        type='YOLOXPoseHeadModule',
        num_classes=1,
        in_channels=256,
        feat_channels=256,
        widen_factor=0.5,
        stacked_convs=2,
        num_keypoints=17,
        featmap_strides=(8, 16, 32),
        use_depthwise=False,
        norm_cfg=dict(type='BN', momentum=0.03, eps=0.001),
        act_cfg=dict(type='SiLU', inplace=True))，
    # 其他模型配置
        ... 
    )
    
    # 构造一个测试模型类来封装 yolox_pose_head进行推理
    class TestYOLOXPoseHeadModel(torch.nn.Module):
    def __init__(self, yolox_pose_head):
        super(TestYOLOXPoseHeadModel, self).__init__()
        self.yolox_pose_head = yolox_pose_head

    def forward(self, x1, x2, x3):
        inputs = [x1, x2, x3]
        data_sample = InstanceData()
        data_sample.set_metainfo(
            dict(ori_shape=(640, 640), scale_factor=(1.0, 1.0)))
        return self.yolox_pose_head.predict(
            inputs, batch_data_samples=[data_sample])
    
    model = TestYOLOXPoseHeadModel(head)
    model.cpu().eval()
    
    # 构造测试输入
    model_inputs = [
        torch.randn(2, 128, 8, 8),
        torch.randn(2, 128, 4, 4),
        torch.randn(2, 128, 2, 2)
    ]
    
    # 获取PyTorch模型的输出
    with torch.no_grad():
        pytorch_output = model(*model_inputs)[0]
    
    # 构造后端模型并获取对应的输出结果
    wrapped_model = WrapModel(model, 'forward')
    rewrite_outputs, _ = get_rewrite_outputs(
        wrapped_model=wrapped_model,
        model_inputs=rewrite_inputs,
        run_with_backend=True,
        deploy_cfg=deploy_cfg)
        
    # 将PyTorch的模型输出格式与后端输出对齐    
        ...
        
    # 比较两个模型的输出结果是否一致以判定是否通过测试
    torch_assert_close(rewrite_outputs, pytorch_output)
```
我们使用一个装饰器来声明该函数为测试函数。这样，在进行单元测试时，系统将自动检测该函数是否通过测试。在测试函数中，我们分别构建 PyTorch模型和后端模型。之后，我们使用相同的输入分别在这两个模型上进行推理，并检查它们的输出是否一致。完整的单元测试代码，可以参考[#PR2231](https://github.com/open-mmlab/mmdeploy/pull/2231)。

## 总结
本文介绍了如何为 MMDeploy 添加和适配自定义模型，并以 YoloX-Pose 为例介绍了模型支持的一般流程。我们重点关注了部署配置文件编写、函数重写以及后端模型支持等方面。通过这些步骤，我们可以将自定义模型集成到 MMDeploy 中，从而利用不同的推理框架加速并优化各种应用场景。在将来，MMDeploy 将继续扩展对更多模型的支持，也欢迎广大开发者参与进来，提供更加灵活和强大的部署解决方案。