# bluebird
##### Promise.reduce
- 遍历异步方法：
```javascript
Promise.reduce(historyResults,function (arr,historyResult) {
                return ApiUsers.find({_id:historyResult._id.user_id},{name:1,qxb_balance:1})
                    .then(function (apiResults) {
                        var estimatedAmount = (historyResult.amount/dateDiff) * 7;
                        if(apiResults[0].qxb_balance<estimatedAmount){
                            arr.push({
                                user_id : apiResults[0]._id.toString(),
                                qxb_balance:apiResults[0].qxb_balance,
                                name:apiResults[0].name,
                                cost:historyResult.amount
                            })
                            return arr;
                        }

                    })
            },[]).then(function (data) {
                    // console.log(data);
                    return resolve(data);
            })
```
##### Promise.props
- 处理一个对象，对象的每一个属性都是异步的。
- getPictures()，getComments()，getTweets()都是异步方法。
```javascript
Promise.props({
 pictures: getPictures(),
 comments: getComments(),
 tweets: getTweets()
}).then(function(result) {
 console.log(result.tweets, result.pictures, result.comments);
});
```
