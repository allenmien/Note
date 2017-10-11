# mongo语句
#### “and、or”
```
db.enterprises.find({}，{$or: [{resolveStatus: "待处理"}, {resolveStatus: "已完成"}], $and: [{zentao_task_id: {$ne: null}}, {zentao_task_id: {$ne: ""}}]})

```
