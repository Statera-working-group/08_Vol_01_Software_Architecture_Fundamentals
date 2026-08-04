**Volume 1 Software Architecture Fundamentals**

# 09. Scalable System Design

## 09.01 Scalability Design Principles: Vertical vs. Horizontal

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

확장성(Scalability)은 현대 소프트웨어 아키텍처(Software Architecture)에서 가장 중요한 품질 속성(Quality Attribute) 가운데 하나이다. 소프트웨어는 사용자(User), 데이터(Data), 서비스(Service), AI 모델(Model), 로봇(Robot), IoT(Internet of Things) 장치가 지속적으로 증가하는 환경에서도 성능(Performance), 안정성(Reliability), 운영 효율(Operation Efficiency)을 유지해야 한다. 따라서 확장성은 단순한 성능 향상이 아니라 미래의 성장에 대응할 수 있는 구조를 설계하는 핵심 원칙이다.

확장성은 성능(Performance)과 혼동되는 경우가 많지만 두 개념은 다르다. 성능은 현재의 작업 부하(Workload)에서 얼마나 빠르게 처리하는지를 의미하며, 응답 시간(Response Time), 처리량(Throughput), CPU 사용률 등을 평가한다. 반면 확장성은 사용자가 증가하거나 데이터가 늘어날 때에도 이러한 성능을 지속적으로 유지할 수 있는 능력을 의미한다. 즉, 확장성은 변화하는 환경에서도 품질을 유지하는 능력이다.

현대 시스템에서는 다양한 형태의 확장성이 요구된다. 사용자 확장성(User Scalability)은 동시 접속자의 증가를 의미하며, 데이터 확장성(Data Scalability)은 데이터베이스(Database)와 파일(File)의 증가를 처리하는 능력이다. 또한 지역 확장성(Geographic Scalability), 조직 확장성(Organizational Scalability), 기능 확장성(Function Scalability), 인프라 확장성(Infrastructure Scalability) 등 여러 측면에서 시스템은 지속적으로 성장할 수 있어야 한다.

확장 전략은 크게 수직 확장(Vertical Scaling, Scale-Up)과 수평 확장(Horizontal Scaling, Scale-Out)으로 구분된다. 수직 확장은 하나의 서버(Server)의 성능을 높이는 방식이며, 수평 확장은 여러 대의 서버를 추가하여 전체 시스템의 처리 능력을 높이는 방식이다. 현대의 대규모 시스템은 두 방식을 적절히 조합하여 사용하는 경우가 많다.

수직 확장(Vertical Scaling)은 기존 서버의 하드웨어를 업그레이드하여 성능을 높이는 방식이다. CPU 코어(Core)를 늘리고, 메모리(RAM)를 확장하며, SSD, GPU, 네트워크(Network) 대역폭 등을 향상시켜 하나의 서버가 더 많은 작업을 수행하도록 만든다. 기존 애플리케이션 구조를 크게 변경하지 않아도 되므로 비교적 구현이 단순하다는 장점이 있다.

수직 확장의 가장 큰 장점은 아키텍처가 단순하다는 것이다. 하나의 서버에서 대부분의 처리가 이루어지므로 데이터 일관성(Data Consistency)을 유지하기 쉽고, 운영(Operation), 백업(Backup), 보안(Security), 모니터링(Monitoring)도 상대적으로 단순하다. ERP(Enterprise Resource Planning), 대규모 데이터베이스, 과학 계산(Scientific Computing), AI 학습(Training) 서버 등에서 널리 사용된다.

데이터베이스(Database)는 대표적인 수직 확장 사례이다. 메모리를 늘려 캐시(Cache)를 확대하고, CPU를 증가시켜 병렬 쿼리(Query)를 처리하며, SSD를 이용하여 입출력(I/O) 속도를 향상시킨다. 특히 높은 트랜잭션(Transaction) 일관성이 필요한 금융 시스템이나 기업용 데이터베이스에서는 수직 확장이 여전히 매우 중요한 전략이다.

AI 학습(Training) 환경도 수직 확장을 적극 활용한다. 여러 개의 GPU를 하나의 서버에 장착하고, GPU 간 고속 인터커넥트(Interconnect)를 이용하여 대규모 딥러닝(Deep Learning) 모델을 학습한다. GPU 메모리, Tensor 연산 성능, CPU 처리 능력을 동시에 향상시켜 복잡한 AI 모델을 효율적으로 학습할 수 있다.

로봇 개발에서도 수직 확장은 중요한 역할을 한다. 디지털 트윈(Digital Twin), 강화학습(Reinforcement Learning), 센서 융합(Sensor Fusion), 자율주행 시뮬레이션(Simulation) 등은 매우 높은 GPU 성능을 요구하므로 하나의 고성능 워크스테이션(Workstation)이나 GPU 서버를 이용하여 개발하는 경우가 많다.

그러나 수직 확장은 명확한 한계를 가진다. 서버는 CPU 소켓(Socket), 메모리 슬롯(Slot), GPU 개수, 전력(Power), 냉각(Cooling) 등의 물리적인 제한이 존재한다. 또한 고성능 장비일수록 비용이 급격히 증가하며, 업그레이드를 위해 시스템을 중단해야 하는 경우도 많다. 따라서 무한정 확장할 수는 없다.

또한 수직 확장은 단일 장애점(Single Point of Failure)을 만들 가능성이 있다. 하나의 서버가 고장 나면 전체 서비스가 중단될 수 있기 때문이다. 이중화(Redundancy)를 적용할 수는 있지만 기본적으로 중앙 집중형(Centralized) 구조라는 한계를 가진다.

수평 확장(Horizontal Scaling)은 여러 대의 서버를 함께 사용하는 방식이다. 기존 서버를 교체하는 것이 아니라 새로운 서버를 계속 추가하여 처리 능력을 증가시킨다. 클라우드(Cloud), 쿠버네티스(Kubernetes), 마이크로서비스(Microservices)는 모두 이러한 수평 확장을 기본 개념으로 사용한다.

수평 확장의 가장 큰 장점은 거의 무한에 가까운 확장이 가능하다는 것이다. 사용자가 증가하면 서버를 추가하고, 트래픽(Traffic)이 감소하면 서버를 제거하여 운영 비용(Cost)을 절감할 수 있다. 이러한 자동 확장(Auto Scaling)은 현대 클라우드 시스템의 핵심 기능이다.

수평 확장은 높은 가용성(Availability)과 장애 허용(Fault Tolerance)도 제공한다. 여러 대의 서버가 동시에 동작하므로 한 대가 고장 나더라도 로드 밸런서(Load Balancer)가 자동으로 다른 서버로 요청을 전달한다. 따라서 서비스 중단 없이 안정적으로 운영할 수 있다.

클라우드 네이티브(Cloud-Native)는 대표적인 수평 확장 구조이다. 컨테이너(Container)를 여러 개 실행하고, 쿠버네티스가 CPU 사용률, 메모리 사용량, 요청(Request) 수 등을 분석하여 자동으로 서버 수를 조절한다. 이를 통해 운영 비용과 성능을 동시에 최적화할 수 있다.

마이크로서비스(Microservices)는 수평 확장을 더욱 효율적으로 만든다. 인증(Authentication), 결제(Payment), 검색(Search), 추천(Recommendation), AI 추론(Inference) 등 각각의 서비스가 독립적으로 확장될 수 있으므로 필요한 서비스만 증설하면 된다. 이는 전체 시스템을 복제하는 것보다 훨씬 효율적이다.

수평 확장에서는 데이터 관리(Data Management)가 매우 중요하다. 데이터베이스 복제(Database Replication), 샤딩(Sharding), 파티셔닝(Partitioning), 분산 캐시(Distributed Cache) 등을 이용하여 데이터를 여러 서버에 분산 저장하고 동시에 처리할 수 있도록 설계한다.

로드 밸런서(Load Balancer)는 수평 확장의 핵심 구성 요소이다. 들어오는 요청을 여러 서버에 균등하게 분배하여 특정 서버에 부하가 집중되지 않도록 한다. 라운드 로빈(Round Robin), 최소 연결 수(Least Connection), 응답 시간(Response Time) 기반 분산 등 다양한 알고리즘이 사용된다.

상태 관리(State Management)는 수평 확장의 가장 어려운 문제 중 하나이다. 여러 서버가 동시에 동작하므로 사용자 세션(Session), 트랜잭션, AI 추론 상태 등을 공유해야 한다. 이를 해결하기 위해 상태 비저장(Stateless) 설계를 지향하며, 필요한 상태는 Redis와 같은 외부 저장소(External Storage)에 관리한다.

서비스 간 통신(Communication)은 수평 확장에서 중요한 설계 요소이다. 동기 통신(Synchronous Communication)은 구현이 쉽지만 서비스 간 결합도가 높아진다. 반면 비동기 메시징(Asynchronous Messaging)은 이벤트(Event) 기반으로 서비스를 연결하여 독립적인 확장성과 높은 안정성을 제공한다.

AI 플랫폼은 수직 확장과 수평 확장을 모두 활용한다. 모델 학습(Training)은 GPU 서버를 이용한 수직 확장이 일반적이며, 모델 추론(Inference)은 여러 개의 추론 서버를 이용하는 수평 확장이 일반적이다. 따라서 AI 시스템은 하이브리드(Hybrid) 확장 구조를 갖는 경우가 많다.

엣지 AI(Edge AI)는 계층형(Hierarchical) 확장 구조를 사용한다. 센서(Sensor), 로봇(Robot), 차량(Vehicle)에서는 로컬(Local)에서 추론을 수행하고, 클라우드에서는 모델 관리(Model Management), 재학습(Retraining), 분석(Analytics)을 수행한다. 이를 통해 응답 속도와 운영 효율을 동시에 확보할 수 있다.

자율주행 로봇과 물리 AI(Physical AI)는 일반적인 클라우드 시스템보다 더욱 복잡한 확장 전략을 요구한다. 로봇 내부에서는 실시간 제어(Real-Time Control), 위치 추정(Localization), 인식(Perception), 경로 계획(Path Planning)을 로컬에서 수행해야 하며, 클라우드에서는 플릿 관리(Fleet Management), AI 모델 업데이트(Update), 디지털 트윈(Digital Twin), 운영 분석(Operation Analytics)을 담당한다.

수백 대 이상의 로봇을 운영하는 플릿(Fleet) 환경에서는 중앙 관리 시스템이 작업(Task Allocation), 충전(Charging), 소프트웨어 업데이트, 운영 모니터링(Monitoring), 원격 진단(Remote Diagnostics)을 수행한다. 이러한 시스템은 수평 확장을 통해 로봇 수가 증가해도 지속적으로 운영될 수 있어야 한다.

확장성은 유지보수성(Maintainability)에도 영향을 미친다. 지나치게 큰 단일 시스템(Monolithic System)은 시간이 지날수록 수정이 어려워지고 기술 부채(Technical Debt)가 증가한다. 반면 지나치게 분산된 시스템은 운영 복잡도가 증가한다. 따라서 적절한 균형(Balance)을 유지하는 것이 중요하다.

경제성(Economics) 역시 확장 전략 선택의 중요한 요소이다. 초기에는 수직 확장이 저렴할 수 있지만 일정 수준을 넘으면 고성능 서버 비용이 급격히 증가한다. 반대로 수평 확장은 초기 설계는 복잡하지만 클라우드 기반의 사용량(Pay-as-you-Go) 과금 모델을 통해 장기적으로 비용을 절감할 수 있다.

관측성(Observability)은 확장성이 증가할수록 더욱 중요해진다. 분산 시스템에서는 로그(Log), 메트릭(Metrics), 분산 추적(Distributed Trace), AI 모니터링, 로봇 텔레메트리(Telemetry), 네트워크 상태 등을 통합적으로 수집하여 시스템 전체를 실시간으로 분석해야 한다.

보안(Security) 역시 확장성과 함께 발전해야 한다. 서버가 증가하면 API, 네트워크, 컨테이너, AI 서비스, 로봇 장치 등 공격 대상도 증가한다. 따라서 제로 트러스트(Zero Trust), 인증(Authentication), 암호화(Encryption), 인증서 관리(Certificate Management), 취약점 분석(Vulnerability Assessment) 등을 함께 적용해야 한다.

현대 소프트웨어에서는 확장성을 개발 이후에 추가하는 것이 아니라 초기 설계 단계부터 고려해야 한다. 이미 완성된 시스템에 확장성을 추가하려면 데이터 구조, 통신 방식, 배포 구조, 보안 모델 등을 모두 수정해야 하는 경우가 많다. 따라서 초기 아키텍처 단계에서 확장 전략을 설계하는 것이 장기적인 비용 절감에 매우 중요하다.

수직 확장과 수평 확장은 서로 경쟁 관계가 아니라 상호 보완적인 기술이다. 실제 대규모 시스템은 데이터베이스는 수직 확장을 적용하고, 애플리케이션 서버는 수평 확장을 적용하는 하이브리드(Hybrid) 구조를 가장 많이 사용한다. AI 시스템 역시 학습은 수직 확장, 추론은 수평 확장을 적용하며, 자율주행 로봇은 로컬 컴퓨터의 수직 확장과 클라우드의 수평 확장을 함께 활용한다.

결론적으로 확장성 설계 원칙(Scalability Design Principles)은 현대 소프트웨어 아키텍처의 핵심 요소이다. 수직 확장(Vertical Scaling)은 단순한 구조와 높은 계산 성능을 제공하며, 수평 확장(Horizontal Scaling)은 무한에 가까운 확장성과 높은 가용성을 제공한다. 클라우드, AI 플랫폼, 자율주행 로봇, 산업 자동화, 물리 AI 시스템에서는 두 방식을 적절히 결합한 하이브리드 확장 전략이 가장 효과적이다. 이러한 확장성 설계를 통해 시스템은 성능, 안정성, 운영 효율, 비즈니스 민첩성을 장기간 유지하면서 지속적으로 성장할 수 있다.

## 09.02 Load Balancing Strategies and Algorithms

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

부하 분산(Load Balancing)은 현대 분산 소프트웨어 시스템(Distributed Software Systems)의 핵심 아키텍처 기술 가운데 하나이다. 클라우드(Cloud), 마이크로서비스(Microservices), 인공지능(AI), 자율주행 로봇(Autonomous Robot), IoT(Internet of Things), 산업 자동화(Industrial Automation), 금융(Finance), 의료(Healthcare) 시스템은 수많은 사용자와 요청(Request)을 동시에 처리해야 한다. 이러한 환경에서 특정 서버(Server)에만 부하가 집중되면 성능 저하와 장애가 발생하므로, 요청을 여러 서버에 효율적으로 분산하는 기술이 반드시 필요하다.

부하 분산은 단순히 네트워크(Network) 트래픽을 나누는 기술이 아니다. 시스템 전체의 성능(Performance), 확장성(Scalability), 가용성(Availability), 신뢰성(Reliability), 운영 효율(Operation Efficiency)을 높이기 위한 핵심 아키텍처 전략이다. 들어오는 요청을 현재 가장 적절한 서버나 서비스로 전달하여 특정 자원(Resource)에 과부하가 발생하지 않도록 하고 전체 시스템의 균형을 유지하는 것이 목적이다.

부하 분산의 핵심 목표는 개별 서버의 성능을 최대화하는 것이 아니라 전체 시스템의 효율을 극대화하는 것이다. 이를 통해 응답 시간(Response Time)을 최소화하고 처리량(Throughput)을 향상시키며, 자원 활용률(Resource Utilization)을 높이고 장애 발생 시에도 서비스를 지속적으로 제공할 수 있도록 한다.

현대 시스템은 매우 동적인 작업 부하(Workload)를 가진다. 시간대에 따라 사용자 수가 달라지고, 이벤트나 할인 행사에서는 트래픽이 폭증하며, AI 추론(Inference)이나 로봇 플릿(Fleet)의 통신량도 지속적으로 변화한다. 따라서 고정된 방식으로 요청을 분배하는 것만으로는 충분하지 않으며, 실시간 운영 상황에 따라 유연하게 요청을 분산하는 기술이 필요하다.

부하 분산은 다양한 계층(Layer)에서 수행된다. 네트워크 계층(Network Layer)은 연결(Connection)을 여러 서버로 분산하며, 전송 계층(Transport Layer)은 TCP와 UDP 세션(Session)을 관리한다. 애플리케이션 계층(Application Layer)은 URL, API(Application Programming Interface), 쿠키(Cookie), 사용자 인증(Authentication) 정보를 분석하여 적절한 서버를 선택한다. 또한 데이터베이스(Database), 메시지 큐(Message Queue), GPU 클러스터(GPU Cluster), 엣지 컴퓨팅(Edge Computing)에서도 각각의 부하 분산 기술이 사용된다.

부하 분산 전략은 크게 정적 부하 분산(Static Load Balancing)과 동적 부하 분산(Dynamic Load Balancing)으로 구분된다. 정적 방식은 미리 정의된 규칙에 따라 요청을 분산하며, 현재 시스템 상태를 고려하지 않는다. 반면 동적 방식은 CPU, 메모리(Memory), 응답 시간(Response Time), 연결 수(Connection Count) 등을 지속적으로 모니터링하여 실시간으로 가장 적합한 서버를 선택한다.

가장 대표적인 정적 알고리즘은 라운드 로빈(Round Robin)이다. 서버들을 순서대로 하나씩 선택하여 요청을 분배하는 방식이다. 모든 서버가 동일한 성능을 가지고 있고 요청의 처리 시간이 비슷할 경우 매우 효율적으로 동작하며 구현도 단순하다.

가중치 라운드 로빈(Weighted Round Robin)은 서버마다 서로 다른 성능을 고려한 방식이다. CPU 성능이 높거나 GPU가 장착된 서버에는 더 많은 요청을 보내고, 상대적으로 성능이 낮은 서버에는 적은 요청을 전달한다. 이 방식은 성능이 서로 다른 서버가 혼합된 환경에서 자원 활용률을 크게 향상시킨다.

