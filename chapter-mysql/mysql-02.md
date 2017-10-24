## MySQL中的日期时间函数

### now(), sysdate()

返回当前日期和时间。

语法

- NOW([fsp])
- SYSDATE([fsp]) 

该函数会返回'YYYY-MM-DD HH:MM:SS'或者YYYYMMDDHHMMSS的结果，依赖于是作为字符串还是数字。

```
select now(),now()+0,now();
```

结果

```
  now(): 2017-10-11 17:52:39
now()+0: 20171011175239
  now(): 2017-10-11 17:52:39
```

使用fsp指定小数点后的精度，最大为6，默认为0

```
select now(),now(1),now(2),now(3),now(4),now(5),now(6);
```

结果

```
 now(): 2017-10-11 17:53:08
now(1): 2017-10-11 17:53:08.9
now(2): 2017-10-11 17:53:08.97
now(3): 2017-10-11 17:53:08.974
now(4): 2017-10-11 17:53:08.9748
now(5): 2017-10-11 17:53:08.97480
now(6): 2017-10-11 17:53:08.974802
```

该函数返回的是语句执行开始时的时间，这点和`sysdate()`不同

```
SELECT NOW(), SLEEP(2), NOW();
结果
   NOW(): 2017-10-11 17:54:19
SLEEP(2): 0
   NOW(): 2017-10-11 17:54:19

SELECT SYSDATE(), SLEEP(2), SYSDATE();
结果
SYSDATE(): 2017-10-11 17:54:49
 SLEEP(2): 0
SYSDATE(): 2017-10-11 17:54:51
```

### localtime(), localtime, localtimestamp(), localtimestamp()

now()的同义词

### date() time()

从日期时间字符串中提取日期部分或者时间部分。

语法

- DATE(expr)
- TIME(expr)

例子

```
SELECT DATE('2003-12-31 01:02:03');
结果
DATE('2003-12-31 01:02:03'): 2003-12-31

SELECT TIME('2003-12-31 01:02:03'), TIME('2003-12-31 01:02:03.000123');
结果
       TIME('2003-12-31 01:02:03'): 01:02:03
TIME('2003-12-31 01:02:03.000123'): 01:02:03.000123
```

### year(), month(), day(), hour(), minute(), second(), microsecond(), weekday()

获取日期时间中的一部分

语法

- year(date)，返回值的范围是1000到9999，如果date是0，则返回0
- month(date)，返回值的范围是1到12，如果是'0000-00-00'或者'2012-00-00'，则返回0
- day(date)，和DAYOFMONTH()是同义词，返回值的范围是1到31，如果是'0000-00-00'或者'2012-00-00'，则返回0
- hour(time)，返回值的范围是0到23，但如果'272:59:59'，则返回272
- minute(time)，返回值的范围是0到59
- second(time)，返回值的范围是0到59
- microsecond(expr)，返回值范围是0到999999
- weekday(date)，返回值(0 = Monday, 1 = Tuesday, … 6 = Sunday)


例子

```
select YEAR('1987-01-01'), MONTH('2008-02-03'), DAYOFMONTH('2007-02-03'), HOUR('10:05:03'), HOUR('272:59:59'), MINUTE('2008-02-03 10:05:03'), SECOND('10:05:03'), MICROSECOND('12:00:00.123456'), MICROSECOND('2009-12-31 23:59:59.000010'), WEEKDAY('2008-02-03 22:23:00'), WEEKDAY('2007-11-06');
结果
                       YEAR('1987-01-01'): 1987
                      MONTH('2008-02-03'): 2
                 DAYOFMONTH('2007-02-03'): 3
                         HOUR('10:05:03'): 10
                        HOUR('272:59:59'): 272
            MINUTE('2008-02-03 10:05:03'): 5
                       SECOND('10:05:03'): 3
           MICROSECOND('12:00:00.123456'): 123456
MICROSECOND('2009-12-31 23:59:59.000010'): 10
           WEEKDAY('2008-02-03 22:23:00'): 6
                    WEEKDAY('2007-11-06'): 1
```

### dayofweek(), dayofmonth(), dayofyear(), weekofyear()

语法

- dayofweek(date) 返回值 (1 = Sunday, 2 = Monday, …, 7 = Saturday)
- dayofmonth(date) 和day()是同义词
- dayofyear(date) 返回值1到366
- weekofyear(date) 返回值1到53，等价于week(date, 3)

例子

```
SELECT DAYOFWEEK('2007-02-03'), DAYOFMONTH('2007-02-03'), DAYOFYEAR('2007-02-03'), WEEKOFYEAR('2008-12-20');
结果
 DAYOFWEEK('2007-02-03'): 7
DAYOFMONTH('2007-02-03'): 3
 DAYOFYEAR('2007-02-03'): 34
WEEKOFYEAR('2008-12-20'): 51
```

### dayname(), monthname()

语法

- dayname(date) 返回周几
- monthname(date) 返回月名称

例子

```
SELECT DAYNAME('2007-02-03'), MONTHNAME('2008-02-03')
输出
  DAYNAME('2007-02-03'): Saturday
MONTHNAME('2008-02-03'): February
```

### makedate(), maketime()

语法

- MAKEDATE(year,dayofyear)  拼接日期dayofyear必须大于0，否则为NULL
- MAKETIME(hour,minute,second)   拼接时间，second可以有小数部分

例子

