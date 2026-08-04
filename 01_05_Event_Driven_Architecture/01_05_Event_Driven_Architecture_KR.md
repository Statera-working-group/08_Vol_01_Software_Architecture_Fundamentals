**Volume 1 Software Architecture Fundamentals**

# 05. Event-Driven Architecture

## 05.01 Event-Driven Architecture Principles and Core Concepts

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 기반 아키텍처(Event-Driven Architecture, EDA)는 현대의 클라우드 네이티브(Cloud-Native), 마이크로서비스(Microservices), 산업 자동화(Industrial Automation), 사물인터넷(Internet of Things, IoT), 로보틱스(Robotics), 인공지능(AI), 금융 시스템(Financial System) 등에서 핵심이 되는 소프트웨어 아키텍처이다. 기존의 요청-응답(Request-Response) 방식이 서비스 간 직접 호출을 중심으로 동작하는 것과 달리, 이벤트(Event)를 중심으로 시스템이 반응(Reactive)하도록 설계하는 것이 가장 큰 특징이다.

이벤트(Event)는 시스템 내부에서 의미 있는 상태 변화(State Change)가 발생했음을 나타내는 사실(Fact)이다. 이벤트는 단순한 데이터(Message)가 아니라 이미 발생한 결과를 표현하며 변경되지 않는 불변 정보(Immutable Information)이다. 고객 가입(Customer Registered), 결제 완료(Payment Completed), 주문 생성(Order Created), 로봇 미션 시작(Robot Mission Started), 검사 완료(Inspection Completed), AI 모델 배포(AI Model Deployed), 센서 이상 감지(Sensor Anomaly Detected) 등이 모두 이벤트에 해당한다.

이벤트 기반 아키텍처의 핵심 철학은 서비스(Service)가 서로 직접 호출하는 대신 이벤트를 발행(Publish)하고, 필요한 서비스가 이를 구독(Subscribe)하여 독립적으로 처리하는 것이다. 이벤트를 발생시키는 서비스는 누가 이벤트를 사용할지 알 필요가 없으며, 이벤트를 소비하는 서비스도 이벤트 생성자의 내부 구현을 알 필요가 없다. 이러한 느슨한 결합(Loose Coupling)은 시스템의 확장성과 유지보수성을 크게 향상시킨다.

기존의 동기식(Synchronous) 구조에서는 하나의 서비스가 다른 서비스를 호출하고 응답(Response)을 기다린다. 이 과정에서 네트워크(Network), 지연시간(Latency), 장애(Failure), 타임아웃(Timeout), 재시도(Retry) 등의 문제가 연쇄적으로 발생할 수 있다. 서비스 간 의존성이 높아질수록 전체 시스템의 복잡성과 장애 전파 가능성도 증가한다.

이벤트 기반 아키텍처는 이러한 문제를 비동기 통신(Asynchronous Communication)으로 해결한다. 이벤트 생산자(Producer)는 이벤트를 발행한 후 소비자의 처리를 기다리지 않는다. 이벤트 브로커(Event Broker)가 이벤트를 저장하고 전달하며, 소비자(Consumer)는 자신의 속도에 맞게 이벤트를 처리한다. 따라서 생산자와 소비자가 서로 영향을 최소화하면서 독립적으로 동작할 수 있다.

좋은 이벤트(Event)는 기술적인 변경 사항이 아니라 비즈니스 의미(Business Meaning)를 표현해야 한다. 예를 들어 DatabaseUpdated와 같은 기술 중심 이벤트보다 CustomerRegistered, PaymentAuthorized, RobotMissionCompleted, BatteryChargingCompleted처럼 업무 관점의 이벤트 이름을 사용하는 것이 바람직하다. 이는 개발자뿐 아니라 비즈니스 담당자도 시스템의 동작을 쉽게 이해할 수 있도록 한다.

이벤트는 반드시 불변성(Immutability)을 가져야 한다. 한 번 발행된 이벤트는 수정되지 않으며, 이후 상태가 변경되면 새로운 이벤트를 생성한다. 예를 들어 PaymentAuthorized 이벤트가 발생한 후 결제가 취소되면 기존 이벤트를 수정하는 것이 아니라 PaymentCancelled 이벤트를 추가로 생성한다. 이러한 방식은 감사(Audit), 이력 관리(History Tracking), 이벤트 재생(Event Replay), 장애 분석(Debugging)에 매우 유리하다.

명령(Command)과 이벤트(Event)는 명확하게 구분되어야 한다. 명령은 어떤 작업을 수행해 달라는 요청(Request)이며 성공 여부가 아직 결정되지 않은 상태이다. 반면 이벤트는 이미 성공적으로 수행된 결과를 의미한다. 예를 들어 Authorize Payment는 명령이고, Payment Authorized는 이벤트이다. 명령은 미래(Future)를 향하고 이벤트는 과거(Past)를 기록한다.

이벤트 생산자(Event Producer)는 자신의 업무를 수행한 후 의미 있는 상태 변화가 발생하면 이벤트를 발행한다. 결제 서비스(Payment Service)는 결제 완료 이벤트를, 재고 서비스(Inventory Service)는 재고 변경 이벤트를, 로봇 제어기(Robot Controller)는 미션 완료 이벤트를 발행한다. 생산자는 이벤트를 누가 사용하는지 알 필요가 없으므로 서비스 간 결합도가 매우 낮다.

이벤트 소비자(Event Consumer)는 자신이 필요한 이벤트만 구독한다. 하나의 CustomerRegistered 이벤트는 마케팅(Marketing), 분석(Analytics), 추천 시스템(Recommendation), 환영 이메일(Welcome Notification), AI 학습(AI Training) 등 여러 서비스가 동시에 사용할 수 있다. 이벤트 하나가 다양한 서비스를 자연스럽게 연결하는 역할을 한다.

이벤트 브로커(Event Broker)는 이벤트 기반 시스템의 중심 인프라이다. 생산자가 발행한 이벤트를 수신하여 저장하고, 이를 구독하는 소비자에게 전달한다. 이벤트 전달, 저장, 재시도(Retry), 순서(Order), 전달 보장(Delivery Guarantee)을 관리하여 생산자와 소비자가 직접 연결되지 않도록 한다.

이벤트 전달 방식에는 여러 가지 패턴이 존재한다. Point-to-Point는 하나의 소비자에게만 전달하며, Publish-Subscribe는 여러 소비자가 동시에 동일한 이벤트를 처리한다. Event Streaming은 이벤트를 장기간 저장하여 이후에도 다시 읽을 수 있도록 지원하며, Queue 기반 구조는 여러 소비자가 작업을 분산 처리하는 데 적합하다.

토픽(Topic)은 관련된 이벤트를 분류하는 논리적인 채널이다. Payment Topic에는 결제 관련 이벤트가, Robotics Topic에는 로봇 이벤트가, Manufacturing Topic에는 생산 이벤트가 저장된다. 소비자는 자신에게 필요한 Topic만 구독하면 되므로 불필요한 데이터를 처리하지 않아도 된다.

메시지 큐(Message Queue)는 생산자와 소비자의 처리 속도가 다를 때 중간에서 데이터를 저장한다. 생산자가 매우 빠르게 이벤트를 생성하더라도 소비자는 자신의 처리 속도에 맞게 하나씩 처리할 수 있다. 이러한 구조는 시스템 전체의 안정성과 확장성을 향상시킨다.

이벤트 스트리밍(Event Streaming)은 이벤트를 처리 후 삭제하지 않고 장기간 보관하는 방식이다. 과거 이벤트를 다시 재생(Event Replay)하여 시스템 상태를 복원하거나 AI 학습 데이터 생성, 디지털 트윈(Digital Twin), 감사(Audit), 시계열 분석(Time-Series Analysis)에 활용할 수 있다.

Apache Kafka는 가장 대표적인 이벤트 스트리밍 플랫폼이다. 높은 처리량(High Throughput), 확장성(Scalability), 데이터 영속성(Durability)을 제공하며 금융(Finance), 산업 자동화(Industrial Automation), AI, IoT, 로보틱스(Robotics) 등 다양한 분야에서 활용된다.

RabbitMQ는 메시지 큐(Message Queue)에 특화된 플랫폼으로 다양한 라우팅(Routing) 방식을 제공한다. Exchange를 이용하여 Direct, Topic, Fanout 등 여러 방식으로 메시지를 전달할 수 있으며 기업 시스템 통합(Enterprise Integration)에 많이 사용된다.

Apache Pulsar는 메시징(Messaging)과 스트리밍(Streaming)을 모두 지원하는 플랫폼이다. 저장(Storage)과 브로커(Broker)를 분리한 구조를 사용하여 대규모 클라우드 환경에서 높은 확장성과 다중 테넌시(Multi-Tenancy)를 제공한다.

MQTT(Message Queuing Telemetry Transport)는 IoT와 로보틱스 분야에서 널리 사용되는 경량 프로토콜이다. 네트워크 품질이 좋지 않은 환경에서도 안정적으로 동작하며 센서(Sensor), 로봇(Robot), 엣지 장치(Edge Device) 간 통신에 적합하다.

DDS(Data Distribution Service)는 실시간 분산 시스템을 위한 미들웨어이다. Robot Operating System 2(ROS 2)의 기본 통신 방식으로 사용되며, 자율주행, 항공우주, 국방, 산업 자동화에서 결정성(Deterministic Communication)과 낮은 지연시간(Low Latency)을 제공한다.

이벤트 스키마(Event Schema)는 이벤트 데이터의 표준 구조를 정의한다. 이벤트 이름(Event Name), 시간(Timestamp), ID, 메타데이터(Metadata), Payload, 버전(Version), 발생 서비스(Source Service) 등을 일정한 형식으로 관리하여 서비스 간 호환성을 유지한다.

스키마 진화(Schema Evolution)는 매우 중요한 관리 요소이다. 새로운 필드는 가능하면 선택적(Optional)으로 추가하고 기존 필드는 삭제하지 않는 것이 좋다. Schema Registry를 이용하면 버전 관리와 호환성(Compatibility)을 자동으로 검증할 수 있다.

메타데이터(Metadata)는 이벤트를 더욱 풍부하게 만든다. Event ID는 이벤트를 고유하게 식별하고, Correlation ID는 하나의 업무 흐름을 연결하며, Causation ID는 어떤 이벤트가 다른 이벤트를 발생시켰는지를 나타낸다. Timestamp와 Source 정보도 운영 분석과 장애 추적에 매우 중요한 역할을 한다.

이벤트 순서(Ordering)는 일부 업무에서 매우 중요하다. Kafka는 동일 파티션(Partition) 안에서는 순서를 보장하지만 전체 시스템의 완전한 순서를 유지하는 것은 비용이 매우 크다. 따라서 일반적으로 동일 고객(Customer), 동일 주문(Order), 동일 로봇(Robot)과 같은 단위에서만 순서를 보장한다.

전달 보장(Delivery Guarantee)은 이벤트 시스템의 중요한 특성이다. At-Most-Once는 빠르지만 메시지 손실이 가능하고, At-Least-Once는 중복 전달이 가능하지만 손실은 없다. Exactly-Once는 중복 없이 한 번만 전달하지만 구현이 매우 복잡하다. 대부분의 시스템은 At-Least-Once와 멱등성(Idempotency)을 함께 사용한다.

멱등성(Idempotency)은 동일한 이벤트를 여러 번 처리하더라도 결과가 한 번 처리한 것과 동일하도록 만드는 성질이다. 이벤트가 중복 전달될 가능성이 있으므로 소비자는 Event ID를 이용하여 이미 처리한 이벤트인지 확인하고 중복 처리를 방지해야 한다.

이벤트 기반 시스템에서는 강한 일관성(Strong Consistency) 대신 최종 일관성(Eventual Consistency)을 사용한다. 각 서비스는 자신의 데이터를 독립적으로 관리하며 이벤트를 통해 다른 서비스와 비동기적으로 동기화한다. 잠시 동안 데이터 차이가 발생할 수 있지만 시간이 지나면 모든 서비스가 동일한 상태로 수렴한다.

이벤트 소싱(Event Sourcing)은 현재 상태(Current State)가 아니라 모든 이벤트(Event)를 저장하는 방식이다. 현재 상태는 이벤트를 처음부터 순차적으로 재생(Replaying)하여 계산한다. 이러한 구조는 감사(Audit), 디버깅(Debugging), 시간 기반 분석(Time Travel Analysis), AI 학습 데이터 생성에 매우 적합하다.

CQRS(Command Query Responsibility Segregation)는 이벤트 기반 아키텍처와 자주 함께 사용된다. Command는 데이터를 변경하고 이벤트를 발생시키며, Query는 이벤트를 이용하여 최적화된 조회 모델(Read Model)을 구성한다. 이를 통해 조회 성능과 확장성을 크게 향상시킬 수 있다.

Saga Pattern도 이벤트 기반 구조와 자연스럽게 결합된다. 여러 서비스에 걸친 긴 업무 프로세스(Long-Running Transaction)를 이벤트를 통해 연결하며, 실패 시에는 보상 이벤트(Compensation Event)를 이용하여 이전 작업을 되돌린다. 이는 분산 트랜잭션(Distributed Transaction)을 대체하는 대표적인 방법이다.

관찰 가능성(Observability)은 이벤트 기반 시스템에서 더욱 중요하다. 이벤트는 비동기적으로 전달되므로 Correlation ID를 이용한 분산 추적(Distributed Tracing), 메트릭(Metrics), 로그(Log), 큐 길이(Queue Depth), 처리 지연(Processing Latency)을 지속적으로 모니터링해야 한다.

보안(Security) 역시 중요한 요소이다. 이벤트 생산자와 소비자는 인증(Authentication)과 권한 관리(Authorization)를 수행해야 하며, 이벤트는 암호화(Encryption), 디지털 서명(Digital Signature), 감사 로그(Audit Log)를 통해 안전하게 보호되어야 한다.

클라우드 네이티브(Cloud-Native)는 이벤트 기반 아키텍처를 더욱 강화한다. Kafka, EventBridge, Event Hubs, Pub/Sub와 같은 관리형 이벤트 서비스를 이용하면 높은 확장성과 가용성을 쉽게 확보할 수 있으며 운영 부담도 크게 줄일 수 있다.

서버리스(Serverless)는 이벤트 기반 구조와 매우 잘 결합된다. 이벤트가 발생할 때만 함수(Function)가 실행되므로 항상 서버를 실행할 필요가 없으며 비용 절감과 자동 확장(Auto Scaling)이 가능하다.

산업 자동화(Industrial Automation)는 이벤트 기반 아키텍처의 대표적인 적용 분야이다. PLC, MES, 품질 검사(Quality Inspection), 예지보전(Predictive Maintenance), 디지털 트윈(Digital Twin), ERP가 모두 이벤트를 통해 연결되며 생산 상태를 실시간으로 공유한다.

로보틱스(Robotics)에서는 위치추정(Localization), 장애물 인식(Obstacle Detection), 미션 시작(Mission Started), 충전 완료(Charging Completed), Fleet 관리(Fleet Management), AI 추론(AI Inference), 센서 이상(Sensor Anomaly) 등이 모두 이벤트로 표현된다. 이벤트 기반 구조는 로봇, 엣지 서버, 클라우드 간의 느슨한 결합과 높은 확장성을 제공한다.

AI 시스템도 이벤트 기반으로 구성할 수 있다. 데이터 수집(Data Ingestion), 전처리(Preprocessing), 모델 학습(Model Training), 검증(Model Validation), 배포(Model Deployment), 추론(Inference), 모델 드리프트(Model Drift Detection), 재학습(Retraining)이 모두 이벤트를 통해 연결되며 지속적인 AI 운영(MLOps)을 지원한다.

주의해야 할 안티패턴(Anti-Pattern)도 존재한다. 이벤트를 RPC처럼 사용하는 것, 지나치게 세분화된 이벤트를 만드는 것, 이벤트를 수정 가능한 데이터처럼 사용하는 것, 스키마 관리를 하지 않는 것, 이벤트 체인을 과도하게 연결하는 것, 이벤트 브로커를 비즈니스 로직 엔진처럼 사용하는 것은 모두 피해야 한다.

결국 이벤트 기반 아키텍처(Event-Driven Architecture)는 단순한 메시징(Messageing) 기술이 아니라 분산 시스템(Distributed System)의 핵심 설계 철학이다. **이벤트(Event)**, **비동기 통신(Asynchronous Communication)**, **느슨한 결합(Loose Coupling)**, **이벤트 브로커(Event Broker)**, **이벤트 스트리밍(Event Streaming)**, **최종 일관성(Eventual Consistency)**, **CQRS**, **이벤트 소싱(Event Sourcing)**, **Saga Pattern**을 함께 활용하면 클라우드 네이티브(Cloud-Native), 엔터프라이즈 시스템(Enterprise System), 산업 자동화(Industrial Automation), 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 분산 AI(Distributed AI), 디지털 트윈(Digital Twin), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼을 위한 확장성, 복원력, 유연성, 지속적 진화를 지원하는 소프트웨어 기반을 구축할 수 있다.

## 05.02 Event Broker Comparison: Kafka / RabbitMQ / NATS

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 브로커(Event Broker)는 이벤트 기반 아키텍처(Event-Driven Architecture, EDA)의 핵심 구성 요소이다. 이벤트 생산자(Producer)와 소비자(Consumer)를 서로 분리하여 비동기 통신(Asynchronous Communication)을 가능하게 하며, 서비스 간 결합도를 크게 낮춘다. 현대의 마이크로서비스(Microservices), 클라우드 네이티브(Cloud-Native), 산업 자동화(Industrial Automation), 로보틱스(Robotics), 사물인터넷(IoT), 인공지능(AI) 시스템은 대부분 이벤트 브로커를 중심으로 데이터를 교환한다.

이벤트 브로커를 선택하는 것은 단순히 메시지를 전달하는 도구를 고르는 것이 아니라 시스템의 확장성(Scalability), 지연시간(Latency), 신뢰성(Reliability), 운영 복잡성(Operational Complexity), 데이터 저장 방식(Storage Strategy), 장애 복구(Fault Tolerance), 유지보수성(Maintainability)을 결정하는 중요한 아키텍처 선택이다. 대표적인 플랫폼으로는 **Apache Kafka**, **RabbitMQ**, **NATS**가 있으며 각각 서로 다른 목적을 위해 설계되었다.

Kafka, RabbitMQ, NATS는 모두 비동기 메시징(Asynchronous Messaging)을 지원하지만 내부 구조와 철학은 매우 다르다. Kafka는 대규모 이벤트 스트리밍(Event Streaming)과 데이터 저장에 최적화되어 있으며, RabbitMQ는 안정적인 엔터프라이즈 메시징(Enterprise Messaging)과 다양한 라우팅(Routing)에 강점을 가진다. NATS는 초저지연(Ultra-Low Latency) 통신과 클라우드 네이티브(Cloud-Native) 환경을 목표로 설계되었다.

이벤트 브로커의 가장 중요한 역할은 생산자와 소비자를 분리하는 것이다. 생산자는 이벤트를 발행(Publish)할 뿐 소비자가 누구인지 알 필요가 없으며, 소비자는 필요한 이벤트만 구독(Subscribe)하여 처리한다. 브로커는 메시지 저장(Storage), 전달(Delivery), 재시도(Retry), 순서(Ordering), 버퍼링(Buffering), 장애 복구(Failure Recovery)를 담당하여 서비스 간 의존성을 최소화한다.

