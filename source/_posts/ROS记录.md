---
title: ROS记录
date: 2019-10-07 11:32:36
categories:
- Linux
- ROS
tags:
- NodeHandle
---

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
如果参数服务器存在/a/b的参数，你的NodeHandle在/a/c工作空间，searchParam()搜索b会得到/a/b. 
如果/a/c/b参数增加，搜索就会得到/a/c/b参数。
   
    
    