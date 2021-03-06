# time模块
##### time.time()用于获取当前时间戳

```
import time;  # 引入time模块

ticks = time.time()
print "当前时间戳为:", ticks

当前时间戳为: 1459994552.51
```
##### 获取当前时间
```
import time

localtime = time.localtime(time.time())
print "本地时间为 :", localtime

本地时间为 : time.struct_time(tm_year=2016, tm_mon=4, tm_mday=7, tm_hour=10, tm_min=3, tm_sec=27, tm_wday=3, tm_yday=98, tm_isdst=0)
```
##### 获取格式化的时间
```
import time

localtime = time.asctime( time.localtime(time.time()) )
print "本地时间为 :", localtime

本地时间为 : Thu Apr  7 10:05:21 2016
```
##### 把日期格式化字符串
```
import time

# 格式化成2016-03-20 11:45:39形式
print time.strftime("%Y-%m-%d %H:%M:%S", time.localtime())

# 格式化成Sat Mar 28 22:24:24 2016形式
print time.strftime("%a %b %d %H:%M:%S %Y", time.localtime())
```
##### 字符串转换为时间戳
```
a = "Sat Mar 28 22:24:24 2016"
print time.mktime(time.strptime(a,"%a %b %d %H:%M:%S %Y"))
```
##### 把字符串转换为时新的字符串
- strptime(str,format):方法分析表示根据格式的时间字符串。返回值是一个struct_time所返回gmtime()或localtime()。
- strftime(format,date_truble):接收以时间元组，并返回以可读字符串表示的当地时间，格式由参数format决定
```
work_time = time.strftime("%Y-%m-%d", time.strptime("2013-05-23", "%Y-%m-%d"))
print work_time

"2013-05-23"
```
#### 格式化符号
```
%y 两位数的年份表示（00-99）
%Y 四位数的年份表示（000-9999）
%m 月份（01-12）
%d 月内中的一天（0-31）
%H 24小时制小时数（0-23）
%I 12小时制小时数（01-12）
%M 分钟数（00=59）
%S 秒（00-59）
%a 本地简化星期名称
%A 本地完整星期名称
%b 本地简化的月份名称
%B 本地完整的月份名称
%c 本地相应的日期表示和时间表示
%j 年内的一天（001-366）
%p 本地A.M.或P.M.的等价符
%U 一年中的星期数（00-53）星期天为星期的开始
%w 星期（0-6），星期天为星期的开始
%W 一年中的星期数（00-53）星期一为星期的开始
%x 本地相应的日期表示
%X 本地相应的时间表示
%Z 当前时区的名称
%% %号本身
```
##### 实例举例

```
import time
parsed_data["date"] = time.strftime('%Y-%m-%d', time.strptime(parsed_data["date"], '%Y-%m-%d %H:%M:%S'))
```
