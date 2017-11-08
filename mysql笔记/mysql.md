# mysql
##### group by
- 有count一定要有group

```
SELECT
	STATUS,
	count(STATUS) AS cnt
FROM
	ods_entities
GROUP BY
	STATUS
ORDER BY
	cnt DESC
LIMIT 1000
```
- Having
```
SELECT
	p_ename,
	p_type1,
	p_court,
	p_dt1,
	p_direct_related,
	COUNT(*) AS cnt
FROM
	ods_properties
WHERE
	p_source = '9'
GROUP BY
	p_ename,
	p_type1,
	p_court,
	p_dt1,
	p_direct_related
HAVING
	cnt > 1
```
##### desc 降序；
```
SELECT
	*
FROM
	`properties`
WHERE
	`p_eid` = "0056b054-a145-4a4b-bde5-699ab39a8708"
AND `p_source` = "8"
ORDER BY
	`p_dt1` DESC;
```
##### 查重之后显示id
```
SELECT
	a.id,
	a.p_id,
	a.p_ename,
	a.p_type1,
	a.p_court,
	a.p_dt1,
	a.p_direct_related
FROM
	ods_properties a
INNER JOIN (
	SELECT
		p_ename,
		p_type1,
		p_court,
		p_dt1,
		p_direct_related,
		COUNT(*) AS cnt
	FROM
		ods_properties
	WHERE
		p_source = '9'
	GROUP BY
		p_ename,
		p_type1,
		p_court,
		p_dt1,
		p_direct_related
	HAVING
		cnt > 1
) b ON a.p_ename = b.p_ename
AND a.p_type1 = b.p_type1
AND a.p_court = b.p_court
AND a.p_dt1 = b.p_dt1
AND a.p_direct_related = b.p_direct_related
WHERE
	p_source = '9';
```
##### mysql分页、分批取数据
- LIMIT如果给定两个参数，第一个参数指定第一个返回记录行的偏移量，第二个参数指定返回记录行的最大数目。
- 这是两个参数，第一个是偏移量，第二个是数目
```
select * from table limit 2, 7; // 返回3-9行,偏移7个
select * from table limit 3,1; // 返回第4行
```
- 一个参数
```
select * from table limit 3; // 返回前3行,默认是0开始
```
- 效率
		- mysql中分页都是用的 limit 10000,20这样的方式.
		- 这样的效率是很低的。
		- 因为要先扫描1W多行才剔除前面的1W行，返回后面的结果。
```
mysql explain SELECT * FROM message ORDER BY id DESC LIMIT 10000, 20
```
- limit 10000,20的意思扫描满足条件的10020行，扔掉前面的10000行，返回最后的20行，每次查询需要扫描超过1W行，性能肯定大打折扣。
- 优化效率
		- 这句表示从id2500 开始，从0行开始以偏移量20 查询下去。
		- 其实传统的limit m,n，相对的偏移一直是第一页，这样的话越翻到后面，效率越差，而上面给出的方法就没有这样的问题。
```
SELECT * FROM table WHERE id >=2500 ORDER BY auto_id asc LIMIT 0,20
```
##### 占位符
```
%s
```
##### 删除
```
DELETE FROM properties WHERE id = %s
```
##### 写入
```
insert into `synchronize`(`mysql_id`,`type`,`created_time`) values('3_0c726e5d-680f-4d8c-a61a-2ceb0f9b9f8a_59cc041d0ec61a7c67e5ecf5','properties','2017-11-06 14:47:13');
```
