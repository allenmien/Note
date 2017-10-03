# 基本常用语法
##### 判断是不是某个dic的键值
```
if u"ten_shs" in temp_ten_shs_dic.keys():
    print "1"
```
##### args和 kwargs？
```Python
# coding:utf-8
def foo(*args, **kwargs):
    print 'args = ', args
    print 'kwargs = ', kwargs
    print '---------------------------------------'


if __name__ == '__main__':
    foo(1, 2, 3, 4)
    foo(a=1, b=2, c=3)
    foo(1, 2, 3, 4, a=1, b=2, c=3)
    foo('a', 1, None, a=1, b='2', c=3)
```
```
Result:

args =  (1, 2, 3, 4)
kwargs =  {}
---------------------------------------
args =  ()
kwargs =  {'a': 1, 'c': 3, 'b': 2}
---------------------------------------
args =  (1, 2, 3, 4)
kwargs =  {'a': 1, 'c': 3, 'b': 2}
---------------------------------------
args =  ('a', 1, None)
kwargs =  {'a': 1, 'c': 3, 'b': '2'}
---------------------------------------
```
- args是一个tuple；
- kwargs是一个dict。
- 同时使用args和kwargs时，必须args要在kwargs前

例子：
```Python
# coding:utf-8
def kw_dict(**kwargs):
    return kwargs
print kw_dict(a=1, b=2, c=3)
```
```
result:

{'a': 1, 'c': 3, 'b': 2}
```
##### 输入函数
```Python
name = raw_input('please enter your name: ')
print 'hello,', name
```
```
result:
please enter your name: maqi
hello, maqi
```