```
SELECT MAKEDATE(2011,31), MAKEDATE(2011,32), MAKEDATE(2011,365), MAKEDATE(2014,365), MAKEDATE(2011,0), MAKETIME(12,15,30), MAKETIME(12,15,30.123);
结果
     MAKEDATE(2011,31): 2011-01-31
     MAKEDATE(2011,32): 2011-02-01
    MAKEDATE(2011,365): 2011-12-31
    MAKEDATE(2014,365): 2014-12-31
      MAKEDATE(2011,0): NULL
    MAKETIME(12,15,30): 12:15:30
MAKETIME(12,15,30.123): 12:15:30.123
```

### timestamp(), unix_timestamp()

语法

- TIMESTAMP(expr), TIMESTAMP(expr1,expr2) 格式化为标准的日期时间字符串，如果有两个参数，则将第二个加到第一个上边
- UNIX\_TIMESTAMP(), UNIX\_TIMESTAMP(date) 返回当前时间或者指定时间的unix时间戳

例子

```
SELECT TIMESTAMP('2003-12-31'), TIMESTAMP('2003-12-31 12:00:00','12:00:00'), UNIX_TIMESTAMP(), UNIX_TIMESTAMP('2015-11-13 10:20:19'), UNIX_TIMESTAMP('2015-11-13 10:20:19.012');
结果
                    TIMESTAMP('2003-12-31'): 2003-12-31 00:00:00
TIMESTAMP('2003-12-31 12:00:00','12:00:00'): 2004-01-01 00:00:00
                           UNIX_TIMESTAMP(): 1507720922
      UNIX_TIMESTAMP('2015-11-13 10:20:19'): 1447381219
  UNIX_TIMESTAMP('2015-11-13 10:20:19.012'): 1447381219.012
```

### curdate(), curtime(), current\_date(), current\_date, current\_time(), current\_time, current\_timestamp(), current\_timestamp

语法

- curdate() 返回当前时刻的日期，格式是'YYYY-MM-DD'或者YYYYMMDD
- CURTIME([fsp]) 返回当前时刻的时间，格式是'HH:MM:SS'或者HHMMSS
- current\_date(), current\_date curdate()同义词
- current\_time(), current\_time curtime()同义词
- current\_timestamp(), current\_timestamp now()同义词

例子

```
select curdate(), curtime(), current_date(), current_date, current_time(), current_time, current_timestamp(), current_timestamp;
结果
          curdate(): 2017-10-11
          curtime(): 19:28:35
     current_date(): 2017-10-11
       current_date: 2017-10-11
     current_time(): 19:28:35
       current_time: 19:28:35
current_timestamp(): 2017-10-11 19:28:35
  current_timestamp: 2017-10-11 19:28:35
```

### date\_format(), time\_format()

语法

- date_format(date,format) 按照指定的格式对日期做格式化
- time\_format(date,format) 和date_format()类似，但是只能使用使用小时、分钟、秒、毫秒的格式

例子

```
SELECT DATE_FORMAT('2009-10-04 22:23:00', '%W %M %Y'), DATE_FORMAT('2007-10-04 22:23:00', '%H:%i:%s');
结果
DATE_FORMAT('2009-10-04 22:23:00', '%W %M %Y'): Sunday October 2009
DATE_FORMAT('2007-10-04 22:23:00', '%H:%i:%s'): 22:23:00

SELECT TIME_FORMAT('100:00:00', '%H %k %h %I %l');
结果
TIME_FORMAT('100:00:00', '%H %k %h %I %l'): 100 100 04 04 4
```

### utc\_date(), utc\_time(), utc\_timestamp(), UTC_DATE, UTC_TIME, UTC_TIMESTAMP

语法

- utc\_date() 返回当前的UTC日期
- utc\_time([fsp]) 返回当前的UTC时间
- utc\_timestamp([fsp]) 返回当前的UTC日期和时间

例子

```
SELECT UTC_DATE(), UTC_DATE() + 0, UTC_TIME(), UTC_TIME() + 0, UTC_TIMESTAMP(), UTC_TIMESTAMP() + 0;
结果
         UTC_DATE(): 2017-10-23
     UTC_DATE() + 0: 20171023
         UTC_TIME(): 07:31:34
     UTC_TIME() + 0: 73134
    UTC_TIMESTAMP(): 2017-10-23 07:31:34
UTC_TIMESTAMP() + 0: 20171023073134
```

### to\_days(), to\_seconds()

语法

- to_days() 将日期转换成天数，第一天是0000-01-01
- to_seconds 将日期转换成秒数，第86400秒是0000-01-01 00:00:00

例子

```
select to_days('0000-01-01'),to_days('0000-01-02'),to_days('2012-12-12'),to_seconds('0000-01-01 00:00:00'),to_seconds('0000-01-01 00:00:01'),to_seconds('2012-12-12 12:12:12');
输出
            to_days('0000-01-01'): 1
            to_days('0000-01-02'): 2
            to_days('2012-12-12'): 735214
to_seconds('0000-01-01 00:00:00'): 86400
to_seconds('0000-01-01 00:00:01'): 86401
to_seconds('2012-12-12 12:12:12'): 63522533532
```

### period\_add(), period\_diff()

### adddate(), addtime(), date\_add(), timestampadd()

### datediff(), timediff(), timestampdiff()

### from\_days(), from\_unixtime()

### sec\_to\_time(), time\_to\_sec()

### subdate(), subtime()

### 其它

convert_tz()

date_sub()

extract()

get_format()

last_day

quarter()

str\_to\_date()

week()

yearweek()

### 参考

- [https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html](https://dev.mysql.com/doc/refman/5.7/en/date-and-time-functions.html)