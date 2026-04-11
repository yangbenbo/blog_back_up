---
title: matlab中常见问题
date: 2019-10-24 19:46:53
categories:
- Software
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

3. 多个数组生成全组合
    `fullfact`, 有时候为了编译不相关变量和所有组合, 可以使用这个函数

        % 得到3组关节角度的全排列, 与使用循环是一致的
        th1 = 0 : 1 : 10;
        th2 = 10 : 1 : 20;
        th3 = 20 : 1 : 30;
        index = fullfact([length(th1), length(th2), length(th3)]);
        spaceJoint = [th1(index(:, 1))', th2(index(:, 2))', th3(index(:, 3))'];
4. matlab变参数个数输入
    - nargin: 参数个数
    - varargin: 函数输入元胞数组     
    参考[支持可变数量的输入](https://ww2.mathworks.cn/help/matlab/matlab_prog/support-variable-number-of-inputs.html)

    比如定义的写gif函数, 默认是追加方式
    ```
    function writeGif(gcf, file, varargin)
    switch nargin
        case 2
            init = false;
        case 3
            init = varargin{1};
    end
    F = getframe(gcf);
    I = frame2im(F);
    [I, map] = rgb2ind(I, 256);
    timeDelay = 0.02;
    if init
        imwrite(I, map, file, 'gif', 'Loopcount', inf, 'DelayTime', timeDelay);
    else
        imwrite(I, map, file, 'gif', 'WriteMode', 'append', 'DelayTime', timeDelay);
    end
    end
    ```
5. 同时给多个变量赋值`deal`
    ```
    [Y1, Y2, Y3, ...] = deal(X) % copies the single input to all the requested outputs. It is the same as Y1 = X, Y2 = X, Y3 = X, ...
    [Y1, Y2, Y3, ...] = deal(X1, X2, X3, ...) % is the same as Y1 = X1; Y2 = X2; Y3 = X3; ...
    ```
6. 数据单维索引变换成二维索引`ind2sub`

    ```
    data = 3 * peaks;
    surf(data);
    [M, I] = max(data(:));
    [x, y] = ind2sub(size(data), I);
    P3 = arrow3([49 1 0], [y x data(x, y)], 'r2', 5, 5, 3);
    ```