랜덤(Random) 알고리즘은 서버를 무작위(Randomly)로 선택한다. 매우 단순한 방식이지만 요청이 충분히 많아지면 자연스럽게 균등한 분산 효과를 얻을 수 있다. 또한 예측 가능한 패턴이 적어 일부 보안(Security) 측면에서도 장점을 가진다.

해시(Hash) 기반 부하 분산은 사용자 IP(IP Address), 세션(Session), 쿠키(Cookie), 사용자 ID(User ID) 등을 해시 함수(Hash Function)에 입력하여 항상 동일한 서버를 선택하는 방식이다. 동일한 사용자는 지속적으로 같은 서버를 사용하므로 세션 유지(Session Persistence)가 필요한 웹 서비스(Web Service)에서 많이 활용된다.

정적 알고리즘은 단순하고 빠르지만 현재 서버의 상태를 고려하지 못하는 한계가 있다. 특정 서버가 과부하 상태이거나 장애가 발생해도 동일한 규칙으로 요청을 전달하기 때문에 비효율이 발생할 수 있다. 이러한 문제를 해결하기 위해 동적 부하 분산 알고리즘이 사용된다.

가장 널리 사용되는 동적 알고리즘은 최소 연결 수(Least Connections) 방식이다. 각 서버가 현재 처리 중인 연결(Connection)의 개수를 확인하여 가장 적은 연결을 가진 서버로 새로운 요청을 전달한다. 요청 처리 시간이 일정하지 않은 시스템에서 매우 효과적으로 동작한다.

가중치 최소 연결 수(Weighted Least Connections)는 서버 성능과 현재 연결 수를 동시에 고려한다. 성능이 높은 서버는 더 많은 연결을 허용하고, 성능이 낮은 서버는 적은 연결만 처리하도록 하여 전체 시스템의 균형을 유지한다.

최소 응답 시간(Least Response Time) 알고리즘은 연결 수보다 실제 응답 시간을 기준으로 서버를 선택한다. 응답 시간이 가장 빠른 서버를 우선적으로 사용하기 때문에 사용자 경험(User Experience)을 향상시키는 데 효과적이다. CPU 사용률, 메모리, 디스크(Disk), 네트워크(Network) 상태 등을 종합적으로 반영할 수 있다.

자원 기반(Resource-Based) 부하 분산은 CPU 사용률, 메모리 사용량, GPU 사용률, 디스크 입출력(I/O), 네트워크 대역폭(Bandwidth), 온도(Thermal) 등을 지속적으로 분석하여 가장 여유 있는 서버를 선택한다. AI 추론 서버나 GPU 클러스터에서는 GPU 메모리와 GPU 사용률이 가장 중요한 선택 기준이 된다.

최근에는 적응형 부하 분산(Adaptive Load Balancing)이 등장하고 있다. 머신러닝(Machine Learning), 강화학습(Reinforcement Learning), 예측 분석(Predictive Analytics)을 이용하여 미래의 트래픽을 예측하고 미리 서버를 준비한다. 단순히 현재 상태에 반응하는 것이 아니라 향후 부하를 예측하여 선제적으로 대응하는 방식이다.

애플리케이션 인식(Application-Aware) 부하 분산은 요청의 내용을 분석하여 적합한 서버를 선택한다. 로그인 요청(Authentication), AI 추론(Inference), 이미지 처리(Image Processing), 데이터 분석(Data Analytics)은 필요한 자원이 다르므로 각각의 요청을 가장 적합한 서버로 전달하여 효율을 높인다.

지역 기반(Geographic) 부하 분산은 사용자의 위치(Location)에 따라 가장 가까운 데이터센터(Data Center)로 요청을 전달한다. DNS(Domain Name System), 애니캐스트(Anycast), CDN(Content Delivery Network)을 이용하여 네트워크 지연(Latency)을 최소화하고 글로벌(Global) 서비스를 효율적으로 운영한다.

클라우드 컴퓨팅(Cloud Computing)은 부하 분산 기술을 크게 발전시켰다. 오토 스케일링(Auto Scaling)은 CPU 사용률, 메모리, 요청 수(Request Count), 큐(Queue) 길이 등을 분석하여 서버를 자동으로 추가하거나 제거한다. 이를 통해 성능과 운영 비용(Cost)을 동시에 최적화할 수 있다.

쿠버네티스(Kubernetes)는 서비스(Service), 인그레스(Ingress), 서비스 메시(Service Mesh)를 이용하여 컨테이너(Container)에 대한 부하 분산을 자동으로 수행한다. 컨테이너가 생성되거나 삭제될 때도 자동으로 새로운 서버 목록을 반영하여 서비스 중단 없이 요청을 처리한다.

서비스 메시(Service Mesh)는 단순한 부하 분산을 넘어 서비스 간 통신 전체를 관리한다. 재시도(Retry), 서킷 브레이커(Circuit Breaker), 트래픽 제어(Traffic Control), 관측성(Observability), 암호화(Encryption), 카나리 배포(Canary Deployment) 등을 함께 제공하여 현대 마이크로서비스의 핵심 기술이 되었다.

AI 플랫폼에서는 일반적인 웹 서비스보다 더욱 복잡한 부하 분산이 필요하다. 이미지 인식(Image Recognition), 자연어 처리(NLP, Natural Language Processing), 음성 인식(Speech Recognition), 생성형 AI(Generative AI)는 각각 필요한 GPU 자원이 다르다. 따라서 GPU 사용률, 메모리, 모델(Model) 위치 등을 고려하여 요청을 적절한 서버에 분배한다.

AI 학습(Training)에서도 부하 분산은 중요하다. 여러 GPU 서버가 동시에 모델을 학습하므로 그래디언트(Gradient), 모델 파라미터(Parameter), 데이터셋(Dataset)을 효율적으로 분산해야 한다. 계산 부하뿐 아니라 GPU 간 통신 부하까지 함께 최적화하는 것이 핵심이다.

자율주행 로봇과 물리 AI(Physical AI)는 독특한 부하 분산 구조를 가진다. 로봇 내부에서는 위치 추정(Localization), 인식(Perception), 경로 계획(Path Planning), 모터 제어(Control)를 로컬(Local)에서 수행해야 하며, 클라우드에서는 AI 모델 업데이트(Update), 플릿 관리(Fleet Management), 디지털 트윈(Digital Twin), 운영 분석(Operation Analytics)을 담당한다.

플릿 관리(Fleet Management)는 수백 대 이상의 로봇을 동시에 관리해야 한다. 작업 할당(Task Allocation), 충전 관리(Charging), 교통 제어(Traffic Management), 원격 업데이트(Remote Update), 텔레메트리(Telemetry) 수집 등을 여러 서버에 분산하여 처리함으로써 대규모 로봇 시스템도 안정적으로 운영할 수 있다.

장애 허용(Fault Tolerance)은 부하 분산의 가장 큰 장점 가운데 하나이다. 헬스 체크(Health Check)를 통해 서버의 상태를 지속적으로 확인하고 장애가 발생한 서버는 자동으로 제외한다. 이후 복구가 완료되면 다시 서비스에 포함시켜 운영 중단 없이 안정적인 서비스를 제공한다.

세션 지속성(Session Persistence)은 일부 시스템에서 중요한 기능이다. 로그인 상태, 장바구니(Shopping Cart), AI 대화(Context), 장시간 수행되는 작업(Long-running Task)은 동일한 서버에서 처리되어야 한다. 이를 위해 쿠키(Cookie), 세션 ID(Session ID), IP 주소 등을 이용한 스티키 세션(Sticky Session)을 사용한다. 그러나 현대 시스템은 상태 비저장(Stateless) 구조를 지향하여 이러한 의존성을 줄이고 있다.

부하 분산은 소프트웨어 배포(Deployment)에도 활용된다. 롤링 업데이트(Rolling Update), 블루-그린 배포(Blue-Green Deployment), 카나리 배포(Canary Release), A/B 테스트(AB Testing)에서는 새로운 버전으로 일부 사용자만 우선 연결하고 문제가 없으면 점진적으로 전체 사용자에게 확장한다.

관측성(Observability)은 효과적인 부하 분산을 위해 필수적이다. 응답 시간, 처리량, CPU, GPU, 메모리, 네트워크, AI 추론 속도, 로봇 텔레메트리 등을 지속적으로 모니터링하여 병목(Bottleneck)을 조기에 발견하고 부하 분산 정책을 지속적으로 개선한다.

보안(Security) 기능도 현대 부하 분산 장비에 통합되고 있다. DDoS(Distributed Denial of Service) 방어, 웹 애플리케이션 방화벽(WAF, Web Application Firewall), TLS 종료(TLS Termination), API Gateway, 접근 제어(Access Control), 속도 제한(Rate Limiting), 제로 트러스트(Zero Trust) 등이 함께 제공되어 부하 분산기가 보안 플랫폼(Security Platform)의 역할도 수행한다.

경제성(Economics)은 부하 분산 설계에서 매우 중요한 요소이다. 효율적인 부하 분산은 서버 활용률을 높여 불필요한 하드웨어 투자와 클라우드 비용을 줄일 수 있다. 특히 사용량 기반(Pay-as-you-Go) 클라우드 환경에서는 자동 확장과 부하 분산을 함께 적용하여 비용을 크게 절감할 수 있다.

모든 환경에서 가장 우수한 단일 알고리즘은 존재하지 않는다. 라운드 로빈은 단순성과 예측 가능성이 뛰어나고, 최소 연결 수는 세션 길이가 다양한 환경에 적합하며, 최소 응답 시간은 사용자 경험을 향상시킨다. 자원 기반 알고리즘은 GPU 클러스터에 적합하고, AI 기반 적응형 알고리즘은 미래의 부하까지 예측할 수 있다. 따라서 시스템의 특성에 맞는 알고리즘을 선택하거나 여러 알고리즘을 조합하는 것이 가장 효과적이다.

결론적으로 부하 분산 전략과 알고리즘(Load Balancing Strategies and Algorithms)은 현대 분산 시스템의 핵심 기술이다. 요청을 여러 서버, 데이터베이스, GPU, AI 플랫폼, 엣지 컴퓨팅, 자율주행 로봇 플릿에 지능적으로 분산함으로써 높은 가용성(Availability), 확장성(Scalability), 성능(Performance), 운영 안정성(Operational Resilience), 비용 효율성(Cost Efficiency)을 동시에 달성할 수 있다. 클라우드, AI, 마이크로서비스, 자율주행 로봇, 물리 AI 시대가 발전할수록 부하 분산 기술은 더욱 지능적이고 자동화된 형태로 발전하며 현대 소프트웨어 아키텍처의 핵심 구성 요소로 자리 잡게 될 것이다.

## 09.03 Caching Architecture: CDN / Distributed Cache / Local Cache

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

캐싱 아키텍처(Caching Architecture)는 현대 소프트웨어 시스템에서 가장 중요한 성능 최적화 기술 가운데 하나이다. 클라우드(Cloud), 마이크로서비스(Microservices), 인공지능(AI), 자율주행 로봇(Autonomous Robot), IoT(Internet of Things), 산업 자동화(Industrial Automation), 금융(Finance), 의료(Healthcare) 시스템은 동일한 데이터를 반복적으로 조회하는 경우가 매우 많다. 모든 요청을 데이터베이스(Database)나 원본 서버(Origin Server)에서 처리하면 응답 시간이 증가하고 서버 부하가 급격히 높아진다. 캐시는 이러한 문제를 해결하기 위해 자주 사용하는 데이터를 빠른 저장소(Fast Storage)에 임시 저장하여 즉시 제공하는 기술이다.

캐싱의 목적은 단순히 응답 속도를 높이는 것이 아니다. 데이터베이스 접근을 줄이고, CPU와 GPU의 계산 부하를 감소시키며, 네트워크(Network) 사용량을 줄이고, 서버(Server)의 처리 능력을 향상시키며, 전체 시스템의 운영 비용(Operation Cost)을 절감하는 것이 핵심 목표이다. 따라서 캐시는 성능(Performance), 확장성(Scalability), 가용성(Availability), 신뢰성(Reliability), 운영 효율(Operation Efficiency)을 동시에 향상시키는 핵심 아키텍처 기술이다.

캐싱은 동일하거나 유사한 데이터가 반복적으로 사용된다는 특성을 이용한다. 사용자 정보(User Profile), 상품 정보(Product Catalog), 설정(Configuration), 인증 토큰(Authentication Token), AI 모델(Model), 지도(Map), 센서 보정값(Sensor Calibration), 디지털 트윈(Digital Twin), 추천 결과(Recommendation), 환경 데이터(Environment Data) 등은 짧은 시간 동안 여러 번 요청되는 경우가 많다. 이러한 데이터를 메모리(Memory)와 같은 빠른 저장소에 보관하면 원본 데이터를 반복적으로 조회할 필요가 없어진다.

현대 시스템에서는 단일 캐시가 아니라 여러 계층(Cache Layer)을 동시에 사용한다. CPU 캐시(CPU Cache)는 프로세서 내부에서 동작하며, 메모리 캐시(In-Memory Cache)는 애플리케이션 성능을 높인다. 로컬 캐시(Local Cache)는 프로그램 내부에서 데이터를 저장하고, 분산 캐시(Distributed Cache)는 여러 서버가 동일한 캐시를 공유한다. CDN(Content Delivery Network)은 전 세계 사용자에게 콘텐츠를 빠르게 제공하며, 브라우저 캐시(Browser Cache), 데이터베이스 캐시(Database Cache), AI 캐시(AI Cache), 엣지 캐시(Edge Cache) 등 다양한 형태가 함께 사용된다.

캐시는 기본적으로 몇 가지 핵심 동작 과정을 가진다. 먼저 캐시 조회(Cache Lookup)를 수행하여 데이터가 존재하는지 확인한다. 데이터가 존재하면 캐시 히트(Cache Hit)가 발생하여 즉시 반환된다. 데이터가 없으면 캐시 미스(Cache Miss)가 발생하며 원본 데이터베이스나 서버에서 데이터를 가져온 후 캐시에 저장한다. 이후 동일한 요청은 캐시에서 바로 처리된다. 저장 공간이 부족하면 캐시 제거(Cache Eviction)가 수행되며 오래된 데이터를 삭제한다.

캐시의 효율성은 캐시 히트율(Cache Hit Ratio)로 평가된다. 히트율이 높을수록 데이터베이스 접근이 줄어들고 전체 시스템의 성능이 향상된다. 그러나 히트율만 높다고 좋은 캐시는 아니다. 데이터 최신성(Freshness), 일관성(Consistency), 메모리 사용량(Memory Usage), 갱신 비용(Update Cost), 운영 복잡도(Operation Complexity)를 함께 고려해야 한다.

로컬 캐시(Local Cache)는 가장 단순하면서도 가장 빠른 캐시이다. 애플리케이션 내부 메모리에 데이터를 저장하기 때문에 네트워크 통신이 필요하지 않으며 매우 낮은 지연 시간(Low Latency)을 제공한다. 설정 정보(Configuration), 상수(Constant), 룩업 테이블(Lookup Table), AI 전처리 값(Preprocessing Data), 센서 보정값, 사용자 세션(Session) 등이 대표적인 저장 대상이다.

하지만 로컬 캐시는 여러 서버 간 데이터를 공유할 수 없다는 한계를 가진다. 클라우드 환경에서는 동일한 애플리케이션이 여러 개의 인스턴스(Instance)로 실행되는데 각각 독립적인 캐시를 유지한다. 따라서 하나의 서버에서 데이터가 변경되어도 다른 서버는 이를 알지 못해 데이터 불일치(Data Inconsistency)가 발생할 수 있다.

이러한 문제를 해결하기 위해 분산 캐시(Distributed Cache)가 사용된다. Redis, Memcached와 같은 시스템은 여러 서버가 하나의 캐시를 공유하도록 구성한다. 모든 애플리케이션 서버는 동일한 캐시를 사용하므로 데이터 일관성을 유지하기 쉽고 데이터베이스의 부하를 크게 줄일 수 있다.

분산 캐시는 확장성도 매우 뛰어나다. 데이터를 여러 캐시 서버에 분산 저장(Partitioning)하여 메모리 용량을 확장할 수 있으며, 새로운 서버를 추가하면 캐시 용량도 함께 증가한다. 이를 통해 대규모 클라우드 서비스에서도 높은 성능을 유지할 수 있다.

분산 캐시에서는 복제(Replication) 기능도 중요하다. 동일한 데이터를 여러 서버에 저장하여 하나의 서버가 장애를 일으켜도 다른 서버가 서비스를 계속 제공할 수 있도록 한다. 장애 조치(Failover), 클러스터(Cluster), 헬스 체크(Health Check)를 함께 적용하면 매우 높은 가용성을 확보할 수 있다.

캐시 일관성(Cache Consistency)은 가장 어려운 문제 가운데 하나이다. 데이터베이스의 내용이 변경되면 캐시도 함께 변경되어야 한다. 즉시 모든 캐시를 갱신하는 강한 일관성(Strong Consistency)은 정확하지만 성능이 떨어질 수 있으며, 시간이 지나면서 점진적으로 동기화하는 최종 일관성(Eventual Consistency)은 성능은 좋지만 잠시 오래된 데이터를 제공할 수 있다.

캐시 무효화(Cache Invalidation)는 "컴퓨터 과학에서 가장 어려운 문제 중 하나"라고 불릴 정도로 중요하다. 데이터가 변경되면 기존 캐시를 제거하거나 갱신해야 한다. TTL(Time-To-Live)은 일정 시간이 지나면 자동으로 삭제하며, 이벤트(Event) 기반 무효화는 데이터베이스 변경 이벤트가 발생하면 즉시 캐시를 삭제한다. 명시적 무효화(Explicit Invalidation)는 애플리케이션이 직접 캐시를 갱신하는 방식이다.

