---
title: ROS中CMakeLists.txt
date: 2019-10-05 19:25:10
categories:
- Linux
tags:
- ROS

---
# 概述
CMake构建系统通过ROS包中的CMakeList.txt来构建软件包。
在catkin 项目中，CMakeList.txt 符合标准的vanilla CMakeList.txt 格式，但稍微有点不同。

# 整体结构

1. 所需CMake版本(cmake_minimum_required)
2. 软件包名称(project())
3. 查找构建所需的其他CMake / Catkin软件包(find_package())
4. 启用Python模块支持(catkin_python_setup())
5. 消息/服务/动作生成器(add_message_files()，add_service_files()，add_action_files())
6. 生成消息/服务/动作等自定义消息(generate_messages())
7. 指定包的构建信息输出(catkin_package())
8. 要建立的库/可执行文件(add_library()/ add_executable()/ target_link_libraries())
9. 测试(catkin_add_gtest())
10. 安装规则(install())

# 软件版本
    cmake_minimum_required(VERSION 2.8.3)
    
# 软件包名称
可通过变量${PROJECT_NAME}来引用项目名称
    
    project(robot_brain)    
# 查找相关CMake包
指明构建package需要的包，使用catkin_make，所以catkin是必备依赖,在此基础上可能需要其他的包

    find_package(catkin REQUIRED)   
    
    find_package(catkin REQUIRED COMPONENTS nodelet)  #第一种方式 合并写
     
    find_package(catkin REQUIRED)   # 第二种方式 分开写
    find_package(nodelet REQUIRED)
## find-package作用
     
 如果CMake通过find_package找到一个包，则会自动生成有关包所在路径的CMake环境变量，
 环境变量描述了包中头文件的位置，源文件的位置，包所依赖的库文件位置。
 这些变量名称以< PACKAGE NAME >_< PROPERTY >的形式出现：
 
 - < NAME >_FOUND - 如果找到库，则设置为true，否则为false
 - < NAME > _INCLUDE_DIRS或 _INCLUDES - 这个包输出的头文件目录
 - < NAME > _LIBRARIES或 _LIBS - 由包导出的库
 - < NAME > _DEFINITIONS - ?
 - …
     
## 为什么Catkin包是组件形式
Catkin的包并不是catkin的真正组成部分。而catkin采用CMake的组件功能，主要是为了节省打字时间。

    #这种方式 会把组件中的包对应的include路径、库导出到catkin前缀的变量中，
    #例如catkin_INCLUDE_DIRS包含多个包的路径
    find_package（catkin REQUIRED COMPONENTS nodelet）

    find_package(nodelet REQUIRED)  # 而这种方式 路径和库不会导入catkin变量，而是nodelet_INCLUDE_DIRS之类的
##　Boost库
如果使用C ++和Boost，则需要用find_package()来找Boost库，并指定Boost中的组件。如果想使用Boost线程，就可以写成：
    
    find_package（Boost REQUIRED COMPONENTS thread）
#　catkin_package()
将catkin特定的信息输出到构建系统上，用于生成pkg配置文件以及CMake文件        
- INCLUDE_DIRS - 导出包的include路径
- LIBRARIES - 导出项目中的库
- CATKIN_DEPENDS - 该项目依赖的其他catkin项目
- DEPENDS - 该项目所依赖的非catkin CMake项目。
- CFG_EXTRAS - 其他配置选项

        catkin_package(
           INCLUDE_DIRS include
           LIBRARIES ${PROJECT_NAME}　　　
           CATKIN_DEPENDS roscpp nodelet
           DEPENDS eigen opencv)
这表示包文件夹中的文件夹“include”是导出头文件的地方。  
${PROJECT_NAME}根据project中的内容生成，此处是robot_brain
“roscpp”+“nodelet”是用来构建/运行此程序包的catkin包，
而“eigen”+“opencv”是用于构建/运行此程序包的非catkin包。   
# 指定构建目标
## 包含路径和库
1. include_directories()

参数应该是调用find_package调用时生成的* _INCLUDE_DIRS变量。
如果使用catkin和Boost，那么include_directories()调用应该如下所示：
        
    include_directories(include ${Boost_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})
第一个参数“include”表示包中的include /目录也是路径的一部分。
2. link_directories()

