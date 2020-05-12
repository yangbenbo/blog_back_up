---
title: Linux + Qt5 + ROS配置Qwtplot3d
date: 2020-05-12 20:09:33
categories:
- Linux
tags:
- Qt
- ROS
---

qwtplot3D是一个Qt第三方库,在OpenGL基础上封装了一层可以用来三维绘图.

##　Qt项目使用QwtPlot3d
1. 安装OpenGL(QwtPlot3d需要OpenGL作为基础)
    这一步网上很多教程,我之前装过,这里略过
2. 下载QwtPlot3d压缩包
    
    a. https://sourceforge.net/projects/qwtplot3d/此链接下载qwtplot3d-0.2.7.zip，使用Qt编译时会出现问题，具体修改措施参考：https://blog.csdn.net/eastonwoo/article/details/37658141
    b. https://sourceforge.net/p/qwtplot3d/code/HEAD/tarball?path=/branches/multiple_curves_0_2_x 此链接下载qwtplot3d-code-r259-branches-multiple_curves_0_2_x.zip，编译顺利进行。
    
    我用的第一种方式,出现的问题 
    
        ‘gluErrorString’ was not declared in this scope err = gluErrorString(errcode);
    解决方式
        
           在include/qwt3d_openglhelper.h这个文件里添加 #include <GL/glu.h>
           打开 qwtplot3d.pro , 在最前面输入下面这一句  LIBS += -lGLU            
3. 打开qwtplot3d.pro, 选择Release
    
    编译后在编译文件夹找到
        
        libqwtplot3d.so  
        libqwtplot3d.so.0  
        libqwtplot3d.so.0.2  
        libqwtplot3d.so.0.2.6
    复制这四个文件到存放lib文件的目录,可以自己指定,我的是
    
        /opt/qt59/lib/x86_64-linux-gnu/qtcreator/
4. 打开qwtplot3d/examples/simpleplot/simpleplot.pro

        # 将第一句改为这二句 其实就是指定刚才生成的动态链接库的位置
        # 其实把库文件放在原本指定的位置../../lib也可以编译成功
        unix:LIBS += -lqwtplot3d -L../../lib
        unix:LIBS += -L/opt/qt59/lib/x86_64-linux-gnu/qtcreator/ -lqwtplot3d
        
    其他项目使用需要生成的库文件.so 以及头文件 .h(qwtplot3d/include下),
    simpleplot只用指定库是因为项目中指定了头文件位置
            
    运行程序如下      
    ![simpleplot](simpleplot.png)       
## ROS CmakeLists.txt使用QwtPlot3d
    
    # 把头文件拷贝到include/qwt 下并包含
    include_directories(${catkin_INCLUDE_DIRS}
        include/qwt)
    # 寻找OpenGL 这是底层库
    find_package(Qt5 COMPONENTS Core Widgets OpenGL REQUIRED)
    # 设置库文件路径
    set(QT_LIBRARIES Qt5::Widgets Qt5::OpenGL)#added
    # 生成可执行程序并链接库
    add_executable(qwtdemo  src/traj_graph.cpp)
    target_link_libraries(qwtdemo ${QT_LIBRARIES} ${catkin_LIBRARIES}
        qwtplot3d )   
注意到上面只处理了头文件包含,对于库文件有多种处理方式,库文件又分为静态库和动态库,可参考[Linux 中的动态链接库和静态链接库是干什么的？](https://www.zhihu.com/question/20484931/answer/69553616)
    
1. 指定相对位置(推荐,成功率高)
        
        # 把库文件.so复制到项目内与src同级目录lib, 然后在CmakeLists.txt指定相对位置
        LINK_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/lib)   ## 添加动态库路径

2. 指定绝对位置
    
        # CmakeLists.txt 添加库文件路径(之前存放的路径)
        LINK_DIRECTORIES(/opt/qt59/lib/x86_64-linux-gnu/qtcreator/)   ## 添加动态库路径
        
3. 添加路径到变量LD_LIBRARY_PATH
    
        # ~/.bashrc 下添加下面这句,然后重开终端启动qtcreator 但是我没成功
        export LD_LIBRARY_PATH=/opt/qt59/lib/x86_64-linux-gnu/qtcreator:${LD_LIBRARY_PATH}
4. 修改/etc/ld.so.conf,添加路径,运行sudo ldconfig命令
    
    这是一个系统动态链接库路径配置文件,我尝试过/etc/ld.so.conf 添加库文件路径
    以及在指定的目录下增加XXX.conf都不行,最后是直接拷贝到包含的一个目录下成功的(/usr/lib/x86_64-linux-gnu/)
                        
## 引用
1. [Ubuntu Linux 16.04 LTS + Qt5.5.1 + Qwtplot3d配置安装](https://blog.csdn.net/qq_41800188/article/details/87891586)
2. [QT5 r 加入qwtplot3d 三维库](https://blog.csdn.net/EastonWoo/article/details/37658141)
3. [linux下添加动态链接库路径的方法](https://blog.csdn.net/zxh2075/article/details/54629318?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.nonecase)
                                          