읽기 전용 캐시(Read-Through Cache)는 캐시에 데이터가 없으면 캐시가 자동으로 데이터베이스를 조회하여 데이터를 저장한다. 애플리케이션은 캐시만 호출하면 되므로 구현이 단순해진다. 반면 캐시 어사이드(Cache-Aside)는 애플리케이션이 직접 캐시를 조회하고 데이터베이스 접근 여부를 결정한다. 현재 가장 널리 사용되는 캐시 구조이다.

쓰기 방식에도 여러 전략이 존재한다. Write-Through Cache는 데이터베이스와 캐시를 동시에 갱신하여 항상 최신 데이터를 유지한다. Write-Back Cache는 먼저 캐시에만 저장하고 나중에 데이터베이스를 갱신한다. Write-Back은 매우 빠르지만 장애 발생 시 데이터 손실 가능성을 고려해야 한다.

콘텐츠 전송 네트워크(CDN, Content Delivery Network)는 캐싱을 전 세계적으로 확장한 기술이다. 이미지(Image), 동영상(Video), 웹 페이지(Web Page), 소프트웨어 다운로드, AI 모델(Model), 지도(Map) 등을 세계 각 지역의 엣지 서버(Edge Server)에 저장하여 사용자가 가장 가까운 서버에서 콘텐츠를 받을 수 있도록 한다.

CDN은 사용자와 가까운 위치에 데이터를 저장하므로 네트워크 지연(Latency)을 크게 줄일 수 있다. 또한 원본 서버(Origin Server)의 부하를 감소시키고 국제 인터넷 백본(Backbone) 사용량을 줄여 대규모 글로벌 서비스(Global Service)의 성능을 크게 향상시킨다.

최근 CDN은 단순한 파일 저장을 넘어 엣지 컴퓨팅(Edge Computing)으로 발전하고 있다. AI 추론(Inference), 데이터 전처리(Preprocessing), IoT 데이터 분석, 로봇 제어 일부를 엣지에서 수행하여 응답 시간을 더욱 줄이고 클라우드 의존성을 낮춘다.

브라우저 캐시(Browser Cache)는 사용자의 웹 브라우저에 이미지, CSS, JavaScript, 폰트(Font), 동영상 등을 저장하는 기술이다. 동일한 웹사이트를 다시 방문하면 인터넷에서 다시 다운로드하지 않고 로컬 저장소(Local Storage)의 데이터를 사용하므로 웹 페이지가 훨씬 빠르게 표시된다.

데이터베이스(Database) 역시 다양한 캐시를 사용한다. 버퍼 풀(Buffer Pool)은 디스크 페이지를 메모리에 저장하며, 쿼리 캐시(Query Cache)는 실행 결과를 저장한다. 실행 계획 캐시(Execution Plan Cache)는 SQL 최적화 결과를 저장하여 반복적인 쿼리(Query) 수행 속도를 크게 향상시킨다.

AI 시스템에서는 특별한 캐시 전략이 사용된다. 동일한 입력에 대한 추론 결과(Inference Result)를 저장하는 추론 캐시(Inference Cache), 특징 벡터(Embedding)를 저장하는 임베딩 캐시(Embedding Cache), 모델 파라미터(Model Parameter)를 저장하는 모델 캐시(Model Cache) 등이 대표적이다. 이를 통해 GPU 계산량을 크게 줄일 수 있다.

대규모 언어 모델(LLM, Large Language Model)은 프롬프트 캐시(Prompt Cache), 토큰 캐시(Token Cache), 컨텍스트 캐시(Context Cache)를 사용한다. 반복적으로 등장하는 프롬프트와 중간 계산 결과를 저장하여 생성형 AI(Generative AI)의 응답 속도를 높이고 GPU 비용을 절감한다.

자율주행 로봇과 물리 AI(Physical AI)는 계층형 캐시(Hierarchical Cache)를 사용한다. 로봇 내부에는 지도(Map), 센서 보정값, AI 모델, 경로(Path), 환경 정보(Environment Information)를 저장하고, 클라우드에는 플릿(Fleet) 관리 정보, 디지털 트윈, 운영 분석 데이터를 저장한다. 이를 통해 네트워크가 끊겨도 로봇은 독립적으로 동작할 수 있다.

플릿 관리(Fleet Management)는 여러 대의 로봇이 동일한 지도, 작업(Task), 소프트웨어 업데이트(Update), 유지보수 정보(Maintenance Information)를 공유하기 때문에 분산 캐시를 적극 활용한다. 자주 사용하는 데이터를 로컬과 클라우드에 동시에 저장하여 통신량을 줄이고 운영 효율을 높인다.

IoT 환경에서도 계층형 캐시가 중요하다. 수많은 센서(Sensor)는 데이터를 먼저 게이트웨이(Gateway)에 저장하고 필터링(Filter), 압축(Compression), 이상 탐지(Anomaly Detection)를 수행한 후 필요한 데이터만 클라우드로 전송한다. 이를 통해 네트워크 사용량과 클라우드 비용을 크게 절감할 수 있다.

캐시 저장 공간이 부족하면 제거 정책(Cache Eviction Policy)을 사용한다. LRU(Least Recently Used)는 가장 오래 사용하지 않은 데이터를 삭제하고, LFU(Least Frequently Used)는 가장 적게 사용한 데이터를 삭제한다. FIFO(First-In First-Out)는 가장 먼저 저장된 데이터를 제거하며, Random은 무작위(Random)로 삭제한다. 최근에는 여러 알고리즘을 조합한 적응형(Adaptive) 제거 정책도 많이 사용된다.

효율적인 캐시 운영을 위해서는 관측성(Observability)이 필수적이다. 캐시 히트율(Hit Ratio), 미스율(Miss Ratio), 메모리 사용량, 제거 횟수(Eviction Count), 동기화 지연(Synchronization Latency), 데이터베이스 부하 감소 효과 등을 지속적으로 모니터링하여 캐시 정책을 최적화한다.

캐시는 민감한 데이터를 저장할 수도 있으므로 보안(Security)도 매우 중요하다. 인증(Authentication), 접근 제어(Access Control), 암호화(Encryption), 키 관리(Key Management), 감사 로그(Audit Log), 메모리 보호(Memory Protection)를 적용하여 개인정보(Personal Information), 금융 데이터(Financial Data), 의료 정보(Medical Information), AI 모델 등을 안전하게 보호해야 한다.

캐싱은 운영 비용 절감에도 큰 역할을 한다. 메모리 비용은 반복적으로 데이터베이스를 확장하거나 GPU 서버를 증설하는 것보다 훨씬 저렴한 경우가 많다. 특히 클라우드에서는 데이터베이스 접근 횟수와 네트워크 사용량이 줄어들어 전체 운영 비용을 크게 절감할 수 있다.

그러나 모든 데이터를 캐싱하는 것이 항상 좋은 것은 아니다. 자주 변경되는 데이터, 거의 재사용되지 않는 데이터, 매우 높은 일관성이 필요한 데이터는 캐시의 효과가 크지 않을 수 있다. 따라서 캐시 적용 여부는 데이터 접근 패턴과 운영 특성을 충분히 분석한 후 결정해야 한다.

결론적으로 캐싱 아키텍처(Caching Architecture)는 현대 소프트웨어 시스템의 핵심 성능 최적화 기술이다. 로컬 캐시(Local Cache)는 가장 빠른 응답을 제공하고, 분산 캐시(Distributed Cache)는 클라우드 환경에서 데이터 공유와 확장성을 지원하며, CDN은 전 세계 사용자에게 콘텐츠를 빠르게 전달한다. 또한 브라우저 캐시, 데이터베이스 캐시, AI 캐시, 로봇 캐시가 함께 계층적으로 동작하여 성능, 확장성, 가용성, 비용 효율성을 동시에 향상시킨다. 이러한 다계층 캐싱 전략은 클라우드, AI, 자율주행 로봇, 산업 자동화, 물리 AI 시대의 핵심 소프트웨어 아키텍처 기술로 자리 잡고 있다.

## 09.04 Database Sharding and Partitioning Strategies

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

데이터베이스 샤딩(Database Sharding)과 파티셔닝(Database Partitioning)은 현대 소프트웨어 아키텍처에서 대규모 데이터를 효율적으로 저장하고 처리하기 위한 핵심 기술이다. 클라우드(Cloud), 금융(Finance), 전자상거래(E-Commerce), 산업 자동화(Industrial Automation), 인공지능(AI), 자율주행 로봇(Autonomous Robot), IoT(Internet of Things), 물리 AI(Physical AI) 시스템은 지속적으로 증가하는 데이터와 사용자 요청을 처리해야 한다. 기존의 단일 데이터베이스(Database)는 CPU, 메모리(Memory), 저장장치(Storage), 네트워크(Network)의 물리적 한계에 도달하게 되므로 데이터를 분산하여 관리하는 구조가 필수적이다.

데이터베이스 확장성(Database Scalability)은 단순히 저장 공간을 늘리는 것이 아니다. 데이터 양(Data Volume), 동시 사용자(Concurrent Users), 트랜잭션(Transaction), 조회(Query), 분석(Analytics), 백업(Backup), 장애 복구(Disaster Recovery)까지 모두 고려해야 한다. 따라서 현대 데이터베이스는 데이터를 어떻게 분산하고 관리할 것인지가 핵심 설계 요소가 된다.

파티셔닝(Partitioning)과 샤딩(Sharding)은 비슷한 개념처럼 보이지만 목적이 다르다. 파티셔닝은 하나의 데이터베이스 내부에서 데이터를 여러 영역으로 나누는 방식이며, 샤딩은 데이터를 여러 개의 독립적인 데이터베이스 서버(Database Server)에 분산하여 저장하는 방식이다. 즉, 파티셔닝은 데이터 구조를 최적화하는 기술이고, 샤딩은 데이터베이스 자체를 수평 확장(Horizontal Scaling)하는 기술이다.

데이터베이스 파티셔닝(Database Partitioning)은 많은 조회(Query)가 전체 데이터가 아닌 일부 데이터만 접근한다는 점에서 시작된다. 예를 들어 고객(Customer)은 지역별, 거래(Transaction)는 날짜별, 센서 데이터(Sensor Data)는 장치(Device)별, 로봇 데이터는 플릿(Fleet)별로 구분할 수 있다. 관련된 데이터를 하나의 파티션(Partition)에 모으면 불필요한 데이터 검색을 줄이고 조회 성능을 크게 향상시킬 수 있다.

수평 파티셔닝(Horizontal Partitioning)은 테이블(Table)의 행(Row)을 기준으로 데이터를 분리한다. 테이블 구조는 동일하지만 서로 다른 데이터 집합을 저장한다. 고객 ID(Customer ID), 지역(Region), 시간(Time), 생산 라인(Production Line) 등이 대표적인 분리 기준이다. 대부분의 조회는 필요한 파티션만 검색하므로 처리 속도가 향상된다.

수직 파티셔닝(Vertical Partitioning)은 테이블의 열(Column)을 기준으로 데이터를 분리한다. 자주 사용하는 컬럼은 하나의 테이블에 모으고, 거의 사용하지 않는 대용량 컬럼은 별도의 테이블에 저장한다. 예를 들어 고객 이름(Name), 연락처(Contact)는 자주 조회하지만 프로필 이미지(Profile Image)는 자주 사용하지 않을 수 있다. 이러한 구조는 메모리 사용량과 캐시(Cache) 효율을 향상시킨다.

기능 파티셔닝(Functional Partitioning)은 업무 영역(Business Domain)을 기준으로 데이터를 분리한다. 고객(Customer), 주문(Order), 재고(Inventory), 회계(Accounting), 인사(Human Resources), AI 실험(AI Experiment), 로봇 운영(Robot Operation) 등 업무별로 독립적인 데이터 구조를 유지한다. 이는 마이크로서비스(Microservices)와 도메인 주도 설계(DDD, Domain-Driven Design)와도 잘 어울리는 구조이다.

범위 파티셔닝(Range Partitioning)은 가장 많이 사용되는 방식 가운데 하나이다. 날짜(Date), 시간(Time), 번호(Number), 생산 Batch 등을 기준으로 일정 범위를 하나의 파티션으로 구성한다. 예를 들어 거래 데이터를 월(Month) 또는 연도(Year) 단위로 저장하면 특정 기간을 조회할 때 필요한 파티션만 검색하므로 성능이 크게 향상된다.

리스트 파티셔닝(List Partitioning)은 특정한 값(Value)에 따라 데이터를 분리한다. 국가(Country), 지역(Region), 부서(Department), 제품(Product), 로봇 플릿(Fleet), 공장(Factory) 등이 대표적인 기준이다. 특정 지역이나 특정 사업부의 데이터를 자주 조회하는 시스템에서 매우 효과적이다.

해시 파티셔닝(Hash Partitioning)은 해시 함수(Hash Function)를 이용하여 데이터를 자동으로 여러 파티션에 균등하게 분산한다. 데이터가 특정 파티션에 집중되는 현상을 줄일 수 있으며 저장 공간 활용률이 매우 우수하다. 반면 범위 조회(Range Query)에서는 여러 파티션을 동시에 검색해야 하는 단점이 있다.

복합 파티셔닝(Composite Partitioning)은 여러 가지 방법을 동시에 사용하는 방식이다. 예를 들어 먼저 날짜 기준으로 나눈 후 다시 지역별 또는 해시 방식으로 세분화한다. 대규모 기업 시스템에서는 이러한 다단계 구조를 많이 사용하며, 조회 성능과 운영 효율을 동시에 확보할 수 있다.

파티셔닝은 조회 성능뿐 아니라 운영(Operation)에도 큰 장점을 제공한다. 백업(Backup), 복원(Restore), 인덱스(Index) 생성, 데이터 압축(Compression), 아카이브(Archive), 삭제(Delete) 등을 전체 데이터가 아니라 필요한 파티션만 대상으로 수행할 수 있으므로 유지보수 시간이 크게 단축된다.

그러나 파티셔닝은 기본적으로 하나의 데이터베이스 시스템 내부에서 동작하기 때문에 서버의 CPU, 메모리, 디스크, 네트워크 한계를 근본적으로 해결하지는 못한다. 데이터가 계속 증가하면 결국 수평 확장을 위해 샤딩(Sharding)이 필요해진다.

데이터베이스 샤딩(Database Sharding)은 데이터를 여러 개의 독립적인 데이터베이스 서버에 분산 저장하는 기술이다. 각 샤드(Shard)는 전체 데이터의 일부만 관리하며 독립적으로 저장, 조회, 백업, 복구, 확장이 가능하다. 새로운 서버를 추가하면 전체 데이터베이스의 처리 능력도 함께 증가한다.

샤딩의 핵심 목적은 저장 공간을 늘리는 것이 아니라 작업 부하(Workload)를 분산하는 것이다. 조회(Query), 저장(Insert), 수정(Update), 인덱스(Index), 백업(Backup), 네트워크(Network) 사용량을 여러 서버로 나누어 처리함으로써 전체 시스템의 처리 능력을 크게 향상시킨다.

효율적인 샤딩을 위해서는 샤드 키(Shard Key)를 적절하게 선택해야 한다. 고객 ID(Customer ID), 회사(Tenant), 지역(Region), 계정(Account), 장치(Device), 로봇 플릿(Fleet), 생산 공장(Factory) 등이 대표적인 샤드 키이다. 좋은 샤드 키는 데이터를 균등하게 분산하면서도 대부분의 조회가 하나의 샤드에서 처리될 수 있도록 해야 한다.

해시 샤딩(Hash Sharding)은 가장 널리 사용하는 방식이다. 샤드 키를 해시 함수에 입력하여 저장할 샤드를 결정한다. 데이터가 균등하게 분산되므로 특정 서버에 부하가 집중되는 것을 방지할 수 있다. 그러나 범위 조회는 여러 샤드를 동시에 검색해야 하는 경우가 많다.

범위 샤딩(Range Sharding)은 고객 번호(Customer ID), 날짜(Date), 거래 번호(Transaction Number) 등을 기준으로 일정 구간을 하나의 샤드에 저장한다. 시간 기반 분석이나 통계 처리에는 매우 유리하지만 특정 범위에 데이터가 집중될 가능성이 있어 주기적인 재조정(Rebalancing)이 필요하다.

디렉터리 샤딩(Directory Sharding)은 별도의 메타데이터(Metadata)를 이용하여 데이터가 어느 샤드에 저장되어 있는지를 관리한다. 데이터 위치를 자유롭게 변경할 수 있어 매우 유연하지만 메타데이터 서버 자체가 중요한 핵심 인프라가 되므로 높은 가용성(Availability)이 요구된다.

지역 기반 샤딩(Geographic Sharding)은 국가(Country), 대륙(Continent), 지역(Region)별로 데이터를 분산한다. 사용자는 가까운 지역의 데이터베이스를 사용하므로 응답 속도가 빨라지고 데이터 주권(Data Sovereignty)과 개인정보 규제(Privacy Regulation)도 만족시킬 수 있다.

SaaS(Software as a Service)에서는 테넌트 기반 샤딩(Tenant-Based Sharding)을 많이 사용한다. 작은 고객은 하나의 샤드를 공유하고, 대기업 고객은 전용 샤드를 사용하는 방식이다. 고객 간 성능 간섭을 줄이고 유지보수와 보안(Security)을 향상시킬 수 있다.

AI 플랫폼은 매우 큰 데이터셋(Dataset)을 처리하기 때문에 샤딩이 필수적이다. 학습 데이터(Training Data), 특징 데이터(Feature Store), 임베딩(Embedding), 모델(Model), 실험 데이터(Experiment), 추론 로그(Inference Log) 등을 여러 샤드에 분산 저장하여 병렬 학습과 대규모 AI 서비스를 지원한다.

벡터 데이터베이스(Vector Database)는 의미 기반 검색(Semantic Search)과 RAG(Retrieval-Augmented Generation)를 위해 수억 개 이상의 벡터(Vector)를 저장한다. 벡터 공간(Vector Space)을 클러스터링(Clustering), 해시(Hash), 계층 구조(Hierarchical Structure) 등으로 샤딩하여 검색 성능을 향상시킨다.