Apache Kafka는 LinkedIn에서 대규모 이벤트 데이터를 처리하기 위해 개발되었다. Kafka는 일반적인 메시지 큐(Message Queue)가 아니라 분산 로그(Distributed Log)를 기반으로 하는 이벤트 스트리밍 플랫폼(Event Streaming Platform)이다. 이벤트를 소비한 이후에도 삭제하지 않고 장기간 저장하므로 동일한 데이터를 여러 소비자가 서로 다른 시점에 반복적으로 사용할 수 있다.

Kafka는 Topic과 Partition 구조를 사용한다. 하나의 Topic은 여러 Partition으로 구성되며 각 Partition은 독립적으로 저장된다. 이벤트는 Partition 내부에서는 순서(Order)가 보장되며, 여러 Partition을 통해 수평 확장(Horizontal Scaling)이 가능하다. 이 구조 덕분에 Kafka는 초당 수백만 건 이상의 이벤트도 처리할 수 있다.

Kafka의 가장 큰 장점은 영속성(Durability)이다. 이벤트는 디스크(Disk)에 저장되고 일정 기간 동안 유지된다. 따라서 이벤트 재생(Event Replay), AI 학습(AI Training), 디지털 트윈(Digital Twin), 감사(Audit), 장애 분석(Debugging), 데이터 분석(Analytics), 머신러닝(Machine Learning) 등에 동일한 데이터를 반복 활용할 수 있다.

Kafka는 스트림 처리(Stream Processing)와도 자연스럽게 연결된다. Kafka Streams, Apache Flink, Apache Spark Streaming과 함께 사용하면 실시간 필터링(Filtering), 집계(Aggregation), 이상 탐지(Anomaly Detection), AI 특징 생성(Feature Engineering), 예측 분석(Predictive Analytics)을 수행할 수 있다.

그러나 Kafka는 운영 복잡성이 상대적으로 높다. Broker, Partition, Replication, Consumer Group, Storage, Cluster 관리 등 다양한 요소를 이해해야 하며, 대규모 클러스터(Cluster)를 운영하려면 상당한 분산 시스템(Distributed System) 경험이 필요하다.

RabbitMQ는 Kafka와 달리 전통적인 엔터프라이즈 메시징 시스템(Enterprise Messaging System)이다. RabbitMQ는 Exchange, Queue, Binding, Routing Key를 이용하여 메시지를 다양한 방식으로 전달한다. 메시지를 장기간 저장하기보다는 안정적으로 전달하는 것이 주요 목적이다.

RabbitMQ의 가장 큰 특징은 유연한 라우팅(Flexible Routing)이다. Direct Exchange는 정확한 키(Key)로 전달하고, Topic Exchange는 패턴(Pattern)에 따라 전달하며, Fanout Exchange는 모든 소비자에게 브로드캐스트(Broadcast)한다. 또한 Headers Exchange는 메시지 헤더(Header)를 기준으로 전달할 수도 있다.

이러한 라우팅 기능 덕분에 RabbitMQ는 주문(Order), 결제(Payment), 알림(Notification), 워크플로우(Workflow), 금융(Finance), 의료(Healthcare), ERP, 업무 프로세스 자동화(Business Process Automation)에 널리 활용된다.

RabbitMQ는 다양한 프로토콜(Protocol)을 지원하는 것도 장점이다. AMQP, MQTT, STOMP 등을 지원하므로 기존 레거시 시스템(Legacy System), 산업용 장비(Industrial Equipment), 모바일(Mobile), IoT 장치와도 쉽게 연동할 수 있다.

RabbitMQ는 신뢰성(Reliability)도 매우 우수하다. Publisher Confirm은 브로커가 메시지를 정상적으로 수신했는지를 확인하며, Consumer Acknowledgement는 소비자가 메시지를 성공적으로 처리한 후에만 Queue에서 제거한다. 또한 Dead Letter Queue는 처리 실패 메시지를 별도로 관리하여 장애 분석을 지원한다.

RabbitMQ는 Kafka보다 처리량(Throughput)은 낮지만 라우팅 기능이 훨씬 풍부하고 운영도 비교적 단순하다. 따라서 대규모 데이터 분석보다 안정적인 업무 처리(Business Workflow)가 중요한 환경에서 매우 적합하다.

NATS는 Kafka와 RabbitMQ와는 다른 철학을 가진 플랫폼이다. 매우 가볍고(Lightweight) 빠르며(Simple and Fast), 클라우드 네이티브 환경에서 초저지연(Ultra-Low Latency) 통신을 제공하는 것을 목표로 설계되었다.

NATS는 Subject 기반 Publish-Subscribe 구조를 사용한다. 메시지는 거의 추가 처리 없이 즉시 전달되며 브로커의 오버헤드(Overhead)가 매우 작다. 따라서 수백 마이크로초(Microsecond) 수준의 매우 낮은 지연시간을 제공할 수 있다.

클라우드 네이티브 시스템에서는 장기간 이벤트를 저장하기보다 빠른 서비스 간 통신(Service-to-Service Communication)이 더욱 중요하다. 서비스 검색(Service Discovery), 상태 전달(Status Update), AI 추론 요청(Inference Request), 엣지 동기화(Edge Synchronization), 로봇 Fleet 관리(Fleet Management)는 모두 빠른 응답성이 중요하기 때문에 NATS가 매우 적합하다.

JetStream은 NATS의 기능을 확장한 저장 계층(Storage Layer)이다. 기존 NATS는 휘발성(Ephemeral) 메시징을 중심으로 설계되었지만 JetStream을 이용하면 메시지 저장(Persistence), 재생(Replay), 복제(Replication), ACK 처리도 지원할 수 있다.

NATS의 또 다른 장점은 운영이 매우 단순하다는 것이다. 메모리 사용량이 적고 설정(Configuration)이 간단하며 설치도 쉽다. 따라서 엣지 컴퓨팅(Edge Computing), 임베디드 시스템(Embedded System), 산업용 게이트웨이(Industrial Gateway), 자율주행 로봇(Autonomous Robot)에서 널리 사용된다.

특히 로보틱스(Robotics)에서는 위치추정(Localization), 센서 데이터(Sensor Data), 배터리 상태(Battery Status), Fleet 명령(Fleet Command), AI 추론 요청(AI Inference Request)처럼 매우 짧은 지연시간이 필요한 통신이 많다. 이러한 환경에서는 Kafka보다 NATS가 훨씬 적합한 경우가 많다.

세 플랫폼의 철학을 비교하면 Kafka는 **데이터 스트리밍(Data Streaming)** 중심, RabbitMQ는 **메시지 전달(Message Delivery)** 중심, NATS는 **실시간 통신(Real-Time Communication)** 중심이라고 이해할 수 있다.

데이터 저장 방식도 서로 다르다. Kafka는 모든 이벤트를 저장하며 장기간 유지한다. RabbitMQ는 메시지가 성공적으로 처리되면 일반적으로 제거된다. NATS는 기본적으로 실시간 전달에 집중하며 JetStream을 사용할 경우 선택적으로 저장 기능을 제공한다.

소비자(Consumer)의 동작 방식도 차이가 있다. Kafka는 Offset을 이용하여 소비자가 어디까지 읽었는지를 관리하므로 같은 데이터를 여러 번 읽을 수 있다. RabbitMQ는 메시지를 소비하면 Queue에서 제거되는 방식이며, NATS는 실시간으로 전달된 메시지를 즉시 처리하는 구조이다.

순서 보장(Ordering)에서도 차이가 있다. Kafka는 Partition 내부에서 강력한 순서를 보장하므로 금융(Finance), 제조(Manufacturing), AI 데이터셋(AI Dataset)에 적합하다. RabbitMQ는 Queue 단위에서 순서를 유지하지만 복잡한 라우팅에서는 일부 차이가 발생할 수 있다. NATS는 순서보다 빠른 응답성과 낮은 지연시간을 우선시한다.

확장성(Scalability)은 Kafka가 가장 뛰어나다. Partition을 통해 매우 높은 처리량을 제공한다. RabbitMQ는 일반적인 기업 시스템 수준의 확장성을 제공하며, NATS는 많은 연결(Connection)을 처리하면서도 매우 낮은 지연시간을 유지하는 데 강점을 가진다.

지연시간(Latency)은 NATS가 가장 낮다. Kafka는 저장과 복제를 수행하므로 약간의 지연이 발생하지만 처리량이 매우 높다. RabbitMQ는 그 중간 수준으로 안정성과 응답성을 균형 있게 제공한다.

운영 복잡성은 Kafka가 가장 높고 NATS가 가장 낮다. Kafka는 분산 저장과 클러스터 관리가 필요하며, RabbitMQ는 상대적으로 관리가 쉽다. NATS는 매우 단순한 구조로 인해 소규모 팀이나 엣지 환경에서도 쉽게 운영할 수 있다.

장애 복구(Fault Tolerance) 방식도 다르다. Kafka는 Replication을 이용하여 데이터를 여러 Broker에 복제한다. RabbitMQ는 Mirrored Queue와 Quorum Queue를 사용한다. NATS는 Cluster와 JetStream을 이용하여 복제를 수행한다.

세 플랫폼 모두 Kubernetes 환경에서 운영할 수 있으며 Operator를 이용한 자동 배포와 관리도 지원한다. 하지만 Kafka는 상대적으로 많은 CPU, 메모리, 저장장치를 요구하며, RabbitMQ와 NATS는 비교적 가벼운 자원으로 운영할 수 있다.

보안(Security)은 세 플랫폼 모두 인증(Authentication), 권한 관리(Authorization), TLS 암호화(Encryption), 인증서(Certificate), ACL(Access Control List)을 지원한다. 대부분의 기업 환경에서는 중앙 인증 시스템과 연동하여 Zero Trust 보안을 구현한다.

관찰 가능성(Observability)도 매우 중요하다. Broker 상태, Queue 길이, Consumer Lag, 처리량(Throughput), 지연시간(Latency), 저장 용량(Storage Usage), 재시도(Retry), 연결 수(Connection Count)를 Prometheus, Grafana, OpenTelemetry 등을 통해 지속적으로 모니터링한다.

AI 시스템에서도 용도에 따라 서로 다른 플랫폼을 선택한다. AI 학습 데이터(AI Training Data)는 Kafka가 적합하며, AI 워크플로우(AI Workflow)와 승인 프로세스는 RabbitMQ가 적합하다. AI 추론 요청(AI Inference Request)처럼 매우 빠른 응답이 필요한 경우에는 NATS가 적합하다.

산업 자동화에서도 동일한 패턴이 나타난다. 공장 센서 데이터와 생산 기록은 Kafka, ERP 및 MES 연동은 RabbitMQ, 로봇과 엣지 장비 간 실시간 통신은 NATS가 자주 사용된다.

실제 대규모 시스템에서는 하나의 브로커만 사용하는 경우보다 **하이브리드 아키텍처(Hybrid Architecture)** 를 사용하는 경우가 많다. Kafka는 장기 이벤트 저장과 분석을 담당하고, RabbitMQ는 업무 프로세스와 메시지 라우팅을 담당하며, NATS는 실시간 제어와 저지연 통신을 담당한다. 각 플랫폼의 장점을 조합하면 훨씬 효율적인 이벤트 기반 시스템을 구축할 수 있다.

반대로 안티패턴(Anti-Pattern)도 존재한다. 단순한 요청-응답 통신을 위해 Kafka를 사용하는 것은 과도한 복잡성을 초래할 수 있으며, 대규모 이벤트 저장을 RabbitMQ로 처리하는 것도 적합하지 않다. 또한 규제(Regulation)나 감사(Audit)가 필요한 시스템에서 NATS의 휘발성 메시지만 사용하는 것도 위험하다.

결국 Kafka, RabbitMQ, NATS는 경쟁 제품이라기보다 서로 다른 목적을 가진 **상호 보완적인(Event Broker)** 기술이다. **Kafka**는 대규모 이벤트 스트리밍과 데이터 분석, **RabbitMQ**는 안정적인 업무 메시징과 유연한 라우팅, **NATS**는 초저지연 클라우드 네이티브 통신과 로보틱스에 가장 적합하다. 따라서 시스템의 **처리량(Throughput)**, **지연시간(Latency)**, **영속성(Persistence)**, **라우팅 복잡도(Routing Complexity)**, **운영 난이도(Operational Complexity)**, **AI 및 로보틱스 요구사항**을 종합적으로 고려하여 적절한 이벤트 브로커를 선택하는 것이 현대 이벤트 기반 아키텍처(Event-Driven Architecture)의 핵심 설계 원칙이다.

## 05.03 Event Schema Design and Versioning

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 스키마(Event Schema) 설계와 버전 관리(Versioning)는 이벤트 기반 아키텍처(Event-Driven Architecture, EDA)의 가장 핵심적인 기술 중 하나이다. Apache Kafka, RabbitMQ, NATS와 같은 이벤트 브로커(Event Broker)는 이벤트를 전달하는 역할을 수행하지만, 장기간 안정적인 시스템 운영은 이벤트 스키마의 품질과 버전 관리 전략에 의해 결정된다. 수백 개의 서비스가 수백만 개의 이벤트를 주고받는 분산 시스템에서는 스키마 설계가 잘못되면 서비스 간 호환성 문제가 빠르게 발생한다.

이벤트 스키마(Event Schema)는 이벤트의 구조(Structure), 의미(Semantics), 데이터 형식(Data Type), 제약 조건(Constraint)을 정의하는 표준 계약(Contract)이다. 이벤트 이름(Event Name), 식별자(ID), 시간(Timestamp), 메타데이터(Metadata), Payload, 데이터 타입(Type), 필수 항목(Mandatory Field), 선택 항목(Optional Field), 검증 규칙(Validation Rule), 버전(Version), 소유자(Owner) 등을 명확하게 정의하여 모든 서비스가 동일한 방식으로 이벤트를 이해하도록 한다.

이벤트 스키마는 단순한 데이터 형식이 아니라 서비스 간의 공식적인 통신 계약(Communication Contract)이다. 이벤트를 생성하는 생산자(Producer)는 스키마를 준수하여 이벤트를 발행해야 하며, 소비자(Consumer)는 해당 스키마를 기반으로 데이터를 해석한다. 서로의 내부 구현을 알 필요 없이 동일한 계약만 유지하면 독립적인 개발과 배포가 가능하다.

좋은 이벤트 스키마는 기술적인 구현이 아니라 비즈니스 의미(Business Meaning)를 표현해야 한다. CustomerRegistered, PaymentAuthorized, InspectionCompleted, RobotMissionFinished, BatteryChargingCompleted와 같은 이름은 누구나 이해할 수 있지만, DatabaseUpdated, CacheChanged, InsertCompleted와 같은 기술 중심 이벤트는 비즈니스 의미를 전달하지 못하고 시스템 간 결합도를 높일 수 있다.

일반적으로 이벤트 스키마는 메타데이터(Metadata)와 Payload 두 부분으로 구성된다. 메타데이터는 이벤트 관리와 추적을 위한 정보를 포함하며, Payload는 실제 비즈니스 데이터를 저장한다. 이러한 분리는 이벤트 관리, 추적, 모니터링, 보안, 운영을 훨씬 효율적으로 수행할 수 있도록 한다.

메타데이터에는 Event ID, Timestamp, Event Type, Schema Version, Source Service, Correlation ID, Causation ID, Tenant ID 등이 포함된다. Event ID는 이벤트를 고유하게 식별하며, Correlation ID는 동일한 업무 흐름(Business Workflow)을 연결하고, Causation ID는 어떤 이벤트가 다른 이벤트를 발생시켰는지를 나타낸다. 이러한 정보는 분산 추적(Distributed Tracing)과 장애 분석(Debugging)에 매우 중요한 역할을 한다.

Payload는 실제 업무 데이터를 포함한다. 그러나 너무 많은 데이터를 포함하면 네트워크(Network)와 저장소(Storage)의 부담이 증가하고, 너무 적은 정보를 포함하면 소비자가 다시 API를 호출해야 하므로 이벤트 기반 구조의 장점이 감소한다. 따라서 필요한 정보만 포함하면서도 소비자가 독립적으로 처리할 수 있도록 적절한 균형을 유지해야 한다.

가능하면 이벤트는 자기 완결형(Self-Contained)으로 설계하는 것이 좋다. 소비자는 이벤트 하나만으로 필요한 처리를 수행할 수 있어야 하며, 추가적인 API 호출 없이 업무를 완료할 수 있어야 한다. 하지만 모든 데이터를 중복 저장하는 것은 저장 공간과 유지보수 비용을 증가시키므로 필요한 수준에서만 중복 정보를 포함하는 것이 바람직하다.

필드 이름(Field Name)은 명확하고 일관성 있게 작성해야 한다. Timestamp, CustomerId, OrderNumber, RobotId, InspectionResult와 같이 의미가 분명한 이름을 사용하는 것이 좋으며, Data, Value, Status와 같이 의미가 모호한 이름은 피해야 한다. 조직 전체에서 동일한 명명 규칙(Naming Convention)을 적용하면 개발자들이 새로운 이벤트를 쉽게 이해할 수 있다.

데이터 타입(Data Type)도 매우 중요하다. 정수(Integer), 실수(Float), 문자열(String), Boolean, Enumeration, Timestamp, Duration, GPS 좌표(Geographical Coordinate) 등의 형식을 명확하게 정의해야 한다. 시간은 일반적으로 ISO-8601 형식과 UTC(Coordinated Universal Time)를 사용하는 것이 국제적인 표준이다.

열거형(Enumeration)은 자유로운 문자열보다 훨씬 안전하다. 예를 들어 InspectionStatus는 Passed, Failed, PendingReview, Cancelled만 허용하도록 정의할 수 있으며, RobotMissionStatus는 Assigned, Executing, Completed, Suspended, Failed, Charging 등으로 제한할 수 있다. 이러한 제한은 데이터 품질과 일관성을 향상시킨다.

필수 항목(Mandatory Field)과 선택 항목(Optional Field)의 균형도 중요하다. 필수 항목은 반드시 존재해야 하는 핵심 정보이며, 선택 항목은 이후 기능 확장을 위해 사용된다. 새로운 기능을 추가할 때는 기존 서비스를 깨뜨리지 않도록 새로운 필드를 Optional로 추가하는 것이 일반적인 전략이다.

복합 객체(Nested Object)를 사용하면 관련 데이터를 논리적으로 묶을 수 있다. 예를 들어 Customer 객체 안에 Address 객체를 포함하거나 Robot 이벤트 안에 Sensor 목록을 포함할 수 있다. 그러나 지나치게 깊은 중첩 구조는 직렬화(Serialization), 검증(Validation), 유지보수를 어렵게 만들 수 있으므로 적절한 수준으로 유지하는 것이 좋다.

이벤트 기반 시스템에서는 관계형 데이터베이스(Relational Database)처럼 완전한 정규화(Normalization)를 사용하는 경우가 드물다. 대부분은 소비자가 추가 조회 없이 처리할 수 있도록 일부 정보를 중복 포함하는 비정규화(Denormalization)를 사용한다. 예를 들어 CustomerId만 전달하는 대신 CustomerName, Region, MembershipLevel도 함께 포함하면 소비자가 독립적으로 처리할 수 있다.

