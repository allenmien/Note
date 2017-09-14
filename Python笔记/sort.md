# sort排序
- sorted 和list.sort 都接受key, reverse定制。但是区别是。list.sort()是列表中的方法，只能用于列表。而sorted可以用于任何可迭代的对象。list.sort()是在原序列上进行修改，不会产生新的序列。所以如果你不需要旧的序列，可以选择list.sort()。 sorted() 会返回一个新的序列。旧的对象依然存在。
- 如果进行降序排列，只需要加上reverse=True
##### 字符串排序
sorted 字符串返回一个list
```
a = '123456'
b = sorted(a)
print b

['1', '2', '3', '4', '5', '6']
```
##### 列表排序
```
a = [1, 4, 5, 2, 3, 6]
b = sorted(a)
print b

[1, 2, 3, 4, 5, 6]
```
##### 字典键排序
```
a = {1: 'q', 3: 'c', 2: 'g'}
b = sorted(a)
print b

a = {1: 'q', 3: 'c', 2: 'g'}
b = sorted(a.keys())
print b

[1, 2, 3]
```
##### 字典值排序
```
a = {1: 'q', 3: 'c', 2: 'g'}
b = sorted(a.values())
print b

['c', 'g', 'q']
```
##### 字典d对键值排序，返回元组列表
```
a = {1: 'q', 3: 'c', 2: 'g'}
b = sorted(a.items())
print b

[(1, 'q'), (2, 'g'), (3, 'c')]
```
##### 按list中某一元素的某一部分，岁元素排序
```
a = ['Chr1-10.txt', 'Chr1-1.txt', 'Chr1-2.txt', 'Chr1-14.txt', 'Chr1-3.txt', 'Chr1-20.txt', 'Chr1-5.txt']
b = sorted(a, key=lambda d: int(d.split('-')[-1].split('.')[0]))
print b

['Chr1-1.txt', 'Chr1-2.txt', 'Chr1-3.txt', 'Chr1-5.txt', 'Chr1-10.txt', 'Chr1-14.txt', 'Chr1-20.txt']
```
##### 对字典列表，按照字典的某一个属性排序
```
temp_ten_shs.sort(key=lambda x: x[u"dt"], reverse=True)
temp_ten_shs_dic = temp_ten_shs[0]
```