자율주행 로봇과 물리 AI는 방대한 운영 데이터를 생성한다. 위치 추정(Localization), LiDAR(Point Cloud), 카메라(Image), 배터리(Battery), 경로(Path), AI 추론 결과(Inference Result), 유지보수(Maintenance) 정보 등을 지속적으로 저장해야 하므로 로봇 플릿(Fleet), 지역(Region), 프로젝트(Project) 단위로 샤딩하는 것이 일반적이다.

IoT 환경 역시 수백만 개의 센서가 지속적으로 데이터를 생성하므로 샤딩이 필수적이다. 센서 ID(Device ID), 게이트웨이(Gateway), 공장(Factory), 지역(Region), 시간(Time)을 기준으로 데이터를 분산 저장하면 실시간 수집(Ingestion)과 분석(Analytics)이 훨씬 효율적으로 이루어진다.

샤딩은 복제(Replication)와 함께 사용되는 경우가 많다. 각 샤드는 Primary 데이터베이스와 여러 Replica를 운영한다. Primary는 쓰기(Write)를 담당하고 Replica는 읽기(Read), 분석, 백업 등을 처리하여 성능과 가용성을 동시에 확보한다.

샤딩의 가장 어려운 문제는 샤드 간 통신(Cross-Shard Communication)이다. 여러 샤드에 저장된 데이터를 동시에 조회하거나 조인(Join)해야 하는 경우 응답 시간이 증가하고 시스템이 복잡해진다. 따라서 설계 단계에서 가능한 한 하나의 샤드 안에서 대부분의 작업이 수행되도록 데이터 모델을 설계하는 것이 중요하다.

분산 트랜잭션(Distributed Transaction)도 중요한 과제이다. 여러 샤드에 걸친 데이터를 동시에 수정하려면 2단계 커밋(2PC, Two-Phase Commit), 사가 패턴(Saga Pattern), 이벤트 기반(Event-Driven) 처리, 최종 일관성(Eventual Consistency) 등을 사용하여 데이터 정합성을 유지해야 한다.

운영 중에는 특정 샤드에 데이터가 집중될 수 있으므로 샤드 재분배(Shard Rebalancing)가 필요하다. 새로운 고객 증가, 특정 지역의 성장, 로봇 플릿 확대 등에 따라 데이터를 다른 샤드로 이동하여 부하를 균등하게 유지한다.

관측성(Observability)은 분산 데이터베이스에서 매우 중요하다. 샤드별 저장 용량(Storage Capacity), 응답 시간(Query Latency), 복제 상태(Replication Health), 네트워크(Network), AI 데이터 증가율, 로봇 텔레메트리(Telemetry) 등을 지속적으로 모니터링하여 병목(Bottleneck)을 조기에 발견해야 한다.

보안(Security)도 샤딩 환경에서는 더욱 중요하다. 인증(Authentication), 접근 제어(Access Control), 암호화(Encryption), 키 관리(Key Management), 감사 로그(Audit Log), 네트워크 분리(Network Isolation)를 적용하여 여러 데이터베이스에 분산된 민감한 정보를 안전하게 보호해야 한다.

샤딩은 경제성(Economics) 측면에서도 유리하다. 매우 비싼 고성능 서버를 계속 업그레이드하는 대신 일반적인 서버를 필요할 때마다 추가하여 점진적으로 확장할 수 있다. 클라우드 환경에서는 사용량 기반(Pay-as-you-Go) 과금 모델과도 잘 맞아 비용 효율성이 높다.

그러나 모든 시스템에 샤딩이 필요한 것은 아니다. 데이터 규모가 작고 사용자 수가 적은 시스템은 단일 데이터베이스가 오히려 관리가 쉽고 성능도 충분하다. 샤딩은 운영 복잡도를 크게 증가시키므로 충분한 규모와 성장 가능성이 있는 경우에 적용하는 것이 바람직하다.

현대 소프트웨어는 파티셔닝, 샤딩, 복제(Replication), 분산 캐시(Distributed Cache), CDN(Content Delivery Network), 이벤트 스트리밍(Event Streaming), 마이크로서비스(Microservices), AI 데이터 관리 등을 함께 결합하여 통합 데이터 플랫폼(Data Platform)을 구축한다. 하나의 기술만 사용하는 것이 아니라 여러 기술을 조합하여 장기적인 확장성과 안정성을 확보하는 것이 현대 데이터베이스 아키텍처의 핵심이다.

결론적으로 데이터베이스 파티셔닝(Database Partitioning)과 샤딩(Database Sharding)은 대규모 데이터 처리를 위한 핵심 확장 기술이다. 파티셔닝은 데이터 구조를 최적화하여 조회 성능과 운영 효율을 높이고, 샤딩은 여러 데이터베이스 서버로 데이터를 분산하여 수평 확장성과 높은 처리 성능을 제공한다. 클라우드, AI, 금융, 산업 자동화, 자율주행 로봇, IoT, 물리 AI 시대에는 이 두 기술이 데이터 플랫폼의 핵심 아키텍처로 자리 잡고 있으며, 지속적으로 증가하는 데이터와 사용자 요구를 안정적으로 처리하기 위한 필수 기술로 활용되고 있다.

## 09.05 Async Processing and Queue-Based Scaling Patterns

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

비동기 처리(Asynchronous Processing)와 큐 기반 확장(Queue-Based Scaling)은 현대 소프트웨어 아키텍처에서 대규모 작업을 효율적으로 처리하기 위한 핵심 기술이다. 클라우드(Cloud), 마이크로서비스(Microservices), 인공지능(AI), 자율주행 로봇(Autonomous Robot), IoT(Internet of Things), 산업 자동화(Industrial Automation), 금융(Finance), 의료(Healthcare), 물리 AI(Physical AI) 시스템은 수많은 요청(Request)을 동시에 처리해야 한다. 모든 요청을 즉시 처리하는 동기 방식(Synchronous Processing)은 시스템의 응답성을 저하시킬 수 있으므로, 긴 작업은 비동기 방식으로 처리하는 구조가 필수적이다.

전통적인 동기 처리(Synchronous Processing)는 클라이언트(Client)가 요청(Request)을 보내면 서버(Server)가 작업을 모두 완료한 후 결과(Response)를 반환하는 방식이다. 로그인(Login), 간단한 조회(Query), 설정(Configuration) 읽기와 같은 짧은 작업에는 적합하지만, AI 추론(Inference), 동영상 변환(Video Transcoding), 대용량 데이터 분석, 로봇 플릿(Fleet) 제어와 같이 오래 걸리는 작업에는 비효율적이다. 작업이 끝날 때까지 연결(Connection)을 유지해야 하므로 서버 자원을 많이 소비하게 된다.

비동기 처리(Asynchronous Processing)는 요청 접수(Request Acceptance)와 실제 작업(Task Execution)을 분리하는 방식이다. 사용자의 요청을 즉시 접수한 후 백그라운드(Background)에서 작업을 수행하며, 사용자는 작업이 완료될 때까지 기다릴 필요가 없다. 따라서 사용자 경험(User Experience)이 향상되고 서버의 응답성(Response Time)도 크게 개선된다.

비동기 처리를 구현하기 위한 핵심 기술이 메시지 큐(Message Queue)이다. 메시지 큐는 작업(Task)을 임시로 저장한 후 처리 가능한 시점에 작업자(Worker)가 가져가 실행하도록 한다. 요청을 생성하는 생산자(Producer)와 실제 작업을 수행하는 소비자(Consumer)를 분리하여 서로 직접 연결되지 않도록 만든다. 이 구조는 시스템의 결합도(Coupling)를 낮추고 확장성을 크게 향상시킨다.

큐 기반 아키텍처(Queue-Based Architecture)의 가장 큰 장점은 시간적 분리(Temporal Decoupling)이다. 요청을 생성하는 시스템과 작업을 수행하는 시스템이 동시에 실행될 필요가 없다. 작업은 큐(Queue)에 안전하게 저장되며, 서버가 일시적으로 중단되거나 유지보수(Maintenance)를 수행하더라도 작업은 손실되지 않는다.

또한 위치 분리(Location Decoupling)도 가능하다. 생산자와 소비자는 서로 다른 서버(Server), 클라우드(Cloud), 지역(Region), 운영체제(OS), 프로그래밍 언어(Language)에서 동작할 수 있다. 오직 메시지(Message)만을 주고받기 때문에 독립적인 개발과 배포가 가능하다.

용량 분리(Capacity Decoupling)는 큐 기반 시스템의 또 다른 중요한 특징이다. 갑작스럽게 많은 요청이 들어오더라도 즉시 처리하지 않고 큐에 저장한 후 작업자가 순차적으로 처리한다. 따라서 순간적인 트래픽(Traffic) 증가에도 서비스는 안정적으로 동작하며 시스템이 과부하(Overload)에 빠지는 것을 방지할 수 있다.

메시지 큐(Message Queue)는 생산자(Producer), 브로커(Broker), 큐(Queue), 소비자(Consumer), 확인 응답(Acknowledgement), 영속 저장(Persistence)으로 구성된다. 생산자는 작업을 생성하고 브로커는 이를 큐에 저장하며, 소비자는 큐에서 작업을 가져와 실행한다. 작업이 정상적으로 완료되면 확인 응답을 보내고, 실패하면 재시도(Retry)를 수행하거나 별도의 큐로 이동시킨다.

큐 기반 시스템은 다양한 메시징 패턴(Messaging Pattern)을 지원한다. Point-to-Point는 하나의 작업을 하나의 소비자가 처리하는 방식이며, Publish-Subscribe는 하나의 이벤트(Event)를 여러 소비자가 동시에 처리한다. Fan-Out은 모든 소비자에게 메시지를 전달하며, Topic 기반은 특정 주제(Topic)를 구독한 서비스만 메시지를 수신한다.

워크 큐(Work Queue)는 가장 많이 사용하는 비동기 패턴이다. 모든 작업을 큐에 저장하고 여러 작업자(Worker)가 동시에 작업을 가져가 처리한다. 작업자는 독립적으로 동작하므로 서버를 추가하면 전체 처리량(Throughput)도 함께 증가하여 자연스럽게 수평 확장(Horizontal Scaling)이 가능해진다.

작업 스케줄링(Task Scheduling)도 큐 기반 구조와 함께 사용된다. 즉시 실행(Immediate Execution), 일정 시간 후 실행(Delayed Task), 예약 실행(Scheduled Task), 반복 실행(Periodic Task)을 지원하여 AI 재학습(Retraining), 데이터 동기화(Synchronization), 시스템 유지보수(Maintenance), 로봇 진단(Diagnostics) 등을 자동으로 수행할 수 있다.

작업자 확장성(Worker Scalability)은 큐 기반 구조의 가장 큰 장점이다. 큐에 작업이 많아지면 새로운 작업자를 자동으로 생성하고, 작업량이 감소하면 작업자를 줄일 수 있다. 쿠버네티스(Kubernetes)와 같은 오케스트레이션(Orchestration) 플랫폼은 큐 길이(Queue Depth), CPU, 메모리(Memory), GPU 사용량 등을 분석하여 자동으로 작업자 수를 조절한다.

로드 밸런싱(Load Balancing)도 큐와 자연스럽게 결합된다. 프론트엔드(Frontend)가 직접 작업자를 선택하는 것이 아니라 여러 작업자가 큐에서 작업을 가져가는 Pull 방식으로 동작한다. 따라서 별도의 복잡한 스케줄러(Scheduler)가 없어도 작업이 자연스럽게 분산된다.

비동기 시스템은 장애 허용(Fault Tolerance) 능력이 매우 뛰어나다. 작업자가 작업을 처리하다가 장애가 발생하면 확인 응답(Acknowledgement)이 전달되지 않으므로 메시지는 큐에 그대로 남아 다른 작업자가 다시 처리할 수 있다. 따라서 서버 장애가 발생해도 작업 손실 없이 서비스를 계속 운영할 수 있다.

재시도(Retry)는 비동기 시스템의 핵심 기능이다. 네트워크(Network), 외부 API(Application Programming Interface), 데이터베이스(Database) 등의 일시적인 오류는 일정 시간 후 다시 실행하면 정상적으로 처리되는 경우가 많다. 따라서 즉시 반복하지 않고 점차 대기 시간을 늘리는 지수 백오프(Exponential Backoff)를 사용하여 시스템 부하를 줄인다.

데드 레터 큐(DLQ, Dead Letter Queue)는 반복적으로 실패하는 작업을 별도로 저장하는 공간이다. 동일한 작업을 무한히 반복하면 시스템 자원을 낭비하게 되므로 일정 횟수 이상 실패하면 DLQ로 이동하여 관리자가 원인을 분석하고 수정하도록 한다.

멱등성(Idempotency)은 비동기 시스템에서 반드시 고려해야 하는 개념이다. 동일한 메시지가 여러 번 처리되더라도 결과는 한 번 실행한 것과 동일해야 한다. 결제(Payment), 주문(Order), 로봇 명령(Command), 소프트웨어 배포(Deployment) 등은 중복 실행되어서는 안 되므로 멱등성이 매우 중요하다.

메시지 순서(Ordering)도 중요한 설계 요소이다. 금융 거래(Transaction), 제조 공정(Manufacturing Process), 로봇 제어(Control)는 순서가 바뀌면 문제가 발생할 수 있다. 반면 일반적인 로그(Log)나 분석(Analytics)은 순서가 크게 중요하지 않을 수도 있다. 따라서 업무 특성에 따라 순서를 보장할지 여부를 결정해야 한다.

우선순위 큐(Priority Queue)는 긴급한 작업을 먼저 처리하는 구조이다. 긴급 경보(Alert), 안전 제어(Safety Control), 의료 데이터(Medical Data), AI 추론(Inference), 결제 승인(Payment Authorization)은 일반 분석 작업보다 높은 우선순위를 부여하여 빠르게 처리한다.

이벤트 기반 아키텍처(Event-Driven Architecture)는 비동기 처리와 매우 잘 어울린다. 회원 가입(User Registration), 결제 완료(Payment Completed), 로봇 도착(Robot Arrived), 센서 이벤트(Sensor Event), AI 모델 배포(Model Deployment) 등 다양한 이벤트가 발생하면 여러 서비스가 이를 독립적으로 처리할 수 있다.

마이크로서비스(Microservices)는 비동기 메시징을 적극 활용한다. 서비스 간 직접 호출(Call)을 최소화하고 메시지 큐를 이용하여 데이터를 교환한다. 따라서 하나의 서비스가 장애가 발생해도 다른 서비스는 독립적으로 동작할 수 있으며 전체 시스템의 안정성이 향상된다.

클라우드 네이티브(Cloud-Native) 환경에서는 오토 스케일링(Auto Scaling)을 이용하여 큐의 길이, CPU 사용률, GPU 사용량 등을 분석하고 작업자를 자동으로 생성하거나 제거한다. 서버리스(Serverless) 환경에서는 작업 하나마다 실행 환경을 자동으로 생성하여 매우 높은 자원 효율성을 제공한다.

AI 플랫폼은 비동기 처리를 적극적으로 활용한다. 모델 학습(Model Training), 하이퍼파라미터 최적화(Hyperparameter Optimization), 데이터 전처리(Preprocessing), 특징 생성(Feature Engineering), 배치 추론(Batch Inference), 벡터 인덱싱(Vector Indexing) 등 대부분의 작업은 오랜 시간이 걸리므로 큐를 통해 순차적으로 처리한다.

AI 추론(Inference)에서는 여러 요청을 모아서 한 번에 처리하는 배치 처리(Batch Processing)가 많이 사용된다. 큐에 일정량의 요청이 쌓이면 GPU에서 한꺼번에 처리하여 GPU 활용률(Utilization)을 높이고 운영 비용(Cost)을 절감할 수 있다.

자율주행 로봇과 물리 AI는 다양한 작업을 동시에 수행한다. 위치 추정(Localization), 센서 융합(Sensor Fusion), 경로 계획(Path Planning), 원격 진단(Remote Diagnostics), 디지털 트윈(Digital Twin), AI 추론, 플릿 관리(Fleet Management) 등은 서로 다른 시간에 수행되므로 비동기 처리 구조가 필수적이다.

플릿 관리 시스템(Fleet Management System)은 수백 대의 로봇을 동시에 제어해야 한다. 작업 할당(Task Assignment), 배터리 모니터링(Battery Monitoring), 원격 업데이트(Remote Update), 텔레메트리(Telemetry), AI 모델 동기화(Model Synchronization)는 모두 큐를 이용하여 안정적으로 처리된다.

IoT 시스템도 비동기 처리 구조를 사용한다. 수백만 개의 센서(Sensor)가 동시에 데이터를 전송하므로 메시지 큐를 통해 데이터를 수집하고 분석 시스템(Analytics), AI 모델, 데이터베이스 등으로 순차적으로 전달한다. 이를 통해 갑작스러운 데이터 증가에도 안정적으로 운영할 수 있다.

산업 자동화(Industrial Automation)에서도 생산 설비(Machine), 품질 검사(Quality Inspection), 예지 보전(Predictive Maintenance), 물류(Logistics) 등의 이벤트(Event)를 비동기적으로 처리한다. 이를 통해 제조 시스템 전체의 안정성과 확장성을 향상시킨다.

워크플로우 오케스트레이션(Workflow Orchestration)은 여러 작업을 순서대로 연결하는 기술이다. 상태 머신(State Machine), 타이머(Timer), 재시도(Retry), 보상 트랜잭션(Compensation Transaction) 등을 함께 사용하여 복잡한 업무 프로세스를 자동으로 관리한다.

비동기 시스템에서는 관측성(Observability)이 매우 중요하다. 큐 길이(Queue Depth), 처리 속도(Processing Rate), 작업자 사용률(Worker Utilization), 재시도 횟수(Retry Count), 실패율(Failure Rate), 메시지 대기 시간(Message Age), AI 추론 대기 시간(Inference Backlog) 등을 지속적으로 모니터링하여 시스템 상태를 관리한다.

