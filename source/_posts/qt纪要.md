---
title: qt记录
date: 2019-11-22 22:23:21
categories:
- program
tags:
- qt
---

不清楚多看qt帮助文档,介绍非常详细

# 常用函数使用
1. QString的arg()函数可以自动替换掉QString中出现的占位符。

        QString("[%1, %2]").arg(x).arg(y);
        // 加粗居中
        QString("<center><h1>Press:(%1, %2)</h1></center>").arg(QString::number(event->x()),QString::number(event->y()));
        
        // 数字转字符串也可以用
        QString::number(num);
2. 打印
    打印出错信息
        
        #define cout qDebug() << "[" << __FILE__ << ":" << __LINE__ <<"]"
    \n正常表示(换行)
        
        qDebug().noquote() << str;  //直接用qDebug() \r和\n都不能正常显示             
3. event事件
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
    
    **注意:**QWidget中有一个mouseTracking属性,该属性用于设置是否追踪鼠标,默认是false(至少鼠标点击一次才会追踪).只有鼠标被追踪时,mouseMoveEvent()才会发出.
    
        EventLabel *label = new EventLabel;
        label->setMouseTracking(true);
4. 控制调用触发信号
    比如模拟按钮点击效果
        
        ui.pushButton->clicked();  
    获取所有button
    
        QList<QPushButton *>  m_button_list = this->findChildren<QPushButton *>();
                      
        
# 绘图
Qt 的绘图系统: 使用QPainter在QPainterDevice上进行绘制，它们之间使用QPaintEngine进行通讯（也就是翻译QPainter的指令）。

绘图设备是指继承QPainterDevice的子类.

- QPixmap   针对屏幕进行了优化,和平台相关,不能对图片进行修改
- QBitmap QBitmap是QPixmap的一个子类，它的色深限定为1(只有两种状态,黑白),可以使用 QPixmap的isQBitmap()函数来确定这个QPixmap是不是一个QBitmap.
- QImage    和平台无关,可以对图片进行修改,可以在线程中绘图,专门为图像的像素级访问做了优化。
- QPicture  保存绘图的状态(**二进制文件**),记录和重现  

QImage与QPixmap之间的转换:
    
    fromImage
    toImage 
        
## Graphics View Framework:提供了一种接口，用于管理大量自定义的 2D 图形元素，并与之进行交互
    
如果你的图像中包含了成千上万的直线、多边形之类，管理这些对象要比管理QPainter的绘制语句容易得多。并且，
这些图形对象也更加符合面向对象的设计要求：一个很复杂的图形可以很方便的复用。

        
# 非模态对话框析构
设置对话框属性即可,避免子函数结束造成对话框对象析构,
或者堆上创建对象一直存在(设置父对象容易造成对话框一直占用资源)

    dialog->setAttribute(Qt::WA_DeleteOnClose);

# 文件
读取:
- QDataStream:二进制文件读取  
- QTextStream:自动将 Unicode 编码同操作系统的编码进行转换;也会将换行符进行转换 

有关文件本身信息,可以通过QFileInfo获取
    
    QFileInfo info(file);
    qDebug() << info.baseName();                        
    // 获取应用程序执行时的当前路径
    QDir::currentPath()

**最好使用 Qt 整型来进行读写,比如程序中的qint32.
这保证了在任意平台和任意编译器都能够有相同的行为.**

## 魔术数字
二进制输出中经常使用的一种技术,放在文件开头用于标示文件

二进制格式是人不可读的，并且通常具有相同的后缀名(比如 dat 之类),
因此我们没有办法区分两个二进制文件哪个是合法的.
所以，我们定义的二进制格式通常具有一个魔术数字，用于标识文件的合法性.

## tcp实现简单文件传输
**数字和特定长度字符串相互转换**
        
    QString str = QString("%1").arg(10, 4, 10, QChar('0'));
    qDebug() <<str<<"num "<<str.toInt();
