# Airflow 
## Ariflow 내용 
### 직전 시간 배치 task 수행 완료 후 다음 task 수행하려면?
- backfill 수행 시 직전 배치의 task 완료 확인 후 수행되어야 하는 task라면 각 task간 의존성을 주어야 한다.
    - [code](./depends_on_past.py)

### helm으로 설치한 airflow에 특정 pip package를 설치하려면?
- pip package가 설치된 이미지 빌드하여 사용
- `PythonVirtualenvOperator` 사용하여 pip package 설치 후 사용
    ```Python
    virtualenv_task = PythonVirtualenvOperator(
        task_id="virtualenv_python",
        python_callable=callable_virtualenv,
        requirements=["colorama==0.4.0"],
        system_site_packages=False,
    )
    ```