보안(Security)도 메시지 큐에서 매우 중요하다. 인증(Authentication), 권한 관리(Authorization), 메시지 암호화(Encryption), 접근 제어(Access Control), 감사 로그(Audit Log), 무결성 검증(Integrity Verification) 등을 적용하여 금융, 의료, AI 모델, 로봇 제어 데이터 등을 안전하게 보호해야 한다.

경제성(Economics)은 비동기 처리의 또 다른 장점이다. 최대 트래픽에 맞추어 항상 많은 서버를 운영하는 대신 큐를 이용하여 작업량에 따라 작업자를 자동으로 조절할 수 있으므로 클라우드 비용을 크게 절감할 수 있다.

그러나 모든 작업을 비동기로 처리하는 것은 바람직하지 않다. 사용자 인증(Authentication), 간단한 조회(Query), 설정 읽기(Configuration)처럼 즉각적인 응답이 필요한 작업은 동기 방식이 더 적합하다. 반대로 AI 학습, 보고서 생성(Report Generation), 영상 처리(Video Processing), 로봇 플릿 관리 등은 비동기 방식이 훨씬 효율적이다.

따라서 현대 시스템은 동기(Synchronous)와 비동기(Asynchronous)를 함께 사용하는 하이브리드(Hybrid) 구조를 채택한다. 즉시 응답이 필요한 기능은 동기로 처리하고, 시간이 오래 걸리는 작업은 메시지 큐를 이용한 비동기 처리로 수행하여 응답성(Response), 확장성(Scalability), 안정성(Reliability), 운영 효율(Operation Efficiency)을 동시에 확보한다.

결론적으로 비동기 처리와 큐 기반 확장 패턴(Async Processing and Queue-Based Scaling Patterns)은 현대 분산 시스템의 핵심 아키텍처이다. 요청과 실행을 분리하고, 메시지 큐를 통해 작업을 안전하게 저장하며, 독립적인 작업자가 병렬 처리함으로써 높은 확장성, 장애 허용성, 운영 안정성, 비용 효율성을 동시에 제공한다. 클라우드, AI, 마이크로서비스, 자율주행 로봇, IoT, 물리 AI 시대에는 이러한 비동기 아키텍처가 대규모 서비스를 안정적으로 운영하기 위한 필수적인 설계 원칙으로 자리 잡고 있다.

## 09.06 Robot Fleet Scaling Design: Tens to Thousands

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

로봇 플릿 확장 설계(Robot Fleet Scaling Design)는 현대 자율주행 로봇(Autonomous Robot) 시스템에서 가장 중요한 아키텍처 요소 가운데 하나이다. 제조(Manufacturing), 물류(Logistics), 의료(Healthcare), 농업(Agriculture), 광산(Mining), 스마트시티(Smart City), 국방(Defense), 산업 검사(Industrial Inspection), 물리 AI(Physical AI) 환경에서는 단일 로봇보다 수백\~수천 대의 로봇이 협력하여 작업하는 형태로 발전하고 있다. 따라서 확장성은 단순히 로봇 수를 늘리는 것이 아니라, 전체 시스템이 성능과 안정성을 유지하면서 지속적으로 성장할 수 있는 능력을 의미한다.

수십 대(Tens)의 로봇과 수천 대(Thousands)의 로봇을 운영하는 것은 아키텍처 요구사항이 완전히 다르다. 소규모 플릿(Fleet)은 중앙 서버(Central Server)가 모든 로봇을 직접 관리할 수 있지만, 로봇 수가 증가하면 통신량(Communication), 계산량(Computation), 데이터(Data), 제어(Control)가 폭발적으로 증가한다. 결국 중앙 집중형(Centralized) 구조는 병목(Bottleneck)과 단일 장애점(Single Point of Failure)이 되므로 분산형(Distributed) 구조가 필수적이다.

확장 가능한 플릿은 기능(Function)을 여러 서비스(Service)로 분리하는 것에서 시작된다. 임무 관리(Mission Management), 경로 계획(Path Planning), 위치 추정(Localization), 텔레메트리(Telemetry), 배터리 관리(Battery Management), AI 추론(Inference), 소프트웨어 배포(Deployment), 디지털 트윈(Digital Twin), 유지보수(Maintenance) 등을 각각 독립적인 서비스로 분리하면 필요한 부분만 독립적으로 확장할 수 있다.

마이크로서비스(Microservices)는 이러한 플릿 구조와 매우 잘 맞는다. 각 서비스는 독립적으로 개발, 배포, 확장이 가능하며 특정 서비스의 부하만 증가하더라도 전체 시스템을 증설할 필요가 없다. 예를 들어 AI 추론 서비스만 GPU 서버를 추가하거나, 텔레메트리 서비스만 확장하는 것이 가능하다.

클라우드 네이티브(Cloud-Native)는 플릿 확장의 핵심 기반이다. 쿠버네티스(Kubernetes)와 같은 오케스트레이션(Orchestration) 플랫폼은 컨테이너(Container)를 자동으로 생성하거나 제거하여 로봇 증가에 맞추어 서비스를 수평 확장(Horizontal Scaling)한다. 따라서 인프라는 실제 작업량에 따라 자동으로 조정된다.

계층형(Hierarchical) 플릿 구조는 대규모 시스템에서 가장 효과적인 방식이다. 모든 로봇이 클라우드와 직접 통신하는 것이 아니라, 먼저 지역 엣지 게이트웨이(Edge Gateway)와 통신하고, 지역 서버가 다시 중앙 클라우드와 연결된다. 이를 통해 통신량을 줄이고 응답 속도를 높일 수 있다.

엣지 컴퓨팅(Edge Computing)은 대규모 플릿에서 매우 중요한 역할을 한다. 로봇은 실시간 제어(Real-Time Control), 장애물 회피(Obstacle Avoidance), 위치 추정(Localization), 센서 융합(Sensor Fusion)을 로컬(Local)에서 수행해야 한다. 반면 클라우드는 AI 학습(Model Training), 디지털 트윈, 운영 분석(Operation Analytics), 장기 데이터 저장을 담당한다.

통신 구조(Communication Architecture)는 플릿의 확장성을 결정하는 핵심 요소이다. 소규모 시스템은 요청-응답(Request-Response) 방식으로 충분하지만, 대규모 시스템은 이벤트 기반(Event-Driven), 발행-구독(Publish-Subscribe), 메시지 큐(Message Queue)를 사용하여 서비스 간 결합도를 낮춘다.

메시지 브로커(Message Broker)는 플릿 운영의 핵심 인프라이다. Apache Kafka, RabbitMQ, MQTT, DDS(Data Distribution Service) 등은 수많은 로봇에서 발생하는 위치 정보, 센서 데이터, AI 결과, 배터리 상태, 진단 정보 등을 안정적으로 전달한다. 큐 기반 구조를 사용하면 순간적인 데이터 증가에도 시스템이 안정적으로 운영된다.

로봇 등록(Robot Registration)과 신원 관리(Identity Management)도 중요하다. 모든 로봇은 고유 ID(ID), 인증(Authentication), 인증서(Certificate), 설정(Configuration), 소프트웨어 버전(Software Version), 하드웨어 정보(Hardware Inventory)를 관리해야 한다. 이를 통해 안전한 통신과 중앙 관리가 가능해진다.

임무 할당(Mission Allocation)은 로봇 수가 증가할수록 더욱 복잡해진다. 단순히 가장 가까운 로봇을 선택하는 것이 아니라 배터리(Battery), 현재 작업(Task), 적재 능력(Payload), 우선순위(Priority), 혼잡도(Congestion), AI 기능(AI Capability) 등을 종합적으로 고려하여 최적의 로봇을 선택해야 한다.

워크플로우 오케스트레이션(Workflow Orchestration)은 여러 로봇과 설비가 협력하는 환경에서 중요하다. 공장에서는 로봇, 자동문(Door), 엘리베이터(Elevator), 충전기(Charging Station), 검사 장비(Inspection Equipment)가 순차적으로 동작해야 하므로 이벤트(Event) 기반 상태 머신(State Machine)으로 작업을 관리한다.

교통 관리(Traffic Management)는 수백 대 이상의 로봇이 동시에 움직일 때 필수적이다. 단순한 충돌 회피(Collision Avoidance)만으로는 충분하지 않으며, 교차로 예약(Intersection Reservation), 경로 최적화(Route Optimization), 혼잡 예측(Congestion Prediction), 우선순위 제어(Priority Control)를 적용해야 한다.

위치 추정(Localization)은 대규모 플릿에서 협력형(Collaborative) 구조로 발전한다. 각각의 로봇이 독립적으로 위치를 계산하는 것이 아니라 여러 로봇이 동시에 환경 정보를 공유하여 지도(Map)의 정확도를 지속적으로 향상시킨다.

지도 관리(Map Management)는 여러 공장, 병원, 물류센터, 캠퍼스 등을 운영하는 기업에서 매우 중요하다. 하나의 거대한 지도를 사용하는 대신 지역별 지도(Map Segment)를 관리하고 버전 관리(Version Control), 차등 업데이트(Differential Update)를 수행하여 필요한 부분만 로봇에 배포한다.

AI는 플릿 운영의 핵심 요소이다. 객체 인식(Object Recognition), 경로 계획(Path Planning), 자연어 처리(NLP, Natural Language Processing), 이상 탐지(Anomaly Detection), 예측(Prediction) 등을 모두 로봇 내부에서 수행하기에는 계산량이 너무 크다. 따라서 실시간 AI는 로컬에서 수행하고 복잡한 AI는 GPU 서버와 클라우드에서 처리하는 하이브리드(Hybrid) 구조를 사용한다.

AI 모델 관리(Model Lifecycle Management)는 수천 대의 로봇에서 매우 중요하다. 새로운 모델(Model)을 배포하고, 점진적 배포(Canary Deployment), 롤백(Rollback), 버전 관리(Version Control), 호환성 검증(Compatibility Verification)을 수행해야 안정적인 운영이 가능하다.

텔레메트리(Telemetry)는 플릿 운영에서 가장 큰 데이터(Data)를 생성한다. 위치(Position), 속도(Velocity), 배터리, 모터(Motor), 센서(Sensor), AI 결과, 환경(Environment), 통신(Network) 정보를 지속적으로 수집한다. 수천 대의 로봇은 하루 수 테라바이트(TB)의 데이터를 생성할 수 있으므로 스트림 처리(Stream Processing)와 계층형 저장(Storage)이 필요하다.

디지털 트윈(Digital Twin)은 실제 로봇을 가상 환경에서 동일하게 표현한다. 위치, 상태(State), 소프트웨어 버전, AI 성능, 유지보수 정보 등을 실시간으로 동기화하여 시뮬레이션(Simulation), 운영 분석, 교육(Training), 장애 분석(Failure Analysis)에 활용한다.

배터리 관리(Battery Management)는 대규모 플릿에서 중요한 서비스이다. 수백 대의 로봇이 동시에 충전하면 전력(Power) 인프라에 큰 부하가 발생한다. 따라서 배터리 상태, 작업 우선순위, 충전기 사용률을 고려하여 예측 충전(Predictive Charging)을 수행해야 한다.

예지 보전(Predictive Maintenance)은 운영 비용을 크게 절감한다. 모터(Motor), 감속기(Gearbox), 배터리, 센서, 컴퓨터, 브레이크(Brake) 등의 상태를 지속적으로 분석하여 고장이 발생하기 전에 유지보수를 수행함으로써 가동률(Availability)을 높일 수 있다.

장애 허용(Fault Tolerance)은 수천 대 규모의 플릿에서 필수적이다. 일부 로봇이나 서버의 장애는 항상 발생한다고 가정해야 한다. 이중화(Redundancy), 자동 복구(Self-Healing), 장애 조치(Failover), 메시지 영속성(Persistence), 롤링 업데이트(Rolling Update)를 통해 전체 시스템은 계속 운영되어야 한다.

네트워크(Network)는 플릿의 성능을 좌우한다. Wi-Fi, Private 5G, 산업용 이더넷(Industrial Ethernet), 메시 네트워크(Mesh Network), 위성 통신(Satellite Communication)을 함께 사용할 수 있으며, QoS(Quality of Service), 데이터 압축(Compression), 우선순위(Priority)를 적용하여 안정적인 통신을 유지한다.

보안(Security)은 로봇 플릿이 증가할수록 더욱 중요해진다. 모든 로봇은 네트워크에 연결된 사이버-물리 시스템(Cyber-Physical System)이므로 인증(Authentication), 권한 관리(Authorization), 암호화(Encryption), 보안 부팅(Secure Boot), 제로 트러스트(Zero Trust), 침입 탐지(Intrusion Detection)를 적용해야 한다.

관측성(Observability)은 플릿 운영의 핵심이다. 로봇 상태(Status), 위치 정확도(Localization Accuracy), 배터리 사용량, CPU, GPU, 센서 건강도(Sensor Health), 네트워크 품질(Network Quality), AI 추론 속도, 작업 완료율(Mission Completion Rate) 등을 지속적으로 모니터링하여 운영자가 전체 플릿을 한눈에 파악할 수 있도록 해야 한다.

AI 기반 관측성(Intelligent Observability)은 단순한 모니터링을 넘어 이상 탐지(Anomaly Detection), 장애 예측(Failure Prediction), 교통 예측(Traffic Prediction), 에너지 최적화(Energy Optimization), 작업 추천(Recommendation)까지 자동으로 수행하여 운영 효율을 크게 향상시킨다.

경제성(Economic Scalability)은 플릿 설계의 중요한 목표이다. 로봇 수가 100배 증가하더라도 운영 인력이 100배 증가해서는 안 된다. 따라서 소프트웨어 배포, 모니터링, 유지보수, AI 모델 관리, 보안 정책 적용, 디지털 트윈 운영까지 최대한 자동화(Automation)해야 한다.

조직 확장성(Organizational Scalability)도 고려해야 한다. 내비게이션(Navigation), AI, 클라우드, 보안, 유지보수, 디지털 트윈, DevOps 팀이 독립적으로 개발하면서도 표준 API(Application Programming Interface)를 통해 협력할 수 있어야 한다.

수십 대(Tens)의 로봇에서 수백 대(Hundreds)로 확장될 때는 통신과 모니터링의 복잡성이 증가하는 수준이지만, 수천 대(Thousands) 이상으로 확장되면 시스템은 완전한 분산 사이버-물리 생태계(Distributed Cyber-Physical Ecosystem)로 변화한다. 따라서 계층형 제어(Hierarchical Control), 클라우드-엣지 협업(Cloud-Edge Collaboration), 자동화, AI 기반 운영이 반드시 필요하다.

현대 로봇 플랫폼은 로봇 내부 자율성(Onboard Autonomy), 엣지 서버(Edge Server), 클라우드(Cloud), 메시지 브로커(Message Broker), 디지털 트윈(Digital Twin), AI 서비스, 관측성 플랫폼(Observability Platform), 보안(Security), 자동화(Automation)를 하나의 통합 아키텍처로 구성한다. 각 계층은 지연 시간(Latency), 계산량(Computation), 안정성(Reliability)에 따라 역할을 분담한다.

결론적으로 로봇 플릿 확장 설계(Robot Fleet Scaling Design)는 단순히 로봇의 수를 늘리는 기술이 아니라, 수천 대의 자율 로봇이 하나의 지능형 생태계(Intelligent Ecosystem)로 협력할 수 있도록 만드는 핵심 소프트웨어 아키텍처이다. 클라우드 네이티브, 마이크로서비스, 엣지 컴퓨팅, AI, 디지털 트윈, 예지 보전, 관측성, 자동화 기술을 통합함으로써 스마트 팩토리(Smart Factory), 지능형 물류(Intelligent Logistics), 자율 운송(Autonomous Transportation), 정밀 농업(Precision Agriculture), 산업 검사(Industrial Inspection), 물리 AI 시스템을 안정적이고 경제적으로 운영할 수 있는 기반을 제공한다.

## 09.07 Distributed Scaling with Edge Computing

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

분산 확장(Distributed Scaling)과 엣지 컴퓨팅(Edge Computing)은 현대 소프트웨어 아키텍처에서 가장 중요한 확장 전략 가운데 하나이다. 자율주행 로봇(Autonomous Robot), 산업 자동화(Industrial Automation), 스마트시티(Smart City), 커넥티드 차량(Connected Vehicle), 의료기기(Medical Device), IoT(Internet of Things), 물리 AI(Physical AI) 시스템은 실시간으로 대량의 데이터를 생성한다. 이러한 데이터를 모두 중앙 클라우드(Cloud)에서 처리하면 통신 지연(Latency), 대역폭(Bandwidth), 비용(Cost), 안정성(Reliability) 문제가 발생하므로 계산을 데이터가 생성되는 위치 가까이에서 수행하는 엣지 컴퓨팅이 필수적인 구조가 되었다.

기존의 클라우드 컴퓨팅(Cloud Computing)은 모든 데이터를 중앙 데이터센터(Data Center)로 전송하여 처리하는 방식이다. 이메일(Email), ERP(Enterprise Resource Planning), 웹 서비스(Web Service)와 같이 수백 밀리초(ms)의 응답 시간이 허용되는 시스템에는 적합하다. 그러나 자율주행, 산업 제어, 의료 장비와 같이 수 밀리초 수준의 응답이 필요한 시스템에서는 네트워크 지연이 치명적인 문제가 될 수 있다.

엣지 컴퓨팅은 계산을 중앙 클라우드에서 로컬(Local) 또는 사용자 가까운 위치로 이동시키는 기술이다. 산업용 컴퓨터(Industrial PC), 엣지 서버(Edge Server), 게이트웨이(Gateway), 로봇 온보드 컴퓨터(Onboard Computer), 통신사 MEC(Multi-access Edge Computing) 등이 데이터를 즉시 처리하여 빠른 응답을 제공한다. 반면 클라우드는 장기 저장(Storage), AI 학습(Model Training), 운영 분석(Analytics), 디지털 트윈(Digital Twin) 등을 담당한다.

