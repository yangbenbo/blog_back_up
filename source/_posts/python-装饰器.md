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


## 引用
1. [廖雪峰python教程 使用@property](https://www.liaoxuefeng.com/wiki/1016959663602400/1017502538658208)             


