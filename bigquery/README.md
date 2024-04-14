# BigQuery 
## BigQuery 내용 
### float 타입으로 표시되는 데이터 적재시 주의사항
- BigQuery는 float 타입을 부동소수점으로 표시하기때문에 DB와는 다르게 적재되는 경우가 생겨 ROUND 함수를 이용하는등 불편함이 생기는 경우가 있음
    -  [BigQuery Data Types](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types)
- 예) [MySQL의 float 타입을 BigQuery에 적재하려면](./MySQL_to_Bigquery.md)
