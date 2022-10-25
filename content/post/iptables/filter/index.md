---
author: skeleton
title: iptables filter表
date: 2022-07-30
description: filter表添加数据包过滤规则
tags:
- filter
categories: 
- iptables
- linux
---

## 准备
安装 iptables：  
`apt-get install iptables-persistent`  
启动 iptables：  
`service iptables start`  
安装 ping：  
`apt-get install inetutils-ping`  
查看防火墙规则：  
`iptablse -L -vn`

## 备份和加载配置
`iptables-save > backup.txt`  
`iptables-restore < backup.txt`

## 查看配置
+ 查看filter表  
    - `iptables -t filter -L -vn` 详细信息（-t filter可以不加，默认就是查看filter表）  
    - `iptables -t filter -l -vvn` 更详细信息
+ 查看nat表  
    - `iptables -t nat -L -vn`   
    - `iptables -t nat -l -vvn`

## 删除配置
`iptables -t [filter/nat/...] -D [Chain] 1`  删除某个表中某个链中的第几个规则  

## 处理动作
+ ACCEPT：允许，同意
+ DROP：丢弃
+ REJECT：–reject-with:icmp-port-unreachable默认，拒绝
+ RETURN：返回调用链
+ REDIRECT：端口重定向
+ LOG：记录日志，dmesg
+ MARK：做防火墙标记
+ DNAT：目标地址转换
+ SNAT：源地址转换
+ MASQUERADE：地址伪装  
<mark>常用 ACCEPT，DROP，DNAT</mark>

## 常用示例
+ 设置默认规则，在其他规则都不满足时走默认规则
    - iptables -P INPUT DROP 丢弃所有收到的数据包
    - iptables -P OUTPUT ACCEPT 允许发送所有数据包
    - iptables -P FORWARD DROP  丢弃所有转发的数据包  

+ 禁止特定源ip访问
    - iptables -A INPUT -s 192.168.70.128 -p all -j DROP
    - iptables -A INPUT -s 192.168.70.0/24 -p tcp -j DROP

+ 禁止范围源ip使用tcp协议访问特定目标ip
    - iptables -A INPUT -m iprange --src-range 172.16.1.5-172.16.1.10 -d 172.16.1.100 -p tcp -j DROP

+ 允许特定ip访问
    - iptables -A INPUT -s 192.168.70.128 -p all -j ACCEPT
    - iptables -A INPUT -s 192.168.70.0/24 -p all -j ACCEPT

+ 禁止特定网段访问22端口
    - iptables -A INPUT -s 192.168.60.0/24 -p all -j DROP

+ 允许特定网段访问22端口
    - iptables -A INPUT -s 192.168.0.0/16 -p tcp -dport 22 -j ACCEPT
    - iptables -A INPUT -s 192.168.0.0/16 -p tcp -dport 22,23 -j ACCEPT
    - iptables -A INPUT -s 192.168.0.0/16 -p tcp -dport 22:30 -j ACCEPT

+ 禁止特定ip访问特定ip
    - iptables -A INPUT -s 192.168.0.0/16 -p all -d 192.168.70.0/24 -j DROP

+ 允许特定ip访问特定ip的端口
    - iptables -A INPUT -s 192.168.0.0/16 -p all -d 192.168.70.129:80 -j ACCEPT

+ 禁止特定ip以外的ip访问
    - iptables -I INPUT ! -s 192.168.70.128 -p all -j DROP

+ 允许本地回环地址正常使用
    - iptables -A INPUT -i lo -j ACCEPT
    - iptables -A OUTPUT -o lo -j ACCEPT
