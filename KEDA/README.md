# KEDA
- KEDA(Kubernetes Event-driven Autoscaler)는 이벤트 기반으로 Kubernetes 워크로드를 **자동 확장(Auto Scaling)**할 수 있도록 도와주는 오픈소스 컴포넌트
- 일반적인 Horizontal Pod Autoscaler(HPA)는 CPU나 메모리 사용률 같은 리소스 기반 지표로 스케일링을 수행하지만, KEDA는 메시지 큐(Pub/Sub, Kafka, RabbitMQ 등), 데이터베이스, HTTP 요청 수, 사용자 정의 메트릭 등 이벤트 기반 지표를 활용할 수 있다는 점이 특징
## 주요 리소스
- [ScaledObject.yaml](./ScaledObject.yaml)
    - 특정 Deployment, StatefulSet 등을 KEDA로 스케일링 대상으로 연결하고, 트리거 조건을 정의
- [TriggerAuthentication.yaml](./TriggerAuthentication.yaml)
    - Scaler가 이벤트 소스에 접근하기 위한 인증 정보를 정의 (예: Pub/Sub 서비스 계정, Kafka 인증 정보 등)

## 동작 방식
```
[Event Source] → [Scaler] → [KEDA Operator] → [HPA] → [Deployment/Pod]
```
### 1. Scaler 감시
KEDA Scaler가 Kafka, Pub/Sub 등 외부 이벤트 소스의 메트릭(예: 메시지 수, 큐 길이 등)을 주기적으로 확인합니다.

### 2. 메트릭 변환
수집된 메트릭은 KEDA Metrics Adapter를 통해 Kubernetes Metrics API로 노출됩니다.

### 3. HPA 연동
HPA가 해당 메트릭을 기준으로 현재 Pod 수와 목표값을 비교하여 스케일링 결정을 내립니다.

### 4. Pod 스케일링
HPA가 Deployment의 replicas 값을 조정합니다.
이벤트가 없으면 Pod 수를 0으로 줄이고, 이벤트가 늘어나면 자동으로 확장합니다.

### 5. 이벤트 소스 업데이트
Scaler는 계속해서 이벤트 소스 상태를 감시하며, HPA의 스케일 결정을 위한 최신 데이터를 제공합니다.