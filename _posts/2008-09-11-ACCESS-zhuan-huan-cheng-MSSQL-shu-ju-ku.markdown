---
layout: post
title:  "ACCESS转换成MSSQL数据库"
excerpt: ACCESS的备注类型（如果数据量非常大的话）转换成mssql后变成ntext类型但是数据库里看不到任何东西，事实上里面是存在数据的，如果读取不出来，可以赋一个变量
date:   2008-09-11 22:11:00 +0800
categories: jekyll update
---   
<!--markdown-->1.ACCESS的备注类型（如果数据量非常大的话）转换成mssql后变成ntext类型但是数据库里看不到任何东西，事实上里面是存在数据的，如果读取不出来，可以赋一个变量
2.select * from Lt_product where ClassID in （1，2，3，4，） order by clicknum desc
这个在ACCESS里是合法的在mssql就抱类型不匹配


<!--more-->


解决办法：AllSortID=left(AllSortID,len(AllSortID)-1)去掉最后一个“，”
源码：

    AllSortID=left(AllSortID,len(AllSortID)-1)
                 strwhere="where ClassID in ("& AllSortID & ") "
                 '================获完毕
                set ProRs=server.createobject("adodb.recordset")
              
                ProSql="select * from Lt_product "&strwhere&"order by clicknum desc"
                'response.write ProSql
                 ProRs.open ProSql,Licns,1,1

3.参照http://hi.baidu.com/luomahu22/blog/item/1dcbf1fa2b5d79ddb48f3189.html
