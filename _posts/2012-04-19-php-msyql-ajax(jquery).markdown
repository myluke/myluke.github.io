---
layout: post
title:  "php+msyql+ajax(jquery) 进度条原创案例"
date:   2012-04-19 23:40:00 +0800
excerpt: 突然下午群里有人问起这个是怎么实现的，自己闲下无事就搞了下，以前也没搞过，网上搜了下，全是一篇文章，转载来转载去的。无语。。
categories: jekyll update
---   
<!--markdown-->突然下午群里有人问起这个是怎么实现的，自己闲下无事就搞了下，以前也没搞过，网上搜了下，全是一篇文章，转载来转载去的。无语。。。
最终效果：![进度条][1]


<!--more-->

其实实现这个并不难，也就50多行代码算上html的页头标准什么的。。。
大概思路是，使用了ajax的递归循环，去更改进度条的宽度。
数据库格式，一个总数，和当前完成数。
![数据库字段][2]


  [1]: http://7vzoi7.com1.z0.glb.clouddn.com/2015/02/680885848.jpg
  [2]: http://7vzoi7.com1.z0.glb.clouddn.com/2015/02/1843448956.jpg
test.php 文件代码如下：

    <?php
      $link=mysql_connect( 'localhost','root', '123456' );
      $conn = mysql_select_db('test',$link);
    if($_GET['ser']){
        $result = mysql_query("select `total`,`comel` from count where id=1",$link);
        $rs = mysql_fetch_array($result);
        if($rs['comel'] == $rs['total']){
            echo (int)100;exit;
        }
        $a = (int)$rs['comel']+1;
        mysql_query("update count set `comel`= $a where id=1",$link);
        $c = $rs['total']<= 0 ? 0 : round(($rs['comel'] / $rs['total']) * 100,1);
        echo $c;exit;
    }
    
    ?>
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <meta name="keywords" content="{$keywords}" />
    <meta name="description" content="{$description}" />
    <title>php+ajax 进度条</title>
    <script type="text/javascript" src="common/js/jquery1.5.1.js"></script>
        <style type="text/css ">
            .rate{text-align: center;width: 600px;margin: auto; position: relative}
            .inner{border: 1px solid #DFDFDF; height: 25px;background-color: #E0ECF6;}
            .text{width: 100px;font-size: 8pt;position: absolute;top:5px;}
        </style>
        <script type="text/javascript">
           $(function () {
               //debugger;
               function get_server_data(){
                   $.getJSON('test.php?ser=recv',function(r){
                       r = parseFloat (r || 0);
                    $('.inner').css('width',r+'px');
                    if(r!=100){
                        $('.text').text(r+'%');
                           get_server_data();
                        //setTimeout(get_server_data,2000);//设置多少秒后请求一次
                    }else{
                        $('.text').text('进度完成');
                    }
                   });
               }
                get_server_data();
            });
        </script>
    </head>
    
    <body>
    <div class="rate">
        <div class="inner"></div>
        <div class="text"></div>
    </div>
    </body></html>
