---
title: Markdown语法
date: 2019-07-14 16:44:31
categories:
- blog
tags:
- program
- blog
mathjax: true
---

<!-- @import "[TOC]" {cmd="toc" depthFrom=2 depthTo=3 orderedList=false} -->

<!-- code_chunk_output -->

- [区块元素](#区块元素)
  - [标题](#标题)
  - [区块引用](#区块引用)
  - [列表](#列表)
  - [代码区块](#代码区块)
  - [分割线](#分割线)
- [区段元素](#区段元素)
  - [链接](#链接)
  - [强调](#强调)
  - [代码](#代码)
  - [图片](#图片)
  - [数学公式](#数学公式)
- [其它](#其它)
  - [自动链接](#自动链接)
  - [反斜杠](#反斜杠)
- [引用](#引用)

<!-- /code_chunk_output -->

## 区块元素

### 标题
Markdown常用语法 =（最高级标题），-（第二阶标题）

	最高级标题
	=
	二级标题
	-

也可以使用#来表示标题
	
	# 最高标题
	## 子标题

### 区块引用

先断好行，< 放在句首  一般标识符和后面内容之间需要**空格**

	> tab 缩进
	> 
	> >支持嵌套
	


### 列表

无序列表使用 * + -，有序列表 数字 + .

	* 无序列表
	1. 有序列表
	

行首出现数字-句点-空白，要避免这样的状况，你可以在句点前面加上反斜杠。

	<1986\. What a great season.
	
### 代码区块

缩进4个空格或者1个制表符，显示成原来的样子

	这是一个普通段落：
    	这是一个代码区块。
	
	
### 分割线

三个以上的* 、 - 、底线 建立分割线，行内没有其它东西，中间可以有空格

	- - -
	***
	——————
	

## 区段元素

### 链接


	This is [an example](http://example.com/ "Title") inline link.
	
	[This link](http://example.net/) has no title attribute.

	
	This is [an example] [1] reference-style link.
然后在文件任意处

	[1]:http://example.com/  "Optional Title Here"

隐式链接

	[Google][]

	[Google]: http://google.com/
	
	
### 强调
使用 * 和 _	包围，**如果强调两边都有空格，就会被当成普通的符号**
	
	**强调文字**

	_strong_
	

### 代码
标记一小段，用  `

	Use the `printf()` function.

代码中间有反引号，可用多个反引开启或者结束代码区域

	``There is a literal backtick ` here.``
	

### 图片
	![Alt text](/path/to/img.jpg)

	![Alt text](/path/to/img.jpg "Optional title")
	
---
### 数学公式
基于Mathjax编写LaTex数学公式,MathJax是一个跨浏览器的JavaScript库，它使用MathML、LATEX和ASCIIMathML标记在Web浏览器中显示数学符号。

1. 标记公式
	公式使用美元符号$标记
	行内公式:公式紧接着文字

		$公式$
	块级公式:公式单独成行

		$$公式$$
2. 上下标
	`^`标示上标, `_`标示下标,上下标多余一个字符则用`{}`括起来作为整体.**上下标可以嵌套**
	如果需要在左右两边都有上下标,用`\sideset`命令
	例如 $\sideset{^1_2}{^3_4}\bigotimes$
3. 分数
	两种方式

		\frac{分子}{分母}	
		分子 \over 分母
	第一种方式中如果分子分母都是单数,则`{}`可以忽略,例如`$\frac12$`表示$\frac12$
4. 括号
	`()` `[]` `|`可以直接表示自己,而`{}`可以用来分组`\{\}`表示自己,也可以使用`\lbrace` 和`\rbrace`来表示
5. 根号
	根号`\sqrt`标记

		\sqrt[开方次数，默认为2]{开方因子}
	非常复杂的表达式用	`$\sqrt{...}^{1/n}$`表示$\sqrt{...}^{1/n}$
6. 省略号
	`\ldots`表示与文本底线对齐的省略号，`\cdots`表示与文本中线对齐的省略号。
7. 矢量
	`\vec`表示矢量

		\vec{矢量值}	
8. 空格
	通常MathJax通过内部策略自己管理公式内部的空间,因此加入空格需要转义,如果需要更大空格,使用`\quad` 与 `\qquad` 
	`$a\ b$` 表示$a\ b$
9. 运算符
	关系运算

		± ：\pm
		×：\times
		÷：\div
		∣：\mid
		∤：\nmid
		⋅⋅：\cdot
		∘：\circ
		∗：\ast
		⨀：\bigodot
		⨂：\bigotimes
		⨁：\bigoplus
		≤：\leq
		≥：\geq
		≠：\neq
		≈：\approx
		≡：\equiv
		∑：\sum
		∏：\prod
		∐：\coprod	
	三角函数

		⊥：\bot
		∠：\angle
		30∘：30^\circ
		sin：\sin
		cos：\cos
		tan：\tan
		cot：\cot
		sec：\sec
		csc：\csc
	对数

		log：\log
		lg：\lg
		ln：\ln

## 其它


### 自动链接


	<http://example.com/>
	

### 反斜杠

输出含有其他意义符号原本符号

	\*literal asterisks\*


## 引用

1. [Markdown 语法说明 (简体中文版)](http://wow.kuapp.com/markdown/)，以及[github 地址](https://github.com/riku/Markdown-Syntax-CN/)
2. [Markdown中文文档](https://markdown-zh.readthedocs.io/en/latest/)
3. [CSDN-markdown语法之如何使用LaTeX语法编写数学公式](https://blog.csdn.net/lanxuezaipiao/article/details/44341645)