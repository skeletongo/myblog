---
author: skeleton
title: iptables nat表
date: 2022-07-31
description: nat表添加ip及端口转发规则
tags:
- nat
categories: 
- iptables
- linux
---

## 常用示例
+ 端口转发，2222端口转发到22端口
    - iptables -t nat -A PREROUTING -d 192.168.70.129 -p tcp --dport 2222 -j DNAT --to-destination 192.168.70.129:22

+ ip及端口转发，将目标192.168.70.129:8080的数据包转发到172.17.0.2:8881
    - iptables -t nat -A PREROUTING -d 192.168.70.129 -p tcp --dport 8080 -j DNAT --to-dest 172.17.0.2:8881

+ 网卡数据包转发，将非docker0网卡且目标端口为8888的数据包转发到172.17.0.2的8887端口
    - iptables -t nat -A PREROUTING ! -i docker0 -p tcp --dport 8888 -j DNAT --to-dest 172.17.0.2:8887

+ ip映射，将172.17.0.1的所有数据包转发到172.17.0.2
    - iptables -t nat -A PREROUTING -d 172.17.0.1 -p all -j DNAT --to-dest 172.17.0.2