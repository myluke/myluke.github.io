---
layout: post
title:  "asp实现 上一条 下一条 实例代码！"
date:   2009-12-26 13:07:00 +0800
excerpt: 记录下自己用的例子
categories: jekyll update
---   
<!--markdown-->    dim showprev
         set showprev=conn.execute("select top 1 * from jcl_photo where id > "&id&" order by id asc;")
         if showprev.eof then
         response.Write "上条没有了"
         else
         response.Write "<a href=""sub.asp?id="&showprev("id")&""">上一条</a>|||||"
         end if
         showprev.close
         set showprev=nothing
        dim shownext
         set shownext=conn.execute("select top 1 * from jcl_photo where id < "&id&" order by id DESC;")
         if shownext.eof then
         response.Write "下条没有了"
         else
         response.Write "<a href=""sub.asp?id="&shownext("id")&""">下一条</a>"
         end if
         shownext.close
         set shownext=nothing