분산 확장의 핵심 목적은 단순히 응답 시간을 줄이는 것이 아니라 작업(Task)을 가장 적절한 위치에서 수행하도록 분산하는 것이다. 실시간 제어(Control), 센서 융합(Sensor Fusion), 장애물 회피(Obstacle Avoidance)는 로컬에서 수행하고, AI 학습, 빅데이터 분석(Big Data Analytics), 글로벌 최적화(Global Optimization)는 클라우드에서 수행하는 계층적(Hierarchical) 구조가 가장 효율적이다.

분산 엣지 아키텍처는 일반적으로 장치 계층(Device Layer), 엣지 계층(Edge Layer), 클라우드 계층(Cloud Layer)으로 구성된다. 장치 계층은 센서(Sensor), 액추에이터(Actuator), 카메라(Camera), LiDAR, PLC(Programmable Logic Controller), 로봇 등을 포함한다. 엣지 계층은 산업용 서버, GPU 서버, 게이트웨이, 지역 데이터센터를 담당하며, 클라우드 계층은 AI 학습, 데이터 분석, 디지털 트윈, ERP 등 전사 시스템을 담당한다.

작업 배치(Workload Placement)는 엣지 컴퓨팅 설계에서 가장 중요한 요소이다. 응답 시간이 중요한 작업은 가장 가까운 장치에서 수행해야 한다. 모터 제어(Motor Control), 긴급 정지(Emergency Stop), 충돌 방지(Collision Avoidance)는 반드시 로컬에서 실행되어야 한다. 객체 인식(Object Recognition), 위치 추정(Localization), 음성 인식(Speech Recognition)은 엣지 서버에서 처리하고, AI 모델 학습, 통계 분석, 운영 최적화는 클라우드에서 수행하는 것이 일반적이다.

AI는 엣지 컴퓨팅을 발전시키는 가장 큰 원동력이다. 딥러닝(Deep Learning)은 산업 자동화, 로봇, 의료, 교통, 농업, 보안 등 다양한 분야에서 사용되지만 모든 추론(Inference)을 클라우드에서 수행하면 네트워크 의존성이 높아진다. 따라서 엣지 AI(Edge AI)는 추론을 로컬에서 수행하고, 클라우드는 학습(Training)과 모델 개선(Model Optimization)을 담당하는 협력 구조를 사용한다.

분산 확장은 수평 확장(Horizontal Scaling)을 기본 원칙으로 한다. 기존 서버를 더 큰 서버로 교체하는 대신 새로운 엣지 노드(Edge Node)를 추가하여 처리 능력을 높인다. 새로운 공장(Factory), 창고(Warehouse), 병원(Hospital), 농장(Farm), 물류센터(Logistics Center)가 추가될 때마다 독립적인 엣지 서버를 설치하여 자연스럽게 시스템을 확장할 수 있다.

컨테이너(Container)는 분산 확장을 매우 쉽게 만든다. 동일한 애플리케이션을 클라우드, 엣지 서버, 산업용 PC, 로봇 컴퓨터에서 동일하게 실행할 수 있기 때문이다. 쿠버네티스(Kubernetes), K3s, MicroK8s와 같은 경량 오케스트레이션(Orchestration) 플랫폼은 이러한 분산 배포를 자동으로 관리한다.

엣지 오케스트레이션(Edge Orchestration)은 클라우드보다 더 복잡한 환경을 관리한다. 엣지는 통신이 자주 끊기고 하드웨어 종류도 다양하며 전력(Power)과 네트워크(Network)가 제한될 수 있다. 따라서 자율 운영(Autonomous Operation), 자동 복구(Self-Healing), 원격 배포(Remote Deployment), 버전 관리(Version Management), 상태 모니터링(Health Monitoring)을 모두 지원해야 한다.

데이터 관리(Data Management)는 계층적으로 수행된다. 원시 데이터(Raw Data)는 장치 근처에 저장하고, 특징 데이터(Feature Data)는 엣지에서 관리하며, 요약 데이터(Summary Data)는 클라우드로 전송하여 장기 분석(Long-Term Analytics)과 AI 학습에 활용한다. 이를 통해 불필요한 네트워크 사용을 크게 줄일 수 있다.

엣지 캐시(Edge Cache)는 자주 사용하는 데이터를 로컬에 저장한다. AI 모델(Model), 지도(Map), 디지털 트윈(Digital Twin), 설정(Configuration), 소프트웨어 패키지(Software Package), 센서 보정값(Sensor Calibration) 등을 저장하여 반복적인 클라우드 접근을 줄이고 응답 속도를 향상시킨다.

이벤트 기반(Event-Driven) 통신은 분산 엣지 시스템과 매우 잘 어울린다. 센서 이벤트(Sensor Event), 로봇 위치(Location Update), 배터리 상태(Battery Status), AI 결과(Inference Result), 유지보수 알림(Maintenance Alert) 등을 메시지(Message) 형태로 전달하여 서비스 간 결합도를 낮추고 확장성을 높인다.

대역폭 최적화(Bandwidth Optimization)는 엣지 컴퓨팅의 중요한 목적이다. 고해상도 영상(Video), LiDAR 포인트 클라우드(Point Cloud), 레이더(Radar), 산업 센서 데이터는 그대로 클라우드에 전송하기에는 너무 크다. 엣지에서 전처리(Preprocessing), 압축(Compression), 특징 추출(Feature Extraction), 이상 탐지(Anomaly Detection)를 수행한 후 필요한 데이터만 클라우드에 전달한다.

분산 저장소(Distributed Storage)는 데이터 중요도에 따라 저장 위치를 달리한다. 실시간 데이터는 로컬에 저장하고, 자주 사용하는 데이터는 엣지 서버에 저장하며, 장기 보관 데이터는 클라우드에 저장한다. 이러한 계층형 저장(Tiered Storage)은 성능과 비용을 동시에 최적화한다.

일관성 관리(Consistency Management)는 분산 시스템에서 중요한 문제이다. 모든 데이터를 즉시 동기화하는 것은 현실적으로 어렵기 때문에 많은 시스템은 최종 일관성(Eventual Consistency)을 사용한다. 로컬은 독립적으로 동작하고, 일정 시간이 지나면 클라우드와 데이터를 동기화하는 방식이다.

장애 허용(Fault Tolerance)은 엣지 컴퓨팅의 중요한 장점이다. 클라우드와 연결이 끊겨도 로봇은 계속 주행할 수 있고, 공장은 생산을 지속할 수 있으며, 의료 장비는 환자를 계속 모니터링할 수 있다. 엣지의 독립적인 계산 능력은 시스템 전체의 안정성을 크게 향상시킨다.

보안(Security)은 분산 구조에서 더욱 중요하다. 모든 엣지 장치는 공격 대상이 될 수 있으므로 인증(Authentication), 권한 관리(Authorization), 암호화(Encryption), 보안 부팅(Secure Boot), 인증서 관리(Certificate Management), 제로 트러스트(Zero Trust)를 적용하여 클라우드와 동일한 수준의 보안을 유지해야 한다.

관측성(Observability)은 분산 시스템을 운영하기 위한 필수 요소이다. CPU, GPU, 메모리(Memory), 저장 공간(Storage), 네트워크 지연(Latency), AI 추론 시간(Inference Latency), 센서 상태(Sensor Health), 배터리 상태(Battery Health) 등을 클라우드와 엣지에서 동시에 모니터링하여 전체 시스템의 상태를 실시간으로 파악한다.

AI 기반 관측성(Intelligent Observability)은 단순한 모니터링을 넘어 이상 탐지(Anomaly Detection), 장애 예측(Failure Prediction), 통신 품질(Network Quality), 장비 열화(Thermal Degradation), 저장 공간 부족(Storage Exhaustion) 등을 미리 예측하여 장애 발생 전에 대응할 수 있도록 지원한다.

자율주행 로봇은 분산 엣지 컴퓨팅의 대표적인 사례이다. 로봇은 위치 추정(Localization), 장애물 회피, 모터 제어, AI 추론을 내부 컴퓨터에서 수행하고, 엣지 서버는 교통 관리(Traffic Management), 협업 지도(Collaborative Mapping), 임무 조정(Mission Coordination)을 담당한다. 클라우드는 AI 모델 학습, 디지털 트윈, 플릿 최적화(Fleet Optimization), 운영 분석을 수행한다.

산업 자동화에서도 동일한 구조를 사용한다. 생산 설비는 로컬에서 안전 제어(Safety Control)를 수행하고, 엣지 서버는 품질 검사(Quality Inspection), 예지 보전(Predictive Maintenance), AI 추론을 수행하며, 클라우드는 ERP, 공급망(Supply Chain), 생산 계획(Production Planning)을 담당한다.

의료 시스템(Healthcare System)은 환자 모니터링(Patient Monitoring), 의료 영상(Medical Imaging), 수술 로봇(Surgical Robot) 등을 엣지에서 처리하고, 연구 데이터와 장기 통계는 클라우드에서 관리한다. 이를 통해 응답성과 개인정보 보호(Privacy)를 동시에 만족시킬 수 있다.

스마트시티(Smart City)는 교통 신호(Traffic Signal), 환경 센서(Environment Sensor), 공공 안전(Public Safety), 에너지 관리(Energy Management)를 엣지에서 제어하며, 도시 전체의 디지털 트윈과 정책 분석은 중앙 클라우드에서 수행한다. 도시가 확장되더라도 지역 단위의 엣지 서버를 추가하여 자연스럽게 확장할 수 있다.

경제성(Economic Sustainability)도 엣지 컴퓨팅의 중요한 장점이다. 모든 데이터를 클라우드에 전송하면 네트워크 비용과 저장 비용이 크게 증가한다. 엣지에서 데이터를 선별하여 전송하면 클라우드 사용량을 줄일 수 있으며 전체 운영 비용도 절감된다.

그러나 분산 엣지 시스템은 중앙 집중형보다 복잡하다. 소프트웨어 배포(Deployment), 버전 관리(Version Management), 데이터 동기화(Synchronization), 보안(Security), 장애 분석(Fault Diagnosis), 운영 관리(Operation Management)가 훨씬 어려워진다. 따라서 자동화(Automation), Infrastructure as Code(IaC), 지속적 배포(CD, Continuous Deployment), 표준 API(Application Programming Interface)가 반드시 필요하다.

결과적으로 현대 시스템은 클라우드와 엣지를 함께 사용하는 하이브리드(Hybrid) 구조를 채택한다. 실시간 작업은 엣지에서 수행하고, 장기적인 분석과 AI 학습은 클라우드에서 수행하여 응답성, 확장성, 안정성, 비용 효율성을 동시에 확보한다.

결론적으로 분산 확장과 엣지 컴퓨팅(Distributed Scaling with Edge Computing)은 차세대 지능형 시스템(Intelligent System)의 핵심 아키텍처이다. 장치(Device), 게이트웨이(Gateway), 엣지 서버(Edge Server), 클라우드(Cloud)가 계층적으로 협력하여 계산을 분산함으로써 자율주행 로봇, 산업 자동화, IoT, 스마트시티, 의료 시스템, AI 플랫폼, 물리 AI 환경에서 높은 성능, 낮은 지연 시간, 높은 안정성, 뛰어난 확장성, 그리고 경제적인 운영을 동시에 실현할 수 있는 기반을 제공한다.

## 09.08 AI Inference Workload Scaling Architecture

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

AI 추론 워크로드 확장 아키텍처(AI Inference Workload Scaling Architecture)는 현대 인공지능(AI) 시스템에서 가장 중요한 설계 분야 가운데 하나이다. 자율주행 로봇(Autonomous Robot), 산업 자동화(Industrial Automation), 스마트 제조(Smart Manufacturing), 금융(Finance), 의료(Healthcare), IoT(Internet of Things), 물리 AI(Physical AI) 시스템은 하루에도 수백만\~수억 건의 AI 추론(Inference)을 수행한다. 따라서 정확한 AI 모델(Model)을 만드는 것뿐만 아니라, 대량의 추론 요청을 빠르고 안정적으로 처리할 수 있는 확장 가능한 아키텍처를 구축하는 것이 핵심 과제가 되었다.

AI 시스템은 크게 학습(Training)과 추론(Inference)으로 구분된다. 학습은 대규모 데이터셋(Dataset)을 이용하여 모델의 파라미터(Parameter)를 생성하는 과정이며 많은 GPU와 긴 시간이 필요하다. 반면 추론은 이미 학습된 모델을 이용하여 새로운 입력(Input)에 대해 결과를 생성하는 과정이다. 모델은 한 번 학습되지만 추론은 서비스가 운영되는 동안 지속적으로 수행되므로 전체 운영 비용과 성능은 대부분 추론 아키텍처에 의해 결정된다.

현대 AI 추론은 매우 동적인(Dynamic) 특성을 가진다. 사용자 수, 시간대(Time), 지역(Region), 공장 생산량, 로봇 운행 수, 이벤트(Event)에 따라 요청(Request)이 크게 변화한다. 따라서 고정된 서버만 사용하는 방식은 자원(Resource) 낭비나 성능 부족을 초래할 수 있다. AI 추론 시스템은 요청량 변화에 맞추어 자동으로 계산 자원을 확장하거나 축소할 수 있어야 한다.

AI 추론 확장은 단순히 GPU를 추가하는 것을 의미하지 않는다. 하드웨어(Hardware), 소프트웨어(Software), 모델(Model), 메모리(Memory), 네트워크(Network), 배치 처리(Batching), 스케줄링(Scheduling), 캐시(Cache), 관측성(Observability), 자동화(Automation)까지 모두 함께 최적화해야 한다. 이러한 요소가 균형을 이루어야 높은 처리량(Throughput)과 낮은 지연 시간(Latency)을 동시에 확보할 수 있다.

수평 확장(Horizontal Scaling)은 AI 추론 시스템에서 가장 많이 사용하는 방식이다. 하나의 강력한 서버만 사용하는 대신 여러 개의 추론 서버(Inference Server)가 동시에 요청을 처리한다. 새로운 서버를 추가할수록 처리 능력이 증가하므로 클라우드(Cloud) 환경에서 매우 효율적인 확장 방법이다.

수직 확장(Vertical Scaling)은 하나의 서버 성능을 높이는 방식이다. 더 강력한 GPU(Graphics Processing Unit), 많은 메모리, 빠른 저장장치(Storage), 고속 네트워크를 적용하여 추론 성능을 향상시킨다. 매우 큰 파운데이션 모델(Foundation Model)은 이러한 방식이 필요하지만 비용과 하드웨어 한계가 있으므로 장기적으로는 수평 확장이 함께 사용된다.

클라우드 네이티브(Cloud-Native)는 AI 추론 확장의 핵심 기반이다. 쿠버네티스(Kubernetes)와 같은 오케스트레이션(Orchestration) 플랫폼은 CPU, GPU, 메모리, 큐(Queue), 응답 시간(Response Time)을 지속적으로 분석하여 추론 서버를 자동으로 생성하거나 제거한다. 이를 통해 실제 요청량에 맞는 자원만 사용하여 비용을 절감할 수 있다.

컨테이너(Container)는 AI 모델을 어디에서나 동일하게 실행할 수 있도록 만든다. AI 모델과 라이브러리(Library), 실행 환경(Runtime)을 하나의 이미지(Image)로 패키징하여 클라우드, 엣지 서버(Edge Server), 산업용 PC, 로봇 컴퓨터에서 동일하게 실행할 수 있다. 따라서 배포(Deployment), 업데이트(Update), 롤백(Rollback)이 매우 쉬워진다.

AI 서빙 프레임워크(Inference Serving Framework)는 추론 서비스를 표준화하는 역할을 한다. 모델 로딩(Model Loading), 요청 처리(Request Processing), 배치(Batching), GPU 관리, 버전 관리(Version Management), 모니터링(Monitoring) 등을 자동으로 수행하여 개발자가 모델 자체에 집중할 수 있도록 지원한다.

로드 밸런싱(Load Balancing)은 AI 추론의 핵심 기술이다. 사용자의 요청을 여러 추론 서버에 균등하게 분산하여 특정 GPU만 과부하가 발생하는 것을 방지한다. CPU와 GPU 사용률(Utilization), 메모리 사용량, 네트워크 상태, 큐 길이(Queue Depth) 등을 고려하여 가장 적합한 서버로 요청을 전달한다.

배치 처리(Batching)는 GPU 활용률을 크게 높이는 기술이다. 여러 개의 추론 요청을 하나로 묶어 동시에 실행하면 GPU의 병렬 처리 능력을 최대한 활용할 수 있다. 그러나 배치가 너무 크면 응답 시간이 증가하므로 현재 요청량에 따라 자동으로 크기를 조절하는 적응형 배치(Adaptive Batching)가 많이 사용된다.

비동기 추론(Asynchronous Inference)은 요청과 실행을 분리하는 구조이다. 사용자는 요청이 접수되었다는 응답을 즉시 받고, 실제 AI 추론은 백그라운드(Background)에서 수행된다. 메시지 큐(Message Queue)를 이용하여 GPU 사용량을 최적화하고 대규모 요청을 안정적으로 처리할 수 있다.

큐 기반 처리(Queue-Based Processing)는 갑작스러운 요청 증가를 흡수하는 역할을 한다. 요청이 순간적으로 증가하더라도 큐에 저장한 후 GPU가 순차적으로 처리하므로 서버가 과부하되는 것을 방지할 수 있다. 필요하면 자동으로 새로운 GPU 서버를 추가하여 처리량을 증가시킨다.

모델 캐시(Model Cache)는 자주 사용하는 AI 모델을 GPU 메모리나 RAM에 계속 유지하는 기술이다. 매번 저장장치에서 모델을 다시 읽지 않아도 되므로 추론 시작 시간이 크게 줄어든다. 특히 대형 언어 모델(LLM, Large Language Model)에서는 필수적인 기술이다.

