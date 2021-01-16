---
title: mysql
date: 2020-04-21 23:15:25
categories:
- Program
tags:
- mysql
---

## 常用语句
    -- 显示数据库
    show databases;
    
    -- 显示表格
    show tables;
    
    -- 创建一个名称为car的数据库。
    create database car;
    
    -- 使用数据库 car
    use car;
    
    -- 删除数据库car
    -- drop database car;
    
    -- 显示表格内容
    select * from tableName;
    
    -- 创建表
    create table brand (id int primary key auto_increment, factory varchar(255), name varchar(255), price int, sum int, sell int, last int);
    
    -- 删除brand表
    -- drop table brand;
    
    -- 插入数据
    insert into brand(factory,name,price,sum, sell, last) values('一汽大众', '奥迪A6', 36, 50, 8, 42);

    -- 更新数据
    -- update brand set sell=5 , last=70 where factory='上海大众' and name='桑塔纳';



## 遇到的问题
### 无法插入中文
1. 临时修改
    - 查看客户端 编码字符 状态
        
            status;  // Client characterset: utf8
            #Server characterset 和 Db     characterset 都是utf8
    - 查看所有编码字符
            
            show variables like'%char%';
    - 修改设置(gbk 也是支持中文)            
        
            set character_set_database=utf8;
            set character_set_server=utf8;
2. 永久修改修改配置文件
    - 针对未创建的表
            
            sudo vim /etc/mysql/my.cnf
            
            # 末尾添加
            [client]
            default-character-set=utf8
            [mysqld]
            character-set-server=utf8

	    /etc/init.d/mysql restart  #重启服务
    - 针对已经创建的表
        
            alter table `tablename` convert to character set utf8;
            
                             
