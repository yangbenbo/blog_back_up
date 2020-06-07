---
title: python基础
date: 2020-05-22 16:52:49
categories:
- program
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

## pybullet 结合 ROS
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

## 引用

1. [在virtualenv中设置：`pip install -e .` vs `python setup.py install`](https://www.jb51.cc/python/241778.html)
2. [如何将自己的Python程序打包--setuptools详解](https://www.jianshu.com/p/9a54e9f3e059)
3. [Python Numpy 如何交换两行和两列](https://blog.csdn.net/qq_35356840/article/details/88557912)
