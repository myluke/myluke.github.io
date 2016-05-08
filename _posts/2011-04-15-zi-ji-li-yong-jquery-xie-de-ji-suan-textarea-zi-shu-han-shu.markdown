---
layout: post
title:  "自己利用jquery写的计算textarea字数函数"
date:   2011-04-15 13:40:00 +0800
excerpt: 自己写的例子,不喜勿看
categories: jekyll update
---   
<!--markdown-->js：

    //计算textarea字数
    function textlimt(){
       var remark=$('#remark').val();
       if(remark.length>100){
      //如果元素区字符数大于最大字符数，按照最大字符数截断；
        $('#remark').val(''+remark.substring(0,100)+'');
       }else{
    //在记数区文本框内显示剩余的字符数；
        $('#snum').html(''+100-remark.length +' numbers');
       }
    }

html:
 

    <textarea class="textarea" name="remark" id="remark" cols="5" rows="" onKeyDown="textlimt();" onKeyUp="textlimt();"style="width:255px;"><br/><span id="snum">100 numbers</span>
