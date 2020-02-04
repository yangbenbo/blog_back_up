---
title: matlab中常见问题
date: 2019-10-24 19:46:53
categories:
- software
tags:
- matlab
---
1. matlab中sin(x) sind(x) 
    
    都是计算正弦的函数　第一个输入单位为弧度　第二个为度

    对于特殊角的三角函数为有理数的　最好使用sind　很多时候为了保证计算精度会使用符号计算

        sin(pi) = 1.2246e-16
        sind(180) = 0
        vpa(sind(180),32) = 0.0   % 设置显示精度　32位
    小数转为有理分式      
      
        rat(0.3)   
2. 关于reshape
    reshape是按照列来重新排列 不要想当然 之前一个sigma_cube数据(1667x3 其中每个3x3是一个sigma)就是排错了         
        
