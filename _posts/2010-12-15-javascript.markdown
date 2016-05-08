---
layout: post
title:  "javascript 时间倒计时代码"
date:   2010-12-15 17:08:00 +0800
excerpt: 例子记录下,自己学习下
categories: jekyll update
---   
<!--markdown-->    <script type="text/javascript" language="javascript">
                //任务剩余时间开始
                var StartDate = new Date("<?php echo date('Y/m/d H:i:s')?>");
                var EndDate = new Date("<?php echo date('Y/m/d H:i:s')?>");
                var TimeDiff = EndDate - StartDate;
    
                function TimeToString(Time){
                    var strTime = '';
                    if( Time<10){
                        strTime = "0" + Time.toString();
                    }else{
                        strTime = Time.toString();
                    }
                    return strTime;
                }
                function TimeComputing(){
                    TimeDiff = TimeDiff - 1000;
                    var DayTime  = 86400000;
                    var HourTime = 3600000;
                    var MinTime  = 60000;
                    var SecTime  = 1000;
                    var DiffDay  = Math.floor(TimeDiff / DayTime);
                    var DiffHour = Math.floor((TimeDiff % DayTime) / HourTime);
                    var DiffMin  = Math.floor((TimeDiff % DayTime % HourTime) / MinTime);
                    var DiffSec  = Math.floor((TimeDiff % DayTime % HourTime % MinTime) / SecTime);
    
                    $('#TimeoutDay').html(TimeToString(DiffDay)+'天');
                    $('#TimeoutHour').html(TimeToString(DiffHour)+'小时');
                    $('#TimeoutMin').html(TimeToString(DiffMin)+'分');
                    $('#TimeoutSec').html(TimeToString(DiffSec)+'秒');
                }
                //任务剩余时间结束
         IntervalId = setInterval('TimeComputing()',1000);
                        TimeComputing();
    </script>
