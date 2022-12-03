---
title: "Linux设置系统代理"
date: 2022-12-04T00:05:08+08:00
tags: [Linux]
categories: 资料
---

```shell
# 设置代理
export http_proxy=http://127.0.0.1:20172
export HTTP_PROXY=http://127.0.0.1:20172

export https_proxy=http://127.0.0.1:20172
export HTTPS_PROXY=http://127.0.0.1:20172

export all_proxy=socks5h://127.0.0.1:20173
export ALL_PROXY=socks5h://127.0.0.1:20173

export no_proxy=localhost,127.0.0.0/8,::1
export NO_PROXY=localhost,127.0.0.0/8,::1

# 查看代理
echo $http_proxy

# 取消代理
unset http_proxy
unset HTTP_PROXY
unset https_proxy
unset HTTPS_PROXY
unset all_proxy
unset ALL_PROXY
unset no_proxy
unset NO_PROXY


# git设置代理
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
# git取消代理
git config --global --unset http.proxy
git config --global --unset https.proxy

```