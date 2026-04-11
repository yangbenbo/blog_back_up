---
title: vim常用命令
date: 2019-09-30 14:59:28
categories:
- Linux

tags:
- vim

---

# 常用命令

![模式切换](Vim.png)
## 插入命令

i 光标前输入 

I 行首输入 

a 光标后输入

A 行尾输入

o 当前行下新开一行 

O 当前行上新开一行
## 定位命令
:set nu   显示行号

:set nonu   为取消行号

w       光标移动到下一个单词  5w 5个单词

b       光标移动到上一个单词

gg       跳到文本首行

G       最后一行

nG      到第n行

:n      到第n行

$       移动到行尾

0       移动到行首   ^


## 删除命令

x 删除光标所在处字符

nx  删除光标所在处后n个字符

dd  删除光标所在行,ndd删除n行

dG  删除光标所在行到文件末尾内容

D   删除光标所在处到行尾内容

:n1,n2d 删除指定范围的行

## 复制和剪切

yy  复制当前行

nyy 复制当前行以下n行

dd  剪切当前行

ndd 剪切当前行以下n行

p、P 粘贴在当前光标所在行下或行上

## 替换和取消

r   取代光标所在处字符

R   从光标所在处开始替换字符,按Esc结束

u   取消上一步操作

## 搜索和搜索替换

/string             搜索指定字符串搜索时忽略大小写 :set ic

n                   搜索指定字符串的下一个出现位置

:%s/old/new/g       全文替换指定字符串

:n1,n2s/old/new/g   在一定范围内替换指定字符串

:set ignorecase     搜索是忽略大小写 noignorecase

## 保存和退出

:w          保存修改

:w new_name 另存为指定文件

:wq         保存修改并退出

ZZ          快捷键,保存修改并退出

:q!         不保存修改退出

:wq!        保存修改并退出(文件所有者及root可使用)

# 技巧
导入命令执行结果 :r !命令

定义快捷键 :map 快捷键 触发命令

范例:    

    : map ^P I#<ESC>
    : map ^B 0x
连续行注释 

    :n1,n2s/^/#/g
    :n1,n2s/^#//g
    :n1,n2s/^/\/\//g
替换 

    :ab mymail samlee@lampbrother.net
# 参考
1. 兄弟连Linux