스키마 검증(Schema Validation)은 잘못된 이벤트가 시스템으로 들어오는 것을 방지한다. 필수 필드 존재 여부, 데이터 타입, 문자열 길이, 숫자 범위, Enumeration 값, Timestamp 형식 등을 검증하여 오류를 사전에 차단할 수 있다. 이러한 검증은 Producer, Broker, Consumer 어느 단계에서든 수행할 수 있다.

이벤트 직렬화(Serialization)는 이벤트를 네트워크로 전송하기 위한 형식이다. JSON은 가장 널리 사용되며 사람이 읽기 쉽다. XML은 기업 시스템에서 많이 사용되지만 데이터 크기가 크다. Apache Avro는 Kafka와 잘 통합되며 Schema Evolution에 강하다. Protocol Buffers(Protobuf)는 높은 성능과 강한 타입(Strong Typing)을 제공하며, Apache Thrift와 MessagePack도 효율적인 직렬화 방식을 제공한다.

직렬화 형식은 시스템 특성에 따라 선택해야 한다. 사람이 읽기 쉬운 것이 중요하면 JSON이 적합하고, 성능과 네트워크 효율이 중요하면 Avro나 Protocol Buffers와 같은 바이너리(Binary) 형식이 적합하다. 실제 시스템에서는 여러 직렬화 방식을 함께 사용하는 경우도 많다.

스키마 진화(Schema Evolution)는 이벤트 기반 시스템에서 가장 중요한 관리 요소 중 하나이다. 분산 시스템에서는 Producer와 Consumer가 동시에 업데이트되지 않으므로 새로운 스키마가 기존 서비스와 계속 호환되어야 한다.

하위 호환성(Backward Compatibility)은 새로운 Producer가 기존 Consumer와 통신할 수 있도록 하는 것이며, 상위 호환성(Forward Compatibility)은 새로운 Consumer가 기존 Producer의 이벤트를 처리할 수 있도록 하는 것이다. 이상적인 시스템은 양방향 호환성(Full Compatibility)을 유지하도록 설계된다.

새로운 필드를 추가할 때는 Optional Field로 추가하는 것이 가장 안전한 방법이다. 기존 Consumer는 해당 필드를 무시하면 되므로 문제가 발생하지 않는다. 반면 새로운 Mandatory Field를 추가하면 기존 Producer가 이를 생성하지 못하므로 시스템 호환성이 깨질 가능성이 높다.

필드를 삭제하는 것은 매우 신중해야 한다. 오래된 Consumer가 해당 필드를 계속 사용할 수 있기 때문이다. 일반적으로 Deprecated 상태를 일정 기간 유지한 후 충분한 마이그레이션(Migration)을 거쳐 삭제하는 것이 안전한 방법이다.

필드의 의미를 변경하는 것은 가장 위험한 변경이다. 기존 필드를 다른 의미로 사용하는 것은 소비자의 오작동을 초래할 수 있으므로 새로운 필드를 추가하고 기존 필드를 Deprecated 처리하는 것이 권장된다.

데이터 타입 변경도 주의해야 한다. Integer를 Long으로 확장하는 것은 비교적 안전하지만 Number를 String으로 변경하거나 Timestamp 형식을 변경하는 것은 기존 Consumer를 쉽게 깨뜨릴 수 있다.

버전 관리(Versioning)는 이러한 변경 사항을 관리하는 방법이다. 일부 조직은 Schema Version을 이벤트 안에 포함하고, 일부는 Schema Registry를 이용하여 버전을 관리한다. 일반적으로 Semantic Versioning을 사용하여 Major는 호환되지 않는 변경, Minor는 기능 추가, Patch는 수정 사항을 의미한다.

Schema Registry는 대규모 이벤트 시스템에서 매우 중요한 역할을 한다. Confluent Schema Registry, Apicurio Registry, AWS Glue Schema Registry 등은 모든 스키마를 중앙에서 관리하며, 새로운 스키마가 기존 버전과 호환되는지를 자동으로 검사한다.

Schema Registry는 새로운 스키마를 등록하기 전에 호환성(Compatibility)을 자동 검증한다. 문제가 있는 변경은 CI/CD 과정에서 차단되므로 운영 환경에서 발생할 수 있는 호환성 문제를 사전에 방지할 수 있다.

Consumer 중심 스키마 진화(Consumer-Driven Schema Evolution)는 Producer가 스키마를 변경할 때 Consumer의 요구사항을 먼저 고려하는 접근 방식이다. Contract Testing과 함께 사용하면 구조뿐 아니라 실제 동작까지 함께 검증할 수 있다.

문서화(Document Management)는 스키마 관리에서 매우 중요하다. 이벤트 의미, 발생 조건, Producer, Consumer, 필드 설명, 예제 Payload, 버전 이력, 마이그레이션 방법 등을 지속적으로 관리해야 조직 전체에서 동일한 이벤트를 올바르게 사용할 수 있다.

거버넌스(Governance)는 스키마 품질을 장기적으로 유지하는 조직적인 활동이다. 아키텍처 리뷰(Architecture Review), API 거버넌스(API Governance), 플랫폼 엔지니어링(Platform Engineering), 이벤트 카탈로그(Event Catalog)를 통해 스키마 품질과 일관성을 유지한다.

도메인 주도 설계(Domain-Driven Design, DDD)는 이벤트 스키마 설계에도 중요한 영향을 준다. Customer 이벤트는 Customer Domain, Payment 이벤트는 Payment Domain, Robot 이벤트는 Robotics Domain, AI 이벤트는 AI Domain에서 관리해야 한다. 이렇게 하면 서비스의 책임과 이벤트의 의미가 명확해진다.

이벤트 카탈로그(Event Catalog)는 조직 전체의 이벤트 목록을 관리하는 저장소이다. 어떤 이벤트가 존재하는지, 누가 생성하고 누가 사용하는지, 버전은 무엇인지, 예제는 어떻게 되는지를 한곳에서 확인할 수 있어 중복 이벤트 생성을 방지하고 재사용성을 높인다.

관찰 가능성(Observability)은 스키마 관리에도 적용된다. 스키마 사용 현황, 버전 분포, 호환성 오류, 직렬화 실패, Deprecated 필드 사용 여부 등을 지속적으로 모니터링하여 운영 중 발생하는 문제를 빠르게 발견할 수 있다.

스키마 변경 시에는 다양한 테스트가 필요하다. 단위 테스트(Unit Test)는 직렬화와 역직렬화를 검증하고, Contract Test는 Producer와 Consumer의 호환성을 검증하며, Integration Test는 전체 이벤트 흐름을 검증한다. CI/CD에서는 이러한 검증을 자동으로 수행하여 잘못된 스키마가 운영 환경으로 배포되는 것을 방지한다.

보안(Security)도 이벤트 스키마 설계에 포함되어야 한다. 개인정보(Personally Identifiable Information), 금융 정보(Financial Information), 의료 정보(Healthcare Data), 산업 기밀(Industrial Secret), AI 모델 정보(AI Model Information)는 최소한으로 포함해야 하며, 암호화(Encryption), 토큰화(Tokenization), 데이터 마스킹(Data Masking), 접근 제어(Access Control)를 함께 적용해야 한다.

AI 시스템에서는 이벤트 스키마가 매우 중요하다. Feature Engineering, 모델 학습(Model Training), 모델 배포(Model Deployment), 추론(Inference), 모델 드리프트(Model Drift), 재학습(Retraining)은 모두 이벤트를 기반으로 수행되므로 스키마가 안정적으로 유지되어야 MLOps가 지속적으로 동작할 수 있다.

산업 자동화에서는 MES, PLC, 품질 검사(Quality Inspection), 디지털 트윈(Digital Twin), 예지보전(Predictive Maintenance), ERP가 서로 이벤트를 교환한다. 표준화된 이벤트 스키마를 사용하면 서로 다른 제조사와 장비 간에도 안정적인 상호 운용성(Interoperability)을 확보할 수 있다.

로보틱스(Robotics)에서는 위치추정(Localization), 센서 데이터(Sensor Data), 장애물 감지(Obstacle Detection), 미션(Mission), Fleet 관리(Fleet Management), AI 추론(AI Inference), 디지털 트윈(Digital Twin) 등이 모두 이벤트로 표현된다. 표준화된 이벤트 스키마는 로봇, 엣지 서버, 클라우드가 독립적으로 진화하면서도 서로 호환성을 유지하도록 해준다.

반드시 피해야 할 안티패턴(Anti-Pattern)도 존재한다. 데이터베이스 구조를 그대로 이벤트에 사용하는 것, 필드 이름을 자주 변경하는 것, Mandatory Field를 무계획하게 추가하는 것, 지나치게 큰 Payload를 사용하는 것, 문서화를 하지 않는 것, 스키마 호환성 검증을 생략하는 것은 장기적으로 시스템 유지보수를 매우 어렵게 만든다.

결국 이벤트 스키마(Event Schema)는 분산 시스템(Distributed System)의 공통 언어(Common Language)이다. **명확한 의미(Semantic Clarity)**, **안정적인 구조(Stable Structure)**, **호환성을 고려한 버전 관리(Compatibility-Aware Versioning)**, **Schema Registry**, **자동 검증(Automated Validation)**, **거버넌스(Governance)**, **문서화(Document Management)** 를 함께 적용해야 한다. 이러한 원칙은 마이크로서비스(Microservices), 클라우드 네이티브(Cloud-Native), 산업 자동화(Industrial Automation), 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 분산 AI(Distributed AI), 디지털 트윈(Digital Twin), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼이 장기간 안정적으로 상호 운용되고 지속적으로 발전할 수 있는 핵심 기반이 된다.

## 05.04 Event Sourcing Deep Dive

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 소싱(Event Sourcing)은 이벤트 기반 아키텍처(Event-Driven Architecture, EDA)의 가장 대표적인 설계 패턴 중 하나이다. 일반적인 데이터베이스(Database)가 현재 상태(Current State)만 저장하는 것과 달리, 이벤트 소싱은 시스템에서 발생한 모든 비즈니스 이벤트(Business Event)를 영구적으로 저장한다. 현재의 상태는 저장된 이벤트를 순서대로 다시 실행(Replaying)하여 계산되므로, 시스템의 모든 변화 과정을 완전하게 추적할 수 있다.

기존 데이터베이스에서는 고객 주소가 변경되면 기존 값을 새로운 값으로 덮어쓴다. 이전 정보는 별도의 감사 로그(Audit Log)가 없으면 사라진다. 그러나 이벤트 소싱에서는 CustomerRegistered, AddressChanged, ContactUpdated와 같은 이벤트를 모두 저장한다. 따라서 현재 주소뿐 아니라 과거의 모든 변경 이력을 그대로 보존할 수 있으며, 특정 시점의 상태도 정확하게 복원할 수 있다.

이벤트 소싱의 핵심 철학은 **이벤트(Event)** 가 시스템의 진실(Source of Truth)이라는 것이다. 현재 데이터(Current State)는 이벤트를 계산한 결과일 뿐이며, 실제 데이터는 이벤트의 연속적인 기록(Event Stream)에 존재한다. 따라서 현재 상태가 어떻게 만들어졌는지를 언제든지 설명할 수 있으며, 모든 데이터의 생성 과정을 명확하게 추적할 수 있다.

이벤트(Event)는 이미 발생한 비즈니스 사실(Business Fact)을 의미한다. 한 번 저장된 이벤트는 절대로 수정하거나 삭제하지 않는다. 만약 고객 정보가 다시 변경되면 기존 이벤트를 수정하는 것이 아니라 새로운 이벤트를 추가한다. 이러한 불변성(Immutability)은 감사(Audit), 장애 분석(Debugging), 규제 준수(Regulatory Compliance), 데이터 신뢰성(Data Integrity)을 크게 향상시킨다.

각 이벤트는 메타데이터(Metadata)와 비즈니스 데이터(Payload)로 구성된다. 메타데이터에는 Event ID, Timestamp, Aggregate ID, Schema Version, Correlation ID, Causation ID, 사용자(User), 서비스(Source Service) 등의 정보가 포함된다. Payload에는 실제 업무 데이터가 저장된다. 이 구조는 이벤트의 추적성과 운영 관리 능력을 크게 향상시킨다.

이벤트 저장소(Event Store)는 이벤트 소싱 시스템의 핵심 데이터베이스이다. 일반적인 관계형 데이터베이스(Relational Database)가 현재 상태를 저장하는 것과 달리, Event Store는 이벤트 스트림(Event Stream)을 저장한다. 고객(Customer), 주문(Order), 로봇(Robot), 설비(Equipment) 등 각각의 객체는 자신만의 이벤트 스트림을 가지며, 모든 상태 변화가 시간 순서대로 저장된다.

도메인 주도 설계(Domain-Driven Design, DDD)에서는 집합체(Aggregate)가 이벤트의 일관성(Consistency)을 유지하는 단위가 된다. 하나의 Aggregate는 자신의 이벤트 스트림만 관리하며, 내부에서는 순서(Order)가 보장된다. 이를 통해 불필요한 전역 동기화(Global Synchronization)를 피하면서도 비즈니스 규칙을 안전하게 유지할 수 있다.

사용자가 명령(Command)을 보내면 Aggregate는 먼저 비즈니스 규칙(Business Rule)을 검증한다. 검증이 성공하면 OrderPlaced, PaymentAuthorized, RobotMissionAssigned와 같은 이벤트를 생성하여 Event Store에 저장한다. 즉, Command는 요청(Request)이고 Event는 이미 완료된 사실(Fact)이라는 점이 명확하게 구분된다.

이벤트 소싱의 가장 큰 특징은 상태 재구성(State Reconstruction)이다. 현재 상태는 데이터베이스에서 직접 읽는 것이 아니라 이벤트를 처음부터 순서대로 다시 실행하여 계산한다. 이벤트 하나하나가 메모리상의 객체 상태를 변경하며, 마지막 이벤트까지 적용하면 현재 상태가 완성된다.

이벤트가 매우 많아지면 재생(Replaying)에 시간이 오래 걸릴 수 있다. 이를 해결하기 위해 스냅샷(Snapshot)을 사용한다. 일정 수의 이벤트마다 현재 상태를 저장해 두고, 이후에는 마지막 Snapshot부터 새로운 이벤트만 재생한다. 이렇게 하면 이벤트 소싱의 장점을 유지하면서도 성능을 크게 향상시킬 수 있다.

Snapshot은 성능 최적화(Performance Optimization)를 위한 캐시(Cache)일 뿐이며, 진짜 데이터는 Event Store에 저장된 이벤트이다. Snapshot이 손상되더라도 Event Store의 이벤트를 다시 재생하면 언제든지 새로운 Snapshot을 생성할 수 있다.

이벤트 소싱은 시간 기반 조회(Temporal Query)에 매우 강력하다. 특정 날짜나 특정 시점의 상태를 그대로 재현할 수 있기 때문이다. 예를 들어 정책 변경 이전의 고객 정보, 생산 설비의 과거 상태, AI 모델 교체 이전의 결과, 로봇의 특정 미션 수행 상황 등을 정확하게 복원할 수 있다.

감사 기능(Auditability)은 이벤트 소싱의 가장 큰 장점 중 하나이다. 누가(User), 언제(When), 어떤 작업(What)을 수행했으며, 왜 현재 상태가 되었는지를 모든 이벤트를 통해 추적할 수 있다. 금융(Finance), 의료(Healthcare), 공공(Government)과 같이 규제가 엄격한 산업에서는 이러한 감사 기능이 매우 중요한 요구사항이다.

장애 분석(Debugging)도 매우 쉬워진다. 실제 운영 환경에서 발생했던 이벤트를 그대로 재생하면 당시의 시스템 상태를 정확하게 복원할 수 있기 때문이다. 분산 시스템에서 발생한 Race Condition, 동시성 문제(Concurrency Issue), AI 추론 오류(Inference Error), 로봇의 이상 동작도 동일한 이벤트를 이용하여 재현할 수 있다.

AI 시스템에서는 이벤트 소싱이 매우 중요한 학습 데이터(Data Source)가 된다. 고객 행동(Customer Behavior), 센서 데이터(Sensor Data), AI 추론 결과(Inference Result), 로봇 주행 기록(Robot Mission History), 생산 설비 데이터(Industrial Telemetry)가 모두 시간 순서대로 저장되므로 머신러닝(Machine Learning)과 강화학습(Reinforcement Learning)에 매우 유용하다.

이벤트 재생(Event Replay)은 장애 복구뿐 아니라 새로운 기능을 만들 때도 활용된다. 새로운 분석 시스템(Analytics), 검색(Search), AI Feature Store, 데이터 웨어하우스(Data Warehouse), 디지털 트윈(Digital Twin), 추천 시스템(Recommendation System)은 기존 이벤트를 다시 재생하여 초기 데이터를 쉽게 생성할 수 있다.

프로젝션(Projection)은 이벤트를 읽어서 조회(Read)용 데이터베이스를 생성하는 과정이다. Event Store는 이벤트만 저장하지만 Dashboard, Report, Search Engine, Fleet Monitoring, AI 분석 시스템은 각각 자신에게 맞는 Projection을 생성하여 최적화된 조회 성능을 제공한다.

이벤트 소싱은 CQRS(Command Query Responsibility Segregation)와 함께 사용하는 경우가 많다. Command는 Event를 생성하고, Query는 Projection을 조회한다. 쓰기(Write)와 읽기(Read)를 완전히 분리하여 각각 독립적으로 최적화할 수 있으므로 대규모 시스템의 성능과 확장성이 크게 향상된다.

Projection은 이벤트를 비동기적으로 처리하므로 최종 일관성(Eventual Consistency)이 발생한다. 이벤트가 저장된 직후에는 Projection이 아직 업데이트되지 않았을 수 있지만 시간이 지나면 모든 조회 데이터가 동일한 상태로 수렴한다.

동시성(Concurrency)은 낙관적 동시성 제어(Optimistic Concurrency Control)를 사용하여 해결한다. Aggregate는 자신의 버전(Version)을 가지고 있으며, 명령을 처리할 때 예상 버전(Expected Version)과 현재 버전이 동일할 때만 이벤트를 저장한다. 버전이 다르면 충돌(Conflict)이 발생하여 다시 처리해야 한다.

이벤트 소싱에서는 이벤트 자체를 수정할 수 없기 때문에 스키마 진화(Schema Evolution)가 중요하다. 오래된 이벤트를 새로운 형식으로 변환하는 업캐스팅(Upcasting)을 사용하여 기존 데이터를 유지하면서 새로운 비즈니스 모델을 적용할 수 있다.

스키마 변경은 매우 신중해야 한다. 수년 동안 저장된 이벤트도 계속 사용되기 때문이다. 새로운 필드는 Optional Field로 추가하고, 기존 필드는 충분한 마이그레이션(Migration) 기간을 거쳐 제거하는 것이 일반적인 방법이다.

Event Store는 일반 데이터베이스와 다른 저장 구조를 사용한다. EventStoreDB는 이벤트 소싱 전용 데이터베이스이며, Kafka는 대규모 이벤트 저장에 자주 사용된다. 관계형 데이터베이스(Relational Database)와 NoSQL도 Append-Only 방식으로 Event Store를 구현할 수 있다.

