---
title: "编译arm64平台WebRTC"
date: 2022-12-06T18:23:34+08:00
tags: [Linux, WebRTC, arm64]
categories: 资料
---
## 前置软件

1、安装Chromium depot tools①

```bash
# 拉取源码
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
# 添加环境变量
echo "export PATH=$PWD/depot_tools:$PATH" > ~/.bashrc
source ~/.bashrc
```

2、执行安装编译依赖脚本②，该脚本需要在第一次同步代码后才有

```bash
./build/install-build-deps.sh
```

3、执行下载编译环境脚本②，该脚本需要在第一次同步代码后才有

```bash
./build/linux/sysroot_scripts/install-sysroot.py --arch=arm64
```

4、安装编译器

```bash
sudo apt-get install binutils-aarch64-linux-gnu
sudo apt-get install gcc-aarch64-linux-gnu g++-aarch64-linux-gnu
```

## 编译

1、拉取源码③，后执行前置软件2和3

```bash
mkdir webrtc-checkout
cd webrtc-checkout
fetch --nohooks webrtc
gclient sync
```

2、切换到主分支

```bash
cd src
git checkout main
```

3、生成编译文件（执行其中一个）

```bash
# 参照④
gn gen out/Linux-arm64 --args="target_os=\"linux\" target_cpu=\"arm64\" is_debug=false rtc_include_tests=false rtc_use_h264=false is_component_build=false use_rtti=true use_custom_libcxx=false rtc_enable_protobuf=false"

# 参照⑤
gn gen out/Linux-arm64 --args="target_os=\"linux\" target_cpu=\"arm64\" is_debug=false rtc_include_tests=false rtc_use_h264=true is_component_build=false use_rtti=true use_custom_libcxx=false rtc_enable_protobuf=false is_clang=false treat_warnings_as_errors=false use_ozone=true rtc_include_pulse_audio=false use_libjpeg_turbo=false use_system_libjpeg=true"
```

4、编译③

```bash
autoninja -C out/Linux-arm64
```

## 引用

1. [Install the Chromium depot tools](https://commondatastorage.googleapis.com/chrome-infra-docs/flat/depot_tools/docs/html/depot_tools_tutorial.html#_setting_up)
2. [WebRTC development - Prerequisite software](https://webrtc.googlesource.com/src/+/main/docs/native-code/development/prerequisite-sw/index.md)
3. [WebRTC development](https://webrtc.googlesource.com/src/+/main/docs/native-code/development/index.md)
4. [Build command for all platforms](https://github.com/webrtc-sdk/webrtc-build/blob/main/docs/build.md#linux-armarm64)
5. [gcc编译webrtc arm64版](https://blog.csdn.net/tjyuanxi/article/details/124693182)


> 原链接：[编译arm64平台WebRTC](/post/compile-arm64-webrtc)