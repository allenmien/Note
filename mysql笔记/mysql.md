# mysql
1. group by
- 有count一定要有group

```
select status,count(status) as cnt from ods_entities group by status order by cnt desc limit 1000
```
2. desc 降序；
