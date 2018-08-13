---
layout: post
title:  "记录分享下数据库oracle与mysql在语法上的区别"
date:   2013-11-20 15:25:00 +0800
excerpt: 数据库oracle与mysql在语法上的区别不是很多，但是也有一些。下面就我遇到的说一下.
categories: jekyll update
---   

<!--markdown-->数据库oracle与mysql在语法上的区别不是很多，但是也有一些。下面就我遇到的说一下。
<!--more-->
```
1，oracle没有offet,limit，在mysql中我们用它们来控制显示的行数，最多的是分页了。oracle要分页的话，要换成rownum。
2，oracle建表时，没有auto_increment，所有要想让表的一个字段自增，要自己添加序列，插入时，把序列的值，插入进去。
3，oracle有一个dual表，当select后没有表时，加上的。不加会报错的。select 1 这个在mysql不会报错的，oracle下会。select 1 from dual这样的话，oracle就不会报错了。
4，对空值的判断，name != ""这样在mysql下不会报错的，但是oracle下会报错。在oracle下的要换成name is not null
5，oracle下对单引号，双引号要求的很死，一般不准用双引号，用了会报
ERROR at line 1:
ORA-00904: "t": invalid identifier
而mysql要求就没有那么严格了，单引号，双引号都可以。
6，oracle有to_number,to_date这样的转换函数，oracle表字段是number型的，如果你$_POST得到的参数是123t456，入库的时候，你还要to_number来强制转换一下，不然后会被当成字符串来处理。而mysql却不会。
7，group_concat这个函数，oracle是没有的，如果要想用自已写方法。oracle 用wm_concat函数
8，mysql的用户权限管理，是放到mysql自动带的一个数据库mysql里面的，而oracle是用户权限是根着表空间走的。
9，group by,在下oracle下用group by的话，group by后面的字段必须在select后面出现，不然会报错的，而mysql却不会。
select 后面的字段也必须出现在group by 里面，除非字段在聚合函数里面，就没有这项要求了！
10，mysql存储引擎有好多，常用的mysiam,innodb等，而创建oracle表的时候，不要这样的，好像只有一个存储引擎。
11，oracle字段无法选择位置，alter table add column before|after，这样会报错的，即使你用sql*plus这样的工具，也没法改字段的位置。
12，oracle的表字段类型也没有mysql多，并且有很多不同，例如：mysql的int,float合成了oracle的number型等。
13，oracle查询时from 表名后面 不能加上as 不然会报错的，select t.username from test as t而在mysql下是可以的。
14，oracle中是没有substring这个函数的，mysql有的。oracle用substr这个函数
oracle与mysql的区别很多，上面只是一些，经常遇到的，并且我记得比较牢的一些。
15、oracle的concat函数只能传两个参数，要连接多个字符串，需要用多次concat，也可以用 || 连接多个字符串
16、oracle对引号要求严格
建表的时候，如果表名或者字段名用了双引号，那个表明或者字段名以后使用的时候都要用双引号，并且区分大小写。php得到的也是区分大小写的。
如果建表的时候没有用双引号，那么以后使用的时候，不用加双引号，并且不区分大小写。但是php得到的都是大写的。
字段的值用单引号括起来，如果里面含有单引号，需要做处理，但单引号前面再加两个单引号。
17、关键字不能用到字段里面，除非加双引号。
file、range、
18、oracle表没有自增长的字段可以用，需要自己程序生成。
你也可以建立一个自增长的序列，然后对表建立insert型的触发器，自增长的序列得到值插入到对应的要自增长的字段里面
19、oracle单个字段值的长度有限制，好像是4000，超过会报错
20、oracle表名或者字段名长度也有限制，好像是30个字符，超过报错。
21、关于空字符串，oracle对与空字符串是当null处理的。insert 或者 update 是如果是空字符串，那么表里面实际得到的是null。
注意：
null 是不等于 任何值
在处理是否为null是要用 is null 或者 is not null
判断 field="" 或者 field!="" 是错误的，要用 field is null 或者 is not null
22、对于上面出现的长度限制，转义问题，PHP处理的时候可以用 oci_bind_by_name ，就不会有问题了。
23、oracle不支持mysql那种批量插入数据，它有自己的格式：
INSERT ALL
INTO (,,,,) values ()
INTO (,,,,) values ()
INTO (,,,,) values ()
select * from dual

```