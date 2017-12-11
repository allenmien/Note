# datetime模块
#### 获取当前日期和时间
```
from datetime import datetime
now = datetime.now() # 获取当前datetime
print(now)

2015-05-18 16:28:07.198690
```
#### 获取指定日期和时间
```
from datetime import datetime
dt = datetime(2015, 4, 19, 12, 20) # 用指定日期时间创建datetime
print(dt)

2015-04-19 12:20:00
```
#### timestamp转换为datetime
```
 from datetime import datetime
t = 1429417200.0
print(datetime.fromtimestamp(t))

2015-04-19 12:20:00
```
#### 把str转化为datetime
```
from datetime import datetime
cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
print(cday)

2015-06-01 18:19:59
```
#### timestamp转换到UTC标准时区的时间
```
from datetime import datetime
t = 1429417200.0
print(datetime.fromtimestamp(t)) # 本地时间

2015-04-19 12:20:00
print(datetime.utcfromtimestamp(t)) # UTC时间

2015-04-19 04:20:00
```
#### 获取现在时间的UTC标准时区的时间
```
datetime.datetime.utcnow()
```
#### datetime转换为str
```
from datetime import datetime
now = datetime.now()
print(now.strftime('%a, %b %d %H:%M'))

Mon, May 05 16:28
```

#### date转换实例

```python
__DATE_FORMATS = [
    "%Y-%m-%d", "%Y-%m-%d %H:%M", "%Y-%m-%dT%H:%M:%S", "%Y-%m-%d %H:%M:%S",
    "%Y%m%d", "%Y%m%d%H%M%S", "%Y年%m月%d日", "%Y年%m月%d日 %H:%M:%S",
    "%Y年%m月%d日 %H:%M", "%Y/%m/%d", "%Y/%m/%d %H:%M:%S", "%Y.%m.%d",
    "%Y.%m.%d %H:%M:%S", "%Y年%m月%d", "%Y年%m月%d %H:%M:%S", "%b %d %Y",
    "%b %d %Y %H:%M:%S", "%b %d, %Y %H:%M:%S", "%b %d, %Y %H:%M:%S %p",
    "%b %d, %Y", "%d-%b-%y"
]


def format_date(date_str, format):
    """
    format date
    :param date_str: 
    :param format: 
    :return: 
    """
    if date_str is None or len(date_str) == 0:
        return ""
    date_str = date_str.replace("&nbsp;", "").strip()
    if len(date_str) == 0:
        return ""
    result = date_str
    for _dateformat in __DATE_FORMATS:
        try:
            temp_date = datetime.datetime.strptime(date_str, _dateformat)
            result = temp_date.strftime(format)
            break
        except:
            result = ""
            continue
    return result
```