모든 이벤트를 저장하므로 저장 공간(Storage)은 계속 증가한다. 따라서 압축(Compression), 백업(Backup), 아카이브(Archive), 콜드 스토리지(Cold Storage), 데이터 보존 정책(Retention Policy)을 함께 운영하여 장기적인 저장 비용을 관리해야 한다.

관찰 가능성(Observability)은 이벤트 소싱에서도 매우 중요하다. Correlation ID와 Causation ID를 이용하면 이벤트 간의 관계를 추적할 수 있으며, Projection Lag, Replay 성능, Snapshot 생성 주기, Event 처리량(Event Throughput) 등을 지속적으로 모니터링하여 운영 상태를 관리한다.

보안(Security)도 매우 중요하다. 개인정보(PII), 금융 데이터(Financial Data), 산업 기밀(Industrial Secret), AI 결과는 Event Store에 장기간 저장되므로 암호화(Encryption), 접근 제어(Access Control), 데이터 마스킹(Data Masking), 토큰화(Tokenization), 감사 로그(Audit Log)를 적용해야 한다. 이벤트는 삭제하기 어렵기 때문에 개인정보 보호(Privacy)를 고려한 설계가 반드시 필요하다.

분산 마이크로서비스(Microservices)는 이벤트를 통해 서로 동기화된다. 복잡한 분산 트랜잭션(Distributed Transaction)을 사용하는 대신 이벤트를 발행하고 각 서비스가 이를 독립적으로 처리하므로 서비스 간 결합도가 크게 낮아지고 확장성도 향상된다.

금융 시스템(Financial System)은 이벤트 소싱을 가장 많이 활용하는 분야 중 하나이다. 계좌(Account), 송금(Transfer), 승인(Authorization), 정산(Settlement), 취소(Reversal) 등의 모든 기록이 영구적으로 저장되므로 감사와 규제 대응이 매우 용이하다.

산업 자동화(Industrial Automation)에서는 생산 기록(Production Record), 설비 유지보수(Maintenance), 품질 검사(Quality Inspection), 센서 이상(Sensor Anomaly), 예지보전(Predictive Maintenance), 디지털 트윈(Digital Twin)이 모두 이벤트 형태로 저장된다. 이를 통해 생산 이력 분석과 원인 분석(Root Cause Analysis)을 매우 정확하게 수행할 수 있다.

로보틱스(Robotics)는 이벤트 소싱이 특히 효과적인 분야이다. 위치추정(Localization), 경로 계획(Path Planning), 장애물 감지(Obstacle Detection), 배터리 상태(Battery Status), 미션 수행(Mission Execution), Fleet 관리(Fleet Management), AI 추론(AI Inference), 유지보수(Maintenance)를 모두 이벤트로 저장하면 로봇의 전체 운행 이력을 정확하게 재현할 수 있다. 또한 AI 학습 데이터와 디지털 트윈 생성에도 매우 유용하다.

AI 플랫폼에서는 데이터 수집(Data Ingestion), 모델 학습(Model Training), 검증(Model Validation), 배포(Model Deployment), 추론(Inference), 모델 드리프트(Model Drift), 재학습(Retraining)이 모두 이벤트로 저장된다. 이를 통해 AI 모델의 동작을 완전히 재현할 수 있으며 설명 가능성(Explainability)과 지속적인 MLOps 운영을 지원한다.

그러나 이벤트 소싱은 모든 시스템에 적합한 것은 아니다. 이벤트 모델링(Event Modeling), DDD, CQRS, Snapshot, Projection, Event Replay, Versioning 등 학습해야 할 개념이 많고 구현도 복잡하다. 단순한 CRUD(Create, Read, Update, Delete) 시스템에서는 이러한 복잡성이 오히려 단점이 될 수 있다.

대표적인 안티패턴(Anti-Pattern)은 데이터베이스 변경을 그대로 이벤트로 만드는 것, 기술적인 이벤트를 사용하는 것, 과거 이벤트를 수정하는 것, Snapshot을 사용하지 않는 것, 버전 관리를 하지 않는 것, 이벤트 이력이 필요하지 않은 단순 시스템에 이벤트 소싱을 적용하는 것이다.

결국 이벤트 소싱(Event Sourcing)은 **현재 상태(Current State)** 가 아니라 **비즈니스의 변화 과정(Business History)** 을 저장하는 아키텍처이다. **불변 이벤트(Immutable Event)**, **Event Store**, **Projection**, **Snapshot**, **Event Replay**, **CQRS**, **Schema Evolution**, **Temporal Query**, **Auditability**를 중심으로 구성되며, 금융(Finance), 산업 자동화(Industrial Automation), 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 디지털 트윈(Digital Twin), 분산 AI(Distributed AI), 클라우드 네이티브(Cloud-Native), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼에서 높은 추적성(Traceability), 설명 가능성(Explainability), 복원력(Resilience), 확장성(Scalability)을 제공하는 매우 강력한 데이터 관리 패턴이다.

## 05.05 Event-Driven Microservices Patterns

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 기반 마이크로서비스(Event-Driven Microservices)는 **마이크로서비스 아키텍처(Microservices Architecture)** 와 **이벤트 기반 아키텍처(Event-Driven Architecture, EDA)** 를 결합한 현대 분산 시스템의 핵심 아키텍처이다. 마이크로서비스는 시스템을 독립적인 비즈니스 서비스(Business Service)로 분리하고, 이벤트 기반 아키텍처는 이러한 서비스들이 직접 호출 대신 이벤트(Event)를 통해 비동기적으로 통신하도록 한다. 이 두 가지를 결합하면 높은 확장성(Scalability), 복원력(Resilience), 독립성(Autonomy), 유지보수성(Maintainability)을 동시에 확보할 수 있다.

기존의 마이크로서비스는 주로 REST나 gRPC 기반의 동기식(Synchronous) 호출을 사용하였다. 하나의 서비스가 다른 서비스를 호출하고 응답(Response)을 기다린 후 다음 작업을 수행한다. 이러한 구조는 서비스 수가 증가할수록 의존성(Dependency)이 커지고, 하나의 장애가 다른 서비스로 연쇄적으로 전파되는 문제(Cascading Failure)를 유발할 수 있다.

이벤트 기반 마이크로서비스는 이러한 문제를 비동기 통신(Asynchronous Communication)으로 해결한다. 서비스는 다른 서비스에게 작업을 요청하지 않고, 자신이 완료한 비즈니스 결과를 이벤트(Event)로 발행(Publish)한다. 필요한 서비스는 해당 이벤트를 구독(Subscribe)하여 독립적으로 처리한다. 이벤트를 발행하는 서비스는 누가 이벤트를 사용할지 알 필요가 없으며, 소비자 역시 생산자의 내부 구조를 알 필요가 없다.

이 구조의 핵심 철학은 서비스(Service)가 기술 기능이 아니라 비즈니스 기능(Business Capability)을 소유해야 한다는 것이다. Customer Service는 고객 이벤트를, Payment Service는 결제 이벤트를, Inventory Service는 재고 이벤트를, Robot Controller는 미션 이벤트를, AI Service는 추론 결과 이벤트를 각각 책임지고 관리한다. 각 서비스는 자신의 도메인(Domain)에 대한 유일한 권한(Authority)을 가진다.

이벤트(Event)는 이미 발생한 비즈니스 사실(Business Fact)을 의미한다. CustomerRegistered, PaymentAuthorized, InventoryReserved, InspectionCompleted, RobotMissionAssigned, AutonomousNavigationCompleted와 같은 이벤트는 이미 완료된 결과를 표현한다. 이는 미래의 행동을 요청하는 Command와 명확히 구분되며, 서비스 간 결합도를 크게 낮추는 핵심 원칙이다.

가장 기본적인 패턴은 **Publish-Subscribe Pattern** 이다. 생산자(Producer)는 이벤트를 브로커(Event Broker)에 발행하고, 소비자(Consumer)는 자신이 필요한 이벤트만 구독한다. 하나의 CustomerRegistered 이벤트는 마케팅(Marketing), AI 추천(Recommendation), 분석(Analytics), 이메일 발송(Notification), 디지털 트윈(Digital Twin) 등 여러 서비스가 동시에 활용할 수 있다.

Publish-Subscribe 구조는 생산자와 소비자를 완전히 분리한다. 새로운 서비스가 추가되더라도 기존 서비스를 수정할 필요가 없다. 단순히 필요한 이벤트를 구독하기만 하면 되므로 조직의 개발 속도와 확장성이 크게 향상된다.

**Event Notification Pattern** 은 가장 단순한 이벤트 전달 방식이다. 이벤트는 "무언가 발생했다"는 사실만 알려주며 상세 정보는 포함하지 않는다. 소비자는 추가적으로 API를 호출하여 필요한 정보를 조회한다. 이벤트 크기는 작지만 소비자가 다시 Producer를 호출해야 하므로 일부 동기식 의존성이 남게 된다.

이를 보완하는 것이 **Event-Carried State Transfer Pattern** 이다. 이벤트 안에 소비자가 필요한 데이터를 함께 포함한다. CustomerRegistered 이벤트에 고객 이름(Name), 지역(Region), 회원 등급(Level)을 포함하거나 RobotMissionCompleted 이벤트에 이동 거리(Distance), 배터리(Battery), 수행 시간(Duration)을 함께 포함하면 소비자는 별도의 API 호출 없이 독립적으로 작업을 수행할 수 있다.

두 방식은 각각 장단점이 있다. Notification 방식은 네트워크 사용량이 적지만 API 호출이 많아지고, Event-Carried 방식은 이벤트 크기가 커지지만 서비스 간 의존성이 크게 감소한다. 실제 시스템에서는 두 가지를 상황에 맞게 함께 사용하는 경우가 많다.

**Competing Consumers Pattern** 은 하나의 Queue를 여러 Consumer가 동시에 처리하는 방식이다. 동일한 이벤트를 여러 Worker가 경쟁적으로 가져가 처리하므로 이미지 처리(Image Processing), AI 추론(AI Inference), 문서 생성(Document Generation), 로봇 데이터 처리(Robot Telemetry)와 같이 대량 작업을 병렬 처리할 수 있다.

Kafka에서는 이를 **Consumer Group** 으로 구현한다. 하나의 Consumer Group은 여러 Partition을 나누어 처리하며, 서로 다른 Consumer Group은 동일한 이벤트를 독립적으로 소비할 수 있다. 예를 들어 Analytics, AI 학습, Billing, Monitoring이 동일한 이벤트를 각각 다른 목적으로 사용할 수 있다.

**Event Streaming Pattern** 은 이벤트를 단순히 전달하는 것이 아니라 지속적으로 저장하는 방식이다. 이벤트는 삭제되지 않고 Event Stream으로 보존되므로 재생(Event Replay), AI 학습, 디지털 트윈(Digital Twin), 감사(Audit), 데이터 분석(Analytics)에 반복 활용할 수 있다. Apache Kafka가 대표적인 구현 기술이다.

**Event Sourcing Pattern** 은 Event Streaming을 더욱 발전시킨 구조이다. 현재 상태(Current State)를 저장하지 않고 모든 이벤트(Event)를 저장하며, 현재 상태는 이벤트를 다시 실행(Replaying)하여 계산한다. 이를 통해 시간 여행(Time Travel), 장애 복구(Recovery), AI 데이터 생성, 감사 기능을 자연스럽게 제공할 수 있다.

**CQRS(Command Query Responsibility Segregation)** 는 Event Sourcing과 함께 자주 사용된다. Command는 Event를 생성하고, Projection은 Event를 이용하여 조회(Read)용 데이터베이스를 구축한다. 쓰기와 읽기를 완전히 분리하여 각각 독립적으로 최적화할 수 있다.

**Saga Pattern** 은 여러 마이크로서비스에 걸친 긴 비즈니스 프로세스(Long Running Transaction)를 처리하는 방법이다. 하나의 글로벌 트랜잭션(Global Transaction)을 사용하는 대신 여러 개의 Local Transaction과 이벤트(Event)를 이용하여 전체 업무를 수행한다. 문제가 발생하면 Compensation Event를 이용하여 이전 작업을 되돌린다.

Saga는 크게 두 가지 방식으로 구현된다. **Choreography** 는 중앙 제어 없이 서비스들이 이벤트만으로 서로 협력하는 방식이며, **Orchestration** 은 중앙 오케스트레이터(Orchestrator)가 전체 업무 흐름을 관리하는 방식이다. Choreography는 높은 자율성을 제공하고, Orchestration은 복잡한 업무를 관리하기 쉽다.

**Outbox Pattern** 은 데이터베이스(Database) 저장과 이벤트 발행(Event Publish)의 일관성을 보장하는 패턴이다. 비즈니스 데이터와 이벤트를 하나의 트랜잭션(Transaction)으로 저장한 후 Outbox를 통해 브로커로 전달한다. 이를 통해 데이터는 저장되었지만 이벤트가 유실되는 문제를 방지할 수 있다.

**Inbox Pattern** 은 이벤트 중복 처리를 방지하는 방법이다. 이벤트 ID(Event ID)를 저장하여 이미 처리한 이벤트는 다시 실행하지 않는다. 이는 At-Least-Once 전달 방식에서 반드시 필요한 패턴이다.

**Idempotent Consumer Pattern** 은 동일한 이벤트가 여러 번 전달되더라도 결과가 한 번 처리한 것과 동일하도록 만드는 패턴이다. 금융(Finance), 재고(Inventory), 로봇 제어(Robot Control)와 같이 중복 실행이 치명적인 시스템에서는 반드시 적용되어야 한다.

**Dead Letter Queue(DLQ)** 는 처리할 수 없는 이벤트를 별도의 Queue에 저장하는 방식이다. 잘못된 이벤트가 전체 시스템을 중단시키지 않도록 격리하며, 운영자가 이후 원인을 분석하고 재처리할 수 있도록 한다.

**Retry Pattern** 은 일시적인 장애를 자동으로 복구한다. Exponential Backoff는 재시도 간격을 점진적으로 늘려 시스템 과부하를 방지하며, Circuit Breaker는 장애가 지속될 경우 호출을 일시적으로 차단하여 전체 시스템을 보호한다.

**Event Filtering Pattern** 은 소비자가 필요한 이벤트만 구독하도록 한다. AI 시스템은 AI 이벤트만, 로봇은 미션 이벤트만, ERP는 주문 이벤트만 구독하여 불필요한 처리 비용을 줄일 수 있다.

**Content-Based Routing Pattern** 은 이벤트 내용(Content)을 분석하여 목적지를 결정한다. 예를 들어 AI Confidence가 일정 수준 이하인 경우에는 사람 검토(Human Review)로 전달하고, 긴급한 설비 이상(Event Severity)이면 즉시 유지보수 시스템으로 전달할 수 있다.

모든 통신을 이벤트로 처리할 필요는 없다. 사용자 로그인(Login), 결제 승인 확인(Payment Confirmation)과 같이 즉각적인 응답이 필요한 경우에는 **Request-Reply Pattern** 이 더 적합하다. 따라서 이벤트 기반 시스템에서도 REST나 gRPC는 계속 중요한 역할을 수행한다.

실제 시스템은 대부분 **Hybrid Communication Pattern** 을 사용한다. 실시간 응답이 필요한 기능은 REST 또는 gRPC를 사용하고, 장시간 처리(Long Running Task), AI 분석, 데이터 동기화, 알림(Notification), 디지털 트윈(Digital Twin), 로봇 Fleet 관리는 이벤트(Event)를 사용한다.

**Event Aggregation Pattern** 은 여러 개의 작은 이벤트를 하나의 요약 이벤트(Summary Event)로 만드는 방식이다. 센서 데이터(Sensor Data)를 생산 통계(Production Summary)로 변환하거나 로봇의 위치 정보를 Fleet 상태 정보로 집계하는 데 활용된다.

**Event Enrichment Pattern** 은 이벤트에 추가 정보를 보강하는 패턴이다. 고객 이벤트에 고객 등급(Customer Level)을 추가하거나, AI 추론 결과에 모델 버전(Model Version)과 신뢰도(Confidence)를 함께 추가하여 소비자가 더욱 풍부한 정보를 활용할 수 있도록 한다.

**Correlation Pattern** 은 서로 관련된 이벤트를 하나의 업무 흐름으로 연결한다. Correlation ID를 이용하여 주문(Order), 결제(Payment), 배송(Shipping), 청구(Billing), 고객 알림(Notification)을 하나의 프로세스로 추적할 수 있으며, 분산 추적(Distributed Tracing)의 핵심 기술이 된다.

**Event Choreography Pattern** 은 중앙 제어 없이 서비스가 이벤트만으로 협력하는 구조이다. 새로운 서비스는 기존 이벤트를 구독하기만 하면 시스템에 쉽게 참여할 수 있으므로 높은 확장성과 자율성을 제공한다.

**Reactive Processing Pattern** 은 이벤트가 발생하는 즉시 시스템이 반응하는 구조이다. 설비 이상이 발생하면 즉시 유지보수를 시작하고, AI 모델 드리프트(Model Drift)가 감지되면 자동으로 재학습(Retraining)을 수행하며, 로봇 장애가 발생하면 Fleet가 즉시 새로운 미션을 재배정한다.

클라우드 네이티브(Cloud-Native)는 이벤트 기반 마이크로서비스를 더욱 강력하게 만든다. Kubernetes는 Consumer를 자동 확장(Auto Scaling)하고, Serverless는 이벤트가 발생할 때만 실행되며, 관리형 이벤트 브로커(Managed Event Broker)는 운영 부담을 크게 줄여준다.

AI 플랫폼에서도 이벤트 기반 구조가 널리 사용된다. 데이터 수집(Data Ingestion), Feature Engineering, 모델 학습(Model Training), 검증(Model Validation), 배포(Model Deployment), 추론(Inference), 드리프트 감지(Model Drift), 재학습(Retraining)이 모두 이벤트를 중심으로 연결되어 지속적인 MLOps를 구현한다.

산업 자동화(Industrial Automation)에서는 MES, PLC, 품질 검사(Quality Inspection), ERP, 디지털 트윈(Digital Twin), 예지보전(Predictive Maintenance)이 이벤트를 통해 연결된다. 생산 공정은 실시간 이벤트를 기반으로 자동 조정되며, 운영 효율성과 생산성이 크게 향상된다.

자율주행 이동로봇(Autonomous Mobile Robot, AMR)은 이벤트 기반 마이크로서비스의 대표적인 사례이다. 위치추정(Localization), 장애물 인식(Obstacle Detection), 미션 할당(Mission Assignment), 배터리 상태(Battery Status), AI 추론(AI Inference), Fleet 관리(Fleet Management), 디지털 트윈(Digital Twin)이 각각 독립적인 서비스로 동작하면서 이벤트를 통해 협력한다. 이 구조는 로봇 시스템의 확장성과 유지보수성을 크게 향상시킨다.

이벤트 기반 마이크로서비스에서도 안티패턴(Anti-Pattern)은 존재한다. 모든 기능을 이벤트로 처리하는 것, 기술 중심 이벤트를 사용하는 것, Producer 데이터베이스를 Consumer가 직접 참조하는 것, 이벤트를 RPC처럼 사용하는 것, 이벤트를 지나치게 세분화하는 것, Schema 관리와 Idempotency를 무시하는 것은 모두 장기적인 유지보수를 어렵게 만든다.

