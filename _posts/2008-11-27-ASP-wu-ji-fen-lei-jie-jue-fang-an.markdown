---
layout: post
title:  "ASP无级分类解决方案"
date:   2008-11-27 23:23:00 +0800
excerpt: 用过的函数记录下
categories: jekyll update
---   
<!--markdown-->    <%
    db="db1.mdb"
    Set Licns = Server.CreateObject("ADODB.Connection") 
    Licnsstr="DBQ="+server.mappath(db)+";DefaultDir=;DRIVER={Microsoft Access Driver (*.mdb)};" 
    On Error Resume Next
    Licns.open Licnsstr
    If Err Then
    err.Clear
    Set Licns = Nothing
    Response.Write "数据库连接出错，请检查连接字串。"
    Response.End
    End If
    action=request.querystring("action")


<!--more-->


    if action="tyadd" then
    ＇所有父级,级别,类型
    name=request.form("name")＇名称,
    pareid=request.form("pareid")
    orders=request.form("orders")
    if pareid<>0 then
    retylist1="select * from ty where id="&pareid
    set retylist01=Licns.execute(retylist1)
    depth=retylist01("depth")+1
    parestr=retylist01("parestr")
    rootid=retylist01("rootid")
    retylist01=close:set retylist01=nothing
    else
    depth=0
    parestr="0"
    set maxrootid=Licns.execute("select max(rootid) from ty")
    rootid=maxrootid(0)+1
    maxrootid.close:set maxrootid=nothing
    if isnull(rootid) then rootid=1
    end if
    set rs=server.createobject("adodb.recordset")
    rs.open "ty",Licns,1,3
    rs.addnew
    rs("name")=name
    rs("pareid")=pareid
    rs("depth")=depth
    rs("orders")=orders
    rs("rootid")=rootid
    rs("child")=0
    rs.update
    rs("parestr")=rs("id")&","&parestr ＇所有父级
    rs.update
    rs.close
    Licns.execute("update ty set child=child+1 where id="&pareid) ＇若栏目增加子栏目,则+1
    response.redirect "index.asp"
    end if
    taadd01=""
    taadd01=taadd01+"<b><font color=red>[无级分类示范]</font></b>制作:默飞 QQ:33224360 更多下载:<a href=http://mofei.xinxiu.com>http://mofei.xinxiu.com</a>"
    taadd01=taadd01+"<form method=post name=mofeiform action=＇?action=tyadd＇>"
    taadd01=taadd01+"名称:<input name=name type=text> 顺序:<input name=orders type=text size=6>"
    taadd01=taadd01+"<select name=pareid>"
    ＇栏目列表,列出属于
    set tylist01=Licns.execute("select * from ty")
    do while not tylist01.eof
    taadd01=taadd01+"<option value=＇"&tylist01("id")&"＇>"&tylist01("name")&"</option>"
    tylist01.movenext
    loop
    tylist01.close
    set tylist01=nothing
    taadd01=taadd01+"<option value=＇0＇>根目录</option>"
    taadd01=taadd01+"</select>"
    taadd01=taadd01+"<input type=submit name=submit01 value=＇增加＇ disabled> 由于是演示,增加按钮被我屏蔽了."
    taadd01=taadd01+"</form>"
    response.write taadd01
    response.write "<table width=100% border=0 align=center cellpadding=2 cellspacing=0>"
    sqllist="select id,name,pareid,depth,child from ty order by rootid,orders"
    ＇栏目ID-0,名称-1,父级-2,级别-3,子集-4
    set rslist = Licns.execute(sqllist)
    SQL=rslist.getrows(-1)
    For iii=0 To Ubound(SQL,2) ＇根据父级来循环
    response.write "<tr height=28><td>"
    if SQL(3,iii)>0 then ＇若不是根目录,输出空格,级别后,加空格
    for i=1 to SQL(3,iii)
    Response.Write " " ＇级别>0后,加空格
    next
    end if
    if SQL(4,iii)>0 then ＇若存在子集,换图标了
    Response.Write " + " ＇明显,存在着子集的,
    else
    Response.Write " - " ＇输出不存在子集的
    end if
    if SQL(2,iii)=0 then ＇如果是根目录的话
    Response.Write "<b>" ＇如果是大类,就用粗体显示了
    end if
    Response.Write SQL(1,iii) ＇这个自然是名称了
    if SQL(4,iii)>0 then ＇若有子集的话,输出了.
    Response.Write "("
    Response.Write SQL(4,iii) ＇输出还有多少子集了
    Response.Write ")"
    end if
    response.write "</td></tr>"
    Next
    response.write "</table>"
    response.write "<br><br><br><br><br><br><br><br>"
    %> 
