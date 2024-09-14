---
title: "YoloV8目标检测模型训练"
date: 2024-09-14T10:17:16+08:00
tags: [yolo, 目标检测]
categories: 资料
draft: false
---

# 图片标注

使用[label-studio](https://github.com/HumanSignal/label-studio)进行标注。

# 模型训练

1. 从 [Git](https://github.com/liusanp/yolov8_train.git) 克隆代码

2. 准备数据集

```shell
# 将label studio导出的yolo格式数据集，用handle_train_data.py切分

# 数据集目录
# xxxx-20240914             // 数据标注根目录
# ├─train                 // 训练集用于模型的学习和训练
# | ├─images              // 图片存放目录
# | └─labels              // 标注结果存放目录
# ├─val                   // 验证集用于模型的调优和验证，避免过拟合
# | ├─images              // 图片存放目录
# | └─labels              // 标注结果存放目录
# ├─test                  // 测试集用于模型的最终评估，衡量其在真实环境中的性能
# | ├─images              // 图片存放目录
# | └─labels              // 标注结果存放目录
# └─data.yaml             // 数据集配置

```

```yaml
# data.yaml配置：
path: xxxx-20240914  # dataset root dir  
train: train  # train images (relative to 'path') 118287 images  
val: val  # val images (relative to 'path') 5000 images  
test: test  # 20288 of 40670 images, submit to   
  
# Classes  
nc: 2  # number of classes  
names: [
'class1',
'class2'
]
```

3. 修改训练参数并启动训练

```python
# 修改train.py
# 主要修改以下几个，其余根据需要修改

# 修改为训练集文件夹名称
data_name = 'xxxx-20240914' 

# 首次训练则启用行
model = YOLO('models/yolov8x.pt')

# 二次训练则启用行
model = YOLO(f'runs/train/train-{data_name}{train_idx}/weights/best.pt')
# 修改从哪次结果继续训练，首次为空
train_idx = '2'

# 如需随机角度内旋转数据增强则修改
degrees=100

```

4. 启动训练

```shell
python train.py
```

# 模型测试

```python
# 修改test.py
# 主要修改以下几个，其余根据需要修改

# 修改为训练集文件夹名称
data_name = 'xxxx-20240914' 

# 修改从哪次结果测试，第一次为空
train_idx = '2'

# 修改为第几次测试结果，第一次为空
detect_idx = '2'

```


> 原链接：[YoloV8目标检测模型训练](/post/yolov8)
