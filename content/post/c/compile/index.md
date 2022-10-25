---
author: skeleton
title: c/c++编译阶段
date: 2022-10-24
description: 
tags: []
categories: 
- c/c++
---

# 四个编译阶段
以编译文件hello.c为例

1. 预编译
```
gcc -E hello.c -o hello.i
```
2. 汇编
```
gcc -S hello.c -o hello.s
```
3. 目标文件
```
gcc -c hello.c -o hello.o
```
4.链接，生成可执行文件
```
gcc hello.c -o hello
```