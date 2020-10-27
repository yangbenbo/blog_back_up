---
title: word排版
date: 2019-12-14 21:39:40
categories:
- software
tags:
- matlab绘图

---

# 点(point,pt)
是印刷所使用的长度单位，用于表示字型的大小，也用于余白（字距、行距）等其他版面构成要素的长度。

![中文字号](中文字号.png)



	
# 使用word模板样式
可将模板中的样式导入到需要用的文档

![导出样式](导出样式.png)
# 中英文对照
学术上专业用语对照查询
[cnki翻译助手](http://dict.cnki.net/dict_result.aspx)
# 图表
**word中千万不要手动拖动图片大小**

在visio 设置图纸大小和字号(pt)

mathtype设置好字号，公式、字母直接复制visio中

图片插入到word后不要拖动调整大小

ps中同理设置字号，图像大小
# 公式
mathtype 可以插入公式序号 引用(双击公式序号) 

可以先在mathtype设置好格式，存储为样板(10pt)

# 批量修改word图片为灰度图或彩图
Alt+F11 进入宏界面->插入->模块->粘贴下列代码->F5运行

    Sub xx()
    Dim i As Integer
    For i = 1 To ActiveDocument.InlineShapes.Count
    If ActiveDocument.InlineShapes(i).Type = 3 Then
    ActiveDocument.InlineShapes(i).PictureFormat.ColorType = msoPictureGrayscale
    End If
    Next
    End Sub

几种样式如下
- 自动：msoPictureAutomatic
- 黑白：msoPictureBlackAndWhite
- 灰度：msoPictureGrayscale
- 冲蚀：msoPictureMixed
- 水印：msoPictureWatermark

# 参考文献 

{% post_link word参考文献交叉引用 交叉引用 %}

# 引用
1. 维基百科 [点 (印刷)](https://zh.wikipedia.org/wiki/%E9%BB%9E_(%E5%8D%B0%E5%88%B7))
2. [Word文档里的图片怎么批量修改图片颜色灰度变回彩色](https://wenwen.sogou.com/z/q807873456.htm)