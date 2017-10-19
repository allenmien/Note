# mongo语句
#### “and、or”
```
db.enterprises.find({}，{$or: [{resolveStatus: "待处理"}, {resolveStatus: "已完成"}], $and: [{zentao_task_id: {$ne: null}}, {zentao_task_id: {$ne: ""}}]})

```
#### $or 查找
```
db..find({$or: [{eid: "000a1642-f772-4bd7-a7e3-772a3e71ebea"}, {eid: "1a458ea1-3b54-414c-a610-edfe00f4b2f4"}]})
```
#### find
- licenses_alls字段存在
```
db.enterprises.find({licenses_alls:{$elemMatch:{$ne:null}}})
db.enterprises.find({licenses_alls:{$ne:null}})
```
- licenses_alls字段不存在
```
db.enterprises.find({judicial_freezes_alls:null})
```
- licenses_alls字段存在，但是为空数组
```
db.enterprises.find({judicial_freezes_alls:{"$size":0}})
```
- 包括null和没有这个字段
```
db.C.find({"c":null})
```
- 仅仅null
```
db.C.find({"c":{"$in":[null],"$exists":true}})
```
