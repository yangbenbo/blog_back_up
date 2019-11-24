---
title: qt记录
date: 2019-11-22 22:23:21
categories:
- program
tags:
- qt
---

# 常用函数使用
1. QString的arg()函数可以自动替换掉QString中出现的占位符。

        QString("[%1, %2]").arg(x).arg(y);
        // 加粗居中
        QString("<center><h1>Press:(%1, %2)</h1></center>").arg(QString::number(event->x()),QString::number(event->y()));
2. event事件
    **event()函数主要用于事件的分发**
    
    事件处理之后一定要调用父类相应事件处理函数,不然信号就不会继续往后穿,或者使用ignore()函数,如果使用accept()函数就相当如接受信号,不继续传
        
    事件对象创建完毕后，Qt 将这个事件对象传递给QObject的event()函数。event()函数并不直接处理事件，而是将这些事件对象按照它们不同的类型，分发给不同的事件处理器（event handler）
        
        bool CustomWidget::event(QEvent *e)
        {
            if (e->type() == QEvent::KeyPress) {
                QKeyEvent *keyEvent = static_cast<QKeyEvent *>(e);//强制类型转换
                if (keyEvent->key() == Qt::Key_Tab) {
                    qDebug() << "You press tab.";
                    return true;
                }
            }
            return QWidget::event(e);//必要的，重新处理其他事件
        }
    - 如果传入事件被处理,需要返回true,否则返回false,那么 Qt 会认为这个事件已经处理完毕，不会再将这个事件发送给其它对象，而是会继续处理事件队列中的下一事件。
    - 在event()函数中，调用事件对象的accept()和ignore()函数是没有作用的，不会影响到事件的传播。
    
    可以通过使用QEvent::type()函数可以检查事件的实际类型，其返回值是QEvent::Type类型的枚举。我们处理过自己感兴趣的事件之后，可以直接返回 true，表示我们已经对此事件进行了处理；**对于其它我们不关心的事件，则需要调用父类的event()函数继续转发**，否则这个组件就只能处理我们定义的事件了。
3. 绘图
Qt 的绘图系统: 使用QPainter在QPainterDevice上进行绘制，它们之间使用QPaintEngine进行通讯（也就是翻译QPainter的指令）。

绘图设备是指继承QPainterDevice的子类.

- QPixmap   针对屏幕进行了优化,和平台相关,不能对图片进行修改
- QBitmap QBitmap是QPixmap的一个子类，它的色深限定为1(只有两种状态,黑白),可以使用 QPixmap的isQBitmap()函数来确定这个QPixmap是不是一个QBitmap.
- QImage    和平台无关,可以对图片进行修改,可以在线程中绘图,专门为图像的像素级访问做了优化。
- QPicture  保存绘图的状态(**二进制文件**),记录和重现  

QImage与QPixmap之间的转换:
        
        fromImage
        toImage             
               