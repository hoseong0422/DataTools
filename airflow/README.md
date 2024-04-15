# Airflow 
## Ariflow 내용 
### 직전 시간 배치 task 수행 완료 후 다음 task 수행하려면?
- backfill 수행 시 직전 배치의 task 완료 확인 후 수행되어야 하는 task라면 각 task간 의존성을 주어야 한다.
    - [code](./depends_on_past.py)
