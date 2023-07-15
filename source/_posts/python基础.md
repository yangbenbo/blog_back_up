---
title: python基础
date: 2020-05-22 16:52:49
categories:
- Program
tags:
- python
---

很多简单模块容易忘,在此记录一下

## 基础操作

1. 查看帮助文档

        import os
        # 主要有3中类型 ,也可以针对具体函数
        dir(os)
        help(os) 
        os.__doc__
2. 查看变量内存地址

        # 需要注意有时候变量的复制是浅拷贝,地址相同的
        # 使用pybullet获取机械臂位置就是这样,机械臂运动之后地址才变了
        print(id(object))
3. 查看当前路径

        import os
        os.getcwd()
4. copy模块

	- 注意python具有即时复制的功能

			a = 1
			b = a
			print(id(a), id(b))
			b = 2
			print(id(a), id(b))

	- 有些模块如numpy没有特别设置不会深拷贝,通常配合copy.deepcopy()

5. python3中@
	矩阵乘法

		Op	Precedence/associativity	Methods
		@	Same as *					__matmul__, __rmatmul__
		@=	n/a							__imatmul__

6. numpy数组交换行列
    
        a = np.array([[1,2,3],[2,3,4],[1,6,5], [9,3,4]])
        a[:,[1,0,2]]
     		

## 安装模块

1. 常用安装命令

        # 安装 conda环境可以使用conda install xxx
        pip install package_name
        pip install package_name==package_version
        # 更新
        pip install -U package_name
        # 显示模块的信息
        pip show package_name
        # 查看对应包简单信息
        pip list |grep package_name
        # 卸载建议这种方式,不会会有残留文件(针对pip install -e .)
        pip uninstall package_name  

2. pip install -e . VS python setup.py install

        Editable   pip                    setup.py
        yes        pip install -e .       python setup.py develop    
        no         pip install .          python setup.py install   

- 可编辑模式安装(develop,可以在当前文件基础上修改)
  
  这种方式会在site-packages下创建一个文件指向项目目录,并不会复制对应代码;
  在项目根目录(setup.py所在)会生成egg-info 文件夹包含这个包的主要信息;
  查看sys.path会发现里面多了一个项目根目录的路径
  
        pip install -e /path/to/your/setup.py  # 项目根目录                  
- install 这种方式会在site-packages生成egg文件,里面包含对应的函数和模块,无法修改

## python2和3用法区别
1. super函数
    super() 函数是用于调用父类(超类)的一个方法。
    super() 是用来解决多重继承问题的，直接用类名调用父类方法在使用单继承的时候没问题，但是如果使用多继承，会涉及到查找顺序（MRO）、重复调用（钻石继承）等种种问题。
    
    python3 可以使用直接使用 super().xxx 代替 super(Class, self).xxx :
    
        #!/usr/bin/python
        # -*- coding: UTF-8 -*-
         
        class A(object):   # Python2.x 记得继承 object
            def add(self, x):
                 y = x+1
                 print(y)
        class B(A):
            def add(self, x):
                super(B, self).add(x)  # python3 直接super().add(x)
        b = B()
        b.add(2)  # 3

## pybullet 结合 ROS
将pybullet中获取的图像通过变换获得对应的点云图
{% pdf OPGL_pointcloud.pdf %}
1. Image  numpy array to sensor_msgs/Image
        
        from cv_bridge import CvBridge
        import numpy as np
        from sensor_msgs.msg import Image
        
        # 确保能都找到正确的opencv2的位置
        import sys
        sys.path.remove('/opt/ros/kinetic/lib/python2.7/dist-packages')
        import cv2
        
        rgb_np_arr, depth_np_arr, seg_mask_np_arr, point_cloud = self.camera.get_camera_frame()
        bridge = CvBridge()
        image_message = bridge.cv2_to_imgmsg(rgb_np_arr[:, :, [2, 1, 0, 3]].astype('uint8'))  # default bgra8
        
