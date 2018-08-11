---
layout: post
title:  "Access数据导入mysql程序实例"
date:   2010-10-18 13:22:00 +0800
excerpt: 由于公司是外包公司，接了个网站改版的活，以前的Access数据类型的网站数据要留下，所以要转到mysql下，研究了几天终于有点眉目了，我感觉这是最简单的方法了吧！php提供了连接Access的方法，转换的原理就是：应用php提供的方法，把Access里面的数据读出来，如果文章里面有以前的ubb代码又需要处理下，文章内容后才能入mysql库，处理后直接写sql插入insert into 语句插入mysql就可以了，当然我只是测试了很小的数据库，大数据量的我就没测试过了，高手们谁有比较好的方法，欢迎赐教
categories: jekyll update
---   
<!--markdown-->由于公司是外包公司，接了个网站改版的活，以前的Access数据类型的网站数据要留下，所以要转到mysql下，研究了几天终于有点眉目了，我感觉这是最简单的方法了吧！php提供了连接Access的方法，转换的原理就是：
应用php提供的方法，把Access里面的数据读出来，如果文章里面有以前的ubb代码又需要处理下，文章内容后才能入mysql库，处理后直接写sql插入insert into 语句插入mysql就可以了，当然我只是测试了很小的数据库，大数据量的我就没测试过了，高手们谁有比较好的方法，欢迎赐教，讨论！目前小弟就这水平了！代码如下：


<!--more-->

```
    <?php
    include 'config.php';
    ?>
    <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
    <html xmlns="http://www.w3.org/1999/xhtml">
    <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title>access转mysql</title>
    </head>
    <body>
    <?php
    $conn = new com("adodb.connection");
    //
    $connstr="provider=microsoft.jet.oledb.4.0;data source=".realpath("#db051008.mdb");
    $conn->open($connstr);//comopen()ִ
    $rs=new com("adodb.recordset");
    //$sql="select * from article where id in(14)";
    $sql='select * from article where bid=\'4\'';
    //echo $sql;
    //exit();
    $rs->open($sql,$conn,1,3);
    $rs->pagesize=400;
    if((trim(intval($_GET['page']))=="")||(intval($_GET['page'])>$rs->pagecount)||(intval($_GET['page'])<=0)){
    $page=1;
    }else{
    $page=intval($_GET['page']);
    }
    if($rs->eof||$rs->bof){
    echo "没数据";
    }else{
      
    $rs->absolutepage=$page;
    $mypagesize=$rs->pagesize;
    while(!$rs->eof && $mypagesize>0){
    $title=iconv('gb2312','utf-8',$rs->fields['title']);
    $main=iconv('gb2312','utf-8',$rs->fields['main']);
    $release_date=iconv('gb2312','utf-8',$rs->fields['posttime']);
    ?>
    <dt><strong>
    <?php
    echo $rs->fields['id'].$title;
    ?>
    </strong></dt>
    <dt>
    <?php
    //处理文章里面的ubb图片
    preg_match_all('/\[uploadimg\](.*)\[\/uploadimg\]/U',$main,$ml);
    $num = count($ml[0]);
    for($i=0;$i<$num;$i++){
    $img = '<img alt="" src="/upload/editor/'.$ml[1][$i].'" />';
    $st = $ml[0][$i];
    $main = str_replace("$st","$img","$main");
    }
    //处理文章里面的ubb的url
    preg_match_all('/\[url\=http\:\/\/www\.shywgh\.org(.+?)\](.+?)\[\/url\]/',$main,$ml_url);
    $num_url = count($ml_url[0]);
    for($i=0;$i<$num_url;$i++){
    $url = '<a href="/upload/editor'.$ml_url[1][$i].'">'.$ml_url[2][$i].'</a>';
    $st_url = $ml_url[0][$i];
    $main = str_replace("$st_url","$url","$main");
    }
    ?></dt>
    <?php
    //写sql语句插入msyql数据库
    $sql_insert="insert into `article` set `column_id`=9,title='$title',content='$main',range_num=1000,release_date='$release_date',lastedit_date='$release_date'";
    //echo $sql_insert;
    $db->query($sql_insert);
    $mypagesize--;
    $rs->movenext;
    }
       }
    //if($page>=2)
    ?>
    共<?php echo $rs->pagecount;?>页 每页20条 共<?php echo $rs->recordcount;?>条 第<?php echo $page;?>页 <a href="?page=1">首页</a> <a href="?page=<?php if($page>1){echo $page-1;}else{echo 1;}?>">上页</a>
    <?php
    $i=$page;
    for($i;$i<=$page+4;$i++){
    if($i<=$rs->pagecount){
    echo "<a href=?page=".$i.">[".$i."]</a>";
    }
    }?>
    <a href="?page=<?php if($page<$rs->pagecount){echo $page+1;}else{echo $rs->pagecount;}?>">下页</a> <a href="?page=<?php echo $rs->pagecount;?>">尾页</a>
    <?php
    $rs->Close(); //关闭记录集对象和数据库连接对象
    ?>
    </body>
    </html>
```