결국 이벤트 기반 마이크로서비스(Event-Driven Microservices)는 **마이크로서비스(Microservices)** 와 **이벤트 기반 아키텍처(Event-Driven Architecture)** 를 결합하여 서비스 간 결합도를 최소화하고 높은 자율성(Autonomy)과 확장성(Scalability)을 제공하는 현대 소프트웨어 아키텍처이다. **Publish-Subscribe**, **CQRS**, **Event Sourcing**, **Saga**, **Outbox**, **Inbox**, **Dead Letter Queue**, **Retry**, **Event Streaming**, **Reactive Processing** 등의 패턴을 적절히 조합하면 클라우드 네이티브(Cloud-Native), 산업 자동화(Industrial Automation), 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 디지털 트윈(Digital Twin), 분산 AI(Distributed AI), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼을 위한 확장 가능하고 유연하며 복원력 높은 분산 시스템을 구축할 수 있다.

## 05.06 Robot Sensor Event Streaming Pipeline Design

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 센서 이벤트 스트리밍 파이프라인(Robot Sensor Event Streaming Pipeline)은 현대 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 산업용 로봇(Industrial Robot), 휴머노이드(Humanoid), 서비스 로봇(Service Robot), 협동로봇(Collaborative Robot), 검사 로봇(Inspection Robot), 농업 로봇(Agricultural Robot), 국방 로봇(Defense Robot), 그리고 차세대 피지컬 AI(Physical AI)의 핵심 아키텍처이다. 로봇은 다양한 센서에서 발생하는 대량의 데이터를 실시간으로 수집, 동기화, 처리, 저장, 분석하여 자율적인 의사결정을 수행해야 하므로 이벤트 스트리밍(Event Streaming) 구조가 필수적이다.

기존 정보 시스템은 비교적 적은 양의 비즈니스 데이터를 처리하지만, 자율주행 로봇은 초당 수천\~수백만 개의 센서 이벤트를 생성한다. 이러한 이벤트는 위치추정(Localization), 환경 인식(Perception), 경로 계획(Path Planning), 제어(Control), AI 추론(AI Inference), 디지털 트윈(Digital Twin), Fleet 관리(Fleet Management), 클라우드 분석(Cloud Analytics), 머신러닝(Machine Learning)에 동시에 활용된다. 따라서 센서 데이터를 효율적으로 관리하는 스트리밍 파이프라인이 시스템의 성능을 결정한다.

현대 로봇은 다양한 종류의 센서를 동시에 사용한다. 카메라(Camera)는 초당 30\~60장의 이미지를 생성하고, LiDAR는 초당 수십 개의 포인트 클라우드(Point Cloud)를 생성한다. IMU(Inertial Measurement Unit)는 수백\~수천 Hz의 가속도와 각속도를 측정하며, Wheel Encoder는 매우 높은 빈도로 주행 정보를 제공한다. GNSS, Radar, Ultrasonic Sensor, Thermal Camera, Force Sensor, Battery Management System, Motor Controller도 각각 독립적인 이벤트를 지속적으로 생성한다.

이벤트 스트리밍 파이프라인의 가장 중요한 목적은 센서와 응용 프로그램(Application)을 분리(Decoupling)하는 것이다. 센서가 각 응용 프로그램과 직접 통신하는 대신 모든 센서는 이벤트(Event)를 발행(Publish)하고, 필요한 시스템만 이를 구독(Subscribe)한다. 위치추정 시스템(Localization)은 IMU, GNSS, Encoder, LiDAR를 구독하고, 장애물 인식(Obstacle Detection)은 Camera와 LiDAR를 구독하며, AI는 통합된 센서 이벤트를 처리한다. Fleet Manager는 로봇 상태 이벤트만 구독하면 된다.

센서 이벤트(Sensor Event)는 특정 시점에 실제 센서가 측정한 물리적 정보(Physical Observation)를 의미한다. CameraFrameCaptured, LiDARScanCompleted, IMUUpdated, WheelEncoderUpdated, GNSSPositionReceived, BatteryStatusUpdated, ObstacleDetected, RobotPoseEstimated, AIInferenceCompleted와 같은 이벤트는 모두 이미 발생한 측정 결과를 표현한다. 이러한 이벤트는 변경되지 않는 불변 데이터(Immutable Data)로 관리된다.

하나의 센서 이벤트는 메타데이터(Metadata)와 Payload로 구성된다. 메타데이터는 이벤트 관리와 동기화를 위한 정보이며, Payload는 실제 센서 데이터를 포함한다. 메타데이터에는 Event ID, Robot ID, Sensor ID, Timestamp, Sequence Number, Event Type, Schema Version, Coordinate Frame, Calibration Version 등이 포함되며, Payload에는 이미지(Image), 포인트 클라우드(Point Cloud), 가속도(Acceleration), GPS 위치(Position), 배터리 정보(Battery Information) 등이 저장된다.

메타데이터에는 Correlation ID와 Causation ID도 포함된다. Correlation ID는 동일한 센서 수집 주기(Sensing Cycle)에 속한 이벤트를 연결하며, Causation ID는 특정 이벤트가 어떤 이전 이벤트에서 생성되었는지를 추적한다. 이러한 정보는 분산 추적(Distributed Tracing), 디지털 트윈(Digital Twin), 장애 분석(Debugging), AI 데이터 생성에 매우 중요하다.

Payload는 센서 종류에 따라 서로 다른 데이터를 포함한다. Camera는 이미지(Frame), 노출(Exposure), Camera Parameter를 포함하며, LiDAR는 Point Cloud, Intensity, Scan Angle을 포함한다. IMU는 가속도, 각속도, 자세(Orientation), 공분산(Covariance)을 제공하고, GNSS는 위도(Latitude), 경도(Longitude), 고도(Altitude), 위성 상태(Satellite Status)를 포함한다. Motor Event는 속도(Speed), 전류(Current), 토크(Torque), Fault 정보를 포함한다.

시간 동기화(Time Synchronization)는 로봇 이벤트 스트리밍에서 가장 중요한 요소 중 하나이다. 서로 다른 센서가 동일한 시점을 기준으로 데이터를 생성해야 정확한 센서 융합(Sensor Fusion)이 가능하다. 예를 들어 Camera 이미지와 LiDAR Point Cloud가 수십 밀리초만 차이가 나도 고속 이동 중인 로봇에서는 큰 오차가 발생할 수 있다.

정밀한 시간 동기화를 위해 Precision Time Protocol(PTP), IEEE 1588, GNSS Time, Hardware Trigger, Deterministic Ethernet 등이 사용된다. 특히 산업용 검사 로봇(Inspection Robot)은 여러 대의 Camera, LiDAR, IMU, GNSS를 하드웨어 수준에서 동기화하여 마이크로초(Microsecond) 수준의 정확도를 확보한다. 단순한 소프트웨어 Timestamp만으로는 고정밀 센서 융합을 구현하기 어렵다.

Timestamp의 정확도는 센서 자체의 정확도만큼 중요하다. 위치추정(Localization), SLAM(Simultaneous Localization and Mapping), 장애물 인식, Motion Estimation, Digital Twin은 동일한 시점의 데이터를 기반으로 계산해야 한다. 따라서 정확한 Timestamp 관리가 로봇 성능을 결정하는 핵심 요소가 된다.

이벤트 직렬화(Event Serialization)는 센서 데이터를 전송하기 위한 형식이다. JSON은 사람이 읽기 쉽지만 데이터 크기가 크다. Protocol Buffers(Protobuf), FlatBuffers, Apache Avro, MessagePack, CBOR 등 바이너리(Binary) 직렬화는 데이터 크기를 줄이고 처리 속도를 높이므로 실시간 로봇 시스템에서 널리 사용된다.

통신 미들웨어(Middleware)는 전체 이벤트 스트리밍 구조를 결정한다. ROS 2는 DDS(Data Distribution Service)를 기본 통신으로 사용하며 QoS(Quality of Service)를 제공한다. Kafka는 장기 저장(Event Streaming)과 AI 학습에 적합하며, NATS는 초저지연(Ultra-Low Latency) 통신에 적합하다. MQTT는 IoT 장치 연결에 많이 사용되고, RabbitMQ는 기업 시스템과 연동할 때 자주 사용된다.

QoS(Quality of Service)는 이벤트 중요도에 따라 전달 방식을 다르게 설정한다. Emergency Stop, Collision Warning, Safety Alert는 반드시 전달되어야 하지만, Camera Frame 일부는 손실되어도 다음 프레임이 들어오므로 큰 문제가 없을 수 있다. 따라서 이벤트의 중요도에 따라 신뢰성(Reliability)을 다르게 설정하는 것이 중요하다.

센서 융합(Sensor Fusion)은 여러 센서 이벤트를 하나의 환경 정보(Environment Representation)로 통합하는 과정이다. EKF(Extended Kalman Filter), UKF(Unscented Kalman Filter), Particle Filter, Factor Graph, Visual-Inertial Odometry, LiDAR-Inertial Odometry, SLAM 등이 Camera, LiDAR, IMU, GNSS, Encoder 이벤트를 지속적으로 결합하여 정확한 위치추정과 환경 인식을 수행한다.

AI는 이벤트 스트리밍 파이프라인에서 매우 중요한 소비자(Consumer)이자 생산자(Producer)이다. Camera Event는 Object Detection 모델을 실행하고, LiDAR Event는 Semantic Segmentation을 수행하며, AI 추론 결과(Inference Result)는 다시 새로운 이벤트로 생성되어 Navigation, Planning, Fleet Manager에서 활용된다.

엣지 컴퓨팅(Edge Computing)은 이벤트 스트리밍의 효율을 크게 높인다. 모든 센서 데이터를 클라우드로 보내는 대신 Edge Computer에서 객체 인식(Object Detection), 위치추정(Localization), 특징 추출(Feature Extraction), AI 추론(Inference)을 수행하고 결과 이벤트만 전송한다. 이를 통해 네트워크 사용량을 크게 줄이면서도 실시간성을 유지할 수 있다.

Event Filtering은 필요한 이벤트만 구독하도록 하는 방식이다. Fleet Manager는 Robot Health만 구독하고, Localization은 IMU와 Encoder만 사용하며, AI 학습은 Camera와 LiDAR 데이터를 저장한다. 이를 통해 불필요한 데이터 처리와 네트워크 부하를 줄일 수 있다.

Event Aggregation은 수많은 작은 이벤트를 하나의 요약 이벤트(Summary Event)로 변환하는 과정이다. 수천 개의 Encoder Event는 Odometry Event로, 수백 개의 Battery Event는 Battery Health Event로, 수많은 Detection Event는 Tracking Event로 변환하여 상위 시스템이 효율적으로 처리할 수 있도록 한다.

Event Enrichment는 기존 이벤트에 추가 정보를 결합하는 방식이다. Localization 결과를 Camera Event에 추가하거나, AI Object Detection 결과에 Confidence와 Tracking ID를 추가하면 소비자는 훨씬 풍부한 정보를 사용할 수 있다. 이러한 정보 보강은 AI와 Digital Twin에서도 매우 중요한 역할을 한다.

계층형 이벤트 처리(Hierarchical Event Processing)는 전체 시스템의 확장성을 향상시킨다. Raw Sensor Event는 Edge에서 처리되고, Perception Event는 중간 계층에서 생성되며, Mission Event와 Fleet Event는 상위 계층으로 전달된다. 이렇게 하면 클라우드에는 의미 있는 고수준 이벤트만 전달되어 네트워크 부하를 크게 줄일 수 있다.

Event Buffering은 네트워크 장애(Network Failure)에 대비하는 기술이다. 로봇 내부 SSD나 Edge Storage에 이벤트를 임시 저장한 후 연결이 복구되면 클라우드와 자동으로 동기화한다. 광산(Mining), 농업(Agriculture), 터널(Tunnel), 국방(Defense)과 같이 통신이 불안정한 환경에서는 매우 중요한 기능이다.

Event Replay는 저장된 센서 이벤트를 다시 실행하는 기능이다. 이를 이용하면 AI 알고리즘 비교, 사고 분석(Incident Investigation), Digital Twin, 시뮬레이션(Simulation), AI Dataset 생성, 디버깅(Debugging)을 동일한 데이터로 반복 수행할 수 있다.

저장 구조(Storage Architecture)는 매우 중요하다. Camera와 LiDAR는 하루에도 수 TB의 데이터를 생성할 수 있으므로 SSD, Data Lake, Cloud Storage, Object Storage를 이용하여 최근 데이터는 빠르게 접근하고 오래된 데이터는 압축(Compression)과 Archive를 통해 저장 비용을 절감한다.

관찰 가능성(Observability)은 이벤트 스트리밍 시스템에서도 필수 요소이다. Sensor Frequency, Latency, Packet Loss, Consumer Lag, CPU, GPU, Network Bandwidth, Replay Time, AI Inference Time 등을 Prometheus, Grafana, OpenTelemetry 등을 이용하여 지속적으로 모니터링한다.

보안(Security)은 모든 이벤트 스트림에 적용되어야 한다. 센서 인증(Authentication), 이벤트 암호화(Encryption), 디지털 서명(Digital Signature), 접근 제어(Access Control)를 적용하여 악의적인 센서 데이터 조작을 방지해야 한다. 특히 산업용 로봇, 의료 로봇, 국방 로봇에서는 매우 중요한 요소이다.

디지털 트윈(Digital Twin)은 이벤트 스트리밍과 매우 밀접하게 연결된다. Robot Pose, Battery, Temperature, Mission Status, AI Result, Maintenance History, Sensor Health 등이 지속적으로 Digital Twin으로 전달되어 실제 로봇과 가상 모델이 항상 동일한 상태를 유지하도록 한다.

Fleet Management 역시 이벤트 스트리밍을 기반으로 동작한다. 각 로봇은 Mission, Localization, Charging, Battery, Obstacle, Maintenance 이벤트를 지속적으로 전송하며 Fleet Manager는 이를 기반으로 작업 분배(Task Allocation), 충전 스케줄(Charging Schedule), 교통 제어(Traffic Management), 예지보전(Predictive Maintenance)을 수행한다.

산업 자동화(Industrial Automation)에서도 이벤트 스트리밍은 핵심 기술이다. 검사 로봇은 품질 검사(Quality Inspection) 이벤트를 MES로 전달하고, 자율운반로봇은 물류(Logistics) 이벤트를 WMS로 전달하며, 농업 로봇은 작물 상태(Crop Status)를 분석 시스템으로 전송한다. 이러한 이벤트 기반 구조는 다양한 산업 분야에서 공통적으로 활용된다.

AI 학습(AI Training)은 센서 이벤트 스트림을 가장 중요한 데이터셋으로 사용한다. Camera Image, LiDAR Point Cloud, Localization, Control Command, Operator Intervention, Mission Result, Environment Map은 모두 머신러닝과 강화학습(Reinforcement Learning)의 핵심 학습 데이터가 된다.

반드시 피해야 할 안티패턴(Anti-Pattern)도 존재한다. 모든 Raw Sensor Data를 그대로 클라우드로 전송하는 것, Timestamp 동기화를 무시하는 것, Metadata와 Payload를 혼합하는 것, 지나치게 큰 이벤트를 생성하는 것, Schema를 관리하지 않는 것, 기술 중심 이벤트를 사용하는 것, Replay 기능을 제공하지 않는 것은 장기적인 유지보수와 성능에 큰 문제를 발생시킨다.

결국 **로봇 센서 이벤트 스트리밍 파이프라인(Robot Sensor Event Streaming Pipeline)** 은 자율주행 로봇의 **신경계(Nervous System)** 와 같은 역할을 수행한다. **센서(Sensor)**, **이벤트(Event)**, **정밀 시간 동기화(Precise Time Synchronization)**, **센서 융합(Sensor Fusion)**, **AI 추론(AI Inference)**, **엣지 컴퓨팅(Edge Computing)**, **디지털 트윈(Digital Twin)**, **Fleet Management**, **클라우드 분석(Cloud Analytics)** 을 하나의 이벤트 기반 구조로 통합함으로써 현대 자율주행 이동로봇(AMR), 산업용 로봇, 서비스 로봇, 분산 AI(Distributed AI), 클라우드 네이티브(Cloud-Native), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼의 확장성(Scalability), 실시간성(Real-Time Capability), 신뢰성(Reliability), 유지보수성(Maintainability)을 동시에 확보할 수 있는 핵심 아키텍처를 제공한다.

## 05.07 Real-Time Event Processing: Stream Processing Frameworks

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

실시간 이벤트 처리(Real-Time Event Processing)와 스트림 처리 프레임워크(Stream Processing Framework)는 현대의 클라우드 네이티브(Cloud-Native), 이벤트 기반 아키텍처(Event-Driven Architecture, EDA), 마이크로서비스(Microservices), 산업 자동화(Industrial Automation), 로보틱스(Robotics), 사물인터넷(Internet of Things, IoT), 금융(Finance), 디지털 트윈(Digital Twin), 그리고 인공지능(AI)의 핵심 기술이다. 기존의 배치 처리(Batch Processing)가 일정 시간 동안 데이터를 모은 후 분석하는 방식이라면, 스트림 처리는 이벤트가 발생하는 즉시 분석하여 실시간 의사결정을 수행한다.

현대 시스템은 더 이상 단순한 트랜잭션(Transaction)을 처리하지 않는다. 자율주행 로봇은 위치(Localization), 센서(Sensor), 장애물(Obstacle), 배터리(Battery), AI 추론(Inference) 이벤트를 지속적으로 생성하고, 공장은 설비 상태(Equipment Telemetry), 품질 검사(Quality Inspection), 생산 이벤트(Production Event)를 실시간으로 생성한다. 금융 시스템은 거래(Transaction)와 이상 거래(Fraud Detection)를 지속적으로 분석하며, 클라우드는 로그(Log), 메트릭(Metric), 트레이스(Trace)를 실시간으로 생성한다.

기존의 배치 처리(Batch Processing)는 일정 시간 동안 데이터를 저장한 후 분석을 수행한다. 하루 단위 보고서, 월간 통계, 장기 데이터 분석에는 적합하지만, 즉각적인 반응이 필요한 시스템에는 적합하지 않다. 자율주행 로봇이 장애물을 몇 분 뒤에 인식하거나 금융 시스템이 하루 뒤에 사기를 탐지하는 것은 현실적으로 불가능하다. 따라서 실시간 처리 기술이 필수적이다.

실시간 이벤트 처리에서는 새로운 이벤트(Event)가 도착하는 즉시 계산이 수행된다. 이벤트 하나가 들어오면 상태(State)가 즉시 갱신되고, 비즈니스 규칙(Business Rule)이 실행되며, AI 추론(Inference), 디지털 트윈(Digital Twin), 알림(Notification), 새로운 이벤트 생성(Event Generation)이 연속적으로 이루어진다. 계산은 일정 시간마다 수행되는 것이 아니라 이벤트 흐름에 따라 지속적으로 수행된다.

이벤트 스트림(Event Stream)은 시간 순서대로 발생하는 이벤트(Event)의 연속이다. 이벤트는 센서(Sensor), 로봇(Robot), AI, 사용자(User), 데이터베이스(Database), 산업 설비(Industrial Equipment), 클라우드 서비스(Cloud Service) 등 다양한 곳에서 생성된다. 스트림 처리 프레임워크(Stream Processing Framework)는 이러한 이벤트를 실시간으로 수신하여 필터링(Filter), 변환(Transform), 집계(Aggregation), 상관 분석(Correlation), 예측(Prediction), 이상 탐지(Anomaly Detection)를 수행한다.

