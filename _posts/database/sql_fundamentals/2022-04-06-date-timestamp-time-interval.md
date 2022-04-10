---
title:  "date와 timestamp, interval 이해"

categories:
  -   "SQL Fundamentals"
tags:
  - [Database, PostgreSQL, SQL, timestamp]

toc: true
toc_sticky: true

date: 2022-04-06
last_modified_at: 2022-04-06
---

### 결론 
1. 원래 파이썬으로 'for(timestamp -> string 변환 -> 앞글자만 추출)'로 무식하게 date를 꺼냈는데, 그냥 to_char(created_at)::date로 처리할 수 있었던 것...
2. date_trunc로 연 첫날, 월 첫날, 마지막날, 신청월 등 다양한 처리를 할 수 있었다. 
3. 바로빌 세금계산서 대량 업로드 시, 'mm-dd' 형식으로 적는 부분이 있는데 이또한 formatting으로 해결할 수 있을 것 같다.<br>
> 인프런의 예 : 만약에 월별 생성된 바우처의 수를 알고 싶을 때에는 포멧팅을 통해서 아래와 같이 만들 수 있는 것 
```sql
select to_char(date_trunc('month', v.created_at), 'mm')
from vouchers v
```

### date/timestamp/time/interval

- date : YYYY-MM-DD, 일자로서 년-월-일 정보를 가짐
- timestamp : YYYY-MM-DD HH24:MI:SS, 일자를 시간정보까지 같이 가짐
- time : HH24:MI:SS 오직 시간정보만 가짐
- interval : N days HH24:MI_SS, 기간

#### string을 timestamp로 변환
- to_date('2022-01-01', 'yyyy-mm-dd') = 2022-01-01
- to_timestamp('2022-01-01', 'yyyy-mm-dd') = 2022-01-01 00:00:00.000 + 0900
- to_timestamp('2022-01-01 14:36:52', 'yyyy-mm-dd hh24:mi:ss') = 2022-01-01 14:36:52.000 + 0900<br>


> *format은 변경이 가능하다.*<br>
*formatting은 중간에 문자를 섞을수도 있다.*

#### 문자열이 아닌 것을 string으로 변환
- to_char(hiredate, 'yyyy-mm-dd') = 1980-12-17

#### 다양한 formatting pattern(참고)
- w : 월의 주(1-5)
- ww : 연의 주(1-52)
- day : monday...
- month : january...
- MONDAY : JANUARY...
- tz : time zone(시간대)

#### postgreSQL에서만 되는 것들(명시적 형변환)
- Date를 Timestamp로 변환 <br>
select to_date('2022-01-01', 'yyyy-mm-dd')::timestamp;
- timestamp를 text로 변환 <br>
select to_timestamp('2022-01-01', 'yyyy-mm-dd')::text;
- timestamp를 Date로 변환 <br>
select to_timestamp('2022-01-01 14:36:52', 'yyyy-mm-dd hh24:mi:ss')::date

#### extract와 date_part를 이용하여 Date/Timestamp에서 년, 월, 일/시간, 분, 초 추출
- extract : <br>
extract(year from hiredate) as year
- date_part : <br>
date_part('year', hiredate) as year

> *date_part를 이용하는 것이 문법적으로 더 쉬워보인다.*

#### interval
- date타입에는 day를 기준으로 숫자를 더하거나 뺄 수 있다.<br>
select to_date('2022-01-01', 'yyyy-mm-dd') + 2
- timestamp타입에 숫자값을 더하거나 빼면 오류가 발생한다. <br>
select to_timestamp('2022-01-01', 'yyyy-mm-dd') + 2 << 이거 안됨!! 
- **timestamp에는 interval 타입을 이용해서 연산을 수행한다.**<br>
select to_timestamp('2022-01-01', 'yyyy-mm-dd') + interval '2 hour' << 이거는 됨!!
> *interval은 date 타입에도 적용할 수 있다.*

#### interval을 이용한 기간 구하기
- date - date를 하면 기간의 정수값이 나온다. <br>
'2022-01-03' - '2022-01-01' = 2
- from과 to를 더하면? : 더하기가 안됩니다. 

#### '지금' 구하기 
- now(), current_timestamp, current_date, current_time

> 이런 방식의 계산도 가능하다 : now() - hiredate = 근속기간 

#### date_trunc() : timestamp 잘라내기
- trunc 사용 예 : <br>
select trunc(99.9999, 2)
- select date_trunc('day', '2022-03-03 14:05:32'::timestamp) = 날짜 이하를 00으로 만들어버린다. 
- select date_trunc('day', to_date('2022-03-03', 'yyyy-mm-dd')) = **date 타입을 잘랐지만, 무조건 자르고나면 timestamp타입으로 나온다. 이를 방지하기 위해서는 명시적 타입변환이 필요함**
> date_trunc('month', orders.created_at::date) = 이렇게 하면 달의 첫날을 구할 수 있다.<br>
year(1월1일), week(월요일) 등을 구할 수 있다. 일요일을 구하고 싶다면 -1

