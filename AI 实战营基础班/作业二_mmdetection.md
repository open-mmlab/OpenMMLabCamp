# 作业2

请参考 MMDetection 文档及教程，基于自定义数据集 balloon 训练实例分割模型，基于训练的模型在样例视频上完成color splash的效果制作，即使用模型对图像进行逐帧实例分割，并将气球以外的图像转换为灰度图像。

color splash样例：
https://github.com/matterport/Mask_RCNN/blob/master/assets/balloon_color_splash.gif 


**注：由于GPU使用资源有限，请同学们尽量在CPU上先调通程序再进行模型的训练。**

## 1. Balloon数据集

balloon是带有mask的气球数据集，其中训练集包含61张图片，验证集包含13张图片。

下载链接：https://github.com/matterport/Mask_RCNN/releases/download/v2.1/balloon_dataset.zip 

## 2. 新数据集的支持

由于需要支持新的数据集，请同学们选择MMDetection支持的以下方法来操作:

1. 将数据集整理为COCO格式
2. 将数据集整理为中间格式
3. 直接实现新数据集的支持
注：方法2，3不直接支持分割任务，需要后处理。这里推荐使用方法1。

## 3. 构建配置文件

从 Model Zoo 中找到 mask rcnn 模型并找到 `configs/mask_rcnn/` 中对应的模型配置文件。将模型下载到环境中，通常放在放置在 `checkpoints` 文件夹中。

可以参考：https://github.com/open-mmlab/mmdetection/tree/master/configs/mask_rcnn 


构建配置文件可以使用继承机制，从选择模型配置中继承，并修改自定义数据集需要的内容。



## 4. 模型微调

通过命令行工具或者 Python API 完成模型微调，并在验证集上完成测试和评价。可以调整参数，或使用不同的预训练模型来获得更高的评分。


## 5. Color splash特效制作

在获取到图像的mask之后，请同学们将图像转为灰度图像，并在mask区域将原图像的像素值拷贝到灰度图像上即可完成特效制作。


测试样例视频请同学们在作业文件夹中下载 `test_video.mp4`


有关视频的读写可以参考以下文档：

- mmcv: 基于opencv的实现 

https://github.com/open-mmlab/mmcv/blob/master/mmcv/video/io.py 

- opencv:

https://opencv-python-tutroals.readthedocs.io/en/latest/py_tutorials/py_gui/py_video_display/py_video_display.html# 

- skvideo: 需要额外安装ffmepg，且需要注意设置压制参数 `pix_fmt` 为 `yuv420p` 以支持主流播放器

http://www.scikit-video.org/stable/modules/io.html#module-skvideo.io 


## 6. 作业提交：

完成视频特效制作之后，请同学们将以下文件上传至自己的github，并将链接按要求提交在对应班级issue：

* 训练模型的配置文件
* 训练好的模型文件（若文件过大，可以存至网盘，将链接放在github readme中）
* 特效制作后的视频文件（若文件过大，可以存至网盘，将链接放在github readme中）
* log文件