스트림 처리(Stream Processing)는 기존의 요청-응답(Request-Response) 방식과 다르다. 사용자가 요청(Request)을 해야 실행되는 것이 아니라 이벤트가 발생하는 즉시 시스템이 자동으로 반응(Reactive Processing)한다. 이러한 반응형(Reactive) 구조는 자율주행, 산업 자동화, AI 시스템에서 매우 중요한 특징이다.

일반적인 스트림 처리 구조는 이벤트 생산자(Event Producer)가 데이터를 생성하고, Kafka, Pulsar, RabbitMQ, NATS, MQTT, DDS 등의 이벤트 브로커(Event Broker)가 이를 전달한다. 이후 스트림 처리 프레임워크(Stream Processing Framework)가 이벤트를 분석하고 새로운 이벤트를 생성하며, AI, Dashboard, Digital Twin, Fleet Manager, ERP, Cloud Analytics 등 다양한 시스템이 이를 활용한다.

스트림 처리에서는 다양한 연산(Operation)이 수행된다. Filtering은 필요한 이벤트만 선택하고, Mapping은 이벤트 형식을 변환하며, Enrichment는 외부 정보를 추가한다. Aggregation은 여러 이벤트를 하나로 요약하고, Window는 시간 단위로 데이터를 묶으며, Join은 여러 스트림을 결합한다. Pattern Detection은 특정 이벤트 패턴을 찾아내고, State Management는 과거 상태를 유지하면서 계산을 수행한다.

Filtering은 가장 기본적인 연산이다. 예를 들어 AI는 신뢰도(Confidence)가 일정 수준 이상인 Object Detection만 처리하고, 공장은 진동(Vibration)이 기준값을 초과한 설비만 분석하며, 금융 시스템은 일정 금액 이상의 거래만 이상 거래 분석 대상으로 사용할 수 있다. 이를 통해 불필요한 계산을 줄이고 시스템 효율을 높일 수 있다.

Transformation은 이벤트 형식을 표준화(Standardization)하는 과정이다. 서로 다른 센서 데이터나 산업 프로토콜을 하나의 공통 이벤트 구조(Common Schema)로 변환하면 이후의 모든 시스템이 동일한 방식으로 데이터를 처리할 수 있다. 이는 상호 운용성(Interoperability)을 크게 향상시킨다.

Enrichment는 이벤트에 추가 정보를 결합하는 과정이다. 고객 거래(Customer Transaction)에 지역(Region) 정보를 추가하거나, Robot Pose에 Map 정보를 추가하고, AI 추론 결과에 모델 버전(Model Version)과 Confidence를 함께 저장하면 이후의 시스템이 훨씬 풍부한 정보를 활용할 수 있다.

Aggregation은 수많은 작은 이벤트를 하나의 요약 정보(Summary Information)로 만드는 과정이다. 수천 개의 센서 데이터는 생산 통계(Production Summary)로, 수백 개의 Robot Pose는 Mission Summary로, 수많은 거래는 일별 금융 통계(Daily Financial Statistics)로 변환할 수 있다.

스트림 처리에서 가장 중요한 개념 중 하나는 윈도우(Window)이다. 이벤트 스트림은 끝이 없기 때문에 일정한 범위에서 계산해야 한다. Tumbling Window는 일정 시간 단위로 데이터를 분리하고, Sliding Window는 일부 구간이 겹치는 방식이며, Session Window는 사용자의 활동 시간에 따라 데이터를 묶는다. Window 선택은 계산 결과에 직접적인 영향을 미친다.

Event Time과 Processing Time은 반드시 구분해야 한다. Event Time은 실제 이벤트가 발생한 시간이고, Processing Time은 시스템이 이벤트를 처리한 시간이다. 네트워크(Network), Edge Computing, Robot 통신에서는 이벤트가 늦게 도착할 수 있으므로 Event Time을 기준으로 계산해야 정확한 결과를 얻을 수 있다.

Watermark는 늦게 도착하는 이벤트(Late Event)를 처리하기 위한 기술이다. 일정 시간까지는 늦게 도착하는 이벤트를 기다리고, 이후에는 해당 Window를 종료하여 계산을 완료한다. Watermark를 적절히 설정하면 정확성과 실시간성 사이의 균형을 유지할 수 있다.

상태 관리(State Management)는 스트림 처리의 핵심 기능이다. AI 이상 탐지(Anomaly Detection)는 과거 데이터를 기억해야 하고, Robot Localization은 이전 위치를 유지해야 하며, 금융 시스템은 고객의 이전 거래 내역을 기반으로 사기를 탐지한다. 따라서 스트림 처리 프레임워크는 지속적으로 상태를 저장하고 관리해야 한다.

실시간 시스템은 수개월 또는 수년 동안 중단 없이 운영되는 경우가 많기 때문에 장애 복구(Fault Tolerance)가 매우 중요하다. 이를 위해 Checkpoint를 생성하여 현재 상태를 저장하고 장애가 발생하면 마지막 Checkpoint부터 다시 시작한다. 이벤트는 Broker에 저장되어 있으므로 필요한 부분만 Replay하여 복구할 수 있다.

Exactly-Once Processing은 가장 높은 수준의 신뢰성을 제공한다. 이벤트가 중복으로 처리되지도 않고, 누락되지도 않도록 보장한다. 이를 위해 Checkpoint, Transaction, Idempotency, Event Replay 등이 함께 사용되며 금융 시스템이나 산업 자동화에서 매우 중요하다.

Apache Flink는 대표적인 스트림 처리 프레임워크이다. Event Time 처리, Watermark, 대용량 State Management, Checkpoint, Complex Event Processing(CEP), SQL 지원, AI Pipeline 등을 제공하며 금융(Finance), 산업 자동화(Industrial Automation), AI, 로보틱스(Robotics)에서 널리 사용된다.

Flink는 Streaming과 Batch를 하나의 엔진에서 모두 처리할 수 있다. Java, Scala, Python, SQL을 지원하며 대규모 분산 환경에서도 높은 확장성과 안정성을 제공한다. 최근에는 가장 강력한 실시간 스트림 처리 플랫폼 중 하나로 평가받고 있다.

Apache Spark Structured Streaming은 기존 Spark 생태계를 기반으로 하는 스트림 처리 기술이다. DataFrame과 SQL을 그대로 사용할 수 있어 기존 Spark 사용자가 쉽게 적용할 수 있으며, 배치(Batch)와 스트림(Stream)을 동일한 프로그래밍 모델로 처리할 수 있다.

Apache Storm은 초기의 대표적인 스트림 처리 프레임워크이다. Spout와 Bolt를 이용하여 이벤트 흐름을 구성하며 매우 낮은 지연시간(Low Latency)을 제공한다. 현재는 Flink와 Spark에 비해 사용 비중이 줄었지만 스트림 처리의 발전에 큰 영향을 준 기술이다.

Apache Samza는 LinkedIn에서 개발한 Kafka 중심(Stream-Centric)의 스트림 처리 프레임워크이다. Kafka와 강하게 통합되어 있으며 Stateful Processing과 Partition 기반 처리를 효율적으로 수행한다.

Kafka Streams는 별도의 클러스터 없이 일반 Java 애플리케이션 안에서 실행되는 스트림 처리 라이브러리이다. Kafka Topic을 직접 처리하며 Join, Aggregation, Window, State Store를 지원한다. 이벤트 기반 마이크로서비스(Event-Driven Microservices)에서 매우 많이 활용된다.

Apache Beam은 실행 엔진과 프로그램을 분리한 통합 프로그래밍 모델(Unified Programming Model)이다. 하나의 프로그램을 작성하면 Flink, Spark, Google Cloud Dataflow 등 여러 실행 환경에서 동일하게 실행할 수 있어 클라우드 독립성(Cloud Portability)을 높여준다.

최근에는 Amazon Kinesis Data Analytics, Google Cloud Dataflow, Azure Stream Analytics, Managed Flink와 같은 관리형(Managed) 스트림 처리 서비스도 많이 사용된다. 인프라를 직접 운영하지 않고도 대규모 실시간 분석 시스템을 구축할 수 있다는 장점이 있다.

Complex Event Processing(CEP)은 여러 이벤트를 조합하여 의미 있는 패턴을 찾는 기술이다. 제조에서는 설비 이상 패턴을 탐지하고, 금융에서는 연속적인 사기 거래를 인식하며, 로보틱스에서는 Localization 오류와 Battery 이상, Obstacle Detection을 함께 분석하여 위험 상황을 판단할 수 있다.

AI는 스트림 처리와 매우 밀접하게 연결된다. Camera Event는 Object Detection을 수행하고, Sensor Event는 Anomaly Detection을 수행하며, AI 결과는 다시 새로운 이벤트가 되어 Planning, Navigation, Digital Twin, Dashboard에서 활용된다. 또한 Online Learning을 이용하면 지속적인 모델 개선도 가능하다.

자율주행 로봇(Autonomous Mobile Robot, AMR)은 스트림 처리의 대표적인 활용 분야이다. Camera, LiDAR, IMU, GNSS, Wheel Encoder는 지속적으로 이벤트를 생성하고, Localization, Mapping, Planning, Navigation, Fleet Management가 이를 실시간으로 처리한다. 모든 처리는 수십 밀리초 이내에 완료되어야 한다.

산업 자동화에서도 스트림 처리는 매우 중요하다. 생산 설비(Telemetry), 품질 검사(Quality Inspection), PLC, MES, Digital Twin, Predictive Maintenance가 모두 실시간 이벤트를 처리하며 생산 효율과 품질을 지속적으로 최적화한다.

디지털 트윈(Digital Twin)은 스트림 처리를 통해 실제 설비와 가상 모델을 항상 동일하게 유지한다. Robot Pose, Battery, Temperature, Sensor Status, Maintenance History, AI Prediction이 모두 이벤트 형태로 전달되어 실시간 동기화가 이루어진다.

엣지 컴퓨팅(Edge Computing)은 스트림 처리의 효율을 높인다. Robot이나 Industrial Gateway에서 먼저 이벤트를 처리하고 중요한 결과만 Cloud로 전송하면 네트워크 부하를 줄이고 지연시간(Latency)을 최소화할 수 있다.

관찰 가능성(Observability)은 스트림 처리에서 매우 중요하다. Event Throughput, Processing Latency, Watermark, Checkpoint Time, Consumer Lag, CPU, Memory, Network, State Size 등을 Prometheus, Grafana, OpenTelemetry 등을 이용하여 지속적으로 모니터링해야 한다.

보안(Security)은 모든 이벤트 스트림에 적용된다. Authentication, Authorization, Encryption, Digital Signature, Audit Log를 적용하여 이벤트가 안전하게 전달되고 저장되도록 해야 한다. 금융, 의료, 산업, 국방 시스템에서는 특히 중요한 요소이다.

스트림 처리에서는 몇 가지 어려움도 존재한다. 대용량 상태(State)는 많은 메모리를 필요로 하며, 늦게 도착하는 이벤트(Late Event)는 계산을 어렵게 만든다. Exactly-Once Processing은 구현이 복잡하며, Join과 Window는 많은 계산 자원을 요구한다. 따라서 적절한 아키텍처 설계가 매우 중요하다.

대표적인 안티패턴(Anti-Pattern)은 스트림 처리 프레임워크를 단순한 메시지 큐(Message Queue)처럼 사용하는 것, Event Time을 무시하는 것, 지나치게 큰 Window를 사용하는 것, 필요 이상의 상태를 저장하는 것, 스트림 처리 중 외부 데이터베이스를 반복 조회하는 것, 여러 비즈니스 기능을 하나의 파이프라인에 모두 넣는 것이다.

결국 **실시간 이벤트 처리(Real-Time Event Processing)** 와 **스트림 처리 프레임워크(Stream Processing Framework)** 는 현대 이벤트 기반 아키텍처(Event-Driven Architecture)의 **지능 처리 계층(Intelligence Processing Layer)** 이다. **Filtering**, **Transformation**, **Enrichment**, **Aggregation**, **Window**, **State Management**, **Checkpoint**, **Exactly-Once Processing**, **Complex Event Processing**, **AI Integration**, **Edge Computing**, **Digital Twin** 을 중심으로 이벤트를 지속적으로 분석하고 새로운 지식을 생성한다. 이러한 기술은 금융(Finance), 산업 자동화(Industrial Automation), 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 클라우드 네이티브(Cloud-Native), 디지털 트윈(Digital Twin), 분산 AI(Distributed AI), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼에서 실시간성(Real-Time Capability), 확장성(Scalability), 복원력(Resilience), 지능성(Intelligence)을 제공하는 핵심 기반 기술이다.

## 05.08 Event-Driven AI Inference Triggering

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 기반 AI 추론 트리거링(Event-Driven AI Inference Triggering)은 인공지능(AI) 추론을 일정한 주기나 사용자의 직접적인 요청(Request)에 의해 실행하는 것이 아니라, 의미 있는 이벤트(Event)가 발생했을 때 자동으로 실행하는 아키텍처이다. 즉, AI 모델은 항상 실행되는 것이 아니라 필요한 상황에서만 동작한다. 이러한 방식은 연산 자원(Resource) 사용을 최소화하면서도 높은 실시간성(Real-Time Capability)과 확장성(Scalability)을 제공하며, 클라우드 네이티브(Cloud-Native), 자율주행 로봇(AMR), 산업 자동화, IoT, 디지털 트윈(Digital Twin), 피지컬 AI(Physical AI)의 핵심 구조가 되고 있다.

기존 AI 시스템은 주로 REST API나 gRPC 기반의 요청-응답(Request-Response) 방식 또는 일정 시간마다 실행되는 배치(Batch) 방식으로 동작하였다. 사용자가 AI 서비스를 호출하거나 일정 시간이 되면 데이터를 분석하였다. 이러한 방식은 구현이 단순하지만 지속적으로 데이터를 확인(Polling)해야 하므로 불필요한 계산이 많고, 시스템 규모가 커질수록 GPU와 CPU 자원을 비효율적으로 사용하게 된다.

이벤트 기반 AI는 이러한 문제를 해결하기 위해 이벤트(Event)를 중심으로 동작한다. 예를 들어 카메라(Camera)가 새로운 영상을 생성하면 객체 인식(Object Detection)을 수행하고, 배터리 상태(Battery Health)가 일정 수준 이하로 떨어지면 예지보전(Predictive Maintenance)을 시작하며, 금융 거래(Transaction)가 발생하면 사기 탐지(Fraud Detection)를 수행한다. 즉, 의미 있는 변화가 발생할 때만 AI가 실행되므로 계산 효율이 크게 향상된다.

이 구조의 핵심 철학은 AI는 모든 데이터를 지속적으로 분석하는 것이 아니라, 실제로 의미 있는 변화(Significant Change)가 발생했을 때만 추론을 수행해야 한다는 것이다. 불필요한 추론(Inference)을 줄이면 GPU 사용량, 전력 소비, 네트워크 부하를 크게 줄일 수 있으며, 동시에 중요한 이벤트에는 즉시 반응할 수 있다.

AI 추론을 유발하는 이벤트는 매우 다양하다. 카메라(Camera), LiDAR, Radar, IMU(Inertial Measurement Unit), 마이크(Microphone), 열화상 카메라(Thermal Camera), GPS, 산업용 센서(Industrial Sensor), PLC, 사용자(User), 데이터베이스(Database), 클라우드 서비스(Cloud Service), 기존 AI 모델의 결과도 모두 새로운 AI 추론의 시작점이 될 수 있다.

AI를 실행시키는 이벤트는 단순한 기술 이벤트(Technical Event)가 아니라 비즈니스 의미(Business Semantics)를 가져야 한다. RobotEnteredInspectionZone, CustomerPurchaseCompleted, BatteryHealthDegraded, MachineVibrationExceededThreshold, HumanDetected, MissionCompleted와 같은 이벤트는 명확한 의미를 가지며 AI 실행 조건으로 적합하다. 반대로 CacheUpdated나 DatabaseChanged와 같은 내부 구현 중심 이벤트는 AI 트리거로 적합하지 않다.

AI 이벤트 역시 메타데이터(Metadata)와 Payload로 구성된다. 메타데이터에는 Event ID, Timestamp, Robot ID, Facility ID, Model ID, Priority, Schema Version, Correlation ID, Causation ID 등이 포함된다. Payload에는 센서 데이터(Sensor Data), 환경 정보(Environment Information), 사용자 정보(User Context), 운용 정보(Operation Context) 등 실제 AI 추론에 필요한 데이터가 포함된다.

AI 추론은 일반적으로 이벤트 필터링(Event Filtering)부터 시작한다. 모든 이벤트를 AI에 전달하면 GPU 자원이 낭비된다. 따라서 Motion Detection, Threshold Filter, Rule Engine 등을 이용하여 실제 분석이 필요한 이벤트만 AI 모델로 전달한다. 예를 들어 움직임이 없는 Camera 영상은 객체 인식을 수행하지 않아도 된다.

가장 단순한 방식은 Threshold-Based Triggering이다. 온도(Temperature), 진동(Vibration), 전류(Current), 배터리(Battery), 위치 오차(Localization Error), AI Confidence 등이 기준값을 초과하면 AI 추론을 시작한다. 구현은 간단하지만 환경 변화에 따라 Threshold를 지속적으로 조정해야 하는 단점이 있다.

Rule-Based Triggering은 여러 조건을 동시에 만족해야 AI를 실행하는 방식이다. 검사 로봇은 Inspection Zone에 도착하고, 속도가 충분히 낮으며, 조명이 적절하고, 안전 상태가 확인되었을 때만 AI 검사 모델을 실행할 수 있다. 이러한 방식은 불필요한 AI 실행을 줄이고 정확도를 높일 수 있다.

Temporal Triggering은 시간(Time)을 고려한 AI 실행 방식이다. 단일 이벤트가 아니라 일정 시간 동안 발생한 이벤트를 분석한다. 예를 들어 설비의 진동이 지속적으로 증가하거나 로봇의 위치 오차가 일정 시간 이상 계속 증가하는 경우에만 AI 진단 모델을 실행한다.

Pattern-Based Triggering은 복합 이벤트 처리(Complex Event Processing, CEP)를 이용한다. 여러 이벤트가 특정 순서로 발생하면 AI를 실행한다. 예를 들어 설비 온도 상승, 전류 증가, 진동 증가가 함께 발생하면 고장 진단(Fault Diagnosis) AI를 시작할 수 있다. 사이버 보안(Cybersecurity)에서는 로그인 실패, 권한 상승, 네트워크 스캔이 연속적으로 발생할 경우 AI 기반 공격 탐지를 수행한다.

State-Aware Triggering은 현재 이벤트뿐 아니라 과거 상태(State)를 함께 고려한다. 스트림 처리(Stream Processing)는 Robot Mission, Customer Session, Digital Twin, Equipment Lifecycle 등의 상태를 계속 유지하고 있으며, 상태 변화(State Transition)가 발생할 때만 AI를 실행한다. 이를 통해 더욱 정확한 판단이 가능해진다.

