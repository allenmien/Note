# vi
##### 更改保存文件
```
vi:command mode
i:Insert mode
esc:command mode
: w filename （输入 「w filename」将文章以指定的文件名filename保存）
: wq (输入「wq」，存盘并退出vi)
: q! (输入q!， 不存盘强制退出vi)
```
##### 去掉所有双引号
```
%s/"//g
```
