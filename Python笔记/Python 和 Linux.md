### 使用python执行sh文件

sh文件

```
#!/bin/sh

cls_path=`pwd`
cls_path=$cls_path/:$cls_path/config/:$cls_path/lib/*:$PATH

java -cp $cls_path com.AlphaX.Foo $1
```

python脚本

```
import os
a = os.system("./start.sh hahah")
print a
```

