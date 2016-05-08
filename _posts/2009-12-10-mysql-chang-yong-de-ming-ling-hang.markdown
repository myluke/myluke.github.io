---
layout: post
title:  "mysql常用的命令行"
date:   2009-12-10 22:56:00 +0800
excerpt: 开始运行-》cmd-》mysql - u root -p 回车，输入密码
categories: jekyll update
---   
<!--markdown-->开始运行-》cmd-》mysql - u root -p 回车，输入密码，
常用mysql内部操作提示符
show databases; — 显示所有数据库列表
use test; –打开库
show tables; –查看找开数据库中所有数据表
describe tableName; — 查询表结构
create table 表名(字段设定表); –创建表
create database 数据库名； –创建数据库
drop database 数据库名; –删除数据库
drop table tablename –删除表结构
delete from 表名; –删除表数据
select * from 表名; — 查询指定表中所有数据、


<!--more-->


RedHat Linux (Fedora Core/Cent OS)

1.启动：/etc/init.d/mysqld start
2.停止：/etc/init.d/mysqld stop
3.重启：/etc/init.d/mysqld restart
Debian / Ubuntu Linux
1.启动：/etc/init.d/mysql start
2.停止：/etc/init.d/mysql stop
3.重启：/etc/init.d/mysql restart
[如何重启MySql] Windows
1.点击“开始”->“运行”（快捷键Win+R）。
2.停止：输入 net stop mysql
3.启动：输入 net start mysql
4.重新启动：net restart mysql
