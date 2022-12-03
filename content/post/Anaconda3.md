---
title: "Anaconda3"
date: 2022-07-10T17:28:13+08:00
tags: [Python, conda]
categories: 资料
draft: false
---

```shell
# 不自启动base，linux环境安装完成后会自动启动默认base环境 
conda config --set auto_activate_base false 
# 切换环境 
conda activate 环境名 
# 创建一个名为test的环境并指定python版本为3(的最新版本)，也可指定详细python版本 
conda create -n test python=3 
conda create -n test python=3.7 
# 复制环境 
conda create -n 新环境名 --clone 旧环境名 
# 列出conda管理的所有环境 
conda env list 
# 列出当前环境的所有包 
conda list 
# 安装python包时要先切换到具体环境 
# conda方式安装python包 -i 指定源 
conda install requests 
# pip方式安装python包 -i 指定源 -r 指定requirements.txt 
pip install requests 
# conda方式卸载requets包 
conda remove requests 
# pip方式卸载requets包 
conda remove requests 
# 删除test环境及下属所有包 
conda remove -n test --all 
# 更新requests包 
conda update requests 
# 导出当前环境的包信息 
conda env export > environment.yaml 
# 用配置文件创建新的虚拟环境 
conda env create -f environment.yaml

# 生成requirements.txt
# 安装pipreqs
pip install pipreqs
# 在当前目录生成
pipreqs . --encoding utf8 --force

# 创建虚拟环境
mkvirtualenv -p python3.7 venvname
# 切换虚拟环境
workon venvname

# 查看conda信息
conda info
conda info --envs

# 安装失败断掉
# 清楚缓存
conda clean --all
# 本地安装
conda install --use-local ./pytorch-1.10.1.tar.bz2

# 切换windows位数
# 打包为exe的话，版本尽量选择python3.6+32位版本，因为win64位系统向下兼容32位程序，
# 但是如果不考虑32位系统的话无所谓，直接python64位版本直接打包，只是只能在win64位系统上跑。
# 从64位切换到32位开发模式: 
set CONDA_FORCE_32BIT=1
# 再切回64位开发模式: 
set CONDA_FORCE_32BIT=0


pip清华源
https://pypi.tuna.tsinghua.edu.cn/simple
```
