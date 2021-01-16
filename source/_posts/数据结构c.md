---
title: 数据结构c++
date: 2020-02-15 09:20:59
categories:
- Program
tags:
- 数据结构

mathjax: true
---

常用复杂度:常数,对数,线性,多项式,指数.
O(1),O(logn)、O(sqrt(n))、O(n)、O(nlogn)、O(n^2)、和O(2^n)

# 输入规模
对算法复杂度的界定都是相对于输入规模而言.

严格地说,所谓待计算问题的输入规模,应严格定义为“用以描述输入所需的空间规模”。

1. 例子
    
        int countOnes(unsigned int n) { //统计整数n癿二迕刢展开中数位1癿总数：O(logn)
            int ones = 0; //计数器复位
            while (0 < n) { //在n缩减至0乀前，反复地
                ones += (1 & n); //检查最低位，若为1则计数
                n >>= 1; //右秱一位
            }
            return ones; //迒回计数
        } //等效亍glibc癿内置函数int __builtin_popcount (unsigned int n)
    现对于n本身数值来说,复杂度为log(n);相对于n的二进制展开的宽度$r=1+\left\lfloor\log _{2} n\right\rfloor$(向下取整),则复杂度为O(r)
    
    这里选择后一种表达方式O(r),前一种称为伪对数复杂度        

# 递归
Fibonacci数:线性递归

    __int64 fib(int n, __int64& prev) { //计算Fibonacci数列第n顷(线性逑弻版):入口形式fib(n, prev)
        if (0 == n) //若刡达逑弻基,则
        
            { prev = 1; return 0; } //直接叏值:fib(-1) = 1, fib(0) = 0
            else { //否则
            __int64 prevPrev; prev = fib(n - 1, prevPrev); //逑弻计算前两顷
            return prevPrev + prev; //其和即为正解  
        }
    } //用辅劣发量记弽前一顷,迒回数列癿弼前顷,O(n)

原二分递归版本中对应于fib(n - 2)的另一次递归,在这里被省略掉了。其对应
的解答,可借助形式参数的机制,通过变量prevPrev“调阅”此前的记录直接获得。

更高效的方式是迭代,这里注意对已经计算出来的变量进行调阅的方式

# 参考
1. [数据结构(c++语言版)第三版_邓俊辉](https://www.xuetangx.com/courses/course-v1:TsinghuaX+30240184+sp/about?jwt_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJvd25lciI6Im5ld194dWV0YW5neCIsInVzZXJfaWQiOjEwODMzMjMzfQ.wmH12o6YjppbHX4TzGdkpjfJHPmG4xSjHlZUBenKAOM&owner=new_xuetangx)