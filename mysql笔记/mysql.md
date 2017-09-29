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
