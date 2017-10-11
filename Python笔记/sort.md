# sort排序
- 默认升序。降序排列，只需要加上reverse=True
- sorted 和list.sort 都接受key, reverse定制。
- list.sort()是列表中的方法，只能用于列表。list.sort()是在原序列上进行修改，不会产生新的序列。
- 而sorted可以用于任何可迭代的对象。sorted() 会返回一个新的序列。旧的对象依然存在。
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
##### 按list中某一元素的某一部分，对列表排序
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
#### sort() cmp
- 实际上sort()方法在不传入参数func的时候 默认cmp为None。
- 调用的是lambda x,y: cmp(x, y),而实际上就是调用cmp函数
- compare(x,y)函数会在x<y时返回负数，在x>y时返回正数，如果x=y则返回0

```Python
numbers=[5,2,9,7]
numbers.sort(lambda a,b:b-a)
numbers
[9,7,5,2]
```
```Python
persons=[{'name':'zhang3','age':15},{'name':'li4','age':12}]
persons.sort(lambda a,b:a['age']-b['age'])
persons
[{'age': 12, 'name': 'li4'}, {'age': 15, 'name': 'zhang3'}]
```
