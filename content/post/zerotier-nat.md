---
title: "zerotier配置节点内网互联"
date: 2023-09-26T15:36:48+08:00
tags: [zerotier, nat, 内网互联]
categories: 资料
draft: false
---

# zerotier配置节点内网互联

## 网络三层NAT配置方法（linux主机）

- 假设zerotier虚拟局域网的网段是192.168.88.0 `局域网A 192.168.1.0` `局域网B 192.168.2.0`
- (如果需要互联)在局域网A和B中需要各有一台主机安装zerotier并作为两个内网互联的网关
- 分别是192.168.1.10（192.168.88.10） 192.168.2.10（192.168.88.20）#括号里面为虚拟局域网的IP地址

### 在zerotier网站的networks里面的Managed Routes下配置路由表,增加如下内容

```bash
#如果单向连接,仅需填写下方一个即可.

192.168.1.0/24 via 192.168.88.10 
192.168.2.0/24 via 192.168.88.20 
```

### 开启内核转发

```bash
#echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
#sysctl -p
```

### 防火墙设置

```bash
# 其中的 zt7nnig26 在不同的机器中不一样，你可以在路由器ssh环境中用 zerotier-cli listnetworks 或者 ifconfig 查询zt开头的网卡名
PHY_IFACE=eth0; ZT_IFACE=zt7nnig26
sudo iptables -t nat -A POSTROUTING -o $PHY_IFACE -j MASQUERADE
sudo iptables -A FORWARD -i $PHY_IFACE -o $ZT_IFACE -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i $ZT_IFACE -o $PHY_IFACE -j ACCEPT
# 保存配置到文件，否则重启规则会丢失
iptables-save
```



> 原链接：[zerotier配置节点内网互联](/post/zerotier-nat)