파라미터 공유(Parameter Sharing)는 여러 사용자가 동일한 모델을 동시에 사용할 때 메모리를 효율적으로 사용하는 방법이다. 동일한 모델을 여러 개 복사하지 않고 하나의 모델을 공유하여 GPU 메모리 사용량을 줄이고 처리 효율을 향상시킨다.

모델 최적화(Model Optimization)는 추론 성능을 크게 향상시킨다. 양자화(Quantization)는 숫자 정밀도를 줄여 메모리와 계산량을 감소시키고, 프루닝(Pruning)은 불필요한 연결(Connection)을 제거한다. 지식 증류(Knowledge Distillation)는 큰 모델의 성능을 작은 모델로 전달하여 추론 속도를 높인다.

AI 추론은 다양한 하드웨어를 함께 사용한다. GPU는 병렬 계산에 가장 적합하며, TPU(Tensor Processing Unit)는 행렬 연산을 최적화한다. FPGA(Field Programmable Gate Array)는 저전력 추론에 적합하고, NPU(Neural Processing Unit)는 스마트폰과 로봇의 엣지 AI에 많이 사용된다. CPU는 전처리와 제어를 담당한다.

엣지 AI(Edge AI)는 실시간성이 중요한 환경에서 사용된다. 자율주행 로봇, 산업 설비, 의료 장비, 드론(Drone), 스마트 카메라는 네트워크가 끊겨도 즉시 추론해야 하므로 로컬에서 AI를 실행한다. 클라우드는 모델 학습과 업데이트를 수행하고, 엣지는 실제 추론을 담당하는 협력 구조를 형성한다.

분산 추론(Distributed Inference)은 하나의 AI 모델을 여러 GPU 또는 여러 서버에 나누어 실행하는 기술이다. 수백억\~수천억 개의 파라미터(Parameter)를 가진 초거대 모델은 하나의 GPU에 모두 저장할 수 없기 때문에 여러 GPU가 협력하여 하나의 추론을 수행한다.

MoE(Mixture of Experts)는 필요한 일부 전문가 모델(Expert Model)만 활성화하는 구조이다. 모든 신경망을 실행하지 않고 입력 데이터에 맞는 일부 모델만 사용하므로 계산량을 크게 줄이면서도 높은 성능을 유지할 수 있다. 최신 대규모 언어 모델에서 많이 사용되는 기술이다.

RAG(Retrieval-Augmented Generation)는 AI 모델이 외부 데이터베이스(Vector Database)에서 관련 정보를 검색한 후 답변을 생성하는 구조이다. 따라서 추론 과정에는 벡터 검색(Vector Search), 임베딩(Embedding), 문맥(Context) 생성, LLM 추론이 함께 수행되므로 전체 추론 파이프라인(Pipeline)의 확장성이 매우 중요하다.

메모리 관리(Memory Management)는 초거대 AI 모델에서 가장 중요한 요소이다. 메모리 풀(Memory Pool), 텐서 재사용(Tensor Reuse), 파라미터 스트리밍(Parameter Streaming), 메모리 오프로딩(Offloading) 등을 사용하여 제한된 GPU 메모리에서도 큰 모델을 실행할 수 있도록 최적화한다.

통신 최적화(Communication Optimization)는 분산 추론에서 매우 중요하다. 여러 GPU가 중간 계산 결과를 지속적으로 교환하므로 고속 네트워크, 비동기 통신(Asynchronous Communication), 텐서 압축(Tensor Compression), 통신 오버랩(Communication Overlap)을 이용하여 통신 시간을 최소화한다.

관측성(Observability)은 AI 추론 시스템 운영의 핵심이다. 처리량(Throughput), 응답 시간(Latency), GPU 사용률(Utilization), 메모리 사용량, 배치 효율(Batch Efficiency), 캐시 적중률(Cache Hit Ratio), 큐 길이, 에너지 소비(Energy Consumption)를 지속적으로 모니터링하여 시스템을 최적화한다.

AI 관측성(AI Observability)은 단순한 시스템 모니터링을 넘어 모델 자체를 분석한다. 추론 신뢰도(Confidence), 데이터 드리프트(Data Drift), 개념 드리프트(Concept Drift), 환각(Hallucination), 공정성(Fairness), 이상 탐지(Anomaly Detection)를 지속적으로 분석하여 모델 품질을 유지한다.

보안(Security)은 AI 추론에서도 매우 중요하다. AI 모델은 중요한 지적 재산(Intellectual Property)이므로 인증(Authentication), 권한 관리(Authorization), 암호화(Encryption), 보안 실행 환경(Trusted Execution Environment), 모델 워터마킹(Watermarking), 접근 제어(Access Control)를 적용하여 모델과 데이터를 보호해야 한다.

운영 비용(Cost Optimization)은 AI 서비스에서 가장 큰 문제 가운데 하나이다. GPU 공유(GPU Sharing), 자동 스케일링(Auto Scaling), 적응형 배치(Adaptive Batching), 모델 최적화, 우선순위 기반 스케줄링(Priority Scheduling), 클라우드와 엣지의 작업 분산을 통해 비용을 최소화하면서도 높은 성능을 유지해야 한다.

자율주행 로봇은 AI 추론 확장 아키텍처의 대표적인 사례이다. 로봇 내부에서는 객체 인식(Object Recognition), 위치 추정(Localization), 장애물 회피, 경로 계획(Path Planning)을 수행하고, 엣지 서버는 협업 지도(Collaborative Mapping), 플릿 관리(Fleet Management), 고성능 비전(Vision)을 담당한다. 클라우드는 AI 학습, 디지털 트윈(Digital Twin), 예지 보전(Predictive Maintenance), 운영 분석을 수행한다.

산업 자동화에서는 제품 검사(Quality Inspection), 이상 탐지(Anomaly Detection), 설비 진단(Equipment Diagnostics), 예지 보전을 AI 추론으로 수행한다. 엣지 서버는 실시간 분석을 담당하고, 클라우드는 생산 공장 전체를 분석하여 장기적인 최적화를 수행한다.

의료 시스템은 의료 영상(Medical Imaging), 환자 모니터링(Patient Monitoring), 수술 로봇(Surgical Robot) 등에서 AI 추론을 수행한다. 민감한 의료 데이터는 병원 내부에서 처리하고, 익명화된 데이터만 클라우드에서 연구와 모델 개선에 활용하여 개인정보 보호와 AI 성능을 동시에 확보한다.

대규모 언어 모델(LLM)은 AI 추론 확장의 가장 어려운 분야이다. 프롬프트 캐시(Prompt Cache), 토큰 캐시(Token Cache), 연속 배치(Continuous Batching), 스트리밍 생성(Streaming Generation), 추측 디코딩(Speculative Decoding), RAG 등을 이용하여 응답 시간을 줄이고 GPU 비용을 절감한다.

장애 허용(Fault Tolerance)은 AI 추론 시스템에서 필수적이다. GPU 장애나 서버 장애가 발생해도 자동 장애 조치(Failover), 롤링 업데이트(Rolling Update), 상태 모니터링(Health Monitoring), 셀프 힐링(Self-Healing)을 통해 AI 서비스는 중단 없이 계속 운영되어야 한다.

현대 AI 플랫폼은 클라우드와 엣지를 함께 사용하는 하이브리드(Hybrid) 추론 구조를 채택한다. 실시간 추론은 엣지에서 수행하고, 복잡한 추론과 대규모 언어 모델은 클라우드에서 수행하여 응답성, 확장성, 비용 효율성을 동시에 확보한다.

결론적으로 AI 추론 워크로드 확장 아키텍처(AI Inference Workload Scaling Architecture)는 대규모 AI 서비스를 안정적으로 운영하기 위한 핵심 기술이다. 클라우드 네이티브, 컨테이너, 자동 스케일링, 로드 밸런싱, 배치 처리, 비동기 처리, 모델 최적화, GPU 활용, 엣지 AI, AI 관측성, 보안 기술을 통합함으로써 자율주행 로봇, 산업 자동화, 의료, 금융, 스마트시티, 디지털 트윈, 물리 AI 환경에서 높은 성능과 낮은 지연 시간, 뛰어난 확장성, 그리고 경제적인 운영을 동시에 실현할 수 있는 기반을 제공한다.

## 09.09 Geo-Distributed Robot Fleet Design

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

지리적으로 분산된 로봇 플릿 설계(Geo-Distributed Robot Fleet Design)는 현대 자율주행 로봇 시스템에서 가장 중요한 아키텍처 분야 가운데 하나이다. 과거에는 하나의 공장이나 물류센터에서 소수의 로봇을 운영하는 것이 일반적이었지만, 현재는 제조(Manufacturing), 물류(Logistics), 의료(Healthcare), 농업(Agriculture), 스마트시티(Smart City), 국방(Defense), 산업 검사(Industrial Inspection), 물리 AI(Physical AI) 분야에서 여러 도시와 국가에 걸쳐 수천 대의 로봇을 동시에 운영하는 시대가 되었다. 따라서 전체 플릿을 효율적으로 관리할 수 있는 글로벌 아키텍처가 필수적인 요소가 되었다.

지리적으로 분산된 플릿은 기존의 중앙 집중형(Centralized) 플릿과 근본적으로 다르다. 단일 공장에서는 하나의 중앙 서버가 모든 로봇을 직접 관리할 수 있지만, 여러 국가와 대륙에 분산된 로봇은 통신 지연(Latency), 네트워크(Network) 품질, 규제(Regulation), 환경(Environment), 운영 정책(Operation Policy)이 모두 다르다. 따라서 하나의 중앙 서버가 모든 것을 제어하는 방식은 더 이상 적합하지 않다.

지리적 분산 플릿의 핵심 목표는 단순히 전 세계의 로봇을 인터넷으로 연결하는 것이 아니다. 각 지역(Region)의 로봇이 독립적으로 운영되면서도, 클라우드(Cloud)를 통해 기업 전체의 운영 정보, AI 모델(AI Model), 유지보수(Maintenance), 운영 분석(Analytics), 디지털 트윈(Digital Twin)을 공유하는 것이다. 즉, 지역 자율성(Local Autonomy)과 글로벌 협업(Global Collaboration)을 동시에 만족해야 한다.

계층형(Hierarchical) 아키텍처는 이러한 구조의 핵심이다. 가장 아래 계층은 개별 로봇(Robot)으로, 위치 추정(Localization), 장애물 회피(Obstacle Avoidance), 경로 계획(Path Planning), 안전 제어(Safety Control), AI 추론(Inference)을 수행한다. 그 위에는 지역 엣지 서버(Regional Edge Server)가 있으며 임무 관리(Mission Management), 교통 관리(Traffic Management), 충전 관리(Charging Management), 텔레메트리(Telemetry)를 담당한다. 최상위에는 글로벌 클라우드(Global Cloud)가 위치하여 AI 학습(Model Training), 운영 분석, 디지털 트윈, 소프트웨어 배포(Deployment)를 수행한다.

지역 자율성(Regional Autonomy)은 지리적 분산 시스템에서 가장 중요한 원칙이다. 클라우드와 연결이 끊기더라도 지역 서버와 로봇은 계속 운영되어야 한다. 임무 수행(Mission Execution), 충돌 방지(Collision Avoidance), 충전 스케줄링(Charging Scheduling), 긴급 대응(Emergency Response)은 모두 지역 시스템에서 독립적으로 처리할 수 있어야 한다. 클라우드는 운영을 지원하는 역할이지, 필수 제어 장치는 아니다.

클라우드와 엣지의 협업(Cloud-Edge Collaboration)은 실시간성과 확장성을 동시에 만족시키는 구조이다. 실시간 제어는 로봇과 엣지에서 수행하고, AI 학습, 운영 최적화(Global Optimization), 디지털 트윈, 장기 분석(Long-Term Analytics)은 클라우드에서 수행한다. 작업의 특성에 따라 적절한 계층에서 계산을 수행하는 것이 핵심이다.

통신 구조(Communication Architecture)는 플릿 확장성을 결정하는 중요한 요소이다. 모든 로봇이 중앙 서버에 지속적으로 요청(Request)을 보내는 방식은 확장성이 낮다. 대신 이벤트 기반(Event-Driven), 발행-구독(Publish-Subscribe), 메시지 브로커(Message Broker)를 이용하여 로봇은 위치(Location), 배터리(Battery), AI 결과(Inference Result), 임무 완료(Mission Complete) 등의 이벤트만 전송하고 필요한 정보만 공유한다.

광역 네트워크(WAN, Wide Area Network)는 국가 간 운영에서 매우 중요한 요소이다. 같은 공장 안에서는 수 밀리초(ms)의 응답 시간이 가능하지만 대륙 간 통신은 수백 밀리초가 걸릴 수 있다. 따라서 실시간 제어는 반드시 지역에서 수행하고, 클라우드는 실시간성이 낮은 업무만 담당하도록 역할을 구분해야 한다.

네트워크 장애(Network Failure)에 대비한 설계도 필수적이다. Wi-Fi, Private 5G, 공용 이동통신(Cellular), 위성 통신(Satellite), SD-WAN(Software Defined Wide Area Network), 메시 네트워크(Mesh Network)를 함께 구성하여 하나의 통신망이 장애가 발생하더라도 다른 통신망으로 자동 전환(Failover)할 수 있도록 설계해야 한다.

로봇 신원 관리(Identity Management)는 글로벌 플릿에서 매우 중요하다. 모든 로봇은 고유 ID(ID), 인증(Authentication), 인증서(Certificate), 운영 이력(Operation History), 하드웨어(Hardware), 소프트웨어 버전(Software Version), 유지보수 기록(Maintenance History)을 중앙에서 관리해야 한다. 이를 통해 전 세계 어느 지역에서도 안전한 인증과 운영이 가능해진다.

소프트웨어 생명주기 관리(Software Lifecycle Management)는 수천 대의 로봇을 운영할 때 필수적인 기능이다. 운영체제(OS), 펌웨어(Firmware), AI 모델, 보안 패치(Security Patch)를 지속적으로 배포해야 하며, 점진적 배포(Canary Deployment), 롤백(Rollback), 버전 관리(Version Management)를 통해 장애 발생 위험을 최소화해야 한다.

AI는 지리적 분산 플릿의 핵심 요소이다. 로봇은 객체 인식(Object Recognition), 위치 추정(Localization), 자연어 처리(NLP, Natural Language Processing), 이상 탐지(Anomaly Detection)를 로컬에서 수행한다. 엣지 서버는 협업 인식(Collaborative Perception)과 공유 지도(Shared Mapping)를 담당하며, 클라우드는 전 세계 데이터를 이용하여 AI 모델을 지속적으로 학습하고 개선한다.

연합 학습(Federated Learning)은 글로벌 AI 운영에서 중요한 기술이다. 지역에서 생성된 데이터를 클라우드로 직접 전송하지 않고, AI 모델의 학습 결과만 공유한다. 이를 통해 개인정보 보호(Privacy), 데이터 주권(Data Sovereignty), 통신 비용(Bandwidth)을 동시에 해결할 수 있다.

디지털 트윈(Digital Twin)은 전 세계 로봇의 가상 복제(Virtual Replica)를 생성한다. 로봇 상태(Status), 위치(Position), AI 성능, 배터리(Battery), 센서(Sensor), 소프트웨어 버전 등을 실시간으로 동기화하여 운영 분석, 시뮬레이션(Simulation), 장애 분석(Failure Analysis), 교육(Training)에 활용한다.

지도 관리(Map Management)는 여러 국가에서 운영하는 플릿에서 매우 중요하다. 하나의 거대한 지도를 사용하는 것이 아니라 지역별 지도(Map Segment)를 독립적으로 관리하고, 버전 관리(Version Control), 차등 업데이트(Differential Update), 접근 제어(Access Control)를 통해 필요한 지도만 해당 지역의 로봇에 배포한다.

임무 관리(Mission Management)는 지역과 글로벌 계층으로 나누어진다. 지역에서는 개별 작업(Task)을 수행하고, 글로벌 수준에서는 여러 국가에 걸친 공급망(Supply Chain), 물류(Logistics), 생산(Manufacturing) 계획을 조정한다. 계층형 오케스트레이션(Hierarchical Orchestration)이 이러한 역할을 담당한다.

플릿 스케줄링(Fleet Scheduling)도 계층적으로 수행된다. 지역 스케줄러(Local Scheduler)는 충전(Charging), 배터리, 작업량을 고려하여 로봇을 운영하고, 글로벌 스케줄러(Global Scheduler)는 생산 계획, 물류 계획, 유지보수 일정을 종합적으로 최적화한다.

텔레메트리(Telemetry)는 글로벌 플릿에서 가장 큰 데이터(Data)를 생성한다. 위치 정보, 센서 데이터, 모터 상태, AI 결과, 네트워크 품질, 환경 정보 등이 지속적으로 생성되며 연간 페타바이트(PB) 규모까지 증가할 수 있다. 따라서 엣지에서 데이터를 요약(Summarization)하고 필요한 정보만 클라우드에 전달하는 계층형 데이터 관리가 필요하다.

분산 저장소(Distributed Storage)는 데이터 중요도에 따라 계층적으로 운영된다. 실시간 데이터는 로컬에 저장하고, 지역 데이터는 엣지 서버에서 관리하며, 장기 데이터는 클라우드에 저장한다. 이러한 구조는 응답성과 비용 효율성을 동시에 확보한다.

관측성(Observability)은 전 세계 플릿을 운영하기 위한 핵심 기능이다. 로봇 상태(Status), AI 성능, 위치 정확도(Localization Accuracy), 배터리 상태, GPU 사용률, 네트워크 지연(Latency), 작업 완료율(Mission Completion Rate) 등을 통합 대시보드(Dashboard)에서 실시간으로 확인할 수 있어야 한다.

AI 기반 관측성(Intelligent Observability)은 단순한 모니터링을 넘어 장애 예측(Failure Prediction), 통신 이상(Network Anomaly), 센서 열화(Sensor Drift), 배터리 수명(Battery Degradation), 교통 혼잡(Traffic Congestion)을 사전에 예측하여 운영 효율을 높인다.

