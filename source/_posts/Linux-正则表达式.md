---
title: Linux 正则表达式
date: 2019-11-02 09:22:39
categories:
- Linux
tags:
- 正则表达式
---

# Linux 的正则表达式
Linux有两套库可以使用POSIX PCRE 前面一种是Linux自带的
1. 分隔符-分隔单词

        \< \>
        \<[a-z]at\>     # 后面这种行也会被匹配到  %$bat!* 
    "单词"指的是两侧由非单词字符分隔的字符串.非单词字符指字母 数字 下划线之外的任何字符
2. 位置匹配
    
        ^a      # a开头
        t$      # t结尾
    
3. 重复
    
        *       重复0次或多次
        +       重复1次或多次
        ?       重复0次或者1次
        {n}     重复n次
        {n,}    重复n次或者更多
        {n,m}   重复n-m次
4. 子表达式-分组
        
        egrep "(or){2,}"  xx/xx     # or重复2次或更多
5. 反义

    放在[]中,注意与匹配开头的 ^ 区别
            
        ^[^y]   # 不以字母y开头的所有行
6. 分支
    正则表达式简单执行"与"的组合 使用| 或者
    
        ^h|t&   # h开头或者t结尾
7. 转义
    \ 取消所有元字符的特殊含义
    
        \.  # 匹配.
8. 逆向引用

    子表达式(分组)捕获的内容可以在正则表达式的其他地方使用  使用 \1 表示引用第一个分组捕获内容
        
        (\<.*\>).?( )*\1    # 表示 匹配 某个单词出现后 紧跟0或1个标点符号以及任意多个空格后再次出现这个单词的行                                        
            