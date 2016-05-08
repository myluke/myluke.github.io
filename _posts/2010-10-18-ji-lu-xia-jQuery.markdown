---
layout: post
title:  "记录下jQuery Ajax今天终于学会了！Ajax实例代码"
date:   2010-10-18 13:25:00 +0800
excerpt: 自己学习过的例子,记录下
categories: jekyll update
---   
<!--markdown-->       $(document).ready(function(){
        $('#click').click(function(){
           var imgid=$('img').attr('id');
            $.ajax({
               type:"POST",
               url: "get-ajax.php",
               data:"id="+imgid,
               datatype: "json",
               timeout: 5000,
               error: function(){
              alert('ajax error!');
              },
             success:function(data){
              if(data!=""){
               //alert(data);
              $("#bigimg").attr("src",data);
              }
               }
            
       });       
        })
        });
    <span id="more-560"></span>
    html代码：
    <div id="click"><img src="images/phone.jpg" id="1"></div>
    <img src="images/logo2.jpg" id="bigimg">
    get-ajax.php文件代码
    <?php
    include 'config.php';
    //这里是获取的如同$_POST[]
    $id=$sr->request('id','int','post');
    $sql = "select * from `banner` where id = $id";
    $rs = $db->query($sql);
    echo $rs[0]['image_url'];
    ?>
