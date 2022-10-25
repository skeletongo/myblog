---
author: skeleton
title: 链接库
date: 2022-10-24
description: 
tags: []
categories: 
- c/c++
---

# c语言链接库的创建和使用
链接库分静态链接库和动态链接库两种，静态链接库被打包到可执行文件中，动态链接库不在可执行程序中

## 静态链接库  
文件后缀为.a,静态库会被完整打包到可执行程序中，程序可直接启动

## 动态链接库
文件后缀为.so,动态库和可执行程序是分离的，所以在启动可执行程序时需要依赖动态库文件

<font color=yellow>动态库和静态库同名，优先使用动态库</font>

## gcc 参数
```
-c 生成目标文件
-o 生成后的文件名
-I 头文件搜索目录
-L 库文件搜索目录
-l 链接库名称
参数和参数值可以连在一起写，例如 -L. 在当前目录寻找库文件
```

## 示例
以下面三个代码文件为例
```
// hello.h 文件

#ifndef HELLO_H
#define HELLO_H

void Hello();

#endif
```

```
// hello.c 文件
#include <stdio.h>

void Hello() {
    printf("hello world");
}
```

```
// main.c 文件
#include <hello.h>

int main() {
    Hello();
    return 0;
}
```

>### 静态链接库创建和使用
>1. 创建目标文件 hello.o
>```
>gcc -I . -c -o hello.o hello.c
>```  
>2. 创建静态链接库
>```
>ar crv libmyhello.a hello.o
>```
>3. 使用静态链接库
>```
>gcc -o hello main.c -L . -l myhello -I .
>或
>gcc main.c libmyhello.a -o hello -I .
>```

>### 动态链接库创建和使用
>1. 创建目标文件 hello.o
>```
>gcc -I . -o hello.o -c hello.c
>```  
>2. 创建动态链接库
>```
>gcc -shared -fPIC -o libmyhello.so hello.o
>```
>3. 使用动态链接库
>```
>gcc -o hello main.c -L. -lmyhello -I .
>或
>gcc main.c libmyhello.so -o hello -I .
>```