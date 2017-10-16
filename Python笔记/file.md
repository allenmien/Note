# 文件模块
##### 读取文件
- 最基本的读文件方法
```Python
file = open(u"./dataid.txt")
        while True:
            line = file.readline()
            if not line:
                break
            job
```
- fileinput模块
```
import fileinput

for line in fileinput.input("sample.txt"):
    pass

```
- 带缓存的文件读取
```
file = open("sample.txt")

while 1:
    lines = file.readlines(100000)
    if not lines:
        break
    for line in lines:
        pass # do something

```
- file对象 for 循环
```
file = open("sample.txt")

for line in file:
    pass # do something
```
- xreadlines迭代器
```
file = open("sample.txt")

for line in file.xreadlines():
    pass # do something
```
##### 写文件
```Python
file_obj = io.open(u"../logs/" + u"data_id" + u".txt", mode=u"a", encoding=u"utf-8")
        file_obj.write(dataid + u"\n")
        file_obj.close()
```
