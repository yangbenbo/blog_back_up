---
title: pyhton 代码规范
date: 2020-12-12 17:01:28
categories:
- Program
tags:
- 代码规范
---


## 编码风格
1. 缩进
    缩进必须使用4个空格，而不是tab，否则容易出现pylint:`bad-indentation.`
    vscode可以将tab转化为空格
    F1 -> 输入indentation space -> 回车  (查看是空格还是tab可以通过选中对应行或者全选)

## c++  vscode 自动格式化
- 设置 -> 搜索 clang format -> 在Clang_format_path 中输入
     
        { BasedOnStyle: Google, UseTab: Never, IndentWidth: 4, TabWidth: 4, BreakBeforeBraces: Custom, AllowShortIfStatementsOnASingleLine: false, IndentCaseLabels: false, ColumnLimit: 100, AccessModifierOffset: -3 }
- 设置 -> 搜索 save format -> 勾选Format on Save

