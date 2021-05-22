---
layout: post
title:  "解决 apple m1 golang /lib64/ld-linux-x86-64.so.2 问题"
excerpt: 最近在看一个golang 项目，在apple m1 芯片 docker 环境里面 总报错 /lib64/ld-linux-x86-64.so.2 no such file or directory 
author: Luke
date:   2021-05-22 13:18:00 +0800
categories: jekyll update
---
# 背景
* 最近在学习一个golang 项目，在apple 的intel 芯片 环境启动是没有任何问题就是风扇转的有点快😄
* 但公司最近福利有提升，给配置了一台apple 最新的 m1芯片的mac pro，发现同样的代码同样的docker 环境golang 项目跑不起来了 😓
* Google了一圈，发现都没有对症的。

# 问题现象
* 启动项目的时候报这个错误 /lib64/ld-linux-x86-64.so.2 no such file or directory
* 这就一脸懵逼了，怎么这个文件没了。对golang 也不是很熟，也不知道为啥会用这个
* 去docker 环境里面看了下，这个文件还真不存在了
* ![dockerls-all](http://static.kanheze.com/ddd.jpg)

# 问题解决
* google 到了 有个国外网友这么干的 ：ln -s ../lib64/ld-linux-x86-64.so.2 /lib/，我试了下，反正没效果。
* 然后自己就搜啊搜啊 各种找，后来发现 docker run 的时候可以加个参数  --platform 脑袋里面突然灵光一闪。
* m1 下面 run 的时候使用 --platform=linux/amd64  是不是就可以了？ 抱着试试的态度，实验一把。
* 居然让我蒙对了。golang 项目正常起来了。