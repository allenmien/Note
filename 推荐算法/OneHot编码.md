# OneHot编码

## One-Hot Encoding

 在实际的机器学习的应用任务中，特征有时候并不总是连续值，有可能是一些分类值，如性别可分为“male”和“female”。在机器学习任务中，对于这样的特征，通常我们需要对其进行特征数字化，如下面的例子：

有如下三个特征属性：

- 性别：["male"，"female"]
- 地区：["Europe"，"US"，"Asia"]
- 浏览器：["Firefox"，"Chrome"，"Safari"，"Internet Explorer"]

对于某一个样本，如["male"，"US"，"Internet Explorer"]，我们需要将这个分类值的特征数字化，最直接的方法，我们可以采用序列化的方式：[0,1,3]。但是这样的特征处理并不能直接放入机器学习算法中。

## One-Hot Encoding的处理方法

对于上述的问题，性别的属性是二维的，同理，地区是三维的，浏览器则是思维的，这样，我们可以采用One-Hot编码的方式对上述的样本“["male"，"US"，"Internet Explorer"]”编码，“male”则对应着[1，0]，同理“US”对应着[0，1，0]，“Internet Explorer”对应着[0,0,0,1]。则完整的特征数字化的结果为：[1,0,0,1,0,0,0,0,1]。这样导致的一个结果就是数据会变得非常的稀疏。

## 实际的Python代码

```python
from sklearn import preprocessing  
  
enc = preprocessing.OneHotEncoder()  
enc.fit([[0,0,3],[1,1,0],[0,2,1],[1,0,2]])  
  
array = enc.transform([[0,1,3]]).toarray()  
  
print array  
```

```python
[[ 1.  0.  0.  1.  0.  0.  0.  0.  1.]]
```