用于添加额外的库路径，但通常不推荐这样做，因为 所有catkin和CMake软件包在find_packaged时都会自动添加链接信息。 
只需写target_link_libraries()中就可以了。但真要写，就按照下面那样来写：

    link_directories(~/my_libs)
## 可执行目标
生成可执行程序对应的源文件

    add_executable(myProgram src/main.cpp src/some_file.cpp)
## 库目标
默认的catkin编译产生的共享库

    add_library(${PROJECT_NAME} ${${PROJECT_NAME}_SRCS})　　#指定要构建的库
## target_link_libraries
指定可执行目标链接的库。 通常在add_executable()调用之后完成。 
如果找不到ros，则添加$ {catkin_LIBRARIES}。

    target_link_libraries(<executableTargetName>, <lib1>, <lib2>, ... <libN>)
大多数情况中不需要使用link_directories()，因为find_package()自动拉入。   
# 消息、服务和响应
## 使用条件
    
    # CMakeLists.txt
    find_package(catkin REQUIRED COMPONENTS message_generation ...)
    add_message_files(...)
    add_service_files(...)
    add_action_files(...)
    generate_messages(...)
    catkin_package(
     ...
     CATKIN_DEPENDS message_runtime ...
     ...)
    
    # package.xml文件中加上 
    <build_depend>message_generation</build_depend>
    <run_depend>message_runtime</run_depend>
1. 如果构建的对象依赖于其他需要构建消息/服务/响应的构建对象，
则需要向目标catkin_EXPORTED_TARGETS添加明确的依赖关系，
以便它们以正确的顺序构建。 这种情况比较常用，除非您的包真的不使用ROS的任何部分。 
但这种依赖关系不能自动传递。some_target是由add_executable()设置的目标的名称）    
    
        add_dependencies(some_target${catkin_EXPORTED_TARGETS})
2. 如果需要构建消息或服务的包以及使用这些消息和/或服务的可执行文件，
则需要为自动生成的消息目标创建明确的依赖关系，以便以正确的顺序构建它们。 
（some_target是由add_executable()设置的目标的名称）

        add_dependencies(some_target ${${PROJECT_NAME}_EXPORTED_TARGETS})    

## 例子
现在有两个依赖于std_msgs和sensor_msgs的消息MyMessage1.msg和MyMessage2.msg，
还有一个自定义服务MyService.srv， message_program是使用这些消息和服务的指令，
以及生成不使用自定义消息、服务的程序 do_not_use_local_messages_program 不使用，
那么CMakeLists.txt应该写成：

    # 构建时依赖项
    find_package(catkin REQUIRED COMPONENTS          
                 message_generation 
                 std_msgs 
    
    # 声明要构建哪些消息
    add_message_files(FILES
                      MyMessage1.msg
                      MyMessage2.msg)
    
    # 声明构建哪些服务
    add_service_files(FILES
                      MyService.srv)
    
    # 声明生成上述消息、服务需要依赖的消息以及服务
    generate_messages(DEPENDENCIES 
                        std_msgs 
                        sensor_msgs)
    
    # 声明运行时依赖项
    catkin_package(CATKIN_DEPENDS 
                    message_runtime 
                    std_msgs sensor_msgs)
    
    # 声明构建生成的可执行文件名称以及依赖项
    add_executable(message_program src/main.cpp)
    add_dependencies(message_program ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
    
    # 声明构建不需要使用自定义消息、服务的可执行文件
    add_executable(does_not_use_local_messages_program src/main.cpp)
    add_dependencies(does_not_use_local_messages_program ${catkin_EXPORTED_TARGETS})
# 问题
1. 之前用矩阵运算c++库:Armadillo 直接作为catkin组件时找不到库

解决方案：find_package() 把Armadillo单独拿出来 后面需要的地方链接到库 
    
    find_package(Armadillo 5.4 REQUIRED)
    target_link_libraries(tp_gmr_node ${catkin_LIBRARIES} ${ARMADILLO_LIBRARIES})
# 引用
1. [ROS中的CMakeLists.txt](https://blog.csdn.net/u013243710/article/details/35795841)
2. [catkin CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt#Finding_Dependent_CMake_Packages)
3. [ROS下的CMakeList.txt编写](https://blog.csdn.net/turboian/article/details/74604052)
 
    
  
    
              