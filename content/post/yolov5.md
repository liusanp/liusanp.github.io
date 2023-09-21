---
title: "YoloV5目标检测模型训练"
date: 2023-09-21T16:30:51+08:00
tags: [yolo, 标注]
categories: 资料
draft: false
---

# YoloV5目标检测模型训练

## 图片标注

使用labelimg进行图片标注
```shell
# 安装
pip install labelimg

# 启动（在image_root目录启动）
# image_root              // 数据标注根目录
# ├─images                // 图片存放目录
# ├─labels                // 标注结果存放目录
# └─labels.txt            // 预加载标签，一行一个

labelimg images labels.txt
```
![Snipaste_2023-03-31_11-52-15.jpg](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/202303311152523.jpg)

## 模型训练

[以yolov5-7.0为例](https://github.com/ultralytics/yolov5/tree/v7.0)

### 1、从GitHub下载zip包或者克隆代码
### 2、准备数据集
```shell
# 数据集目录
# image_root              // 数据标注根目录
# ├─train                 // 训练集
# | ├─images              // 图片存放目录
# | └─labels              // 标注结果存放目录
# ├─val                   // 验证集
# | ├─images              // 图片存放目录
# | └─labels              // 标注结果存放目录
# └─data.yaml             // 数据集配置

data.yaml配置：
```
```yaml
path: data/image_root  # dataset root dir  
train: train  # train images (relative to 'path') 118287 images  
val: val  # val images (relative to 'path') 5000 images  
# test: test-dev2017.txt  # 20288 of 40670 images, submit to   
  
# Classes  
nc: 2  # number of classes  
names: [
'class1',
'class2'
]
```
### 3、下载模型

从[Github releases](https://github.com/ultralytics/yolov5/releases)下载预训练模型，可根据需要选择模型大小：yolov5s.pt、yolov5m.pt、yolov5l.pt、yolov5x.pt。将下载好的模型放在yolov5源码的models目录中，然后将选择的模型对应yaml文件中的nc修改为自己的分类数。

### 4、修改训练参数并启动训练
```python
# 修改train.py中的parse_opt()函数
# 主要修改以下几个，其余根据需要修改
def parse_opt(known=False):  
	# 初始模型权重
    parser.add_argument('--weights', type=str, default=ROOT / 'models/yolov5m.pt', help='initial weights path')  
    # 模型配置
    parser.add_argument('--cfg', type=str, default=ROOT / 'models/yolov5m_2.yaml', help='model.yaml path')  
    # 训练数据集配置
    parser.add_argument('--data', type=str, default='image_root/data.yaml', help='dataset.yaml path')  
    # 超参配置
    parser.add_argument('--hyp', type=str, default=ROOT / 'data/hyps/hyp.scratch-low.yaml', help='hyperparameters path')  
    # 训练迭代数
    parser.add_argument('--epochs', type=int, default=100, help='total training epochs')  
    # 训练批次大小
    parser.add_argument('--batch-size', type=int, default=12, help='total batch size for all GPUs, -1 for autobatch')  
```
### 5、根据训练结果修改超参
```yaml
lr0: 0.01  # initial learning rate (SGD=1E-2, Adam=1E-3)  
lrf: 0.001  # final OneCycleLR learning rate (lr0 * lrf)
```
## 模型预测

使用detect.py预测


> 原链接：[Yolov5](/post/yolov5)
