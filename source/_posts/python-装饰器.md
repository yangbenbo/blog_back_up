---
title: python 装饰器
date: 2020-11-10 14:00:30
categories:
- Program
tags:
- python
---

## python装饰器 @property

绑定属性时有时候我们只想只读类的属性值,或者有限制的更改类的属性值

可以使用get()和set()方法,但是略显复杂,使用装饰器可以更简单,python内置的@property装饰器负责把方法变成属性调用

    class Student(object):
    
        @property
        def score(self):
            return self._score
    
        @score.setter
        def score(self, value):
            if not isinstance(value, int):
                raise ValueError('score must be an integer!')
            if value < 0 or value > 100:
                raise ValueError('score must between 0 ~ 100!')
            self._score = value
把getter方法变成属性时只需要@property,@score.setter负责把setter方法变成属性赋值,对于只读属性可只定义getter方法

**这种方式对于单个值设置只读是很有帮助的,但是对于列表,numpy数组等就无法防止对列表中单个数据的修改**

    import numpy as np
    class testnp(object):
        def __init__(self):
            self._data = [1, 1]
            self._npdata = np.asarray([1, 1, 1])

        @property
        def data(self):
            return self._data
        
        @property
        def npdata(self):
            return self._npdata
            # return self.get_readOnly_view(self._npdata)

        def get_readOnly_view(self, data):
            res = data.view()
            res.flags.writeable = False
            return res

    test = testnp()
    print "初始值:"
    print test.data
    print test.npdata
    data = test.data
    npdata = test.npdata
    # 测试赋值前
    # 可以单独修改
    # test.data[0] = 10 # 原始数据修改 [10,1]
    # test.npdata[0] = 10     # 原始数据修改 [10, 1, 1]
    # 整体无法修改
    # test.data = [10,10] # AttributeError: can't set attribute
    # test.npdata = np.asarray([10, 10, 10])  # AttributeError: can't set attribute
    # 测试赋值后
    # 新变量整体操作不影响
    # data = [10,10]  # 原始数据保留
    # npdata = np.asarray([10, 10, 10])   # 原始数据保留
    # 新变量操作元素改变数据
    # data[0] = 10    #   原始数据修改 [10, 1]
    # npdata[0] = 10  # 原始数据修改 [10, 1, 1]
使用@property
- 只作用于赋值前的数据.如果赋值给新变量,那么新变量可以再赋值,不过不会影响到原始数据(新变量是一个新的对象)
- 只能作用于整个变量.对于列表和`numpy`数组,能够防止对于整体数据的修改,如果对列表中元素修改会修改到原始数据,不论是赋值前后

从装饰器也可以看出来返回的值就是对象数据本身,虽然整体值不让改,但是元素的值可以改;
- list:python中一般会使用写时复制功能,所以新变量赋值再更改之后就是另一个变量,和之前的数据没有关系,变量名可以看成是指向了另一块内存区域
- numpy数组:numpy自身优化了效率,需要调用`numpy.copy`才能深度拷贝成为另一个变量,否则即使赋值给变量,对新变量的操作还是对同一块内存区域的操作

解决办法:
- 列表可以返回一个拷贝`return self._data[:]`,如果数据量大是很不利的
- numpy可以返回一个只读属性的view,见上面测试代码.为了利用@property把获取数据的方法变成属性使用,可以直接把返回的只读numpy数组使用@property再次封装.这样整体和单独变量都不会被修改,如果需要修改,那么深度拷贝返回值即可`numpy.copy(test.npdata)`

## 引用
1. [廖雪峰python教程 使用@property](https://www.liaoxuefeng.com/wiki/1016959663602400/1017502538658208)
2. [Is this a correct way to create a read-only view of a numpy array?](https://stackoverflow.com/questions/60810463/is-this-a-correct-way-to-create-a-read-only-view-of-a-numpy-array)          
