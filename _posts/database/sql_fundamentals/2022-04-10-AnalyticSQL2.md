---
title:  "Analytic SQL 개요"

categories:
  -   "SQL Fundamentals"
tags:
  - [Database, PostgreSQL, SQL]

toc: true
toc_sticky: true

date: 2022-04-10
last_modified_at: 2022-04-10
---

### 요약
- 랭크나 분위 등을 이용해서 인기있는 강의, 많이 듣는 강사, 많이 이용하는 기업 등의 정보를 출력할 수 있겠다. 
- 나중에 group.tag가 생기면 스타트업/대기업/중소기업 구분해서 순위를 매길 수도 있다.(group by를 하지 않고)

## analytic sql = window function

group by는 원본 데이터 집합의 레벨을 변경하여 적용하기 때문에 이용이 어려울 수 있다. 
analytic sql은 원본 데이터 집합의 레벨을 그대로 유지하면서 사용함.
window를 이용하여 row 단위의 집합 연산을 수행할 수 있다. 

|유형|함수|
|:----:|:-----------|
|순위/비율 함수|rank, dense_rank, row_number, percent_rank, cume_dist, ntile|
|집계함수|sum, max, min, avg, count|
|lead/lag|lead, lag|
|first_value / last_value|first_value, last_value|
|inverse percentile|percentile_cont, percentile_disc|

### 사용법
: 원본 데이터의 레벨을 그대로 유지하면서, 그루핑 레벨에서 자유롭게 window의 이동과 크기를 조절하면서 analytic 수행
```sql
{analytic function}(arg1, arg2...)
OVER(
  {partition 절(partition by)} : 그룹화 컬럼명
  {sorting 절(order by)} : 정렬 컬럼명(window 이동방향 기준 컬럼명)
  {window 절} : window 범위(row, range)
)
```
### 순위
- 일반적인 순위 : rank, dense_rank, row_number
- 0~1 사이 정규화 순위 : cume_dist, percent_rank
- 분위 : ntile

#### 1. 일반 순위 : 공동순위를 정하는 기준이 조금씩 다름
- rank : 공동순위가 있을 경우 다음 순위는 공동순위 개수만큼 밀려서 정함 <br>
1,2,2,4 또는 1,2,2,2,5
- dense_rank : 공동순위가 있더라도 다음 순위는 바로 이어서 정함<br>
1,2,2,3 또는 1,2,2,2,3
- row_number : 공동순위가 있더라도 반드시 unique한 순위를 정함<br>
1,2,3,4,5 (기준 없이 정함)

> 참고: order by smth desc ***nulls first***를 하면 null값을 1순위로 해줄 수 있다. (당연히 last도 가능)
> 근데 default가 nulls first라서 실수할 수 있음. 

> 업무적으로 순위를 정하는 경우가 많은데, row_number가 전반적으로 많이 사용된다. <br>
> 인프런의 예 : 그룹 신청액 수 랭킹
```sql
with a as (
	select od.group_id, g.name, sum(ct.pay_price) as pp
	from carts ct 
	inner join orders od on od.id = ct.order_id 
	inner join groups g on g.id = od.group_id 
	where od.deleted_at is null and od.group_id is not null and od.created_at > '2022-03-31' and ct.pay_price > 0
	group by od.group_id, g.name
	order by group_id desc
)
select *
	, rank() over(order by a.pp desc) as rank
	, dense_rank() over(order by a.pp desc) as dense_rank 
	, row_number() over(order by a.pp desc) as row_number 
from a 
```
![image](https://user-images.githubusercontent.com/53845159/162615951-debc2f37-5a21-498c-a484-9f26f7d76333.png)


