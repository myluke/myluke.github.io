---
layout: post
title:  "验证半角英文、电话号码、邮箱格式合法的asp函数"
excerpt: 自己用过的例子记录下
date:   2008-10-01 22:31:00 +0800
categories: jekyll update
---   
<!--markdown-->    <%
    Function validate(ByVal str,ByVal number)
    Dim temp,reg
    Set reg = new regexp
       reg.ignorecase=true
       reg.global=true
    Select Case CStr(number)
       ' 英文+空格
       Case "0" temp = "^[a-zA-Z ]+$"
       ' 数字+横杠
       Case "1" temp = "^[0-9\-]+$"
       ' 半角数字
       Case "2" temp = "^\d+$"
       ' 邮箱地址
       Case "3" temp = "^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$"
       Case Else temp = number
    End Select
    reg.pattern = temp
    validate = reg.test(Trim(str))
    Set reg = Nothing
    End Function
    %>
