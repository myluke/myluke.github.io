---
layout: post
title:  "工作中用的小工具"
excerpt: 登录机器，phpstorm增加代码扫描，TextBar
author: Luke
date:   2018-08-13 13:18:00 +0800
categories: jekyll update
---   

分享几个工作中用到的小工具，希望能帮到大伙。其实最实用的是一次登录几十台机器查日志。

# 一、登录机器查日志

> 每次都要登录10台机器查询，登录=>输入命令,重复操作10次。

##  可以一次解决了
### 1、前提要搞定一个命令可以登录线上机器，再安装`iterm2`和`oh-my-zsh`，安装这里不细说。

### 2、一键登录机器脚本内容如下
* 1、新建脚本/Users/用户名/up.sh
```
#!/bin/sh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null  -A 58OA账号@xxxxx.com  -t "ssh work@10.xxx.$1"
```

* 2、编辑 vim /Users/用户名/.zshrc
```
添加内容
alias up='/Users/用户名/up.sh'
```


* 3、启用生效命令：`source /Users/用户名/.zshrc`

### 3、设置iterm2

* 1、按键command+O
* ![步骤1](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang1.png)
* 2、
* ![步骤2](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang2-1.png)
* 3、返回第一步选中要登录的机器，然后点击NewTab按钮，等待登录。
* 4、然后按键command+shift+i,同时操作所有登录的机器。

# 二、PhpStorm 安装静态代码扫描插件

> * sonar 质量标准扫描。

## 步骤

* 1、在线安装，不行就用第二种硬盘安装
* ![步骤1](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang3.png)
* 2、下载插件包 链接: [https://pan.baidu.com/s/1l1OJKeyDu1ZTivQ6GsQ8Gw](https://pan.baidu.com/s/1l1OJKeyDu1ZTivQ6GsQ8Gw) 密码: yqum
* 3、硬盘安装，选中下载的压缩包不要解压
* ![步骤2](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang4.png)
* 4、配置安居客静态扫描规则
* ![步骤3](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang5.png)
* 5、创建授权token：https://sonar.xxx.com/account/security/
* ![步骤4](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang7.png)
* 6、填写token完成
* ![步骤5](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang6.png)
* 7、PhpStorm 扫描结果
* ![步骤6](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang8.png)

# 三、TextBar：彻底玩转你的 Menubar

## 文章链接：[https://www.waerfa.com/textbar](https://www.waerfa.com/textbar)
## 演示图
![演示图](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang9.png)
# 四、善用 Popclip 插件

## 文章链接：[https://www.ifanr.com/app/507343](https://www.ifanr.com/app/507343)
## 演示图
![演示图](http://7vzoi7.com1.z0.glb.clouddn.com/fenxiang10.jpg)