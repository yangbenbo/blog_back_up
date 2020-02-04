---
title: 'QT坐标系统 '
date: 2020-02-03 17:31:33
categories:
- program
tags:
- qt
---

QPaintDevice、QPaintEngine和QPainter是 Qt 绘制系统的三个核心类

坐标系统，也就是QPaintDevice上面的坐标。默认坐标系统位于设备的左上角，也就是坐标原点 (0, 0)。x 轴方向向右；y 轴方向向下。在基于像素的设备上（比如显示器），坐标的默认单位是像素，在打印机上则是点（1/72 英寸）.

为了避免获取不到真实的坐标值,建议使用QRectF.这个类的两个函数QRectF::right()和QRectF::bottom()是正确的。

QPainter是一个状态机.QPainter提供了内置的函数：save()和restore()。save()就是保存下当前状态；restore()则恢复上一次保存的结果。这两个函数必须成对出现：QPainter使用栈来保存数据，每一次save()，将当前状态压入栈顶，restore()则弹出栈顶进行恢复.

# 坐标变换
QPainter:逻辑坐标 QPaintDevice:物理坐标

QPainter,window代表窗口坐标,viewport代表物理坐标,这两个是映射关系(坐标scale)

setWindow() 设置窗口,实际绘制窗口;
setViewport() 设置物理绘制区域

**QPainter使用的都是逻辑坐标,注意和物理坐标的对应关系**

![坐标变换关系](coordinate.png)

QPainter传入的是逻辑坐标（也称为世界坐标），逻辑坐标可以通过变换矩阵转换成窗口坐标，窗口坐标通过 window-viewport 转换成物理坐标（也就是设备坐标）.

整个回执流程可以看成直接在设置的setWindow()里面绘制,可能需要平移旋转绘制坐标系,所得图像经过物理坐标和窗口坐标的缩放比例进行缩放得到
    
    painter.translate(400, 0); // 绘制用坐标系向右平移 400px
     
# 引用
1. [坐标系统](https://www.devbean.net/2012/11/qt-study-road-2-coordinate-system/)
