# 管道聚合
##### 示例代码
```javascript
var date = new Date('2017-2-22');
var time = new Date('2017-2-1');

db.apicallhistories.aggregate(
        [
            { $match : { created_date : { $gt :time ,$lte : date} } },

            {$group:{_id :{user_id:"$user_id",method_name:"$method_name",status:"$status"},count:{$sum:1},
                    totalcost:{$sum:"$cost"},avg_timespent:{$avg:"$time_spent"},max_timespent:{$max:"$time_spent"},min_timespent:{$min:"$time_spent"}

            }}
        ]
    )
```