为防止TCP粘包,通常在开头增加一个或一组固定长度数字表示数据包的大小,类型(类型可用enum+switch)等

    //封包
    quint16 totalLen = numLen + buff.size();  //数据位长度可自定义,和一次读取大小有关
    QString lenStr = QString("%1").arg(totalLen, numLen, 10, QChar('0'));
    buffSend.append(lenStr).append(buff);
    tcpsocket->write(buffSend);
    buffSend.clear();
    //解包
    quint16 lenMsg = QString(mybuffer.left(4)).toInt();
    QByteArray msg = mybuff.mid(numLen, lenMsg-numLen);
    mybuff = mybuff.right(mybuff.size()-lenMsg);  //总缓存区
其实这里有考虑过使用QDataStream,但是由于通过"<<"写入会有其他信息,
导致大小不是想要的,通常使用writeRawData来避免  

## 多线程
    
    robotarm = new robotArm();  //robotArm继承QObject
    connect(ui.pushButton, &QPushButton::clicked, robotarm, &robotArm::InitPos);
    QThread *pthread = new QThread(this);
    robotarm->moveToThread(pthread);
    pthread->start();

## Qt信号自定义数据类型
原生的信号与槽连接传参，只支持基本的数据类型，比如char,int, float,double.
直接使用自定义信号,比如结构体,类等编译不会报错,执行会报错
有两种方法
1. 注册自定义类型
    
        #include<QMetaType>       
        
        qRegisterMetaType<joydata>("joydata"); //自定义类型对应的变量和名称
2. 使用直接连接方式`Qt::DrectConnection`,这种方式在需要把耗时槽函数放在子线程不适用(队列连接)
        
        connect(&subThread,SIGNAL(ui.pushButton, &QPushButton::clicked, robotarm, &robotArm::InitPos, Qt::DrectConnection);
        
## QSharedMemory实现进程间通信
分享端的逻辑：

1.创建一个QSharedMemory，并设置一个key值；

2.查看这个内存是不是被使用，如果被使用就断开；

3.调用create进行创建；(即使报已经存在也没关系,连接上照样使用)

4.使用memcpy把要写入的数据放入QSharedMemory中（内部一般会使用互斥锁技术，锁住资源）

 

读取端逻辑：

1.创建一个QSharedMemory，把key值设置为分享端相同的key值；

2.使用attach连接上这个QSharedMemory；(**先调用isAttached()判断是否需要attach，如果已经attach则再调用attach会返回false**)

3.以读取字节的方式读取QSharedMemory中的数据（内部一般会使用互斥锁技术，锁住资源）；

4.使用detach端口对这个QSharedMemory的连接。        

**注意：分享端create后会自动attach上,不用调用detach,否则共享内存直接释放掉了.
接收端每次attach,detach.需要保证始终有一个进程attach在sharedmemory上,通常是分享的一端**

分享端:
    
    m_shared_memory = new QSharedMemory("shared", this);
    if(m_shared_memory->isAttached()){
        if(!m_shared_memory->detach()){
            qDebug() << "shared memory detach failed";
            return;
        }
    }
    
    if(!m_shared_memory->create(1)){
        qDebug()<<m_shared_memory->errorString()<<" created failed!";
        return;
    }
    
    m_shared_memory->lock();
    char *to = static_cast<char*>(m_shared_memory->data());
    char msg = 'L';
    memcpy(to, &msg, 1);
    m_shared_memory->unlock();
数据接受端:
    
    m_shared_memory = new QSharedMemory("shared", this);
    if(!m_shared_memory->isAttached()){
        if(!m_shared_memory->attach()){
            qDebug() << "attach shared memory failed";
            return;
        }
    }
    m_shared_memory->lock();
    char *to = static_cast<char*>(m_shared_memory->data());
    char msg = 'L';
    memcpy(to, &msg, 1);
    m_shared_memory->unlock();
    m_shared_memory->detach();

## 引用
1. [Qt信号与槽传递自定义数据类型——两种解决方法](https://www.cnblogs.com/tid-think/p/9300457.html)
2. [Qt工作笔记-进程间的通信（通过QSharedMemory）](https://it1995.blog.csdn.net/article/details/81738155?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-5.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromBaidu-5.control)                 
                         