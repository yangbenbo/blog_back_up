---
title: mysql
date: 2020-04-21 23:15:25
categories:
- program
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



## 遇到的问题
1. 无法插入中文
    - 查看客户端 编码字符 状态
        
            status;  // Client characterset: utf8
    - 查看所有编码字符
            
            show variables like'%char%';
    - 修改设置(gbk 也是支持中文)            
        
            mysql>set character_set_database=utf8;
            mysql>set character_set_server=utf8;            
