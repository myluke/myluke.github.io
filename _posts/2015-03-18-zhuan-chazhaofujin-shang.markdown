---
layout: post
title:  "[转]查找附近的xxx 球面距离以及Geohash方案探讨（上）"
date:   2015-03-18 13:01:00 +0800
categories: jekyll update
excerpt: 随着移动终端的普及，很多应用都基于LBS功能，附近的某某（餐馆、银行、妹纸等等）。

---   
<!--markdown-->随着移动终端的普及，很多应用都基于LBS功能，附近的某某（餐馆、银行、妹纸等等）。

基础数据中，一般保存了目标位置的经纬度；利用用户提供的经纬度，进行对比，从而获得是否在附近。

目标：
查找附近的XXX，由近到远返回结果，且结果中有与目标点的距离。

针对查找附近的XXX，提出两个方案，如下：

一、方案A:
===============================================================

抽象为球面两点距离的计算，即已知道球面上两点的经纬度；
点（纬度，经度），A($radLat1,$radLng1)、B($radLat2,$radLng2);

优点：通俗易懂，部署简单便捷

缺点：每次都会查询数据库，性能堪忧


<!--more-->


1、推导

通过余弦定理以及弧度计算方法，最终推导出来的算式A为：


    $s = acos(cos($radLat1)*cos($radLat2)*cos($radLng1-$radLng2)+sin($radLat1)*sin($radLat2))*$R;

目前网上大多使用Google公开的距离计算公司，推导算式B为：


    $s = 2*asin(sqrt(pow(sin(($radLat1-$radLat2)/2),2)+cos($radLat1)*cos($radLat2)*pow(sin(($radLng1-$radLng2)/2),2)))*$R;

其中 :
$radLat1、$radLng1，$radLat2，$radLng2 为弧度

$R 为地球半径

2、通过测试两种算法，结果相同且都正确，但通过PHP代码测试，两点间距离，10W次性能对比，自行推导版本计算时长算式B较优，如下：

//算式A
0.56368780136108float(431)
0.57460689544678float(431)
0.59051203727722float(431)

//算式B
0.47404885292053float(431)
0.47808718681335float(431)
0.47946381568909float(431)

3、所以采用数学方法推导出的公式:


    <?php
     
        //根据经纬度计算距离 其中A($lat1,$lng1)、B($lat2,$lng2)
        public static function getDistance($lat1,$lng1,$lat2,$lng2)
        {   
            //地球半径
            $R = 6378137;
     
            //将角度转为狐度
            $radLat1 = deg2rad($lat1);
            $radLat2 = deg2rad($lat2);
            $radLng1 = deg2rad($lng1);
            $radLng2 = deg2rad($lng2);
             
            //结果
            $s = acos(cos($radLat1)*cos($radLat2)*cos($radLng1-$radLng2)+sin($radLat1)*sin($radLat2))*$R;
     
            //精度
            $s = round($s* 10000)/10000;
     
            return  round($s);
        }
     
    ?>

4、在实际应用中，需要从数据库中遍历取出符合条件，以及排序等操作，

将所有数据取出，然后通过PHP循环对比，筛选符合条件结果，显然性能低下；所以我们利用下Mysql存储函数来解决这个问题吧。

4.1、创建Mysql存储函数,并对经纬度字段建立索引

    DELIMITER $$
     
    CREATE DEFINER=`root`@`%` FUNCTION `GETDISTANCE`(lat1 DOUBLE, lng1 DOUBLE, lat2 DOUBLE, lng2 DOUBLE) RETURNS double
     
    READS SQL DATA
     
    DETERMINISTIC
     
    BEGIN
     
    DECLARE RAD DOUBLE;
     
    DECLARE EARTH_RADIUS DOUBLE DEFAULT 6378137;
     
    DECLARE radLat1 DOUBLE;
     
    DECLARE radLat2 DOUBLE;
     
    DECLARE radLng1 DOUBLE;
     
    DECLARE radLng2 DOUBLE;
     
    DECLARE s DOUBLE;
     
    SET RAD = PI() / 180.0;
     
    SET radLat1 = lat1 * RAD;
     
    SET radLat2 = lat2 * RAD;
     
    SET radLng1 = lng1 * RAD;
     
    SET radLng2 = lng2 * RAD;
     
    SET s = ACOS(COS(radLat1)*COS(radLat2)*COS(radLng1-radLng2)+SIN(radLat1)*SIN(radLat2))*EARTH_RADIUS;
     
    SET s = ROUND(s * 10000) / 10000;
     
    RETURN s;
     
    END$$
     
    DELIMITER ;

4.2、查询SQL

通过SQL，可设置距离以及排序；可搜索出符合条件的信息，以及有一个较好的排序


    SELECT *,latitude,longitude,GETDISTANCE(latitude,longitude,30.663262,104.071619) AS distance FROM  mb_shop_ext where 1 HAVING distance<1000 ORDER BY distance ASC LIMIT 0,10


转载地址：http://www.wubiao.info/372