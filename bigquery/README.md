# BigQuery 
## BigQuery 적재 
### float 타입으로 표시되는 데이터 적재시 주의사항
- BigQuery는 float 타입을 부동소수점으로 표시하기때문에 DB와는 다르게 적재되는 경우가 생겨 ROUND 함수를 이용하는등 불편함이 생기는 경우가 있음
    -  [BigQuery Data Types](https://cloud.google.com/bigquery/docs/reference/standard-sql/data-types)
- 예) [MySQL의 float 타입을 BigQuery에 적재하려면](./MySQL_to_Bigquery.md)
### Streaming Insert 중복 제거
- [insert 요청시 request body에 insertId 추가하여 요청](https://cloud.google.com/bigquery/docs/streaming-data-into-bigquery?hl=ko)
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
    - 중복 row 확인
    ```SQL
    SELECT
        * EXCEPT row_number
    FROM
        (
            SELECT
                *,
                ROW_NUMBER OVER(PARTITION BY key_column) AS row_number
            FROM
                target_table
        )
    WHERE
        row_number > 1
    ```
    
## BigQuery 운영 및 관리
### 미사용 테이블 찾기
- [Query](find_unused_tables.sql)
- dataset과 table 이름을 이용하여 특정 기간동안 조회 이력이 없는 테이블 조회

### 샤딩 테이블 찾기
- [Query](count_sharding_tables.sql)
- 프로젝트의 전체 테이블 중 테이블중 각 테이블의 prefix와 샤딩테이블의 경우 갯수, 적재 시작일, 종료일 확인

### 커스텀 롤 생성
- [Code](generate_bq_custom_role.py)
- BigQuery 사용자의 커스텀 롤을 생성하여 권한 관리 목적
    - 예약쿼리 생성 및 수정 권한이 필요한경우 Editor 권한 부여
    - 기본 Role로 적용시 Admin 권한 필요하여 권한 분리
