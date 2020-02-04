---
title: c++函数指针
date: 2019-11-16 16:51:52
categories:
- program
tags:
- c++
---

# 定义函数指针
定义一个指针指向一个无参数无返回值的函数

    void (*funcPtr)();
**大多数申明都是以右-左-右动作的方式工作**

针对复杂的定义,最好处理方式是从中间开始向外扩展.
"中间开始"("funcPtr是一个..."),向右(没有东西,被括号挡住了),向左发现"*"("...指针指向一个..."),向右发现空参数列表("...没有带参数的函数..."),
向左发现void("funcPtr是一个指针,它指向一个不带参数并返回void的函数)

这里*funcPtr需要括号是避免混淆 有void 指针 
    void *funcPtr();    //一个返回void指针的函数,而不是一个变量
# 复杂定义
    void * (*(*fp1)(int))[10];
    
    float (*(*fp2)(int,int,float))(int);
    
    typedef double (*(*(*fp3)())[10])();
    fp3 a;    
    
    int (*(*fp4())[10])();
- fp1 是一个指向函数的指针,接受int参数,返回一个指向含有10个void指针数组的指针    
- fp2 是一个指向函数的指针,该函数接受int,int,double,返回一个指向函数的指针,该函数接受int,返回float
- fp3 指向函数的指针,该函数无参数,返回一个指向含有10个指向函数指针数组的指针,这些函数不接受参数且返回double 又定义a是ftp3类型的一个
- fp4 是一个返回指针的函数,改指针指向含有10个函数指针的数组,这些函数返回整型值    

# 使用函数指针
函数func()的地址 就是func 或者使用更加明显的语法&func()
    
    void (*fp)() = func;
指向函数的指针数组
    
    #define DF(N) void N() { cout << "function " #N" called..." <<endl; }
    
    DF(a);DF(b);DF(c);
    void (*func_table[])() = {a,b,c};
            
             
    


# 引用
1. [C++编程思想](https://book.douban.com/subject/6558198/)