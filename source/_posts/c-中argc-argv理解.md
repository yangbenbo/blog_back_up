---
title: c++中argc argv理解
date: 2020-01-22 17:27:07
categories:
- Program
tags:
- c++
- c
---
经常发现main函数这样写
 
    int main(int argc, char *argv[]){}
先来看关于char的几种类型

# `char * ,char ** ,char a[ ] ,char *a[ ]`
    
    char * : 指向字符的指针,本质上存储是一个地址,地址中对应的内容为char
    char a[ ]:字符数组,数组里面每一个都是一个char
    char** a:二级指针,a是一个指向char * 的指针,保存的是char*的地址
    char *a[ ] :[ ]的优先级高于*,所以首先是一个数组,数组里面元素为指向char *的指针

c语言没有字符串,用字符数组代替;c语言中字符串常量本质上是一个地址

**C语言中操作字符串是通过它在内存中的存储单元的首地址进行的,这是字符串的终极本质**

    char *s;
    s = "China";   //"China"字符串常量,存放的是C的地址
    
**字符指针可以用 间接操作符 \*取其内容,也可以用数组的下标形式 [],数组名也可以用 \*操作,因为它本身表示一个地址**    

# argc argv

1. argc为int:运行程序所带参数个数(包括程序本身名字,这点类似Linux下bash编程)
2. char* argv[]:存放内容包括 程序绝对路径+程序运行所带参数(全部转化为字符串)

    
# 引用
1. [深入 `char * ,char ** ,char a[ ] ,char *a[]` 内核](https://blog.csdn.net/daiyutage/article/details/8604720)
2. {% post_link 临时记录 操作符优先级 %}    