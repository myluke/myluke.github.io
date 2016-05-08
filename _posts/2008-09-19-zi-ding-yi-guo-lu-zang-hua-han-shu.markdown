---
layout: post
title:  "自定义过滤脏话函数"
excerpt: 自己用过的例子
date:   2008-09-19 22:25:00 +0800
categories: jekyll update
---   
<!--markdown-->    <%Function htmlInfo(fString)
       Dim fs,fss,i,xx
       If not isnull(fString) then
        fs = "这里是要过滤的内容用逗号隔开（例如：xx,xxxx,xxx）"
        fss = split(fs,",")
        for i=0 to ubound(fss)-1
         xx = "*"
         For j=1 To Len(fss(i))
          xx = xx & "*"
         next
         fString = Replace(fString, fss(i), xx)
         xx = ""
        next
       End If
       htmlInfo = fString
    End Function
    %>
