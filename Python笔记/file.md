# 文件模块
##### 读取文件
```Python
file = open(u"./dataid.txt")
        while True:
            line = file.readline()
            if not line:
                break
            job
```
##### 写文件
```Python
file_obj = io.open(u"../logs/" + u"data_id" + u".txt", mode=u"a", encoding=u"utf-8")
        file_obj.write(dataid + u"\n")
        file_obj.close()
```
