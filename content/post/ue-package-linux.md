---
title: "UE5在Windows打包Linux项目"
date: 2022-12-05T20:40:21+08:00
tags: [UE5, 打包, Linux]
categories: 资料
---

1、在官网下载交叉编译工具链：[https://docs.unrealengine.com/5.0/zh-CN/linux-development-requirements-for-unreal-engine/](https://docs.unrealengine.com/5.0/zh-CN/linux-development-requirements-for-unreal-engine/)

![ue-package-linux-1](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/ue-package-linux-1.png)

2、默认安装后在 `cmd` 中输入 `%LINUX_MULTIARCH_ROOT%x86_64-unknown-linux-gnu\bin\clang++ -v` 测试是否安装成功

3、重启ue5

4、刷新平台状态

![ue-package-linux-2](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/ue-package-linux-2.png)

5、如果打包平台前出现Linux图标，则已经可以打包Linux项目

6、选择对应平台打包

![ue-package-linux-3](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/ue-package-linux-3.png)


> 原链接：[UE5在Windows打包Linux项目](/post/ue-package-linux)