이벤트 기반 AI는 스트림 처리 프레임워크(Stream Processing Framework)와 함께 사용하는 경우가 많다. Apache Flink, Kafka Streams, Spark Structured Streaming, Apache Beam 등은 이벤트를 지속적으로 분석하면서 AI를 실행할 시점을 결정한다. 즉, 스트림 처리가 AI의 오케스트레이터(Orchestrator) 역할을 수행한다.

엣지 컴퓨팅(Edge Computing)은 이벤트 기반 AI에서 매우 중요한 역할을 한다. 모든 센서 데이터를 클라우드로 보내는 대신 Edge Device에서 먼저 이벤트를 분석하고, 실제로 AI가 필요한 경우에만 GPU를 실행하거나 클라우드 AI를 호출한다. 이를 통해 네트워크 부하를 줄이고 응답 속도를 크게 향상시킬 수 있다.

다단계 추론(Multi-Stage Inference)은 효율성을 높이는 대표적인 구조이다. 먼저 작은 경량 모델(Lightweight Model)이 이벤트를 빠르게 분석하고, 이상이 감지되면 대형 AI 모델(Large Foundation Model)이나 멀티모달 AI(Multimodal AI)를 실행한다. 자율주행 로봇에서는 Jetson 기반 Edge AI가 1차 판단을 수행하고, 복잡한 상황에서는 클라우드 GPU가 추가 추론을 수행하는 구조가 자주 사용된다.

AI 역시 새로운 이벤트를 생성하는 Producer가 된다. Object Detection은 Object Event를 생성하고, Object Tracking은 Trajectory Event를 생성하며, Trajectory Prediction은 Navigation Event를 생성한다. 즉 AI는 Event Consumer이면서 동시에 Event Producer 역할을 수행한다.

AI 오케스트레이션(AI Orchestration)은 여러 AI 모델을 하나의 이벤트 흐름으로 연결한다. Computer Vision, Speech Recognition, Localization, Sensor Fusion, Predictive Maintenance, Large Language Model, Reinforcement Learning이 각각 이벤트를 주고받으며 협력하여 복합적인 의사결정을 수행한다.

모델 선택(Model Selection)도 이벤트 기반으로 수행할 수 있다. 실내에서는 Indoor Localization Model을 사용하고, 실외에서는 GNSS 기반 모델을 사용하며, 검사 대상 제품(Product Type)에 따라 서로 다른 Vision Model을 선택할 수 있다. 이벤트가 어떤 모델을 사용할지를 결정하는 것이다.

우선순위 기반 추론(Priority-Aware Scheduling)은 GPU 자원이 부족할 때 매우 중요하다. Collision Avoidance, Emergency Stop, Safety Inspection과 같은 안전 관련 AI는 가장 높은 우선순위를 가지며 즉시 실행된다. 반면 Recommendation, Reporting, AI Retraining은 낮은 우선순위로 백그라운드에서 실행된다.

분산 GPU 스케줄링(Distributed GPU Scheduling)은 Edge와 Cloud를 함께 사용하는 구조이다. 경량 AI는 Jetson과 같은 Edge GPU에서 실행하고, 복잡한 Foundation Model은 Cloud GPU에서 실행한다. 이벤트(Event)는 지연시간(Latency), 모델 크기(Model Size), 네트워크 상태(Network Condition), 에너지(Energy)에 따라 적절한 GPU로 자동 전달된다.

디지털 트윈(Digital Twin)은 이벤트 기반 AI와 매우 밀접하다. 센서 이벤트가 Digital Twin을 지속적으로 업데이트하고, Digital Twin은 시뮬레이션(Simulation)을 수행하여 새로운 AI 이벤트를 생성한다. 이러한 양방향(Event Feedback Loop) 구조를 통해 미래를 예측하고 최적의 운영 전략을 계산할 수 있다.

자율주행 이동로봇(Autonomous Mobile Robot, AMR)은 이벤트 기반 AI의 대표적인 사례이다. Camera Event는 Object Detection을 실행하고, LiDAR Event는 Obstacle Segmentation을 수행하며, Localization Error는 SLAM 보정을 시작한다. Battery Event는 Predictive Maintenance를 실행하고, Mission Completed는 Inspection AI를 수행한다. 모든 AI는 필요한 순간에만 실행된다.

AMR에서는 계층형 AI(Hierarchical AI)가 자주 사용된다. 저수준(Low-Level) 센서 이벤트는 Edge에서 처리되고, 중간 계층에서는 Perception Event를 생성하며, 고수준(High-Level) AI는 Mission Planning과 Navigation Decision을 수행한다. 이러한 구조는 실시간성과 확장성을 동시에 확보할 수 있다.

산업 자동화(Industrial Automation)에서도 이벤트 기반 AI는 매우 중요하다. 설비 상태(Telemetry)는 Predictive Maintenance를 실행하고, 검사 카메라는 Defect Detection을 수행하며, 생산 이벤트는 생산 최적화(Production Optimization)를 수행한다. 모든 AI는 운영 이벤트에 의해 자동으로 실행된다.

의료(Healthcare)에서는 생체 신호(Biometric Signal)가 AI를 실행한다. 심박수(Heart Rate)가 급격히 변하면 부정맥(Arrhythmia) 분석을 수행하고, 산소 포화도(Oxygen Saturation)가 낮아지면 호흡기 질환 분석을 수행하며, 의료 영상(Medical Image)이 생성되면 진단 AI가 자동 실행된다.

사이버 보안(Cybersecurity)은 이벤트 기반 AI를 가장 많이 사용하는 분야 중 하나이다. 로그인 실패(Login Failure), 비정상 접근(Unauthorized Access), 악성코드(Malware), 권한 상승(Privilege Escalation) 이벤트가 발생하면 AI가 공격 유형을 분석하고 대응 전략을 제시한다.

금융(FinTech)에서도 거래(Transaction)가 AI를 실행한다. 결제는 Fraud Detection을 시작하고, 시장 변동성(Market Volatility)은 Portfolio Optimization을 수행하며, 대출 신청(Loan Application)은 Credit Risk Assessment AI를 자동으로 실행한다.

MLOps에서도 이벤트 기반 구조가 활용된다. 데이터 수집(Data Ingestion)은 Feature Engineering을 시작하고, 학습 완료(Model Training Complete)는 Validation을 실행하며, Validation 성공은 Deployment를 시작한다. 모델 성능 저하(Model Drift)는 Retraining 이벤트를 발생시켜 AI를 지속적으로 개선한다.

관찰 가능성(Observability)은 이벤트 기반 AI에서 매우 중요하다. Event Throughput, GPU Utilization, Inference Latency, Queue Length, Model Selection Frequency, Prediction Confidence, Network Bandwidth 등을 지속적으로 모니터링해야 한다. OpenTelemetry, Prometheus, Grafana를 이용하여 전체 AI 추론 과정을 추적할 수 있다.

보안(Security)은 이벤트 기반 AI에서도 반드시 적용되어야 한다. 이벤트 인증(Authentication), 접근 제어(Authorization), 암호화(Encryption), 디지털 서명(Digital Signature), 감사 로그(Audit Log)를 통해 악성 이벤트가 AI를 잘못 실행하지 못하도록 보호해야 한다. 특히 산업, 의료, 국방 분야에서는 매우 중요한 요소이다.

이벤트 기반 AI에도 여러 가지 어려움이 존재한다. 너무 작은 이벤트에도 AI를 실행하면 GPU 자원이 낭비되고, 반대로 필터링이 너무 강하면 중요한 이벤트를 놓칠 수 있다. 또한 이벤트 순서(Ordering), GPU 스케줄링, 모델 버전(Model Version), 스키마 관리(Schema Management)도 함께 고려해야 한다.

대표적인 안티패턴(Anti-Pattern)은 모든 이벤트마다 AI를 실행하는 것, 기술 중심 이벤트를 사용하는 것, 우선순위를 고려하지 않는 것, 고빈도 이벤트에서 동기식(Synchronous) AI를 사용하는 것, 추론 이력(Traceability)을 저장하지 않는 것, Edge와 Cloud를 동일한 방식으로 처리하는 것이다.

결국 **이벤트 기반 AI 추론 트리거링(Event-Driven AI Inference Triggering)** 은 **이벤트(Event)** 를 중심으로 AI를 필요한 순간에만 실행하는 현대 AI 아키텍처이다. **Event Filtering**, **Rule Engine**, **Stream Processing**, **Edge Computing**, **AI Orchestration**, **Distributed GPU Scheduling**, **Digital Twin**, **MLOps**, **Priority Scheduling**, **Observability** 를 통합하여 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 산업 자동화(Industrial Automation), 클라우드 네이티브(Cloud-Native), 디지털 트윈(Digital Twin), 분산 AI(Distributed AI), 그리고 차세대 피지컬 AI(Physical AI) 플랫폼에서 높은 실시간성(Real-Time Capability), 확장성(Scalability), 자원 효율성(Resource Efficiency), 지능성(Intelligence), 그리고 자율성(Autonomy)을 제공하는 핵심 AI 실행 구조를 구현한다.

## 05.09 Event-Driven Fleet Orchestration

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 기반 플릿 오케스트레이션(Event-Driven Fleet Orchestration)은 다수의 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 자동운반차(Automated Guided Vehicle, AGV), 검사 로봇(Inspection Robot), 물류 로봇(Warehouse Robot), 협동로봇(Collaborative Robot), 서비스 로봇(Service Robot), 농업 로봇(Agricultural Robot), 국방 로봇(Defense Robot), 그리고 차세대 피지컬 AI(Physical AI)를 효율적으로 관리하기 위한 핵심 아키텍처이다. 중앙 서버가 지속적으로 로봇을 조회(Polling)하는 대신, 각 로봇이 의미 있는 이벤트(Event)를 자율적으로 생성하고 공유하여 전체 플릿(Fleet)을 협력적으로 운영하는 구조이다.

기존의 플릿 관리 시스템(Fleet Management System)은 중앙 서버가 모든 로봇에게 반복적으로 상태를 요청하는 폴링(Polling) 방식을 사용하였다. 서버는 배터리(Battery), 위치(Localization), 작업 상태(Mission Status), 장애(Fault), 센서(Sensor) 정보를 계속 조회해야 했으며, 로봇 수가 증가할수록 네트워크 부하(Network Load)와 처리 지연(Latency)이 급격히 증가하였다. 또한 중앙 서버 장애는 전체 플릿 운영에 영향을 주는 단일 장애점(Single Point of Failure)이 되었다.

이벤트 기반 플릿 오케스트레이션은 이러한 문제를 해결하기 위해 로봇이 스스로 이벤트를 발행(Publish)하도록 설계된다. MissionAssigned, NavigationStarted, LocalizationUpdated, ObstacleDetected, BatteryLow, ChargingRequested, DockingCompleted, InspectionFinished, EmergencyStopActivated, PredictiveMaintenanceRequired, MissionCompleted와 같은 이벤트가 발생하면 이를 즉시 공유하고, 필요한 서비스만 해당 이벤트를 구독(Subscribe)하여 처리한다.

이 구조의 핵심 철학은 로봇 간 협력이 명령(Command) 중심이 아니라 의미 있는 운영 이벤트(Operational Event)를 중심으로 이루어져야 한다는 것이다. 로봇은 자신의 상태를 능동적으로 보고하는 이벤트 생산자(Event Producer)가 되고, 플릿 관리 시스템(Fleet Manager)은 이벤트를 분석하여 최적의 의사결정을 수행하는 이벤트 소비자(Event Consumer)가 된다. 이를 통해 응답성(Responsiveness), 확장성(Scalability), 복원력(Resilience), 자율성(Autonomy)을 크게 향상시킬 수 있다.

플릿 오케스트레이션(Fleet Orchestration)은 단순한 작업 할당(Task Assignment)만 수행하는 것이 아니다. 미션 스케줄링(Mission Scheduling), 로봇 할당(Robot Allocation), 교통 제어(Traffic Management), 충전 관리(Charging Coordination), 위치 동기화(Localization Synchronization), 안전 관리(Safety Monitoring), 예지보전(Predictive Maintenance), 소프트웨어 배포(Software Deployment), 디지털 트윈(Digital Twin), AI 워크로드 분산(AI Workload Distribution), 클라우드 및 엣지 자원 관리(Edge-Cloud Resource Management)까지 전체 시스템을 통합적으로 관리한다.

플릿 이벤트(Fleet Event)는 반드시 비즈니스 의미(Business Semantics)를 가져야 한다. RobotArrivedAtDestination, ChargingStationReserved, TrafficIntersectionBlocked, BatteryHealthDegraded, InspectionResultGenerated, WarehouseZoneClosed, FleetCapacityExceeded, CollisionRiskDetected와 같은 이벤트는 실제 운영에 의미가 있다. 반면 DatabaseUpdated, CacheRefreshed와 같은 내부 구현 이벤트는 플릿 오케스트레이션에 적합하지 않다.

모든 이벤트는 메타데이터(Metadata)와 Payload를 포함한다. 메타데이터에는 Event ID, Timestamp, Robot ID, Fleet ID, Mission ID, Priority, Correlation ID, Schema Version, Software Version 등이 포함된다. Payload에는 Robot Pose, Battery Status, Mission Progress, Localization Accuracy, Obstacle Information, Inspection Result, AI Prediction, System Diagnostics 등의 실제 운영 정보가 저장된다.

미션 관리(Mission Lifecycle Management)는 플릿 오케스트레이션의 핵심 기능이다. 고객 요청(Customer Request), 생산 일정(Production Schedule), 물류(Logistics), 검사 계획(Inspection Plan) 등이 MissionCreated 이벤트를 생성하면 Fleet Manager는 로봇의 위치, 배터리, 장비 성능, 작업 부하를 고려하여 가장 적합한 로봇을 선택한다. MissionAssigned 이벤트를 받은 로봇은 즉시 작업을 시작하며 MissionProgress와 MissionCompleted 이벤트를 지속적으로 보고한다.

이벤트 기반 시스템에서는 동적 작업 할당(Dynamic Mission Allocation)이 가능하다. 장애물, 교통 혼잡, 배터리 부족, 긴급 작업, 장비 고장 등이 발생하면 새로운 이벤트가 생성되고 Fleet Manager는 즉시 미션을 재배치(Reallocation)한다. 따라서 운영 환경이 계속 변화하더라도 전체 플릿은 최적의 효율을 유지할 수 있다.

교통 관리(Traffic Orchestration)는 대규모 플릿에서 매우 중요한 기능이다. 여러 로봇이 동일한 교차로(Intersection), 통로(Corridor), 엘리베이터(Elevator), 충전소(Charging Station)를 공유하므로 각 로봇은 Localization, Planned Path, Velocity, Obstacle Event를 지속적으로 공유한다. Fleet Manager는 이를 분석하여 충돌 가능성을 미리 예측하고 안전한 이동 순서를 결정한다.

배터리 관리(Battery Management) 역시 이벤트 기반으로 수행된다. 로봇은 Battery Level, Charging Request, Charging Progress, Battery Health, Estimated Remaining Time 이벤트를 지속적으로 전송한다. Fleet Manager는 이를 기반으로 충전 시점을 조정하여 충전소 혼잡을 방지하고, 전체 플릿이 항상 충분한 작업 능력을 유지하도록 관리한다.

충전소(Charging Station)도 이벤트 생산자(Event Producer)가 된다. Charging Station은 Available, Reserved, ChargingStarted, ChargingCompleted, FaultDetected 이벤트를 생성한다. 로봇은 이 정보를 기반으로 거리(Distance), 대기 시간(Queue Length), 충전 우선순위(Priority)를 고려하여 가장 적절한 충전소를 선택한다.

위치 정보(Localization)도 지속적으로 공유된다. 각 로봇은 Pose, Localization Confidence, Map Update, Landmark Detection, GNSS Correction 이벤트를 생성하며 Fleet Manager는 이를 통합하여 전체 공장의 공간 상태를 실시간으로 유지한다. 이를 통해 Traffic Optimization과 Digital Twin의 정확도를 크게 향상시킬 수 있다.

협력 인식(Cooperative Perception)은 여러 로봇이 서로의 센서 정보를 공유하는 구조이다. 한 로봇이 발견한 장애물, 사람(Human), 공사 구역(Construction Zone), 위험 지역(Hazard Zone)을 이벤트로 전송하면 다른 로봇들도 이를 즉시 활용할 수 있다. 이를 통해 동일한 환경을 반복적으로 인식할 필요가 없어지고 전체 플릿의 안전성과 효율성이 향상된다.

AI는 이벤트 기반 플릿 오케스트레이션에서 매우 중요한 역할을 한다. AI는 Mission Completion Time, Battery Degradation, Equipment Failure, Traffic Congestion, Energy Consumption, Localization Reliability 등을 지속적으로 예측하며 Prediction Event를 생성한다. Fleet Manager는 이러한 AI 이벤트를 활용하여 더욱 최적화된 의사결정을 수행한다.

예지보전(Predictive Maintenance)은 AI와 이벤트 기반 구조가 가장 잘 결합된 사례이다. Motor Temperature, Wheel Vibration, Battery Impedance, CPU Usage, Communication Quality 등의 이벤트를 AI가 분석하여 PredictiveMaintenanceRecommended 이벤트를 생성한다. Fleet Manager는 고장이 발생하기 전에 유지보수(Maintenance)를 계획하여 운영 중단을 최소화한다.

디지털 트윈(Digital Twin)은 모든 로봇의 이벤트를 실시간으로 반영한다. Robot Pose, Mission Status, Battery, AI Result, Maintenance History, Environmental Information이 지속적으로 Digital Twin을 업데이트한다. Digital Twin은 시뮬레이션(Simulation)을 수행하여 새로운 운영 전략을 계산하고 다시 Fleet Manager에게 최적화 이벤트를 제공한다.

이벤트 스트리밍(Event Streaming)은 플릿 오케스트레이션의 기반이 된다. Apache Kafka는 이벤트 저장(Event Persistence)과 AI 학습에 적합하며, DDS는 실시간 로봇 통신에 적합하고, NATS는 초고속 클라우드 통신을 제공한다. MQTT는 IoT 장치 연결에 적합하며 RabbitMQ는 기업 시스템과의 연계에 자주 사용된다. 실제 시스템은 여러 미들웨어를 함께 사용하는 경우가 많다.

QoS(Quality of Service)는 이벤트 중요도에 따라 통신 품질을 다르게 설정한다. Emergency Stop, Collision Warning, Human Detection은 가장 높은 우선순위를 가지며 반드시 빠르게 전달되어야 한다. 반면 Battery Status나 Telemetry는 다소 지연되어도 운영에 큰 문제가 없으므로 낮은 우선순위를 사용할 수 있다.

스트림 처리(Stream Processing)는 플릿 이벤트를 지속적으로 분석한다. Apache Flink, Kafka Streams, Spark Structured Streaming 등은 이벤트를 필터링(Filter), 집계(Aggregation), 상관 분석(Correlation), 이상 탐지(Anomaly Detection)하여 운영 상태를 실시간으로 분석한다. 단순히 이벤트를 전달하는 것이 아니라 새로운 운영 지식을 생성하는 역할을 수행한다.

복합 이벤트 처리(Complex Event Processing, CEP)는 여러 이벤트를 함께 분석한다. Localization Error와 Obstacle Detection, Navigation Delay가 동시에 발생하면 지도(Map)에 문제가 있다고 판단할 수 있다. 여러 로봇에서 동시에 Battery Degradation이 발생하면 충전 설비 문제를 의심할 수 있다. 이러한 복합 분석은 운영 효율을 크게 향상시킨다.

