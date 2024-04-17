# BigQuery 
## BigQuery 내용 
### float 타입으로 표시되는 데이터 적재시 주의사항
- BigQuery는 float 타입을 부동소수점으로 표시하기때문에 DB와는 다르게 적재되는 경우가 생겨 ROUND 함수를 이용하는등 불편함이 생기는 경우가 있음
    -  [BigQuery Data Types](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types)
- 예) [MySQL의 float 타입을 BigQuery에 적재하려면](./MySQL_to_Bigquery.md)
### Streaming Insert 중복 제거
- insert 요청시 request body에 insertId 추가하여 요청
    ```JSON
    {
    "kind": string,
    "skipInvalidRows": boolean,
    "ignoreUnknownValues": boolean,
    "templateSuffix": string,
    "rows": [
        {
        "insertId": string,
        "json": {
            object
        }
        }
    ],
    "traceId": string
    }
    ```
    - insertId 속성을 사용하여 최대 1분 동안 최선형 중복 삭제를 지원