사이버 보안(Cybersecurity)은 글로벌 플릿에서 가장 중요한 요소 가운데 하나이다. 모든 로봇, 엣지 서버, 클라우드, 운영자 콘솔(Console), 디지털 트윈은 인증(Authentication), 암호화(Encryption), 보안 부팅(Secure Boot), 제로 트러스트(Zero Trust), 침입 탐지(Intrusion Detection)를 적용하여 안전하게 보호되어야 한다.

국가마다 규제(Regulation)가 다르기 때문에 지역별 정책도 지원해야 한다. 개인정보 보호법, 산업 안전 규정, AI 규제, 데이터 저장 위치(Data Residency), 수출 규제(Export Control) 등이 국가마다 다르므로 동일한 시스템에서도 지역별 운영 정책을 다르게 적용할 수 있어야 한다.

경제성(Economic Optimization)도 매우 중요한 설계 목표이다. 클라우드는 필요한 만큼만 사용하고, 엣지는 여러 지역이 함께 공유하며, AI 추론은 가장 비용 효율적인 위치에서 수행하도록 작업을 분산해야 한다. 이를 통해 운영 비용을 최소화하면서도 높은 성능을 유지할 수 있다.

조직 확장성(Organizational Scalability)도 함께 고려해야 한다. AI 팀, 클라우드 팀, 로봇 팀, 보안 팀, 디지털 트윈 팀, DevOps 팀이 독립적으로 개발하면서도 표준 API(Application Programming Interface)를 통해 상호 협력할 수 있는 구조가 필요하다.

재해 복구(Disaster Recovery)는 글로벌 운영에서 반드시 고려해야 한다. 특정 지역의 데이터센터나 공장이 장애를 일으키더라도 다른 지역에서 서비스를 계속 운영할 수 있도록 지역 간 복제(Cross-Region Replication), 자동 장애 조치(Failover), 독립 운영(Local Autonomy)을 지원해야 한다.

지속 가능성(Sustainability)도 중요한 설계 요소이다. 엣지 AI를 이용하여 불필요한 데이터 전송을 줄이고, 예측 충전(Predictive Charging)과 에너지 최적화(Energy Optimization)를 통해 전력 소비를 최소화하며, 지역에서 처리 가능한 작업은 지역에서 수행하여 탄소 배출(Carbon Emission)을 줄이는 친환경(Green Computing) 아키텍처를 구축할 수 있다.

지리적으로 분산된 플릿은 글로벌 지식 공유(Global Knowledge Sharing)를 가능하게 한다. 하나의 공장에서 학습한 AI 모델과 운영 경험은 다른 국가의 공장에도 즉시 적용될 수 있다. 따라서 전 세계 로봇이 함께 학습하고 발전하는 지속적 학습(Continuous Learning) 생태계를 구축할 수 있다.

결국 지리적 분산 로봇 플릿 설계(Geo-Distributed Robot Fleet Design)는 단순히 여러 지역의 로봇을 연결하는 기술이 아니라, 전 세계에 분산된 수천 대의 자율 로봇을 하나의 지능형 생태계(Intelligent Ecosystem)로 통합하는 핵심 아키텍처이다. 클라우드-엣지 협업(Cloud-Edge Collaboration), 계층형 제어(Hierarchical Control), AI, 디지털 트윈, 연합 학습(Federated Learning), 관측성, 자동화, 사이버 보안 기술을 통합함으로써 글로벌 물류(Global Logistics), 스마트 제조(Smart Manufacturing), 의료 로봇(Healthcare Robotics), 스마트시티(Smart City), 정밀 농업(Precision Agriculture), 산업 검사(Industrial Inspection), 물리 AI 환경을 안정적이고 효율적으로 운영할 수 있는 기반을 제공한다.

## 09.10 Scalability Testing: Load and Stress Test Methodology

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

확장성 테스트(Scalability Testing), 부하 테스트(Load Testing), 스트레스 테스트(Stress Testing)는 현대 소프트웨어 아키텍처에서 시스템이 증가하는 작업량(Workload)에서도 안정적인 성능(Performance), 응답성(Response Time), 신뢰성(Reliability), 경제성(Economic Efficiency)을 유지할 수 있는지를 검증하는 핵심 방법론이다. 클라우드(Cloud), AI 플랫폼(AI Platform), 자율주행 로봇(Autonomous Robot), 산업 자동화(Industrial Automation), IoT(Internet of Things), 디지털 트윈(Digital Twin), 물리 AI(Physical AI) 시스템은 지속적으로 증가하는 사용자와 데이터, 연산량을 처리해야 하므로 이러한 테스트는 필수적인 개발 과정이다.

확장성 테스트는 기능 테스트(Functional Testing)와 목적이 다르다. 기능 테스트는 시스템이 올바르게 동작하는지를 확인하지만, 확장성 테스트는 사용자가 증가하거나 데이터가 커질 때 시스템의 성능이 어떻게 변화하는지를 분석한다. 작은 규모에서는 정상적으로 동작하는 시스템도 수천 명의 사용자나 수백 대의 로봇이 동시에 접속하면 병목(Bottleneck)이 발생할 수 있으므로, 아키텍처 자체를 검증하는 과정이 필요하다.

성능 테스트(Performance Testing)는 여러 가지 방법으로 구분된다. 부하 테스트는 예상되는 실제 운영 환경의 작업량을 지속적으로 가하여 성능을 측정한다. 스트레스 테스트는 시스템의 한계를 넘는 과도한 부하를 발생시켜 장애 발생 시점과 복구 능력을 확인한다. 스파이크 테스트(Spike Testing)는 갑작스러운 트래픽 증가에 대한 대응 능력을 평가하며, 장시간 테스트(Soak Testing)는 메모리 누수(Memory Leak), 자원 고갈(Resource Exhaustion), 성능 저하를 확인하기 위해 수시간에서 수일 동안 지속적으로 시스템을 운영한다.

확장성 테스트의 가장 중요한 목적은 실제 서비스 전에 병목 지점(Bottleneck)을 발견하는 것이다. CPU, GPU, 메모리(Memory), 데이터베이스(Database), 저장장치(Storage), 네트워크(Network), 메시지 큐(Message Queue), 캐시(Cache), 동기화(Synchronization), 알고리즘(Algorithm) 등은 모두 잠재적인 병목 요소가 될 수 있다. 이러한 문제를 사전에 발견하면 운영 중 발생할 수 있는 장애를 크게 줄일 수 있다.

효과적인 확장성 테스트를 위해서는 현실적인 작업 부하 모델(Workload Model)을 먼저 정의해야 한다. 실제 서비스에서는 시간(Time), 지역(Region), 계절(Season), 생산 일정(Production Schedule), 물류(Logistics), 로봇 운행(Robot Mission) 등에 따라 요청(Request)이 크게 변한다. 따라서 테스트 환경도 이러한 실제 사용 패턴을 최대한 반영해야 신뢰성 있는 결과를 얻을 수 있다.

작업 부하 특성 분석(Workload Characterization)은 테스트의 기초가 된다. 동시 사용자 수(Concurrent Users), 요청 빈도(Request Rate), 세션(Session) 길이, 데이터 크기(Data Size), AI 추론(Inference), 데이터베이스 접근(Database Access), 네트워크 트래픽(Network Traffic), 센서 데이터(Sensor Data) 등을 분석하여 실제 운영 환경과 유사한 테스트 시나리오를 구성한다.

확장성은 일반적으로 사용자(User), 데이터(Data), 계산(Computation)의 세 가지 관점에서 평가한다. 사용자 수가 증가할 때 성능이 유지되는지, 데이터베이스 크기가 증가할 때 처리 속도가 유지되는지, AI 추론이나 시뮬레이션(Simulation)과 같은 계산량이 증가할 때 시스템이 안정적으로 동작하는지를 모두 확인해야 한다.

성능 지표(Metric)는 확장성 테스트의 핵심 평가 기준이다. 응답 시간(Response Time), 처리량(Throughput), CPU와 GPU 사용률(Utilization), 메모리 사용량, 저장장치 처리 속도(Storage Throughput), 네트워크 대역폭(Bandwidth), 큐 길이(Queue Depth), 캐시 적중률(Cache Hit Ratio), 에너지 소비(Energy Consumption), 오류율(Error Rate) 등을 종합적으로 분석하여 시스템의 상태를 평가한다.

응답 시간은 평균값(Average)만으로는 충분하지 않다. 평균은 정상적이더라도 일부 요청이 매우 느릴 수 있기 때문이다. 따라서 중앙값(Median), 95퍼센타일(P95), 99퍼센타일(P99), 최대 응답 시간(Maximum Latency) 등을 함께 분석하여 실제 사용자 경험(User Experience)을 평가한다.

동시성 테스트(Concurrency Testing)는 수천\~수백만 개의 요청이 동시에 발생할 때 시스템이 어떻게 동작하는지를 분석한다. 스레드(Thread), 비동기 처리(Asynchronous Processing), 이벤트(Event), 메시지 큐, 데이터베이스 연결(Connection Pool), GPU 공유(GPU Sharing) 등을 분석하여 동시 처리 능력을 평가한다.

부하 테스트(Load Testing)는 예상되는 운영 수준까지 점진적으로 작업량을 증가시키면서 시스템의 성능을 측정한다. CPU와 GPU 사용량, 응답 시간, 메모리, 처리량 등을 지속적으로 기록하여 확장성이 선형적으로 증가하는지 확인한다. 이상적인 구조라면 자원을 추가할수록 처리량도 거의 비례하여 증가해야 한다.

스트레스 테스트(Stress Testing)는 설계 용량을 초과하는 부하를 의도적으로 발생시킨다. 이를 통해 메모리 부족, 데이터베이스 병목, GPU 포화(Saturation), 통신 지연(Network Congestion), 데드락(Deadlock), 서비스 중단(Service Failure) 등을 확인할 수 있다. 목표는 시스템을 망가뜨리는 것이 아니라 어떤 방식으로 실패(Failure)하는지를 이해하는 것이다.

우수한 시스템은 급격한 장애 대신 점진적 성능 저하(Graceful Degradation)를 보여야 한다. 예를 들어 AI 플랫폼은 추론 정확도를 조금 낮추거나, 로봇 플릿(Fleet)은 우선순위가 낮은 임무를 연기하며, 클라우드는 일부 요청을 대기시키는 방식으로 핵심 기능은 계속 유지해야 한다.

스파이크 테스트(Spike Testing)는 갑작스러운 트래픽 증가를 평가한다. 이벤트(Event), 프로모션(Promotion), 생산 증가, 긴급 상황(Emergency), 대규모 로봇 임무 시작과 같이 순간적으로 요청이 폭증할 때 자동 확장(Auto Scaling), 로드 밸런싱(Load Balancing), 메시지 큐가 정상적으로 동작하는지를 확인한다.

장시간 테스트(Soak Testing)는 수시간 또는 수일 동안 동일한 부하를 유지하여 시스템의 장기 안정성을 평가한다. 메모리 누수, 로그(Log) 증가, 저장 공간 부족(Storage Exhaustion), 캐시(Cache) 문제, 운영체제(OS) 자원 고갈 등을 확인할 수 있으며 실제 운영 환경에서 매우 중요한 테스트이다.

용량 테스트(Capacity Testing)는 시스템이 안정적으로 처리할 수 있는 최대 용량(Maximum Capacity)을 측정한다. 응답 시간, 처리량, GPU 사용률, AI 추론 성능, 로봇 임무 완료율 등을 기준으로 실제 서비스 가능한 최대 사용자 수와 최대 로봇 수를 계산한다.

탄력성 테스트(Elasticity Testing)는 클라우드 네이티브(Cloud-Native) 환경에서 매우 중요하다. CPU, GPU, 메모리, 큐 길이 등을 기준으로 컨테이너(Container)나 서버(Server)가 자동으로 생성되고 제거되는지 확인하여 비용과 성능이 동시에 최적화되는지를 평가한다.

분산 시스템(Distributed System)은 일반적인 애플리케이션보다 테스트가 훨씬 복잡하다. 마이크로서비스(Microservices), 메시지 브로커(Message Broker), 서비스 디스커버리(Service Discovery), 컨테이너(Container), 엣지 컴퓨팅(Edge Computing), 클라우드(Cloud)가 함께 동작하므로 개별 서비스뿐 아니라 전체 시스템의 협력 동작을 함께 분석해야 한다.

데이터베이스(Database)는 가장 흔한 병목 요소이다. 쿼리(Query), 인덱스(Index), 복제(Replication), 샤딩(Sharding), 연결(Connection Pool), 캐시(Cache), 읽기(Read), 쓰기(Write) 성능을 각각 측정하여 데이터 증가에 따른 확장성을 분석한다.

캐시(Cache)는 시스템 성능에 큰 영향을 준다. 캐시 적중률(Cache Hit Ratio), 초기 캐시 생성(Cache Warm-up), 캐시 제거(Eviction), 분산 캐시(Distributed Cache) 동기화 등을 테스트하여 데이터베이스 접근을 얼마나 줄일 수 있는지 평가한다.

메시지 큐(Message Queue)는 비동기 시스템에서 핵심 요소이다. 큐 길이(Queue Depth), 메시지 처리량(Message Throughput), 소비자(Consumer) 수, 재시도(Retry), 영속성(Persistence), 파티셔닝(Partitioning) 등을 평가하여 대규모 이벤트(Event) 처리 능력을 검증한다.

AI 추론(Inference)은 일반적인 웹 서비스보다 훨씬 복잡하다. GPU 사용률(Utilization), 모델 로딩(Model Loading), 배치 처리(Batching), GPU 메모리(Graphics Memory), 추론 지연(Inference Latency), 에너지 소비(Energy Consumption)를 모두 측정하여 AI 서비스의 확장성을 평가한다.

자율주행 로봇 플릿은 별도의 확장성 테스트가 필요하다. 단순한 웹 요청이 아니라 로봇 수 증가, AI 추론 증가, 센서 데이터 증가, 텔레메트리(Telemetry), 지도(Map), 디지털 트윈(Digital Twin), 교통 관리(Traffic Management)를 동시에 증가시키면서 시스템이 정상적으로 동작하는지를 확인해야 한다.

지리적으로 분산된(Geo-Distributed) 시스템은 여러 국가와 지역에서 테스트를 수행해야 한다. 국가 간 통신 지연, 네트워크 품질, 클라우드 지역(Cloud Region), 엣지 서버(Edge Server), 데이터 복제(Replication), AI 모델 배포 등을 종합적으로 검증하여 글로벌 서비스의 안정성을 확보한다.

장애 주입(Fault Injection)은 실제 장애를 인위적으로 발생시키는 테스트이다. CPU, GPU, 데이터베이스, 네트워크, 메시지 큐, 엣지 서버, AI 서비스 등을 일부러 중단시켜 시스템이 자동으로 복구(Self-Healing)하는지 확인한다.

카오스 엔지니어링(Chaos Engineering)은 장애 주입을 더욱 발전시킨 개념이다. 운영 환경과 유사한 상태에서 지속적으로 장애를 발생시켜 시스템의 복원력(Resilience)을 검증한다. 자동 장애 조치(Failover), 메시지 영속성(Persistence), 중복(Redundancy), 자동 복구(Self-Healing)를 평가하는 대표적인 방법이다.

관측성(Observability)은 확장성 테스트의 핵심 기반이다. CPU, GPU, 메모리, 저장장치, 네트워크, AI 추론 시간, 센서 데이터, 로그(Log), 트레이스(Trace), 메트릭(Metric)을 실시간으로 수집하여 병목 현상을 정확하게 분석한다.

분산 추적(Distributed Tracing)은 마이크로서비스 환경에서 매우 중요하다. 하나의 요청(Request)이 여러 서비스를 거치면서 어디에서 지연(Latency)이 발생하는지를 확인할 수 있으며, 네트워크 병목, 데이터베이스 지연, 인증(Authentication), AI 추론 시간을 정확하게 분석할 수 있다.

테스트 결과는 통계 분석(Statistical Analysis)을 통해 해석해야 한다. 여러 번 반복 측정하여 신뢰 구간(Confidence Interval)을 계산하고, 회귀 분석(Regression Analysis), 큐잉 이론(Queueing Theory), 용량 예측(Capacity Forecasting)을 통해 향후 시스템 확장 계획을 수립한다.

경제성(Economic Efficiency)도 중요한 평가 요소이다. GPU, 클라우드, 네트워크, 저장장치, 전력(Power), 냉각(Cooling), 유지보수 비용을 모두 고려하여 단순한 최고 성능이 아니라 비용 대비 성능(Performance per Dollar)을 평가해야 한다.

확장성 테스트는 개발 완료 후 한 번 수행하는 작업이 아니라 지속적 통합(CI, Continuous Integration)과 지속적 배포(CD, Continuous Deployment)에 포함되어야 한다. 새로운 기능이나 AI 모델이 추가될 때마다 자동으로 성능을 검증하여 성능 저하(Regression)를 조기에 발견해야 한다.

운영 환경(Production)의 실제 데이터를 테스트에 지속적으로 반영하는 것도 중요하다. 실제 사용자 패턴, 로봇 운행 기록, AI 추론 요청을 분석하여 테스트 시나리오를 개선하면 운영 환경과 테스트 환경의 차이를 최소화할 수 있다.

결론적으로 확장성 테스트, 부하 테스트, 스트레스 테스트 방법론(Scalability Testing, Load Testing, and Stress Test Methodology)은 현대 소프트웨어 아키텍처의 품질을 객관적으로 검증하는 핵심 기술이다. 현실적인 작업 부하 모델, 자동화된 테스트, 관측성, 통계 분석, 카오스 엔지니어링, AI 및 로봇 전용 평가 기법을 통합함으로써 클라우드, 자율주행 로봇, 산업 자동화, 디지털 트윈, 엣지 컴퓨팅, 물리 AI 환경에서도 지속적으로 확장 가능하고 안정적이며 경제적인 차세대 소프트웨어 시스템을 구축할 수 있다.
