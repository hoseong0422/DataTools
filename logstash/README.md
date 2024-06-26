# Logstash
1. Applycation 서버에서 파일로 로그를 저장하고 File Input, Kafka Output을 사용하는 파이프라인에서의 로그 유실 발생
- Loglotate의 CopyTruncate 과정중에 로그 유실 확인
    - Copy 후 Truncate가 수행되는 사이에 적재된 로그 유실발생 확인
    - File Input의 stat_interval 조절하여 유실 최소화
    - 가장 좋은 방법은 postrotate script 이용하여 lotate 수행 후 Application Logger가 새로 생성된 로그파일을 바라보도록 재시작 시그널 날려주는 방법으로 보임
        - [예시 rotate 설정](logrotate/test_rotate)
- [Logstash 설정 파일](kafka_to_es.conf)
    - 로그 중복 적재 해소
        - auto_commit_interval_ms 조절
            - default 5000 -> 2000
        - enable_auto_commit 변경
            - true -> false
            - false로 할경우 메시지를 Fetch 할때마다 commit
                - `If true, periodically commit to Kafka the offsets of messages already returned by the consumer. If value is false however, the offset is committed every time the consumer fetches the data from the topic.`