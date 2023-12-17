# Logstash
1. Applycation 서버에서 파일로 로그를 저장하고 File Input, Kafka Output을 사용하는 파이프라인에서의 로그 유실 발생
- Loglotate의 CopyTruncate 과정중에 로그 유실 확인
    - Copy 후 Truncate가 수행되는 사이에 적재된 로그 유실발생 확인
    - File Input의 stat_interval 조절하여 유실 최소화
    - 가장 좋은 방법은 postrotate script 이용하여 lotate 수행 후 Application Logger가 새로 생성된 로그파일을 바라보도록 재시작 시그널 날려주는 방법으로 보임
- [Logstash 설정 파일](https://github.com/hoseong0422/elk/logstash/file_to_kafka.conf)