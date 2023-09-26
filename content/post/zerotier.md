---
title: "自建zerotier-planet"
date: 2023-09-26T14:49:56+08:00
tags: [zerotier, planet, 组网]
categories: 资料
draft: false
---

# 自建zerotier-planet

## 必要条件

- 具有公网ip的服务器
- 安装 docker
- 安装 docker-compose
- 防火墙开放TCP端口 4000/9993/3180 和UDP端口 9993

## 用法

### 部署ztncui

```yaml
version: '2.0'
services:
    ztncui:
        container_name: ztncui
        restart: always
        environment:
            - MYADDR=1.1.1.1 #改成自己的服务器公网IP
            - HTTP_PORT=4000
            - HTTP_ALL_INTERFACES=yes
            - ZTNCUI_PASSWD=password #默认用户admin的密码
        ports:
            - '4000:4000' # web控制台入口
            - '9993:9993'
            - '9993:9993/udp'
            - '3180:3180' # planet/moon文件在线下载入口，如不对外提供。可防火墙禁用此端口。
        volumes:
            - './zerotier-one:/var/lib/zerotier-one'
            - './ztncui/etc:/opt/key-networks/ztncui/etc'
            # 按实际路径挂载卷， 冒号前面是宿主机的， 支持相对路径
        image: keynetworks/ztncui
```

### 创建planet和moon

```bash
git clone https://github.com/Jonnyan404/zerotier-planet
OR
git clone https://gitee.com/Jonnyan404/zerotier-planet

cd zerotier-planet
docker cp mkmoonworld-x86_64 ztncui:/tmp
docker cp patch.sh ztncui:/tmp
docker exec -it ztncui bash /tmp/patch.sh
docker restart ztncui
```

浏览器访问 `http://ip:4000` 打开web控制台界面。

浏览器访问 `http://ip:3180` 打开planet和moon文件下载页面（亦可在项目根目录的`./ztncui/etc/myfs/`里获取）。

- 用户名:admin
- 密码:password

**note:** 如果未指定密码,可执行 `docker exec -it ztncui cat /var/log/docker-ztncui.log|grep Password` 获取密码.

## 各客户端配置planet

客户端主要为Windows, Mac,Linux, Android

### Windows 客户端

先安装官方zerotier客户端，将生成的 `planet` 文件覆盖粘贴到 `C:\ProgramData\ZeroTier One` 中(这个目录是个隐藏目录，需要运允许查看隐藏目录才行)，然后重启zerotier-one服务。
![搜索服务](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/202309261519533.png)
![重启服务](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/202309261520758.png)
使用管理员身份打开PowerShell 执行命令加入组网，看到join ok字样就成功了。

```bash
PS C:\Windows\system32> zerotier-cli join 网络id(就是在网页里面创建的那个网络)
200 join OK
PS C:\Windows\system32>
```

登录管理后台可以看到有个新的客户端，勾选Authorized授权接入。
![授权](https://ll.lao4g.top/d/Oneindex/FILE/BlogImg/202309261534037.png)
查看连接情况：

```bash
PS C:\Windows\system32> zerotier-cli peers
200 peers
<ztaddr>   <ver>  <role> <lat> <link> <lastTX> <lastRX> <path>
fcbaeb9b6c 1.8.7  PLANET    52 DIRECT 16       8994     1.1.1.1/9993
fe92971aad 1.8.7  LEAF      14 DIRECT -1       4150     2.2.2.2/9993
PS C:\Windows\system32>
```

可以看到有一个 PLANTET 和 LEAF 角色，连接方式均为 DIRECT(直连)，到这里就加入网络成功了。

### Linux 客户端

1. 安装linux客户端软件 `curl -s https://install.zerotier.com | sudo bash`
2. 进入目录 /var/lib/zerotier-one，替换目录下的 planet 文件
3. 重启 zerotier-one 服务(systemctl restart zerotier-one)
4. 加入网络 zerotier-cli join 网络 id
5. 管理后台同意加入请求
6. zerotier-cli peers 可以看到 planet 角色

### 安卓客户端配置

[ZerotierFix](https://github.com/kaaass/ZerotierFix)
[Zerotier 非官方安卓客户端发布：支持自建 Moon 节点 - V2EX](https://www.v2ex.com/t/768628)

## 私有 zerotier-planet 的优势:

- 解除官方 25 的设备连接数限制
- 提升手机客户端连接的稳定性

## Reference Link

- [zerotier的planet服务器（根服务器）的搭建踩坑记](https://www.emengweb.com/p/zerotier的planet服务器（根服务器）的搭建踩坑记。无需zerotier官网账号。)
- [https://www.mrdoc.fun/doc/443/](https://www.mrdoc.fun/doc/443/)
- [https://github.com/key-networks/ztncui-aio](https://github.com/key-networks/ztncui-aio)




> 原链接：[自建zerotier-plant](/post/zerotier)
