---
title: Markdown语法
date: 2019-07-14 16:44:31
categories:
- program
tags:
- program
- blog

---

区块元素
===

标题
---
Markdown常用语法 =（最高级标题），-（第二阶标题）

	最高级标题
	=
	二级标题
	-
	

区块引用
---
先断好行，< 放在句首  一般标识符和后面内容之间需要**空格**

	> tab 缩进
	> 
	> >支持嵌套
	


列表
---
无序列表使用 * + -，有序列表 数字 + .

	* 无序列表
	1. 有序列表
	

行首出现数字-句点-空白，要避免这样的状况，你可以在句点前面加上反斜杠。

	<1986\. What a great season.
	
代码区块
---
缩进4个空格或者1个制表符，显示成原来的样子

	这是一个普通段落：
    	这是一个代码区块。
	
	
分割线
---
三个以上的* 、 - 、底线 建立分割线，行内没有其它东西，中间可以有空格

	- - -
	***
	——————
	

区段元素
===
链接
---

	This is [an example](http://example.com/ "Title") inline link.
	
	[This link](http://example.net/) has no title attribute.

	
	This is [an example] [1] reference-style link.
然后在文件任意处

	[1]:http://example.com/  "Optional Title Here"

隐式链接

	[Google][]

	[Google]: http://google.com/
	
	
强调
---
使用 * 和 _	包围，**如果强调两边都有空格，就会被当成普通的符号**
	
	**强调文字**

	_strong_
	

代码
---
标记一小段，用  `

	Use the `printf()` function.

代码中间有反引号，可用多个反引开启或者结束代码区域

	``There is a literal backtick ` here.``
	

图片
---
	![Alt text](/path/to/img.jpg)

	![Alt text](/path/to/img.jpg "Optional title")
	

其它
===

自动链接
---

	<http://example.com/>
	

反斜杠
---
输出含有其他意义符号原本符号

	\*literal asterisks\*


引用
===
1. [Markdown 语法说明 (简体中文版)](http://wow.kuapp.com/markdown/)，以及[github 地址](https://github.com/riku/Markdown-Syntax-CN/)
2. [Markdown中文文档](https://markdown-zh.readthedocs.io/en/latest/)