2. Pointcloud numpy to sensor_msgs/PointCloud2     
    
        import ros_numpy
        import numpy as np
        from sensor_msgs.msg import PointCloud2, PointField
        
        number = point_cloud.shape[0] * point_cloud.shape[1]
        point_cloud_arr = point_cloud.reshape([number, 3])
        data = np.zeros(number, dtype=[
            ('x', np.float32),
            ('y', np.float32),
            ('z', np.float32)
        ])
        data['x'] = point_cloud_arr[:, 0]
        data['y'] = point_cloud_arr[:, 1]
        data['z'] = point_cloud_arr[:, 2]

        cloud_msg = ros_numpy.msgify(PointCloud2, data)
        cloud_msg.header.frame_id = "map"
        
3. pointcloud from image deepth
    reshape 默认是按行排列, 按列需要order='F'
    
    矩阵求逆 np.linalg.inv(x)
    
    矩阵乘积 np.matmul(a, b)
        
        stepX = 10
        stepY = 10
        pointCloud = np.empty([np.int(self.img_height/stepY), np.int(self.img_width/stepX), 4])
        projectionMatrix = np.asarray(self.projection_matrix).reshape([4,4],order='F')
        viewMatrix = np.asarray(view_matrix).reshape([4,4],order='F')
        tran_pix_world = np.linalg.inv(np.matmul(projectionMatrix, viewMatrix))
        for h in range(0, self.img_height, stepY):
            for w in range(0, self.img_width, stepX):
                x = (2*w - self.img_width)/self.img_width
                y = -(2*h - self.img_height)/self.img_height  # be careful！ deepth and its corresponding position
                z = 2*depth_np_arr[h,w] - 1
                pixPos = np.asarray([x, y, z, 1])
                position = np.matmul(tran_pix_world, pixPos)

                pointCloud[np.int(h/stepY),np.int(w/stepX),:] = position / position[3]          

## python代码中查看当前编译器版本
        
    import sys
    print(sys.version)  # python2      

## 脚本中指定编译器
相当于写死了python3路径;

    #!/usr/bin/python3
去环境设置寻找python3目录,推荐这种写法
    
    #!/usr/bin/env python3      

