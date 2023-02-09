# 使用MMSegmentation，在自己的数据集上，训练语义分割模型
1. 数据集标注（可选）

使用Labelme、LabelU等数据标注工具，标注多类别语义分割数据集，并保存为指定的格式。

2. 数据集整理

划分训练集、测试集

3. 使用MMSegmentation训练语义分割模型

在MMSegmentation中，指定预训练模型，配置config文件，修改类别数、学习率。

4. 用训练得到的模型预测

获得测试集图片或新图片的语义分割预测结果，对结果进行可视化和后处理。

5. 在测试集上评估算法的速度和精度性能

6. 使用MMDeploy部署语义分割模型（可选）

# 参考单类别语义分割数据集
组织病理切片小鼠肾小球：https://zihao-openmmlab.obs.cn-east-3.myhuaweicloud.com/20230130-mmseg/dataset/Glomeruli-dataset.zip

电子显微镜粒子：https://www.kaggle.com/datasets/batuhanyil/electron-microscopy-particle-segmentation

农作物病虫害叶片：https://www.kaggle.com/datasets/fakhrealam9537/leaf-disease-segmentation-dataset

农作物地块：https://www.kaggle.com/datasets/khlaifiabilel/pastis

水下场景：https://www.kaggle.com/datasets/ashish2001/semantic-segmentation-of-underwater-imagery-suim

西红柿种子：https://www.kaggle.com/datasets/juanma9901/tomatoseedsdatasetjm

肾小球：https://www.kaggle.com/datasets/baesiann/glomeruli-hubmap-external-1024x1024

卫星建筑物：https://www.kaggle.com/datasets/hyyyrwang/buildings-dataset

荧光显微镜小鼠脑切片发光神经元-实例分割：https://www.kaggle.com/datasets/nbroad/fluorescent-neuronal-cells

混凝土裂缝：https://www.kaggle.com/datasets/jakubniemiec/concrete-crack-images

# 参考多类别语义分割数据集
高分辨率航拍-多类别：https://www.kaggle.com/datasets/titan15555/uavid-semantic-segmentation-dataset

衣物：https://www.kaggle.com/datasets/rajkumarl/people-clothing-segmentation

海洋生物：https://www.kaggle.com/datasets/crowww/a-large-scale-fish-dataset

腿和脚趾：https://www.kaggle.com/datasets/tapakah68/legs-segmentation

无人机航拍：https://www.kaggle.com/datasets/santurini/semantic-segmentation-drone-dataset

无人机航拍：https://www.kaggle.com/datasets/bulentsiyah/semantic-drone-dataset
