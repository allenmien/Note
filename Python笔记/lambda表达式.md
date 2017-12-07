# lambda表达式与函数式编程
##### lambda后面跟一个或多个参数
```
f = lambda x,y,z : x+y+z  
print f(1,2,3)  
6

g = lambda x,y=2,z=3 : x+y+z  
print g(1,z=4,y=5)  
8
```

