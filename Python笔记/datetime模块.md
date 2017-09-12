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
#### timestamp转换到UTC标准时区的时间
```
from datetime import datetime
t = 1429417200.0
print(datetime.fromtimestamp(t)) # 本地时间

2015-04-19 12:20:00
print(datetime.utcfromtimestamp(t)) # UTC时间

2015-04-19 04:20:00
```
#### datetime转换为str
```
from datetime import datetime
now = datetime.now()
print(now.strftime('%a, %b %d %H:%M'))

Mon, May 05 16:28
```