## 正则表达式
使用正则表达式可以非常方便从文本中获取到需要的值, 相对matlab速度更快
[正则表达式 - 教程](https://www.runoob.com/regexp/regexp-tutorial.html), 验证正则表达式是否正确参考[正则表达式在线测试|菜鸟工具](https://c.runoob.com/front-end/854/)

- 字符匹配
普通字符：普通字符按照字面意义进行匹配，例如匹配字母 "a" 将匹配到文本中的 "a" 字符。
元字符：元字符具有特殊的含义，例如 \d 匹配任意数字字符，\w 匹配任意字母数字字符，. 匹配任意字符（除了换行符）等。
- 量词
*：匹配前面的模式零次或多次。
+：匹配前面的模式一次或多次。
?：匹配前面的模式零次或一次。
{n}：匹配前面的模式恰好 n 次。
{n,}：匹配前面的模式至少 n 次。
{n,m}：匹配前面的模式至少 n 次且不超过 m 次。
- 字符类
[ ]：匹配括号内的任意一个字符。例如，[abc] 匹配字符 "a"、"b" 或 "c"。
[^ ]：匹配除了括号内的字符以外的任意一个字符。例如，[^abc] 匹配除了字符 "a"、"b" 或 "c" 以外的任意字符。
- 边界匹配
^：匹配字符串的开头。
$：匹配字符串的结尾。
\b：匹配单词边界。
\B：匹配非单词边界。
- 分组和捕获
( )：用于分组和捕获子表达式。
(?: )：用于分组但不捕获子表达式。
- 特殊字符
\：转义字符，用于匹配特殊字符本身。
.：匹配任意字符（除了换行符）。
|：用于指定多个模式的选择。

常用正则:
- 数字匹配并分组`(-?\d+\.?\d*(?:e[+-]?\d+)?)`
- 匹配所有`[\s\S]`, `\s`匹配所有空白符, 包括换行, `\S`非空白符，不包括换行
- 匹配一个数字字符`\d`, 等价于 [0-9]
        
        # 匹配带字符`UT: a0: 数字`
        import re
        a0Reg = re.compile(r"[\s\S]+UT: a0: (-?\d+\.?\d*(?:e[+-]?\d+)?)")
        line = "fhdsa UT: a0: 1.23"
        m = a0Reg.match(line)
        if m:
                print(float(m.group(i + 1))) # 索引0 对应整个字符串, 1开始为分组内部的内容

python中可视化可以用`matplot`, 如果想要更好的绘图可以使用matlab, 这是只是用python提取数据, 存放在txt可以用`numpy`, 先创建list, 然后转换numpy格式

    class GetData:
    def __int__(self):
        self.version = 1.0

    @staticmethod
    def get_joint(data_file, data_reg, num_jnt):
        """Get several data from file with regular expression

        Used for analyze data

        Args:
            data_file (str): The path of the file to read
            data_reg (Pattern object): regular expression pattern
            num_jnt (int): number of data

        Returns:
            t_arr: time in millisecode
            jnt_arr: data array
        """
        data_list = []
        t = []
        with open(data_file, "rt", encoding='utf-8') as fp:
            for line in fp:
                m = data_reg.match(line)
                if m:
                    datime = line[1:27]
                    time_array = datetime.datetime.strptime(datime, "%Y-%m-%d %H:%M:%S.%f")
                    time_stamp = time.mktime(time_array.timetuple()) + time_array.microsecond / 1e6
                    t.append(time_stamp)
                    data_line = []
                    for i in range(num_jnt):
                        data_line.extend([float(m.group(i + 1))])
                    data_list.append(data_line)
        data_arr = np.asarray(data_list)
        t_arr = np.asarray(t)
        return t_arr, data_arr

    @staticmethod
    def get_matrix(data_file, data_text, data_reg, num_row, num_col):
        jnt_list = []
        t = []
        with open(data_file, "rt", encoding='utf-8') as fp:
            for line in fp:
                if data_text in line:
                    next_line = fp.readline()
                    m = data_reg.match(next_line)
                    if m:
                        datime = line[12:27]
                        t_tmp = datetime.datetime.strptime(datime, "%H:%M:%S.%f")
                        t.append(t_tmp)
                        jnt = []
                        for i in range(num_row * num_col):
                            jnt.extend([float(m.group(i + 1))])
                        jnt_list.append(jnt)
        jnt_arr = np.asarray(jnt_list)
        return t, jnt_arr

## 函数注释
好的函数注释能够快速确认该如何使用, 特别时对于阅读老代码是非常有用的
参考[Google 开源项目风格指南](https://zh-google-styleguide.readthedocs.io/en/latest/google-python-styleguide/python_style_rules/)
- google推荐
    ```
    def func(path, field_storage, temporary):
        '''基本描述

        详细描述

        Args:
            path (str): The path of the file to wrap
            field_storage (FileStorage): The :class:`FileStorage` instance to wrap
            temporary (bool): Whether or not to delete the file when the File instance is destructed

        Returns:
            BufferedFileStorage: A buffered writable file descriptor
        '''
        pass

    ```

也可以通过库函数跳转到定义, 参考官方的注释方式

## 遇到的问题
1. ImportError: cannot import name 'PackageFinder'

        curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && python get-pip.py
    

## 引用

1. [在virtualenv中设置：`pip install -e .` vs `python setup.py install`](https://www.jb51.cc/python/241778.html)
2. [如何将自己的Python程序打包--setuptools详解](https://www.jianshu.com/p/9a54e9f3e059)
3. [Python Numpy 如何交换两行和两列](https://blog.csdn.net/qq_35356840/article/details/88557912)
4. [Dissecting the Camera Matrix, Part 2: The Extrinsic Matrix](http://ksimek.github.io/2012/08/22/extrinsic/)
5. [OpenGL投影矩阵(Projection Matrix)构造方法](https://zhuanlan.zhihu.com/p/73034007)
6. [OpenGL 101: Matrices - projection, view, model](https://solarianprogrammer.com/2013/05/22/opengl-101-matrices-projection-view-model/)
