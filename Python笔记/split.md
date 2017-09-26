# split
```Python
#python中不存在单个字符的运算，只有字符串函数  
>>> s="www.google.com"  
>>> s  
'www.google.com'  
>>> s.split('.')                 #无参数全部切割  
['www', 'google', 'com']  
>>> s.split('.',1)               #分隔一次  
['www', 'google.com']  
>>> s.split('.')[1]              #取出被'.'分开的第一个字符串  
'google'  
>>> s.split('.',-1)              #小于0 的参数为全部切割  
['www', 'google', 'com']  
>>> s1,s2,s3=s.split('.',2)      #s1,s2,s3分别赋值得到被切割的三个部分  
>>> s1  
'www'  
>>> s2  
'google'  
>>> s3  
'com'  
>>> s='''''call                    #切割换行符！
me
baby'''  
>>> s  
'call\nme\nbaby'  
>>> print s                     #print显示效果  
call  
me  
baby  
>>> s.split('\n')  
['call', 'me', 'baby']  
>>> s="hello world!<[www.google.com]>byebye"  #经典样例  
>>> s  
'hello world!<[www.google.com]>byebye'  
>>> s.split('[')[1].split(']')[0]  
'www.google.com'  
>>> s.split('[')[1].split(']')[0].split('.')  
['www', 'google', 'com']  
```