계층형 오케스트레이션(Hierarchical Orchestration)은 대규모 플릿에서 자주 사용된다. 개별 로봇은 Local Autonomy를 수행하고, Edge Server는 지역 단위(Local Region)의 로봇을 관리하며, 중앙 Fleet Manager는 전체 공장의 스케줄과 AI 분석을 담당한다. 이러한 다계층 구조는 실시간성과 확장성을 동시에 확보할 수 있다.

엣지 컴퓨팅(Edge Computing)은 플릿 오케스트레이션의 성능을 향상시킨다. Edge는 근거리 로봇을 실시간으로 제어하고, Cloud는 장기적인 최적화(Long-Term Optimization), AI 학습(Model Training), Enterprise Analytics를 담당한다. Hybrid Edge-Cloud 구조는 지연시간을 줄이면서도 높은 계산 능력을 제공한다.

플릿 오케스트레이션은 기업 시스템(Enterprise System)과도 긴밀하게 연결된다. MES(Manufacturing Execution System)는 생산 우선순위를 전달하고, WMS(Warehouse Management System)는 물류 작업을 생성하며, ERP(Enterprise Resource Planning)는 재고 이동을 관리하고, CRM(Customer Relationship Management)은 고객 요청을 전달한다. 이벤트 기반 구조는 로봇과 기업 시스템을 자연스럽게 통합한다.

소프트웨어 배포(Software Deployment)도 이벤트 기반으로 수행된다. SoftwareDeploymentAvailable 이벤트가 발생하면 로봇은 미션 완료 후 적절한 시점에 업데이트를 수행하고 DeploymentCompleted 이벤트를 생성한다. 이를 통해 플릿 전체를 중단하지 않고도 점진적인 소프트웨어 업그레이드가 가능하다.

사이버 보안(Cybersecurity) 역시 이벤트 중심으로 동작한다. Authentication Failure, Unauthorized Access, Firmware Integrity Violation, GPS Spoofing, Network Scanning 이벤트가 발생하면 AI 기반 보안 시스템이 즉시 대응하며, 문제가 있는 로봇을 격리(Isolation)하여 전체 플릿을 보호한다.

관찰 가능성(Observability)은 이벤트 기반 플릿 운영에서 필수 요소이다. Event Throughput, Mission Completion Rate, Robot Utilization, Charging Efficiency, Localization Accuracy, AI Prediction Accuracy, Fleet Productivity 등을 지속적으로 모니터링하며, Distributed Tracing을 통해 미션 생성부터 완료까지 전체 과정을 추적할 수 있다.

보안(Security)은 모든 이벤트에 적용된다. 로봇 인증(Authentication), 암호화(Encryption), 디지털 서명(Digital Signature), 접근 제어(Authorization), 감사 로그(Audit Log)를 적용하여 이벤트의 위변조를 방지한다. 특히 산업 자동화, 의료, 국방, 물류 분야에서는 매우 중요한 요소이다.

이벤트 기반 플릿 오케스트레이션에서도 여러 설계상의 어려움이 존재한다. 중앙 집중화가 지나치면 병목(Bottleneck)이 발생하고, 반대로 지나친 분산 구조는 전체 최적화를 어렵게 만든다. 또한 지나치게 많은 Telemetry 이벤트는 네트워크를 과부하시킬 수 있으며, 이벤트 스키마(Schema)가 일관되지 않으면 시스템 간 연동이 어려워진다.

대표적인 안티패턴(Anti-Pattern)은 지속적인 폴링(Polling)으로 로봇 상태를 조회하는 것, 내부 구현 중심 이벤트를 사용하는 것, 서비스 간 데이터베이스를 직접 공유하는 것, 모든 Telemetry를 동일한 중요도로 처리하는 것, 이벤트 우선순위를 무시하는 것, 메시지 브로커(Message Broker)에 비즈니스 로직을 구현하는 것이다.

결국 **이벤트 기반 플릿 오케스트레이션(Event-Driven Fleet Orchestration)** 은 **이벤트(Event)** 를 중심으로 다수의 자율주행 로봇을 협력적으로 운영하는 핵심 아키텍처이다. **Mission Management**, **Traffic Orchestration**, **Battery Optimization**, **Predictive Maintenance**, **AI Optimization**, **Digital Twin**, **Stream Processing**, **Edge-Cloud Orchestration**, **Enterprise Integration**, **Cybersecurity**, **Observability** 를 하나의 이벤트 기반 구조로 통합함으로써 물류(Logistics), 제조(Manufacturing), 병원(Hospital), 공항(Airport), 스마트시티(Smart City), 농업(Agriculture), 국방(Defense), 그리고 미래의 피지컬 AI(Physical AI) 환경에서 수백\~수천 대의 로봇을 높은 확장성(Scalability), 자율성(Autonomy), 복원력(Resilience), 안전성(Safety), 그리고 운영 효율성(Operational Efficiency)을 유지하면서 지능적으로 협업시키는 핵심 운영 플랫폼을 제공한다.

## 05.10 Testing and Debugging Event-Driven Systems

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

이벤트 기반 시스템(Event-Driven System)의 테스트(Test)와 디버깅(Debugging)은 현대 분산 소프트웨어에서 가장 어려운 분야 중 하나이다. 기존의 단일 애플리케이션(Monolithic Application)은 요청(Request)과 응답(Response)의 흐름이 비교적 명확하지만, 이벤트 기반 아키텍처(Event-Driven Architecture, EDA)는 수많은 서비스(Service)가 비동기(Asynchronous) 이벤트(Event)를 통해 독립적으로 동작한다. 따라서 전체 실행 흐름이 분산되고 예측하기 어려워지므로 기존 테스트 방법만으로는 충분하지 않다.

기존 소프트웨어 테스트는 함수(Function)나 API를 호출하고 결과를 확인하는 방식이었다. 그러나 이벤트 기반 시스템에서는 하나의 이벤트가 수십 개의 서비스에서 동시에 처리되고, 각 서비스가 다시 새로운 이벤트를 생성한다. 예를 들어 CustomerRegistered 이벤트 하나가 인증(Authentication), 사기 탐지(Fraud Detection), 추천 시스템(Recommendation), 알림(Notification), AI 학습, 디지털 트윈(Digital Twin), 분석(Analytics)을 동시에 실행할 수 있다. 따라서 개별 서비스가 아니라 전체 이벤트 흐름(Event Flow)을 검증해야 한다.

이벤트 기반 시스템 테스트의 목적은 단순히 하나의 서비스가 정상적으로 동작하는지를 확인하는 것이 아니라, 이벤트(Event)가 생성된 이후 모든 Producer, Broker, Consumer, Stream Processing, AI Pipeline, Database, Orchestrator가 올바르게 협력하여 기대한 비즈니스 결과(Business Outcome)를 만드는지를 검증하는 것이다.

가장 먼저 검증해야 하는 것은 이벤트 스키마(Event Schema)이다. 이벤트는 서비스 간의 계약(Contract)이므로 Field Name, Data Type, Optional Field, Version, Validation Rule, Business Meaning이 정확해야 한다. 스키마가 변경되면 수많은 Consumer가 동시에 영향을 받을 수 있으므로 이벤트 구조를 자동으로 검증하는 것이 매우 중요하다.

이벤트 스키마는 지속적으로 발전하기 때문에 버전 관리(Versioning) 테스트도 필수적이다. 새로운 필드가 추가되어도 기존 Consumer가 정상적으로 동작해야 하며, 이전 버전과의 호환성(Backward Compatibility)과 새로운 버전과의 호환성(Forward Compatibility)을 모두 검증해야 한다. Schema Registry를 이용하면 이러한 호환성을 자동으로 관리할 수 있다.

단위 테스트(Unit Test)는 이벤트 기반 시스템에서도 가장 기본이 되는 테스트이다. Event Handler, Filter, Transformation, Routing, Serialization, Validation, State Update와 같은 개별 기능은 외부 시스템 없이 Mock Event를 이용하여 독립적으로 검증한다. 이를 통해 비즈니스 로직의 정확성을 빠르게 확인할 수 있다.

Producer 테스트는 이벤트 생성이 올바른지 확인하는 과정이다. Event ID, Timestamp, Correlation ID, Schema Version, Payload, Priority 등이 정확하게 생성되는지 검증해야 하며, 중복 이벤트(Duplicate Event)나 불완전한 Payload가 생성되지 않아야 한다. Producer는 구현 세부사항이 아니라 실제 비즈니스 이벤트(Business Event)를 생성해야 한다.

Consumer 테스트는 이벤트를 수신한 서비스가 올바르게 처리하는지 검증한다. Consumer는 이벤트를 정상적으로 역직렬화(Deserialize)하고 Schema를 검증하며, 비정상 이벤트를 적절히 거부하고, 데이터베이스를 올바르게 갱신하며, 필요한 경우 새로운 이벤트를 생성해야 한다. 또한 동일한 이벤트가 여러 번 전달되어도 결과가 동일해야 한다.

계약 테스트(Contract Testing)는 Producer와 Consumer 사이의 약속(Contract)을 검증하는 방법이다. Producer는 약속된 이벤트 구조를 유지하고 Consumer는 해당 구조를 올바르게 처리해야 한다. Consumer-Driven Contract Testing을 적용하면 Producer가 변경되더라도 Consumer가 요구하는 인터페이스를 유지할 수 있어 독립적인 개발이 가능하다.

통합 테스트(Integration Test)는 여러 서비스가 실제 메시지 브로커(Message Broker)를 통해 함께 동작하는지를 검증한다. Kafka, RabbitMQ, NATS, MQTT, DDS 등을 통해 이벤트를 전달하고 Consumer가 이를 정상적으로 처리하는지를 확인한다. 이를 통해 실제 운영 환경과 유사한 통신을 검증할 수 있다.

종단 간 테스트(End-to-End Test)는 전체 업무 흐름을 검증한다. 고객 주문(Customer Order), 제조 공정(Manufacturing Process), 자율주행 로봇 미션(Robot Mission), 금융 거래(Transaction), 예지보전(Predictive Maintenance) 등이 이벤트를 통해 끝까지 정상적으로 수행되는지를 확인한다. 이는 가장 현실적인 테스트 방식이다.

이벤트 순서(Event Ordering)는 매우 중요한 테스트 항목이다. 금융 거래나 생산 공정은 이벤트 순서가 바뀌면 심각한 오류가 발생할 수 있지만, 분석(Analytics)이나 Notification은 순서가 크게 중요하지 않을 수도 있다. 따라서 어떤 이벤트는 순서를 반드시 유지해야 하고, 어떤 이벤트는 병렬 처리가 가능한지를 명확히 검증해야 한다.

전달 보장(Delivery Guarantee)도 반드시 테스트해야 한다. At-Least-Once, At-Most-Once, Exactly-Once 전달 방식에 따라 시스템 동작이 달라진다. 동일한 이벤트를 여러 번 보내더라도 결과가 한 번 처리한 것과 동일해야 하며(Idempotency), 이벤트가 누락되거나 중복되어도 전체 시스템의 일관성이 유지되어야 한다.

장애 주입(Fault Injection)은 이벤트 기반 시스템의 중요한 테스트 기법이다. Broker 장애, 네트워크(Network) 장애, Database 장애, AI 서비스 장애, Storage 장애 등을 의도적으로 발생시켜 Retry, Circuit Breaker, Dead Letter Queue(DLQ), Checkpoint Recovery가 정상적으로 동작하는지를 확인한다.

카오스 엔지니어링(Chaos Engineering)은 실제 운영 환경과 유사하게 무작위 장애(Random Failure)를 발생시키는 기법이다. Event Broker를 재시작하거나 Consumer를 강제로 종료하고, 네트워크를 분리하거나 AI 서비스를 중단시켜도 시스템이 자동으로 복구(Self-Recovery)되는지를 검증한다. 이를 통해 실제 운영 환경에서의 복원력을 높일 수 있다.

이벤트 재생(Event Replay)은 이벤트 기반 시스템의 가장 큰 장점 중 하나이다. Kafka와 같은 Event Streaming 플랫폼은 모든 이벤트를 저장하므로 동일한 이벤트를 반복 실행하여 새로운 알고리즘이나 AI 모델을 비교할 수 있다. Robot Telemetry, 금융 거래, 생산 이벤트를 Replay하여 동일한 조건에서 성능을 비교하고 문제를 분석할 수 있다.

시간 기반(Time-Dependent) 테스트도 매우 중요하다. 스트림 처리(Stream Processing)는 Event Time, Processing Time, Watermark, Window를 기반으로 계산하기 때문에 늦게 도착하는 이벤트(Late Event), 순서가 뒤바뀐 이벤트(Out-of-Order Event), 시간 동기화(Time Synchronization)를 모두 검증해야 한다.

상태 관리(State Management)는 이벤트 기반 시스템의 핵심이다. Fraud Detection은 고객의 과거 거래를 기억해야 하고, Robot Localization은 이전 위치를 유지해야 하며, Predictive Maintenance는 설비의 장기적인 상태를 관리해야 한다. 따라서 Checkpoint, Snapshot, Recovery가 정확히 동작하는지 반드시 테스트해야 한다.

동시성 테스트(Concurrency Testing)는 여러 Consumer가 동시에 이벤트를 처리할 때 발생하는 Race Condition, Deadlock, Resource Contention, Duplicate Processing 등을 검증한다. 대규모 로봇 플릿(Fleet), 금융 시스템, AI 플랫폼에서는 매우 중요한 테스트 항목이다.

부하 테스트(Load Testing)는 이벤트 처리량(Event Throughput)이 증가할 때 시스템이 어떻게 동작하는지를 측정한다. Event Throughput, Consumer Lag, CPU, GPU, Memory, Queue Depth, Network Bandwidth 등을 측정하여 시스템이 얼마나 확장 가능한지를 확인한다.

스트레스 테스트(Stress Testing)는 시스템의 한계를 찾는 과정이다. Broker, Stream Processor, AI Server, Database, GPU Server에 과도한 이벤트를 전달하여 언제 성능이 저하되고 어떤 방식으로 장애가 발생하는지를 분석한다. 이를 통해 실제 운영 한계를 미리 파악할 수 있다.

성능 테스트(Performance Benchmarking)는 이벤트가 생성되어 최종 결과가 나올 때까지의 전체 지연시간(Latency)을 측정한다. Event Publish, Broker, Consumer, AI Inference, Database Update, Digital Twin Update, Notification까지 전체 시간을 분석하여 병목(Bottleneck)을 찾는다.

보안 테스트(Security Testing)는 이벤트의 인증(Authentication), 권한(Authorization), 암호화(Encryption), 디지털 서명(Digital Signature), 무결성(Integrity), 접근 제어(Access Control)를 검증한다. 악성 이벤트(Malicious Event), Replay Attack, Message Tampering, 권한 상승(Privilege Escalation)이 시스템에 영향을 주지 않아야 한다.

AI가 포함된 이벤트 기반 시스템은 추가적인 검증이 필요하다. AI는 항상 동일한 결과를 출력하지 않으므로 Prediction Accuracy, Confidence, Model Drift, Inference Latency, Explainability 등을 지속적으로 평가해야 한다. 또한 AI Trigger, Model Selection, GPU Scheduling, AI Event Generation도 함께 테스트해야 한다.

자율주행 이동로봇(Autonomous Mobile Robot, AMR)은 이벤트 기반 테스트가 특히 중요하다. Camera, LiDAR, IMU, Localization, Planning, Navigation, Fleet Coordination, Safety Monitoring은 모두 이벤트 기반으로 동작한다. 따라서 단순한 소프트웨어 테스트뿐 아니라 SIL(Software-in-the-Loop), HIL(Hardware-in-the-Loop), 디지털 트윈(Digital Twin)을 이용한 실제 환경 검증이 필요하다.

디지털 트윈(Digital Twin)은 테스트 환경에서도 중요한 역할을 한다. 저장된 이벤트를 이용하여 실제 공장이나 물류센터를 가상으로 재현하고 새로운 AI 알고리즘이나 Fleet 정책을 반복적으로 검증할 수 있다. 운영 시스템을 중단하지 않고도 다양한 시나리오를 안전하게 시험할 수 있다는 장점이 있다.

관찰 가능성(Observability)은 이벤트 기반 시스템 디버깅의 핵심이다. 분산 추적(Distributed Tracing), 구조화 로그(Structured Logging), 메트릭(Metrics)을 이용하여 이벤트가 Producer에서 Broker를 거쳐 Consumer까지 이동하는 전체 과정을 추적한다. Correlation ID와 Causation ID는 하나의 이벤트가 여러 시스템으로 전파되는 전체 흐름을 연결하는 중요한 역할을 한다.

중앙 집중 로그(Centralized Logging)는 Producer, Broker, Consumer, AI Service, Database, Robot Controller, Cloud Infrastructure에서 생성되는 로그를 하나의 시스템에 모아 분석한다. Event ID, Robot ID, Mission ID, Timestamp, Trace ID를 함께 저장하면 장애 분석과 디버깅이 매우 쉬워진다.

메트릭(Metrics)은 시스템 상태를 지속적으로 측정한다. Event Throughput, Consumer Lag, Processing Latency, Checkpoint Success Rate, Dead Letter Queue, GPU Utilization, AI Accuracy, Robot Availability, Mission Completion Rate 등을 지속적으로 모니터링하여 이상 상태를 조기에 발견할 수 있다.

CI/CD(Continuous Integration/Continuous Deployment)에서는 모든 테스트가 자동으로 수행되어야 한다. Unit Test, Contract Test, Integration Test, Replay Test, Security Test, Performance Test, AI Regression Test를 자동으로 실행하여 문제가 없는 경우에만 새로운 버전을 운영 환경에 배포한다.

대표적인 안티패턴(Anti-Pattern)은 Unit Test만 수행하는 것, 중복 이벤트를 고려하지 않는 것, 정상 상황만 테스트하는 것, Replay 기능을 제공하지 않는 것, Schema Version을 관리하지 않는 것, Observability를 구축하지 않는 것, AI를 항상 결정적(Deterministic)이라고 가정하는 것, Correlation ID 없이 이벤트를 처리하는 것이다. 이러한 방식은 실제 운영에서 장애 원인을 찾기 어렵게 만든다.

결국 **이벤트 기반 시스템의 테스트와 디버깅(Testing and Debugging Event-Driven Systems)** 은 **자동화 테스트(Automated Testing)**, **Contract Testing**, **Replay**, **Fault Injection**, **Chaos Engineering**, **Distributed Tracing**, **Observability**, **AI Validation**, **Digital Twin**, **Performance Benchmarking**, **Security Testing** 을 통합적으로 수행해야 한다. 이러한 체계를 구축해야만 클라우드 네이티브(Cloud-Native), 이벤트 기반 아키텍처(Event-Driven Architecture), 자율주행 이동로봇(Autonomous Mobile Robot, AMR), 산업 자동화(Industrial Automation), 디지털 트윈(Digital Twin), 분산 AI(Distributed AI), 그리고 미래의 피지컬 AI(Physical AI) 플랫폼에서 높은 신뢰성(Reliability), 확장성(Scalability), 복원력(Resilience), 유지보수성(Maintainability), 그리고 안전성(Safety)을 지속적으로 확보할 수 있다.
