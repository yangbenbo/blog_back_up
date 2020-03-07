---
title: c++ string与c风格字符串
date: 2020-02-22 15:08:52
categories:
- program
tags:
- 字符串
---

## 字符串 
字符串实际上是使用 null 字符 '\0' 终止的一维字符数组，大小比单词多一个

不需要把 null 字符放在字符串常量的末尾。C++ 编译器会在初始化数组时，自动把 '\0' 放在字符串的末尾。

建议使用c++ string

    char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'}; 
    char greeting[] = "Hello";  

### c++操作c风格字符串
复制,连接,查找,比较,**需要#include \<cstring\>**

**strlen(s1)获得的长度不包含空字符'\0',string类的length()也是一样,**
    
    #include <cstring>  
    strcpy(s1, s2);   //复制字符串 s2 到字符串 s1
    strcat(s1, s2);     //连接字符串 s2 到字符串 s1 的末尾
    strlen(s1);         //返回字符串 s1 的长度 不包含空字符'\0'
    strcmp(s1, s2);     //如果 s1 和 s2 是相同的，则返回 0；如果 s1<s2 则返回值小于 0；如果 s1>s2 则返回值大于 0
    strchr(s1, ch);     //返回一个指针，指向字符串 s1 中字符 ch 的第一次出现的位置
    strstr(s1, s2);     //返回一个指针，指向字符串 s1 中字符串 s2 的第一次出现的位置
    
### string
c++ 支持上述所有操作,并增加了功能

     append() -- 在字符串的末尾添加字符 直接用str1+str2也可以
     find() -- 在字符串中查找字符串
     insert() -- 插入字符
     length() -- 返回字符串的长度 不包含空字符'\0'
     replace() -- 替换字符串
     substr() -- 返回某个子字符串
     ...

## 遇到的问题 
1. 常量字符串不能修改
    **p指向常量字符串(位于常量存储区),内容不能修改**    
    
        // p a存放在栈区,"string"字面值存放在常量区
        char *p = "string"; //可以访问单个字符p[2] 不能直接更改p[2] = 'I'
        char a[] = "string"; //这里字符数组会把"string"拷贝到栈区,可以直接更改a[2] = 'I' 
        
    一、程序的内存分配
    - 栈区(stack)：由编译器自动分配释放，存放函数的参数值，局部变量值等；
    - 堆区(heap)：一般由程序员分配释放，若程序员不释放，程序结束时可能由操作系统OS回收。注意它与数据结构中的堆是两回事，**分配方式倒是类似于链表**； （malloc,new,free,delete）
    - 全局区(静态区 static)：全局变量和静态变量的存储是放在一块的，初始化的全局变量和静态变量在一块区域，未初始化的全局变量和未初始化的静态变量在相邻的另一块区域。程序结束后由系统释放；
    - 文字常量区：常量字符串就是放在这里，程序结束后由系统释放；
    - 程序代码区：存放函数体的二进制代码。
    
    栈区系统分配,速度快,但是程序员无法控制;堆区用起来方便,但是速度慢,容易产生内存碎片
    

##引用
1. [char *p = "abcdefg"； 常量字符串"abcdefg"位于静态存储区，通过p不能修改该字符串常量](https://blog.csdn.net/Castiellee929/article/details/88628617)    
2. [char *p = "abcd"; 和 char a[] = "abcd"的区别](https://blog.csdn.net/dongfanglanyi/article/details/81386752?depth_1-utm_source=distribute.pc_relevant.none-task&utm_source=distribute.pc_relevant.none-task)    
 