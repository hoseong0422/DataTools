# Elasticsearch, Kibana 사용중 이슈 해결 내용
## Elasticsearch
### 특정 데이터 노드에 용량큰 인덱스가 쏠려있을 경우
- reroute API를 이용해서 노드간 인덱스를 reroute 수행
    ```
    POST /_cluster/reroute
    {
        "commands": [
                {
                "move": {
                    "index": "{index_name}",
                    "shard": 0,
                    "from_node": "{source_node}",
                    "to_node": "{target_node}"
                }
            }
        ]
    }
    ```
- number_of_shard의 개수를 조절하여 특정 데이터 노드에 용량 큰 인덱스 쏠림 방지
    - 인덱스 생성시에 적용되는 옵션이므로 인덱스 템플릿 설정에서 추가
    - 템플릿 설정에서 total_field limit 설정도 추가 가능
    ```
    {
        "index": {
            "number_of_shards": "2",
            "mapping": {
            "total_fields": {
                "limit": "2000"
                }
            }
        }
    }
    ```