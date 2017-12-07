# underscore
#####1_.sortBy
对象排序，字符串sortBy逆序加-号是排不出来的，可以采用sortBy().revrese()的方法。

方法1：
```
var _ = require("underscore");
var a = ["2014-12-03","2015-06-10","2014-12-23","2015-04-21"];

var b = _.sortBy(a,(o)=>{
    return o
}).reverse();

console.log(b);

结果：
[ '2015-06-10', '2015-04-21', '2014-12-23', '2014-12-03' ]
```
方法2：
```
var _ = require("underscore");
var arr = ["2014-12-03", "2015-06-10", "2014-12-23", "2015-04-21"];

var b = arr.sort((a, b) => {
    return a < b ? 1 : -1
})

console.log(b);
结果：
[ '2015-06-10', '2015-04-21', '2014-12-23', '2014-12-03' ]
```
#####2._.reduce
_.reduce(list, iteratee, [memo], [context])
list:数组；
iteratee： function(memo, num){ return memo + num; }
              memo是初始值，num是list里的每一个元素
memo：初始值
```
var _ = require("underscore");
var a = [1,2,3,4,5]
var b = 3;
var c = _.reduce(a,function(memo,o){return memo + o*b}
,0)
console.log(c);

```
首先memo = 0，
第一次：memo = memo + o x b = 0+1 x 3 = 3,
第二次：memo = 3+2*3 = 9,
...
c = memo

#####3._.extend
list添加新属性：
```
_.extend({name: 'moe'}, {age: 50});
=> {name: 'moe', age: 50}
```
#####4._.groupBy
把长度为200的数组，按照100的长度，分为一个长度为2的数组。

function (num, index) {}
num代表数组的元素，
index代表该元素在数组的索引值。

```
var _ = require("underscore");
var a = [1,2,3,4,5,...];
a.length =200;
b = _.groupBy(a,function (num, index) {
    return Math.floor(index / 100);
})
[[100],[100]]
```
#####5._.flatten
如果是一个数组，外面嵌套了很多层，可以一次性取到裸的数组，如：
```
var _ = require("underscore");
var a = [1,[2],[[3,4]],[[[5]]]];
var b = _.flatten(a);
console.log(b);
[ 1, 2, 3, 4, 5 ]
```
如果是一个数组，里面有多个元素，每个元素嵌套了多层，想要每个元素只减少一维可以设置参数为true，如：
```
var _ = require("underscore");
var a = [1,[2],[[3,4]],[[[5]]]];
var b = _.flatten(a,true);
console.log(b);
[ 1, 2, [ 3, 4 ], [ [ 5 ] ] ]
```
#####6._.partition
拆分一个数组为两个数组：  第一个数组其元素都满足条件， 而第二个的所有元素均不能满足predicate条件。
```
var _ = require("underscore");
var a = _.partition([0, 1, 2, 3, 4, 5], function(num){
    return num <3;
});
console.log(a);
[ [ 0, 1, 2 ], [ 3, 4, 5 ] ]
```
#####7._.union
并集，合并去重顺序
```
_.union([1, 2, 3], [101, 2, 1, 10], [2, 1]);
=> [1, 2, 3, 101, 10]
```
#####8._.difference
_.difference(a, b)
去除掉a数组中包含，b数组中的元素
```
_.difference([1, 2, 3, 4, 5], [5, 2, 10]);
=> [1, 3, 4]
```
#####9._.range
快速创建数组
```
_.range(10);
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
_.range(1, 11);
=> [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
_.range(0, 30, 5);
=> [0, 5, 10, 15, 20, 25]
_.range(0, -10, -1);
=> [0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
_.range(0);
=> []
```
#####10._.pairs
把一个对象转变为一个[key, value]形式的数组。
```
_.pairs({one: 1, two: 2, three: 3});
=> [["one", 1], ["two", 2], ["three", 3]]
```
#####11._.pick
返回一个对象中想要的属性，其他属性不显示
```
_.pick({name: 'moe', age: 50, userid: 'moe1'}, 'name', 'age');
=> {name: 'moe', age: 50}
```
#####12._.isEqual
执行两个对象之间的优化深度比较，确定他们是否应被视为相等。
```
var stooge = {name: 'moe', luckyNumbers: [13, 27, 34]};
var clone  = {name: 'moe', luckyNumbers: [13, 27, 34]};
stooge == clone;
=> false
_.isEqual(stooge, clone);
=> true

var _ = require("underscore");
var a = _.range(0,10);
var b = _.range(1,10);
var c = _.isEqual(a,b);
console.log(c);
false
```
