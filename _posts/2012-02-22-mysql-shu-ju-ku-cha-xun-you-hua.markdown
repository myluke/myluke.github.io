---
layout: post
title:  "mysql数据库查询优化"
date:   2012-02-22 11:47:00 +0800
excerpt: 面试官问了个这样的问题，说:一张表 user 里面就 两字段 name pass，如果这张表有1亿条数据，用什么办法能让它查询的效率最高？
categories: jekyll update
---   
<!--markdown-->面试官问了个这样的问题，说
一张表 user 里面就 两字段 name pass，如果这张表有1亿条数据，用什么办法能让它查询的效率最高？
以前做网站这块还没考虑这样的问题，再说以前搞的数据量也没这么大，一时间有点猛，想不起什么方法......
犹豫了2-3秒，想到了给字段加索引，面试官又说了那如果这张表有8亿数据呐，当时有点紧张其他的也没说出什么。
 


<!--more-->


回来的路上想了下，又和群友们讨论了下，分析了以下原因：
 1、这两个字段类型用char类型。
 2、给字段加索引，不用扫描整张表。
 3、分表存放数据，如：user1，user2，user3......
 4、把表类型换成MyISAM类型（MyISAM 检索速度比innodb快）,缺点更新锁整张表，占用空间大。
 5、数据库读写分离，分主从库，主库负责写，从库负责读取数据，从库数据延迟20几毫秒。

innodb类型表支持事务，更新只锁行，不锁全表。
目前想到的就这么多了，不过也有弊端，更新索引的时候就比较麻烦了。
