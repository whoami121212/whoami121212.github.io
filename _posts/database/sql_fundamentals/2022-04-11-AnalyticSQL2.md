---
title:  "Aggregate Analytic과 window 상세"

categories:
  -   "SQL Fundamentals"
tags:
  - [Database, PostgreSQL, SQL, Window]

toc: true
toc_sticky: true

date: 2022-04-11
last_modified_at: 2022-04-11
---

### 요약
- avg(pay_price) over (partition by group_id order by created_at) 등을 이용해서 그래프를 만들면 그룹당 시간이 지나면서 이용금액 이동평균선을 구할 수 있을 것 같습니다. 아니면 group by로 월별 이용금액으로 묶어도 월별 평균 이용금액 이동평균선이 될 수도 있고 
- over()안에 아무것도 안 넣더라도, group by를 실행하지 않고 row단위로 집계함수를 실행할 수 있다는 것은 큰 장점으로 보인다. 

집계 : sum(), max(), min(), avg(), count()
aggregate analytic sql : 집계함수를 window를 이용하여 로우 레벨로 자유롭게 집계할 수 있는 기능을 제공 

```sql
{analytic function}(arg1, arg2...)
OVER(
  {partition 절(partition by)} : 그룹화 컬럼명
  {sorting 절(order by)} : 정렬 컬럼명(window 이동방향 기준 컬럼명)
  {window 절} : window 범위(rows, range)
)
```
- sum()에 partition by가 있으면 부분합을 보여주고, order by까지 있으면 부분합 내에서 누적합을 계산한다. 
- window절 : 가장 syntax가 길어서 공부가 많이 필요하다. rank에는 필요없음. order by를 쓰는 순간 default로 윈도우가 생긴다. <br>
default window : ***rows between unbounded preceding and current row***
- 만약 order by 절이 없다면 window는 해당 partition의 모든 row를 대상으로 한다. 
- partition절도 없다면 window는 전체 데이터의 row를 대상으로 한다. 

### max(), min() analytic sql
- over()를 이용해서 누적 최대/최소 금액을 구할 수 있음. 



