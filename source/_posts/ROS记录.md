---
title: ROS记录
date: 2019-10-07 11:32:36
categories:
- ROS
tags:
- NodeHandle
---

## 基本操作
**ros需要等待初始化完成，否则直接在话题上发布可能导致接受不到**
1. urdf
通过SolidWorks插件导出的URDF文件，它默认使用的碰撞检测模型和可视化模型是一样的。为了提高运动规划的执行速度，你可以使用MeshLab来简化模型（.stl或.dae零件）的点和面。

        check_urdf test.urdf //检查模型
        urdf_to_graphiz test.urdf //创建pdf文件,说明关节信息,方便查看
        roslaunch urdf_tutorial display.launch model:=<RELATIVE_PATH_TO_URDF> // rviz中查看urdf模型
2. 查看tf

        rosrun tf view_frames
        evince frames.pdf
        rosrun tf tf_echo [reference_frame] [target_frame]
        rosrun rviz rviz -d `rospack find turtle_tf`/rviz/turtle_rviz.rviz
3. 环境变量

        env |grep ROS
4. 参数服务器
指定参数类型

        std::string s;
        n.param<std::string>("my_param", s, "default_value");  //有时编译器需要字符串类型的提示
5. solidworks导出urdf
通过SolidWorks插件导出的URDF文件，它默认使用的碰撞检测模型和可视化模型是一样的。为了提高运动规划的执行速度，你可以使用MeshLab来简化模型（.stl或.dae零件）的点和面。

## 私有名称
NodeHandle创建时有自己的命名空间,访问私有名称

    ros::init(argc, argv, "my_node_name");
    ros::NodeHandle nh1("~");               //    命名空间/my_node_name
    ros::NodeHandle nh2("~foo");            //    命名空间/my_node_name/foo
    
    // 可以把这两行换成下面的
    ros::NodeHandle nh;
    nh.getParam("~name", ... );
    
    ros::NodeHandle nh("~");
    nh.getParam("name", ... );     //  /my_node_name/name
常用参数设置
    
    getParam()   
    param()
    setParam()
    deleteParam() 
    hasParam()
    searchParam()
    // 有时需要提示类型
    n.param<std::string>("my_param", s, "default_value");
如果参数服务器存在/a/b的参数，你的NodeHandle在/a/c工作空间，searchParam()搜索b会得到/a/b. 
如果/a/c/b参数增加，搜索就会得到/a/c/b参数。

## 设置消息级别
1. 源代码加入
        
        #define ROSCONSOLE_MIN_SEVERITY ROSSONSOLE_SEVERITY_ERROR   //error等级
2. CmakeLists.txt
    
        add_definition(-ROSCONSOLE_MIN_SEVERITY = ROSSONSOLE_SEVERITY_ERROR)
3. 配置文件内容 比较灵活　不需要重新编译
        
        log4j.logger.ros.package_name=ERROR
        <env name="ROSCONSOLE_CONFIG_FILE" value = "$(find package_name)/config/xxx.config" />                                
4. 命名的消息可以单独设置日志级别　名字可以是一个模块或者其他

        ROS_INFO_STREAM_NAMED("named_msg", "my message");
        log4j.logger.ros.package_name.named_msg=ERROR
5. 按条件显示消息和过滤

        ROS_INFO_STREAM_COND(val<0, "my message");
        ROS_INFO_STREAM_FILTER()    #暂时用不到　待补充
6. 显示消息的方式　单次　可调(频率)　组合

        ROS_INFO_STREAM_ONCE("my message");
        ROS_INFO_STREAM_THROTTLE(2, "my message");  //每隔两秒输出
7. rqt_console  rqt_logger_level　运行时修改级别

8. 检测功能包所有原件潜在问题，出现异常情况 roswtf
    
        roscd package_name
        roswtf    
9. 消息记录
    
        rosbag record -a   #记录所有
        <node pkg="rosbag" type="record" name="bag_record" args="/topic_name" />  #消息记录包默认在~/.ros路劲下　除非使用-o(前缀) -O(文件命名)
        rosbag play xxx.bag

## 节点生命周期
- 当第一个ros::NodeHandle创建时候，会调用ros::start()
- 最后一个ros::NodeHandle销毁时，会调用 ros::shutdown() (这将关闭所有打开的订阅、发布、服务调用和服务服务器)



# 遇到的问题        
## remap 重映射
这个坑了我好久

**作用范围**：范围内随后的所有声明(<launch> <node> <group>)

1. 重命名已经存在的话题
        
        <!-- 已经存在话题moveit_group/joint_states move_group需要订阅joint_states-->
        <launch>
            <node pkg="catch_control" type="joint_state_maintain_node" name="joined_joint_state_publisher" respawn="true" output="screen">
                    <remap from="moveit_group/joint_states" to="joint_states"/>
            </node>
        </launch>

2. 将别的话题映射到自己的订阅的主题上
特别要注意这种方式的使用　**作用范围很大**
        
        <!-- 已经存在话题moveit_group/joint_states move_group需要订阅joint_states-->
        <launch>
            <remap from="joint_states" to="moveit_group/joint_states"/>
            <include file="$(find UR5e_robotiq_moveit_config)/launch/move_group.launch" />
        </launch>
这种方式还是出现了下面问题　但是用第一种结合异步更新没有出现

    Failed to fetch current robot state
    
    Didn't received robot state (joint angles) with recent timestamp within 1 seconds.
    Check clock synchronization if your are running ROS across multiple machines!       
        
简单理解：**remap就是把作用范围内的名称直接替换掉 用替换掉的名称对外开放 实现两个话题通信** 

所有ROS节点内的资源名称都可以在节点启动时进行重映射。
命名重映射采用语法：`name:=new_name`

例如将chatter重映射为/wg/chatter，在节点启动时可以使用如下方式重映射命名：

        rosrun rospy_tutorials talker chatter:=/wg/chatter

ROS的命名解析是在命名重映射之前发生的。所以当我们需要“foo：=bar”时，会将节点内的所有foo命名映射为bar，而如果我们重映射“/foo：=bar”时，ROS只会将全局解析为/foo的名称重映射为bar。
可以通过下表总结命名重映射与命名解析之间的关系。
![命名重映射](name_remap.png)

    
## Failed to fetch current robot state

    ros::AsyncSpinner async_spinner(6);
    async_spinner.start();
    
    async_spinner.stop();   #需要关闭的时候用
节点初始化时加上这句　异步处理更新回调函数　感觉可能是moveit内部节点回调函数处理  

## clion编译经常卡死
查看内存和cpu使用率都有盈余，后来发现是因为launch了很多节点，发布了很多数据，关闭就好了  


# 引用
1. [ROS中Remap标签详解,举例说明其两种用法](https://blog.csdn.net/xingdou520/article/details/85686752)