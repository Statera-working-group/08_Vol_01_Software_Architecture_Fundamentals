**Volume 1 Software Architecture Fundamentals**


# 06. Robotics Software Architecture

##  

## 06.01 Unique Requirements of Robot Software Architecture

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

Traditional software architecture was primarily designed for information systems, enterprise applications, web services, and cloud platforms whose primary responsibility was the manipulation of digital information. Robot software architecture, however, extends far beyond digital computation because every software decision eventually influences a physical machine operating in an unpredictable environment. Unlike conventional software, robot software continuously receives sensory information from the physical world, reasons about uncertain situations, makes autonomous decisions, and issues commands that immediately affect motors, actuators, manipulators, and mobile platforms. Consequently, robot software architecture must simultaneously satisfy computational efficiency, physical safety, deterministic timing, environmental adaptability, and long-term maintainability. These characteristics fundamentally distinguish robotics software from nearly every other category of software engineering.

The defining characteristic of robot software is its permanent interaction with the physical world. Enterprise software may process thousands of database transactions every second without directly influencing human safety, while robot software controls moving mechanical systems capable of transporting heavy payloads, operating industrial equipment, or interacting with humans in shared workspaces. Every software defect therefore possesses the potential to become a physical failure. An incorrect control command may cause a robot arm to collide with equipment, an autonomous mobile robot to strike an obstacle, or a medical robot to produce hazardous motion. Because software errors can propagate into physical damage, architecture must be designed under the assumption that failures will eventually occur and appropriate mechanisms must already exist to detect, isolate, recover from, or safely stop the system.

Robot software architecture must also accommodate continuous perception rather than discrete user requests. Most enterprise systems respond only after receiving a transaction from a user or another application. Robots instead operate within continuous perception loops where cameras, LiDARs, radars, depth sensors, IMUs, GNSS receivers, force sensors, tactile sensors, encoders, microphones, and numerous additional devices constantly generate new observations. These heterogeneous data streams differ significantly in sampling frequency, bandwidth, latency, synchronization accuracy, and reliability. The software architecture therefore requires carefully designed sensor pipelines capable of acquiring, timestamping, synchronizing, filtering, fusing, and distributing information throughout the system without introducing unacceptable delays.

Timing requirements represent another major distinction. Conventional software generally emphasizes throughput and scalability, whereas robot software must satisfy deterministic response times. Control algorithms frequently execute at frequencies ranging from hundreds of Hertz to several kilohertz. Motor controllers often require update periods measured in microseconds or milliseconds, while perception algorithms may process images at thirty to sixty frames per second, and planning modules may update trajectories several times per second. The architecture must therefore integrate hard real-time control, soft real-time perception, asynchronous planning, logging, visualization, cloud communication, and operator interfaces without allowing non-critical workloads to interfere with deterministic execution.

Concurrency is another unavoidable property of robotics software. A modern autonomous robot simultaneously performs localization, mapping, perception, object detection, semantic understanding, path planning, motion control, obstacle avoidance, battery management, diagnostics, communication, cybersecurity monitoring, user interaction, and cloud synchronization. These functions execute concurrently on heterogeneous computing hardware including microcontrollers, CPUs, GPUs, FPGAs, AI accelerators, and distributed edge devices. The architecture must define clear communication mechanisms, resource ownership, synchronization strategies, scheduling priorities, and isolation boundaries to prevent race conditions, deadlocks, resource starvation, and unpredictable execution behavior.

Another unique architectural requirement originates from uncertainty. Unlike business software operating within relatively controlled databases, robots perceive incomplete, noisy, ambiguous, and sometimes contradictory information. Cameras may experience poor illumination, LiDAR measurements may contain reflections, GNSS signals may become unavailable, wireless communication may be interrupted, and moving obstacles may behave unpredictably. Consequently, robot software architecture must incorporate probabilistic reasoning, confidence estimation, sensor redundancy, uncertainty propagation, and fault detection mechanisms throughout the entire processing pipeline. Instead of assuming perfect information, the architecture assumes that every observation contains some degree of uncertainty.

Scalability within robot software differs substantially from scalability in cloud computing. Cloud applications typically scale by adding computational resources to process larger numbers of requests. Robots must scale across multiple dimensions simultaneously. Computational workloads increase as perception models become more sophisticated. Sensor bandwidth expands as higher resolution devices are introduced. Fleet size grows from individual robots to hundreds or thousands of coordinated autonomous systems. Mission complexity increases through collaborative behaviors involving multiple heterogeneous robots. Architecture must therefore support computational scalability, communication scalability, functional scalability, and organizational scalability while preserving deterministic behavior for safety-critical components.

Modularity becomes especially important because robotic platforms evolve continuously throughout their operational lifetime. Sensors may be replaced, actuators upgraded, AI models retrained, navigation algorithms improved, communication technologies modernized, or entirely new manipulation capabilities introduced. Hardware abstraction layers, standardized interfaces, middleware services, message definitions, and component-based software organization enable these changes without requiring complete redesign of the overall system. Loose coupling and high cohesion therefore become essential architectural principles that facilitate long-term maintainability.

Robot software architecture must also integrate multiple abstraction layers operating at different temporal and functional scales. Low-level embedded controllers directly regulate electrical currents and actuator torque. Mid-level controllers coordinate joint motion, vehicle kinematics, localization, mapping, perception, and navigation. High-level reasoning systems perform mission planning, semantic understanding, human interaction, cloud connectivity, and AI-based decision making. Although these layers operate independently, they must cooperate through carefully designed interfaces that preserve abstraction while minimizing latency and communication overhead.

Safety requirements occupy a far more central architectural position than in most software domains. Functional safety cannot simply be added after implementation. Instead, safety mechanisms must influence architectural decisions from the earliest design stages. Emergency stop handling, watchdog monitoring, heartbeat supervision, redundant sensing, collision avoidance, speed limitation, safety zones, fault isolation, graceful degradation, and safe-state transitions should all be considered architectural capabilities rather than isolated application features. Every critical software component should define both its normal operating behavior and its behavior during abnormal conditions.

Fault tolerance likewise becomes an architectural necessity. Components inevitably fail because sensors become disconnected, communication networks experience packet loss, processors overheat, batteries become depleted, localization confidence decreases, or AI inference services exceed acceptable latency. Robot software architecture should therefore support redundancy, health monitoring, component restart, fallback algorithms, degraded operating modes, distributed supervision, and automatic recovery whenever possible. The objective is not merely preventing failures but ensuring that failures remain localized and predictable.

Energy awareness represents another characteristic that distinguishes robotics software from server-based applications. Robots possess finite battery capacity that directly influences mission duration, computational capability, thermal limits, and operational availability. Architecture should therefore support adaptive workload scheduling, power-aware AI inference, dynamic sensor activation, intelligent compute allocation, and mission optimization based upon remaining energy reserves. Computational efficiency becomes not only an optimization objective but also a functional requirement affecting operational success.

Thermal management similarly influences software architecture because embedded AI computers frequently operate under strict temperature constraints. Intensive GPU inference, continuous sensor processing, and high-performance mapping algorithms generate substantial heat that may reduce processor frequency through thermal throttling. Architectural strategies including workload distribution, edge-cloud partitioning, asynchronous inference scheduling, dynamic model selection, and adaptive resource allocation help maintain acceptable thermal conditions while preserving application performance.

Communication architecture must also accommodate highly heterogeneous networking environments. Robots may transition between Ethernet, Wi-Fi, 5G, LTE, private industrial wireless networks, satellite communication, or complete communication loss. Consequently, robot software cannot assume continuous network connectivity. Local autonomy, intermittent synchronization, delayed message delivery, store-and-forward communication, eventual consistency, and resilient edge operation become essential architectural principles. A robot should remain operational even when disconnected from centralized infrastructure.

Modern robots increasingly incorporate artificial intelligence as a primary architectural element rather than an isolated application. Deep neural networks perform perception, language understanding, manipulation planning, anomaly detection, predictive maintenance, and autonomous reasoning. AI components introduce additional architectural requirements including model lifecycle management, inference scheduling, hardware acceleration, uncertainty estimation, dataset versioning, model rollback, online monitoring, explainability, and continuous deployment. Traditional software modules and AI models must coexist within a unified architecture despite fundamentally different execution characteristics.

Hardware heterogeneity further differentiates robot software architecture. Embedded microcontrollers execute deterministic control loops, Linux computers perform high-level coordination, GPUs accelerate deep learning inference, DSPs process sensor signals, and cloud servers perform large-scale analytics or fleet optimization. The architecture must distribute functionality according to computational characteristics while minimizing communication latency and preserving deterministic behavior where required. Proper partitioning between embedded systems, edge computing, and cloud infrastructure significantly influences overall system performance.

Robotic systems also require precise temporal synchronization across distributed sensing devices. Cameras, LiDARs, IMUs, GNSS receivers, wheel encoders, radar sensors, and external measurement systems frequently operate with independent clocks. Accurate sensor fusion requires timestamp consistency through protocols such as Precision Time Protocol or hardware-trigger synchronization. Architectural support for unified timing enables reliable localization, perception, mapping, and sensor fusion under dynamic operating conditions.

Verification and validation requirements are substantially more demanding because robot behavior emerges from interactions among software, hardware, environment, and human operators. Testing must therefore progress through multiple stages including unit testing, integration testing, software-in-the-loop simulation, hardware-in-the-loop validation, digital twin experimentation, field testing, stress testing, fault injection, long-duration endurance evaluation, and safety certification. The architecture should facilitate observability by providing comprehensive logging, tracing, telemetry, replay capability, diagnostics, and performance measurement throughout every subsystem.

Human-robot interaction introduces additional architectural complexity. Operators require intuitive interfaces for monitoring robot health, assigning missions, visualizing sensor data, overriding autonomous behavior, responding to alarms, and analyzing historical performance. Architecture should separate presentation logic from autonomy while maintaining secure, reliable, and low-latency communication between operators and robotic platforms. Multiple users with different authorization levels may simultaneously interact with robots through local interfaces, web dashboards, mobile applications, or fleet management systems.

Cybersecurity has become inseparable from robot architecture because robots increasingly operate as network-connected cyber-physical systems. Secure boot, encrypted communication, authenticated software updates, hardware identity, access control, intrusion detection, runtime integrity verification, certificate management, and zero-trust communication should be integrated throughout the architecture. Security mechanisms must protect not only digital assets but also physical safety by preventing unauthorized control of autonomous machines.

Maintainability remains one of the most important long-term architectural objectives. Industrial robots often remain operational for ten years or longer while continuously receiving software updates, hardware replacements, AI model improvements, and new functional capabilities. Well-defined interfaces, standardized middleware, modular component design, configuration management, version compatibility, continuous integration, automated testing, and documentation collectively reduce lifecycle costs while improving reliability and upgrade flexibility.

Finally, robot software architecture should be viewed not merely as the organization of software modules but as the complete structural foundation of an intelligent cyber-physical system. It must integrate computation, communication, perception, control, artificial intelligence, safety, reliability, maintainability, scalability, cybersecurity, and human interaction into a coherent engineering framework capable of operating continuously within uncertain physical environments. Unlike conventional software architecture, which primarily manages information flow, robot software architecture governs both information flow and physical behavior simultaneously. This dual responsibility makes robotics software architecture one of the most demanding disciplines within modern software engineering and establishes the foundation upon which autonomous mobile robots, industrial manipulators, humanoids, service robots, collaborative robots, and future Physical AI systems are built.

기존의 **소프트웨어 아키텍처(Software Architecture)** 는 주로 정보 처리와 데이터 관리, 웹 서비스(Web Service), 기업용 시스템(Enterprise System)을 대상으로 발전해 왔다. 반면 **로봇 소프트웨어 아키텍처(Robot Software Architecture)** 는 디지털 정보뿐 아니라 실제 물리 환경(Physical Environment)과 지속적으로 상호작용해야 한다는 점에서 근본적으로 다르다. 로봇은 주변 환경을 인식하고 판단하며, 그 결과를 실제 모터와 액추에이터(Actuator)에 전달하여 움직임을 만들어 내기 때문에 모든 소프트웨어 결정이 현실 세계에 직접적인 영향을 미친다.

따라서 로봇 소프트웨어는 단순히 계산 성능만 우수해서는 충분하지 않다. 실시간성(Real-Time), 안전성(Safety), 신뢰성(Reliability), 환경 적응성(Adaptability), 유지보수성(Maintainability)을 동시에 만족해야 하며, 이러한 요구사항은 일반적인 응용 프로그램과는 전혀 다른 수준의 아키텍처 설계를 필요로 한다.

일반적인 기업용 소프트웨어는 데이터베이스(Database)의 정보를 처리하거나 사용자 요청(Request)을 처리하는 것이 주된 역할이다. 그러나 로봇은 센서(Sensor)를 통해 실제 환경을 지속적으로 관찰하고, 그 결과에 따라 물리적인 행동을 수행한다. 잘못된 데이터 처리나 소프트웨어 오류는 단순한 서비스 장애가 아니라 충돌(Collision), 장비 파손, 사람의 부상과 같은 실제 사고로 이어질 수 있다. 따라서 로봇 소프트웨어는 항상 물리적인 위험을 고려하여 설계되어야 한다.

로봇 소프트웨어는 환경으로부터 끊임없이 데이터를 수집한다. 카메라(Camera), 라이다(LiDAR), 레이더(Radar), 깊이 카메라(Depth Camera), 관성측정장치(IMU), 위성항법시스템(GNSS), 힘 센서(Force Sensor), 엔코더(Encoder) 등 다양한 센서가 서로 다른 주기와 속도로 데이터를 생성한다. 이러한 데이터는 시간 동기화(Time Synchronization), 필터링(Filtering), 센서 융합(Sensor Fusion), 상태 추정(State Estimation)을 거쳐야만 의미 있는 정보로 활용될 수 있다.

이처럼 서로 다른 센서가 동시에 데이터를 생성하기 때문에 로봇 아키텍처는 데이터 흐름(Data Flow)을 효율적으로 관리해야 한다. 각 센서의 지연(Latency), 정확도(Accuracy), 신뢰도(Reliability)를 고려하여 필요한 정보만 적절한 시점에 전달하는 것이 매우 중요하다.

로봇 소프트웨어는 일반적인 서버(Server) 소프트웨어보다 훨씬 엄격한 실시간성(Real-Time)을 요구한다. 모터 제어(Motor Control)는 수백에서 수천 헤르츠(Hz)의 제어 주기로 동작해야 하며, 영상 처리(Image Processing)는 초당 수십 프레임(Frame)을 처리해야 한다. 경로 계획(Path Planning)은 수백 밀리초 단위로 새로운 경로를 생성해야 하고, 장애물 회피(Obstacle Avoidance)는 순간적인 환경 변화에도 즉시 대응해야 한다.

이러한 다양한 작업들은 서로 다른 실행 주기를 가지면서 동시에 수행된다. 따라서 제어(Control), 인지(Perception), 계획(Planning), 사용자 인터페이스(User Interface), 로그(Log), 클라우드 통신(Cloud Communication)이 서로 간섭하지 않도록 우선순위(Priority)와 스케줄링(Scheduling)을 명확하게 설계해야 한다.

동시성(Concurrency)은 로봇 소프트웨어의 대표적인 특징이다. 현대의 자율주행 로봇은 위치추정(Localization), 지도작성(Mapping), 객체 인식(Object Detection), 의미 이해(Semantic Understanding), 경로 생성(Path Generation), 배터리 관리(Battery Management), 자가 진단(Self-Diagnosis), 네트워크 통신(Network Communication) 등을 동시에 수행한다.

이러한 기능들은 CPU, GPU, FPGA, 마이크로컨트롤러(MCU) 등 서로 다른 하드웨어에서 병렬적으로 실행된다. 따라서 자원 공유(Resource Sharing), 동기화(Synchronization), 경쟁 상태(Race Condition), 교착 상태(Deadlock)를 방지할 수 있는 구조가 반드시 필요하다.

또 다른 중요한 특징은 불확실성(Uncertainty)을 항상 고려해야 한다는 점이다. 현실 세계에서는 카메라가 역광을 받을 수도 있고, 라이다가 반사 오류를 일으킬 수도 있으며, GNSS 신호가 끊기거나 무선 통신이 불안정해질 수도 있다. 따라서 로봇은 항상 완전하지 않은 정보를 기반으로 판단해야 한다.

이를 위해 로봇 아키텍처는 확률적 추정(Probabilistic Estimation), 신뢰도 평가(Confidence Estimation), 센서 중복(Redundancy), 이상 탐지(Fault Detection)와 같은 기능을 기본적으로 포함해야 한다. 모든 센서 데이터에는 일정 수준의 오차와 불확실성이 존재한다는 가정을 전제로 설계하는 것이 바람직하다.

확장성(Scalability)의 의미도 일반적인 클라우드 시스템과 다르다. 일반 서버는 더 많은 사용자를 처리하기 위해 서버 수를 증가시키면 되지만, 로봇은 계산량 증가뿐 아니라 센서 수 증가, AI 모델의 복잡도 증가, 로봇 대수 증가, 임무(Mission)의 복잡도 증가까지 모두 고려해야 한다.

따라서 로봇 소프트웨어는 계산 확장성(Computational Scalability), 통신 확장성(Communication Scalability), 기능 확장성(Functional Scalability), 그리고 플릿(Fleet) 수준의 확장성을 동시에 만족할 수 있도록 설계되어야 한다.

모듈화(Modularity)는 장기간 운영되는 로봇 시스템에서 매우 중요한 설계 원칙이다. 실제 산업용 로봇은 수년 동안 운영되면서 새로운 센서가 추가되고, AI 모델이 교체되며, 제어 알고리즘(Control Algorithm)이 지속적으로 개선된다.

이를 위해 하드웨어 추상화 계층(Hardware Abstraction Layer, HAL), 표준 인터페이스(Standard Interface), 미들웨어(Middleware), 컴포넌트(Component) 기반 설계를 적용하면 특정 장치가 변경되더라도 전체 시스템을 다시 작성할 필요가 없다. 높은 응집도(High Cohesion)와 낮은 결합도(Low Coupling)는 이러한 유지보수성을 향상시키는 핵심 원칙이다.

로봇 소프트웨어는 여러 계층(Layer)으로 구성된다. 가장 아래에는 전류와 토크를 제어하는 임베디드 제어기(Embedded Controller)가 존재하며, 그 위에는 위치 제어(Motion Control), 위치추정(Localization), 지도작성(Mapping), 인지(Perception), 내비게이션(Navigation)이 위치한다.

최상위 계층에서는 임무 계획(Mission Planning), 의미 기반 추론(Semantic Reasoning), 사람-로봇 상호작용(Human-Robot Interaction), 클라우드 연동(Cloud Integration), AI 기반 의사결정(AI Decision Making)이 수행된다. 각 계층은 독립성을 유지하면서도 효율적으로 정보를 교환해야 한다.

안전성(Safety)은 로봇 아키텍처에서 가장 중요한 설계 요소 가운데 하나이다. 안전 기능은 개발이 끝난 이후에 추가되는 기능이 아니라, 초기 아키텍처 설계 단계부터 포함되어야 한다. 비상 정지(Emergency Stop), 감시 타이머(Watchdog), 하트비트(Heartbeat), 충돌 회피(Collision Avoidance), 속도 제한(Speed Limitation), 안전 영역(Safety Zone), 안전 정지(Safe State) 등은 모두 시스템 구조의 일부로 설계되어야 한다.

각 소프트웨어 컴포넌트(Component)는 정상 상태뿐 아니라 오류(Failure)가 발생했을 때 어떻게 동작할 것인지까지 정의해야 하며, 안전한 복구(Recovery)가 가능한 구조를 갖추어야 한다.

고장 허용(Fault Tolerance) 역시 필수적인 요구사항이다. 센서 연결이 끊기거나, 네트워크(Network)가 불안정해지거나, 배터리(Battery)가 부족하거나, AI 추론(AI Inference)이 지연되는 상황은 실제 환경에서 자주 발생한다.

따라서 아키텍처는 건강 상태 모니터링(Health Monitoring), 자동 재시작(Auto Restart), 백업 알고리즘(Fallback Algorithm), 성능 저하 운전(Graceful Degradation), 분산 감시(Distributed Supervision) 등을 제공하여 전체 시스템이 중단되지 않도록 해야 한다.

에너지 관리(Energy Management)는 모바일 로봇(Mobile Robot)에서 매우 중요한 요소이다. 서버는 지속적으로 전원을 공급받지만, 로봇은 배터리 용량이 제한되어 있기 때문에 남은 에너지에 따라 임무 수행 시간이 결정된다.

따라서 AI 추론 최적화(AI Inference Optimization), 계산 자원 분배(Resource Allocation), 센서 활성화 제어(Sensor Activation Control), 전력 인식 스케줄링(Power-Aware Scheduling)을 통해 에너지 소비를 최소화하는 구조가 필요하다.

열 관리(Thermal Management)도 중요한 설계 요소이다. GPU를 사용하는 엣지 컴퓨터(Edge Computer)는 장시간 AI 추론을 수행하면 발열이 증가하고, 일정 온도를 넘으면 성능 저하(Thermal Throttling)가 발생할 수 있다.

따라서 작업 분산(Workload Distribution), 엣지-클라우드 분산 처리(Edge-Cloud Distribution), 모델 선택(Model Selection), 동적 자원 관리(Dynamic Resource Management)를 통해 시스템 온도를 안정적으로 유지하는 것이 중요하다.

통신 아키텍처(Communication Architecture)는 다양한 네트워크 환경을 고려해야 한다. 로봇은 유선 이더넷(Ethernet), Wi-Fi, 5G, LTE, 산업용 무선망을 사용할 수 있으며, 때로는 완전히 네트워크가 끊긴 상태에서도 동작해야 한다.

따라서 로컬 자율성(Local Autonomy), 지연 허용 통신(Delay-Tolerant Communication), 저장 후 전달(Store-and-Forward), 최종 일관성(Eventual Consistency)을 지원하여 네트워크 상태와 관계없이 임무를 지속할 수 있어야 한다.

최근의 로봇은 인공지능(AI)을 핵심 구성 요소로 포함한다. 객체 인식(Object Recognition), 음성 이해(Speech Understanding), 조작 계획(Manipulation Planning), 이상 탐지(Anomaly Detection), 예측 유지보수(Predictive Maintenance)는 대부분 딥러닝(Deep Learning)을 기반으로 수행된다.

따라서 AI 모델 관리(Model Lifecycle Management), 추론 스케줄링(Inference Scheduling), GPU 가속(Hardware Acceleration), 데이터셋 버전 관리(Dataset Versioning), 모델 모니터링(Model Monitoring) 등이 기존 소프트웨어와 하나의 통합 아키텍처 안에서 관리되어야 한다.

로봇은 CPU, GPU, FPGA, DSP, MCU 등 다양한 프로세서를 동시에 사용한다. 각각의 장치는 처리 성능과 목적이 다르므로 적절한 기능 분할(Function Partitioning)이 매우 중요하다. 실시간 제어는 MCU에서, AI 추론은 GPU에서, 고수준 판단은 CPU에서 수행하는 방식으로 역할을 분리하면 전체 시스템의 성능과 안정성을 향상시킬 수 있다.

또한 여러 센서가 동일한 시간 기준(Time Base)을 사용하도록 정밀한 시간 동기화가 필요하다. 카메라(Camera), 라이다(LiDAR), IMU, GNSS 등의 데이터는 정확한 타임스탬프(Timestamp)를 기반으로 결합되어야 하므로 **정밀 시간 프로토콜(Precision Time Protocol, PTP)** 이나 하드웨어 트리거(Hardware Trigger)가 중요한 역할을 수행한다.

검증 및 검증(Verification and Validation)은 로봇 분야에서 특히 중요하다. 소프트웨어 단위 테스트(Unit Test)뿐 아니라 통합 테스트(Integration Test), 소프트웨어 인 더 루프(SIL), 하드웨어 인 더 루프(HIL), 디지털 트윈(Digital Twin), 실제 환경(Field Test)을 모두 수행해야 한다. 이를 위해 아키텍처는 로그(Log), 추적(Tracing), 원격 모니터링(Telemetry), 데이터 재생(Replay), 진단(Diagnostics) 기능을 기본적으로 제공해야 한다.

사람-로봇 상호작용(Human-Robot Interaction, HRI) 역시 중요한 요소이다. 운영자는 임무를 생성하고, 로봇 상태를 모니터링하며, 긴급 상황에서는 수동 개입(Manual Override)을 수행할 수 있어야 한다. 따라서 사용자 인터페이스(User Interface)는 자율 시스템과 독립적으로 설계되면서도 실시간으로 안전하게 정보를 교환할 수 있어야 한다.

사이버보안(Cybersecurity)은 현대 로봇에서 필수적인 요소이다. 안전한 부팅(Secure Boot), 암호화 통신(Encrypted Communication), OTA 업데이트(Over-the-Air Update), 접근 제어(Access Control), 침입 탐지(Intrusion Detection), 인증서 관리(Certificate Management), 제로 트러스트(Zero Trust) 보안 모델은 단순한 IT 기능이 아니라 물리적 안전까지 보호하는 핵심 아키텍처 요소가 된다.

마지막으로 유지보수성(Maintainability)은 장기간 운영되는 로봇에서 매우 중요한 품질 속성(Quality Attribute)이다. 산업용 로봇은 10년 이상 사용되는 경우가 많으며, 이 기간 동안 새로운 기능과 AI 모델이 지속적으로 추가된다. 모듈화된 구조와 표준 인터페이스, 자동화된 테스트, 지속적 통합(Continuous Integration), 체계적인 버전 관리(Version Management)는 전체 시스템의 수명주기 비용을 크게 절감할 수 있다.

결론적으로 **로봇 소프트웨어 아키텍처(Robot Software Architecture)** 는 단순한 소프트웨어 모듈의 구조가 아니라, 계산(Computation), 통신(Communication), 인지(Perception), 제어(Control), 인공지능(AI), 안전(Safety), 보안(Security), 신뢰성(Reliability), 유지보수성(Maintainability)을 하나의 통합된 **사이버-물리 시스템(Cyber-Physical System)** 으로 구현하기 위한 기반 기술이다. 일반 소프트웨어가 정보의 흐름을 관리하는 데 집중한다면, 로봇 소프트웨어는 정보의 흐름과 물리적인 행동을 동시에 제어해야 한다는 점에서 현대 소프트웨어 공학(Software Engineering) 분야에서도 가장 복잡하고 도전적인 아키텍처 영역으로 평가된다.

##  

## 06.02 3T Task-Behavior-Control Layered Architecture Model

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

The rapid evolution of autonomous robots has significantly increased the complexity of robot software. Modern robots no longer execute simple repetitive motions but instead perceive dynamic environments, interpret semantic information, plan missions, coordinate multiple subsystems, interact with humans, and continuously adapt their behavior according to changing situations. As robotic systems become increasingly intelligent, software architecture must provide a clear separation of responsibilities while maintaining efficient communication between decision-making, behavioral coordination, and low-level control. One of the most influential architectural approaches developed to address these challenges is the **3T (Task-Behavior-Control) Layered Architecture Model**, which organizes robot intelligence into three hierarchical layers that operate at different temporal and functional levels.

The 3T architecture was originally proposed to bridge the gap between high-level symbolic reasoning and low-level reactive control. Earlier robot architectures often relied exclusively on centralized planning or purely reactive behaviors. Deliberative architectures generated complete plans before execution but struggled to respond to unexpected environmental changes. Reactive architectures responded quickly to sensor inputs but lacked long-term planning and mission-level reasoning. The 3T model integrates the advantages of both approaches by introducing an intermediate behavioral layer that connects strategic mission planning with real-time control. This layered organization enables robots to remain both goal-oriented and highly responsive.

The highest layer of the architecture is the **Task Layer**, which represents the deliberative intelligence of the robotic system. This layer focuses on mission planning, task decomposition, global reasoning, resource allocation, and long-term decision making. Instead of directly controlling motors or processing raw sensor data, the Task Layer interprets user objectives and converts them into executable missions. It reasons about priorities, constraints, available resources, environmental knowledge, operational policies, and overall mission objectives.

The Task Layer typically operates on time scales measured in seconds or even minutes because strategic decisions rarely require millisecond responsiveness. Examples include assigning warehouse delivery missions to autonomous mobile robots, determining the inspection order of industrial equipment, planning cleaning routes for service robots, scheduling collaborative robot operations in manufacturing cells, or allocating inspection tasks across an entire robot fleet. These planning activities involve optimization algorithms, symbolic reasoning, knowledge graphs, semantic maps, AI planners, scheduling engines, and mission management software.

Within this layer, robot software frequently interacts with enterprise systems such as Manufacturing Execution Systems (MES), Warehouse Management Systems (WMS), Enterprise Resource Planning (ERP), fleet management platforms, cloud orchestration systems, and digital twins. The Task Layer therefore represents the primary interface between business objectives and robotic execution. Rather than controlling individual movements, it answers questions such as what should be done, why it should be done, when execution should begin, and which robot should perform each mission.

Once strategic objectives have been determined, responsibility shifts to the **Behavior Layer**, which forms the core of the 3T architecture. This layer acts as an intelligent coordinator that translates abstract missions into concrete robot behaviors. It determines how individual actions should be organized, sequenced, monitored, interrupted, resumed, or modified according to changing environmental conditions.

Unlike the Task Layer, which reasons symbolically, the Behavior Layer reasons operationally. It continuously observes both internal robot states and external environmental conditions while coordinating multiple behavioral modules. This layer often employs Finite State Machines (FSM), Hierarchical State Machines (HSM), Behavior Trees (BT), Hybrid Automata, Petri Nets, or AI-based policy managers to represent complex execution logic.

Behavior management becomes particularly important because robots rarely perform isolated actions. A simple warehouse delivery mission may require localization, obstacle avoidance, elevator usage, door communication, battery monitoring, docking, payload handling, confirmation with cloud services, and return navigation. The Behavior Layer coordinates these activities while responding dynamically to unforeseen events such as blocked pathways, temporary communication failures, moving obstacles, or emergency stop requests.

Behavior arbitration is another critical responsibility. Multiple behaviors may compete simultaneously for control of the robot. Obstacle avoidance may conflict with trajectory tracking. Emergency stop may override manipulation tasks. Battery charging may interrupt normal operations. Human safety behaviors may temporarily suspend autonomous navigation. The Behavior Layer continuously prioritizes these competing objectives while maintaining system stability and mission continuity.

Behavior execution also requires continuous monitoring of execution progress. Each behavior reports its operational status, including initialization, execution, completion, failure, timeout, interruption, recovery, or cancellation. These execution states allow higher-level mission planners to make informed decisions regarding replanning, task reassignment, or mission termination.

The lowest layer is the **Control Layer**, which directly interacts with robot hardware and physical processes. This layer is responsible for deterministic real-time execution of motion commands, actuator control, sensor acquisition, trajectory tracking, force regulation, stabilization, motor synchronization, and feedback control. Unlike higher layers that operate with symbolic information, the Control Layer processes continuous numerical signals and executes control algorithms with strict timing requirements.

Control loops typically execute at frequencies ranging from hundreds to several thousands of Hertz. Motion controllers continuously compute actuator commands using sensor feedback from encoders, IMUs, force sensors, torque sensors, wheel odometry, joint position sensors, and various hardware interfaces. Algorithms such as PID control, Model Predictive Control (MPC), impedance control, adaptive control, feedforward compensation, disturbance rejection, and state estimation are commonly implemented within this layer.

Deterministic execution is absolutely essential because even small timing variations may produce unstable robot behavior. Consequently, the Control Layer frequently operates on real-time operating systems, embedded microcontrollers, FPGA-based controllers, or PREEMPT_RT Linux environments. Hardware interrupts, deterministic scheduling, real-time communication buses, and synchronized clocks ensure consistent execution timing.

Although the three layers perform different responsibilities, they must cooperate continuously through well-defined interfaces. The Task Layer generates high-level objectives and delegates them to the Behavior Layer. The Behavior Layer transforms those objectives into coordinated behavioral sequences while monitoring execution. The Control Layer executes low-level commands and provides continuous feedback regarding robot state, actuator performance, and environmental interaction. Information flows upward while commands flow downward, establishing a bidirectional control hierarchy.

One of the greatest strengths of the 3T architecture lies in temporal separation. Strategic planning, behavioral coordination, and motor control naturally require different execution frequencies. Separating these activities prevents computationally expensive planning algorithms from interfering with deterministic control loops while allowing each layer to optimize independently according to its timing requirements.

Functional separation also significantly improves software maintainability. Modifications to mission planning algorithms rarely affect motor controllers. Improvements in obstacle avoidance do not require redesigning enterprise task schedulers. New robotic hardware can often be integrated by replacing components within the Control Layer while preserving higher-level mission logic. This modularity substantially reduces lifecycle maintenance costs and accelerates system evolution.

The architecture also supports heterogeneous computing platforms. High-level AI planning may execute on cloud servers or powerful edge GPUs. Behavioral coordination typically runs on Linux-based robot computers using middleware such as ROS2. Deterministic control executes on embedded controllers, real-time processors, or motor control units. This computational partitioning enables efficient utilization of specialized hardware while minimizing unnecessary communication latency.

Modern implementations increasingly integrate artificial intelligence within all three layers. At the Task Layer, Large Language Models (LLMs), Large Action Models (LAMs), knowledge graphs, semantic planners, and reinforcement learning systems assist mission planning and decision making. Within the Behavior Layer, AI policies, learned behavior arbitration, adaptive task sequencing, and online optimization improve execution flexibility. At the Control Layer, neural network controllers, adaptive parameter estimation, learned dynamics compensation, and AI-assisted fault detection enhance low-level control performance without compromising deterministic safety mechanisms.

Behavior Trees have become particularly popular within the Behavior Layer because they provide modularity, reusability, readability, and hierarchical organization. Unlike finite state machines that become increasingly complex as transitions multiply, Behavior Trees naturally represent fallback strategies, parallel execution, conditional evaluation, and recovery behaviors. Consequently, many modern autonomous mobile robots, industrial inspection robots, service robots, and humanoids employ Behavior Trees as the central execution engine of the intermediate layer.

The 3T architecture is especially valuable for autonomous mobile robots operating in dynamic environments. Consider an industrial inspection robot assigned to inspect multiple production stations. The Task Layer determines the inspection schedule based on production priorities. The Behavior Layer coordinates navigation, localization, docking, safety monitoring, sensor activation, image acquisition, AI inference, report generation, and communication with cloud systems. The Control Layer executes wheel control, steering commands, obstacle avoidance trajectories, actuator movements, and sensor synchronization in real time. Each layer focuses exclusively on responsibilities appropriate to its level of abstraction.

Similarly, warehouse logistics robots benefit from the separation of concerns provided by the architecture. Fleet optimization algorithms assign transportation tasks at the Task Layer. Behavioral modules coordinate navigation, elevator usage, automatic door interaction, charging decisions, and collision avoidance. Motor controllers execute precise wheel velocity commands while continuously compensating for disturbances and wheel slip.

Industrial manipulators also demonstrate the effectiveness of this model. Production management software defines assembly objectives at the Task Layer. Behavioral coordination manages grasping, tool changes, safety verification, visual inspection, and force-controlled assembly sequences. Servo controllers regulate joint positions, velocities, torque outputs, and force feedback with deterministic timing at the Control Layer.

Humanoid robots further illustrate the importance of layered intelligence because they simultaneously perform locomotion, manipulation, language interaction, perception, social communication, balance control, and whole-body coordination. High-level conversational objectives originate within the Task Layer, interaction behaviors are coordinated by the Behavior Layer, and balance, gait generation, and joint stabilization execute within the Control Layer. The separation prevents conversational reasoning from interfering with dynamic balance control, which requires millisecond-level responsiveness.

Fault tolerance is naturally supported within the layered architecture. The Control Layer continuously monitors actuator health, sensor status, communication integrity, and execution timing. When abnormalities occur, diagnostic information is propagated upward to the Behavior Layer, which determines appropriate recovery strategies such as retrying operations, selecting alternative behaviors, entering degraded operating modes, or requesting human intervention. If mission objectives can no longer be achieved safely, the Task Layer may terminate or replan the mission while maintaining overall operational consistency.

Safety mechanisms also benefit from the hierarchical organization. Immediate emergency responses, including emergency stop activation, collision prevention, torque limitation, and speed restriction, remain within the Control Layer to guarantee deterministic response. Operational safety policies, such as safe navigation around humans, restricted-area enforcement, and collaborative operation management, reside within the Behavior Layer. Organizational safety policies, including mission authorization, access control, scheduling restrictions, and regulatory compliance, are managed by the Task Layer. This distribution ensures that safety decisions occur at the most appropriate level while avoiding unnecessary delays.

Communication between layers should remain asynchronous whenever possible to minimize blocking behavior and improve robustness. Message-oriented middleware, publish-subscribe communication, event-driven coordination, shared world models, and service-oriented interfaces enable each layer to evolve independently without creating excessive coupling. Standardized interfaces also simplify testing, simulation, hardware replacement, and future architectural evolution.

Simulation and verification become significantly easier because each layer can be validated independently before full system integration. Mission planning algorithms may be tested through software simulation, behavioral logic through digital twins and scenario replay, and control algorithms through hardware-in-the-loop testing. Layer-specific verification reduces debugging complexity while improving confidence in overall system reliability.

Scalability represents another major advantage of the architecture. As robotic capabilities expand, additional planning modules, behavioral components, or control algorithms can be integrated without requiring complete redesign. New AI perception modules, collaborative behaviors, cloud orchestration services, fleet intelligence, digital twin synchronization, or predictive maintenance systems can be incorporated into the appropriate layer while preserving existing functionality.

The emergence of Physical AI further reinforces the relevance of the 3T architecture. Future robots increasingly integrate foundation models, world models, vision-language-action systems, semantic reasoning engines, and lifelong learning capabilities. These advanced AI systems naturally align with the Task Layer for high-level reasoning, the Behavior Layer for adaptive execution, and the Control Layer for deterministic physical interaction. Rather than replacing the layered model, AI extends its capabilities by improving decision quality, behavioral flexibility, and control precision while preserving architectural clarity.

Ultimately, the 3T Task-Behavior-Control Layered Architecture Model remains one of the most effective organizational frameworks for intelligent robotic software because it balances deliberative reasoning, adaptive behavioral coordination, and deterministic real-time control within a coherent hierarchical structure. By clearly separating strategic mission management, operational behavior execution, and hardware-level control, the architecture enables robotic systems to achieve scalability, maintainability, safety, responsiveness, and long-term extensibility. As autonomous robots continue evolving toward AI-native Physical Intelligence systems, the 3T architecture provides a robust architectural foundation capable of supporting increasingly complex autonomous behaviors while maintaining predictable, safe, and reliable interaction with the physical world.

현대의 자율 로봇(Autonomous Robot)은 단순히 반복 작업만 수행하는 기계가 아니라, 주변 환경을 인식하고 상황을 이해하며 스스로 판단하고 행동하는 지능형 시스템(Intelligent System)으로 발전하고 있다. 이러한 복잡성을 효과적으로 관리하기 위해서는 소프트웨어를 기능별로 체계적으로 분리하면서도 각 기능이 유기적으로 협력할 수 있는 구조가 필요하다. 이러한 요구를 해결하기 위해 제안된 대표적인 구조가 **3T(Task-Behavior-Control) 계층형 아키텍처(Layered Architecture)** 이다.

3T 아키텍처는 **작업(Task)**, **행동(Behavior)**, **제어(Control)** 의 세 계층으로 로봇의 지능을 분리하여 각각의 역할을 명확하게 정의한다. 각 계층은 서로 다른 시간 주기(Time Scale)와 책임(Responsibility)을 가지며, 명확한 인터페이스(Interface)를 통해 정보를 교환한다. 이러한 구조는 대규모 로봇 시스템에서도 높은 확장성과 유지보수성을 제공한다.

초기의 로봇 시스템은 크게 두 가지 방식으로 발전하였다. 하나는 모든 계획을 미리 계산하는 심의형 구조(Deliberative Architecture)이고, 다른 하나는 센서 입력에 즉시 반응하는 반응형 구조(Reactive Architecture)이다. 심의형 구조는 장기 계획에는 강하지만 환경 변화에 즉시 대응하기 어려웠고, 반응형 구조는 빠르게 대응할 수 있지만 장기적인 목표를 수행하기에는 한계가 있었다.

3T 아키텍처는 이 두 방식을 결합하였다. 높은 수준에서는 장기적인 임무를 계획하고, 중간 계층에서는 상황에 따라 행동을 조정하며, 가장 아래 계층에서는 실시간으로 하드웨어를 제어한다. 이를 통해 계획 능력과 즉각적인 반응성을 동시에 확보할 수 있다.

가장 상위 계층은 **작업 계층(Task Layer)** 이다. 이 계층은 로봇의 전략적 의사결정(Strategic Decision Making)을 담당하며, 사용자의 목표를 이해하고 이를 여러 개의 실행 가능한 임무(Mission)로 분해한다. 또한 작업 우선순위(Priority), 자원(Resource), 시간(Time), 운영 정책(Policy), 환경 정보(Environment Knowledge)를 종합적으로 고려하여 전체 임무를 계획한다.

작업 계층은 일반적으로 수 초에서 수 분 단위로 동작한다. 예를 들어 창고에서 어떤 물품을 먼저 운반할 것인지, 공장에서 어떤 설비를 먼저 검사할 것인지, 여러 대의 로봇 가운데 어떤 로봇이 특정 작업을 수행할 것인지를 결정하는 것이 이 계층의 역할이다.

이 계층에서는 최적화 알고리즘(Optimization Algorithm), 의미 기반 계획기(Semantic Planner), 지식 그래프(Knowledge Graph), 인공지능 계획기(AI Planner), 스케줄러(Scheduler) 등이 사용된다. 실제 산업 현장에서는 제조실행시스템(MES), 창고관리시스템(WMS), 전사적자원관리(ERP), 플릿 관리 시스템(Fleet Management System), 디지털 트윈(Digital Twin) 등과 연동되는 경우가 많다.

작업 계층은 "어떤 작업을 수행할 것인가(What)", "왜 수행하는가(Why)", "언제 수행하는가(When)", "어떤 로봇이 수행하는가(Who)"를 결정하는 계층이며, 실제 모터를 제어하지는 않는다.

작업 계층에서 생성된 임무는 **행동 계층(Behavior Layer)** 으로 전달된다. 행동 계층은 3T 아키텍처의 핵심이라 할 수 있으며, 추상적인 임무를 실제 행동으로 변환하는 역할을 수행한다. 즉 "무엇을 할 것인가"를 "어떻게 수행할 것인가"로 구체화한다.

행동 계층은 현재 로봇의 상태와 주변 환경을 지속적으로 관찰하면서 여러 행동을 적절한 순서로 실행하고 필요하면 중단하거나 다시 시작한다. 또한 새로운 상황이 발생하면 행동을 수정하거나 다른 행동으로 전환하는 역할도 수행한다.

행동 계층에서는 **유한 상태 머신(Finite State Machine, FSM)**, **계층형 상태 머신(Hierarchical State Machine, HSM)**, **행동 트리(Behavior Tree, BT)**, **페트리넷(Petri Net)**, **하이브리드 오토마타(Hybrid Automata)** 등이 대표적으로 사용된다. 최근에는 행동 트리가 가장 널리 사용되는 구조로 자리 잡고 있다.

실제 로봇은 하나의 행동만 수행하지 않는다. 예를 들어 창고 운송 로봇은 이동, 장애물 회피, 엘리베이터 사용, 자동문 통과, 도킹(Docking), 화물 적재, 클라우드 통신, 충전 등을 하나의 임무 안에서 수행해야 한다. 행동 계층은 이러한 다양한 작업을 올바른 순서와 조건으로 연결하여 실행한다.

여러 행동이 동시에 실행되려고 할 때 우선순위를 결정하는 것도 행동 계층의 중요한 역할이다. 예를 들어 장애물 회피는 경로 추종(Trajectory Tracking)보다 우선되어야 하며, 비상 정지(Emergency Stop)는 모든 작업보다 우선한다. 또한 배터리 부족 시에는 현재 임무보다 충전 행동이 우선될 수도 있다.

행동 계층은 각 행동의 실행 상태도 지속적으로 관리한다. 행동은 초기화(Initialization), 실행(Execution), 완료(Completion), 실패(Failure), 시간 초과(Timeout), 중단(Interruption), 복구(Recovery), 취소(Cancellation) 등의 상태를 가지며, 이러한 정보는 상위 작업 계층으로 전달되어 새로운 계획을 수립하는 데 활용된다.

가장 아래 계층은 **제어 계층(Control Layer)** 이다. 이 계층은 실제 하드웨어(Hardware)를 직접 제어하며, 센서 데이터를 읽고 모터를 제어하며 실시간 피드백 제어(Feedback Control)를 수행한다. 다른 계층이 기호(Symbolic) 정보를 처리하는 것과 달리 제어 계층은 연속적인 물리량(Continuous Physical Signals)을 처리한다.

제어 계층에서는 PID 제어(PID Control), 모델 예측 제어(Model Predictive Control, MPC), 임피던스 제어(Impedance Control), 적응 제어(Adaptive Control), 상태 추정(State Estimation), 외란 보상(Disturbance Rejection) 등이 사용된다. 이러한 알고리즘은 매우 짧은 주기로 실행되어야 한다.

일반적으로 제어 루프(Control Loop)는 수백 헤르츠에서 수천 헤르츠의 주기로 실행된다. 따라서 실시간 운영체제(Real-Time Operating System, RTOS), PREEMPT_RT Linux, FPGA, MCU 등이 사용되며, 인터럽트(Interrupt), 결정론적 스케줄링(Deterministic Scheduling), 실시간 통신 버스(Real-Time Bus)를 통해 일정한 실행 시간을 보장한다.

세 계층은 서로 독립적으로 존재하는 것이 아니라 긴밀하게 협력한다. 작업 계층은 임무를 생성하고, 행동 계층은 이를 실제 행동으로 변환하며, 제어 계층은 하드웨어를 움직인다. 반대로 센서 정보와 실행 결과는 제어 계층에서 행동 계층으로, 다시 작업 계층으로 전달되어 지속적인 피드백(Feedback)이 이루어진다.

3T 구조의 가장 큰 장점은 시간적인 분리(Temporal Separation)이다. 장기 계획은 수 초 단위로 수행되고, 행동 관리는 수백 밀리초 단위로 수행되며, 제어는 수 밀리초 이하에서 수행된다. 이러한 분리는 계산량이 많은 AI 계획기가 실시간 제어를 방해하지 않도록 해준다.

기능적인 분리(Functional Separation)도 큰 장점이다. 예를 들어 새로운 임무 계획 알고리즘을 추가하더라도 모터 제어 소프트웨어는 변경할 필요가 없다. 반대로 새로운 모터를 적용하더라도 상위의 임무 관리 프로그램은 거의 수정하지 않아도 된다.

3T 구조는 이기종 컴퓨팅(Heterogeneous Computing) 환경에도 적합하다. 작업 계층은 클라우드(Cloud)나 고성능 GPU 서버에서 실행될 수 있고, 행동 계층은 ROS2 기반 리눅스(Linux) 컴퓨터에서 실행되며, 제어 계층은 MCU나 FPGA에서 실행될 수 있다. 이러한 기능 분할은 각 장치의 성능을 최대한 활용하도록 해준다.

최근에는 세 계층 모두에서 인공지능(AI)이 활용되고 있다. 작업 계층에서는 대규모 언어 모델(Large Language Model, LLM), 대규모 행동 모델(Large Action Model, LAM), 지식 그래프(Knowledge Graph)가 임무 계획을 지원한다. 행동 계층에서는 AI 정책(AI Policy), 행동 최적화(Behavior Optimization), 강화학습(Reinforcement Learning)이 적용된다. 제어 계층에서는 신경망 기반 제어기(Neural Controller), AI 외란 보상, AI 고장 진단(Fault Detection)이 활용되고 있다.

특히 행동 트리(Behavior Tree)는 최근 로봇 소프트웨어에서 가장 많이 사용되는 구조이다. 상태 머신보다 모듈화(Modularity)가 뛰어나며, 재사용성(Reusability), 가독성(Readability), 병렬 처리(Parallel Execution), 복구 동작(Recovery Behavior)을 쉽게 표현할 수 있기 때문이다. 따라서 자율주행 로봇, 서비스 로봇, 산업용 로봇, 휴머노이드(Humanoid)까지 다양한 분야에서 행동 계층의 핵심 구조로 활용되고 있다.

산업용 자율이동로봇(Autonomous Mobile Robot, AMR)을 예로 들면, 작업 계층은 검사 순서를 결정하고, 행동 계층은 이동·도킹·센서 활성화·영상 촬영·AI 분석·보고서 생성을 조정하며, 제어 계층은 바퀴 제어, 조향(Steering), 속도 제어, 장애물 회피를 실시간으로 수행한다. 각 계층은 자신의 역할만 담당하므로 전체 시스템이 매우 체계적으로 구성된다.

창고 물류 로봇도 동일한 구조를 가진다. 작업 계층은 운송 작업을 할당하고, 행동 계층은 엘리베이터 이용, 자동문 통과, 충전 여부 등을 관리하며, 제어 계층은 바퀴 속도와 조향각을 지속적으로 계산하여 안정적인 이동을 수행한다.

산업용 매니퓰레이터(Manipulator)에서는 작업 계층이 조립 순서를 계획하고, 행동 계층은 그리핑(Gripping), 공구 교환(Tool Change), 비전 검사(Vision Inspection), 힘 제어(Force Control)를 조정하며, 제어 계층은 각 관절(Joint)의 위치, 속도, 토크를 실시간으로 제어한다.

휴머노이드(Humanoid)는 더욱 복잡한 구조를 가진다. 작업 계층에서는 사람과의 대화와 임무를 계획하고, 행동 계층에서는 걷기, 물체 조작, 사회적 상호작용을 조정하며, 제어 계층에서는 균형 유지(Balance), 보행(Gait), 전신 제어(Whole Body Control)를 수행한다. 이러한 계층 분리는 복잡한 AI 연산이 균형 제어를 방해하지 않도록 한다.

3T 구조는 고장 허용(Fault Tolerance)에도 매우 유리하다. 제어 계층은 센서 이상, 모터 오류, 통신 장애 등을 지속적으로 감시하며, 이상이 발생하면 행동 계층이 재시도(Retry), 대체 행동(Fallback), 성능 저하 운전(Graceful Degradation), 사람 호출(Human Intervention) 등을 수행한다. 최종적으로 작업 계층은 전체 임무를 다시 계획하거나 종료할 수 있다.

안전성(Safety)도 계층별로 분리된다. 비상 정지와 충돌 회피는 제어 계층에서 즉시 수행되고, 사람 주변 안전 운행과 작업 순서는 행동 계층에서 관리된다. 전체 작업 승인과 운영 정책은 작업 계층에서 관리되므로 각 수준에서 적절한 안전 기능을 수행할 수 있다.

계층 간 통신은 가능하면 비동기(Asynchronous) 방식으로 구성하는 것이 바람직하다. 발행-구독(Publish-Subscribe), 이벤트 기반(Event-Driven), 서비스(Service), 공유 월드 모델(Shared World Model) 등을 활용하면 계층 간 결합도를 낮추고 독립적인 개발과 유지보수가 가능해진다.

검증(Verification)과 시험(Validation)도 계층별로 수행할 수 있다. 작업 계층은 시뮬레이션(Simulation)으로 검증하고, 행동 계층은 디지털 트윈(Digital Twin)으로 시험하며, 제어 계층은 하드웨어 인 더 루프(Hardware-in-the-Loop, HIL)를 통해 검증할 수 있다. 이러한 단계별 검증은 전체 시스템의 신뢰성을 크게 향상시킨다.

확장성(Scalability) 역시 뛰어나다. 새로운 AI 모듈이나 센서를 추가할 때 해당 계층만 수정하면 되므로 기존 구조를 크게 변경하지 않아도 된다. 플릿 관리(Fleet Management), 디지털 트윈, 예측 유지보수(Predictive Maintenance), 클라우드 AI 등의 새로운 기능도 적절한 계층에 쉽게 추가할 수 있다.

최근 등장한 **피지컬 AI(Physical AI)** 역시 3T 구조와 매우 잘 어울린다. 파운데이션 모델(Foundation Model), 월드 모델(World Model), 비전-언어-행동 모델(Vision-Language-Action Model, VLA)은 작업 계층에서 고수준 추론을 수행하고, 행동 계층에서는 적응형 행동을 생성하며, 제어 계층에서는 안전하고 결정론적인 물리 제어를 담당한다. 즉 AI는 기존 3T 구조를 대체하는 것이 아니라 각 계층의 지능을 더욱 향상시키는 역할을 수행한다.

결론적으로 **3T 작업-행동-제어 계층형 아키텍처(Task-Behavior-Control Layered Architecture)** 는 전략적인 임무 계획, 상황에 따른 행동 조정, 실시간 하드웨어 제어를 명확하게 분리함으로써 복잡한 로봇 시스템을 효율적으로 구현할 수 있도록 해준다. 이러한 구조는 확장성(Scalability), 유지보수성(Maintainability), 안전성(Safety), 실시간성(Real-Time), 신뢰성(Reliability)을 동시에 확보할 수 있는 가장 대표적인 로봇 소프트웨어 아키텍처 가운데 하나이며, 앞으로 **AI 네이티브 로봇(AI-Native Robot)** 과 **피지컬 AI(Physical AI)** 시대에도 핵심적인 기반 구조로 계속 활용될 것으로 기대된다.

##  

## 06.03 Component-Based Robot Architecture: ROS2 Composition

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

As robotic systems continue to increase in complexity, traditional monolithic software architectures become increasingly difficult to maintain, extend, test, and optimize. Modern robots simultaneously perform perception, localization, mapping, planning, control, artificial intelligence inference, communication, diagnostics, cloud connectivity, and human interaction. Developing these diverse functions within a single executable rapidly leads to tightly coupled software, excessive dependencies, inefficient resource utilization, and reduced maintainability. To overcome these limitations, modern robotics has increasingly adopted **Component-Based Software Architecture (CBSA)**, in which complex robot functionality is decomposed into reusable, independent software components. Within the ROS2 ecosystem, this architectural philosophy is realized through **ROS2 Composition**, a mechanism that enables multiple software components to execute efficiently within a common runtime while preserving modularity and isolation.

Component-Based Architecture is founded on the principle that every major robotic capability should exist as an independent software module with clearly defined interfaces, explicit responsibilities, and minimal coupling to other modules. Each component encapsulates its own internal implementation while exposing standardized communication interfaces through topics, services, actions, or parameters. Other components interact only through these public interfaces rather than relying upon implementation details. This separation significantly improves software maintainability while allowing individual components to evolve independently.

The concept resembles modular hardware engineering. Just as electrical engineers replace individual sensors or controllers without redesigning an entire robot, software engineers should replace perception algorithms, localization systems, planning modules, or AI inference engines without affecting unrelated parts of the software stack. Component-based architecture therefore treats software functionality as interchangeable building blocks rather than permanently interconnected code.

ROS2 was specifically designed to support highly modular robotics software. Unlike earlier monolithic robotics frameworks, ROS2 introduces composition mechanisms that enable multiple nodes to coexist within a single process while maintaining logical independence. This capability provides significant performance improvements compared to launching every node as an independent operating system process.

In classical ROS architectures, each node executes as an individual process with its own memory space, communication buffers, scheduling context, and operating system overhead. While this model provides excellent fault isolation, it also introduces communication latency due to serialization, deserialization, inter-process communication, context switching, and duplicated memory allocations. As robots become increasingly dependent upon high-bandwidth sensors such as multiple cameras, LiDARs, depth sensors, and AI inference pipelines, these communication costs become substantial.

ROS2 Composition addresses these challenges by allowing multiple components to execute inside a common executable process known as a **Component Container**. Although components remain logically independent, they share memory resources and communicate through highly optimized intra-process communication mechanisms. Data can often be transferred without repeated serialization or memory copying, substantially reducing CPU utilization and communication latency.

At the center of ROS2 Composition lies the concept of the **Component** itself. A component is a dynamically loadable software module that inherits from the standard ROS2 node interface while exposing additional metadata required for runtime loading. Instead of compiling every node into a fixed executable, developers compile reusable shared libraries that may later be loaded into various runtime environments depending on application requirements.

Dynamic loading introduces tremendous flexibility. The same localization component may operate within simulation, laboratory testing, industrial deployment, or cloud-assisted environments without recompilation. Components become reusable software assets that can be assembled into different robot configurations according to mission requirements.

A Component Container serves as the runtime environment responsible for loading, executing, monitoring, and unloading multiple components. Containers may host only a few tightly coupled modules or dozens of interacting components depending upon computational resources and system architecture. Developers may create multiple containers to separate safety-critical modules from AI inference workloads or to distribute computation across heterogeneous processors.

This organization introduces an additional architectural level between individual nodes and complete robotic systems. Rather than viewing software only as collections of processes, ROS2 Composition organizes systems into reusable components grouped within containers that collectively form distributed robotic applications.

One of the most significant advantages of composition is **Intra-Process Communication (IPC)** optimization. Traditional inter-process communication requires message serialization before transmission and deserialization upon reception. Large sensor messages, particularly high-resolution images or point clouds, consume considerable computational resources during this process.

Within a composed container, publishers and subscribers often exchange shared pointers directly without serializing data. This technique, commonly referred to as zero-copy or near zero-copy communication depending on middleware implementation, dramatically reduces memory bandwidth, processor utilization, and communication latency. Real-time perception pipelines therefore achieve significantly higher throughput than equivalent multi-process implementations.

For example, consider a perception pipeline consisting of camera acquisition, image rectification, object detection, semantic segmentation, tracking, and visualization. In traditional process-based execution, each stage repeatedly serializes and copies large image buffers. Under ROS2 Composition, these modules may execute inside one container while exchanging image references through optimized memory mechanisms, reducing latency while preserving modular organization.

Component-based architecture also improves resource utilization. Individual operating system processes require separate memory allocations, scheduling structures, synchronization objects, communication buffers, and runtime environments. Large robotic systems may contain hundreds of nodes, producing considerable operating system overhead. Composition reduces duplicated infrastructure by allowing components to share common runtime resources while maintaining independent execution logic.

Another major advantage involves startup flexibility. Instead of statically determining which software modules belong to each executable during compilation, components may be loaded dynamically at runtime according to mission requirements. Inspection robots may activate additional AI analysis components only when approaching inspection targets. Service robots may dynamically load speech processing modules when interacting with humans. Autonomous vehicles may enable computationally intensive mapping modules only during exploration phases.

Dynamic composition also supports adaptive system reconfiguration. Future AI-native robots will increasingly modify software configurations while operating. Components may be replaced with newer AI models, upgraded perception algorithms, alternative localization systems, or mission-specific planners without restarting the complete robotic application. This capability aligns closely with Software-Defined Robotics and continuous deployment architectures.

ROS2 Composition integrates naturally with object-oriented software engineering principles. Each component encapsulates its internal data structures, algorithms, configuration parameters, lifecycle states, and communication interfaces. Public APIs define interactions with external components while implementation details remain hidden. This encapsulation simplifies maintenance because modifications inside one component rarely propagate throughout the remainder of the system.

Loose coupling represents another essential design objective. Components should communicate only through standardized ROS interfaces rather than directly invoking one another\'s internal functions. Topics provide asynchronous data streams, services support request-response interactions, actions enable long-running operations, and parameters allow runtime configuration. These communication abstractions prevent unnecessary dependencies while improving software portability.

High cohesion is equally important. Every component should perform one clearly defined responsibility. A localization component estimates robot pose. A mapping component constructs environmental maps. A planning component computes trajectories. A motor controller regulates actuator commands. Components performing unrelated responsibilities should generally remain separate because single-purpose modules are easier to understand, verify, optimize, and reuse.

Component granularity requires careful architectural consideration. Components that are excessively large resemble monolithic applications and reduce reuse. Components that are excessively small produce unnecessary communication overhead and management complexity. Appropriate granularity usually corresponds to stable functional boundaries such as perception, localization, mapping, planning, navigation, diagnostics, or hardware abstraction.

Hardware abstraction naturally benefits from component-based organization. Device drivers, sensor interfaces, actuator controllers, communication gateways, GNSS receivers, LiDAR interfaces, cameras, IMUs, force sensors, battery management systems, and industrial fieldbus interfaces each become independent hardware abstraction components. Higher-level software remains completely independent of specific hardware implementations because interaction occurs exclusively through standardized interfaces.

Component-based design also facilitates heterogeneous computing. Computationally intensive AI inference components may execute on GPU-equipped edge computers while deterministic control components execute on embedded microcontrollers or real-time processors. Middleware transparently manages communication across distributed hardware while preserving consistent software interfaces. This architectural flexibility allows developers to optimize computational placement according to latency, bandwidth, thermal constraints, power consumption, and hardware availability.

ROS2 Lifecycle Nodes integrate effectively with component-based architecture by introducing managed execution states. Components transition through well-defined lifecycle phases including unconfigured, inactive, active, finalized, and shutdown states. This explicit lifecycle management enables deterministic startup sequences, controlled activation, safe shutdown procedures, and fault recovery. Safety-critical robotic systems particularly benefit because hardware initialization, sensor calibration, and communication establishment occur under controlled conditions.

Fault isolation remains an important architectural consideration. While composition improves communication efficiency, multiple components executing inside one process also share process-level failure risks. A fatal software error within one component may terminate the entire container. Consequently, architects frequently separate safety-critical controllers, AI inference modules, visualization software, and experimental algorithms into different containers based upon reliability requirements. This hybrid deployment strategy balances communication efficiency with fault containment.

Modern robotic systems therefore often employ mixed architectures combining both composed and independent processes. High-bandwidth perception pipelines execute within composed containers to minimize latency. Safety controllers execute separately for maximum reliability. User interfaces operate independently to avoid influencing real-time computation. Cloud communication modules remain isolated because network failures should never compromise deterministic robot control.

Testing and verification become significantly simpler under component-based architecture. Individual components may undergo unit testing independently before integration. Mock communication interfaces replace physical hardware during automated testing. Simulation environments substitute sensor components without modifying higher-level algorithms. Hardware-in-the-loop validation focuses exclusively on low-level control components while perception algorithms execute using recorded sensor datasets. This modular verification process substantially improves software quality while reducing debugging complexity.

Continuous Integration and Continuous Deployment workflows also benefit. Independent components compile separately, maintain individual version histories, and undergo isolated regression testing. Software updates affect only modified components rather than rebuilding complete robotic applications. Package managers distribute reusable binary components across multiple robot platforms while maintaining consistent dependency management.

Large industrial robotics projects frequently involve multiple development teams working simultaneously. Component-based architecture naturally supports parallel software development because perception engineers, navigation specialists, AI researchers, cloud developers, safety engineers, and hardware teams may develop independent components concurrently while interacting through stable interface definitions. Clear interface contracts reduce integration conflicts and accelerate collaborative development.

Artificial Intelligence introduces additional motivations for component-based robotics architecture. AI models evolve much faster than traditional software. Object detection networks, foundation models, language models, semantic mapping systems, reinforcement learning policies, and visual localization algorithms are continuously retrained and updated. Encapsulating each AI capability within independent components allows model replacement without modifying surrounding software infrastructure.

ROS2 Composition also supports runtime parameterization. Each component exposes configurable parameters controlling algorithm behavior, hardware interfaces, calibration values, operational limits, AI thresholds, communication settings, and diagnostic options. Runtime parameter modification enables adaptive behavior without recompilation while supporting automated configuration management across large robot fleets.

Observability becomes another architectural strength. Every component independently publishes diagnostic information, performance metrics, execution timing, resource utilization, health status, and operational events. Fleet management systems aggregate these distributed telemetry streams to monitor entire robot populations. Performance bottlenecks become easier to identify because each component reports independent operational statistics.

Cybersecurity considerations similarly benefit from component isolation. Authentication, authorization, encrypted communication, certificate management, secure parameter storage, and access control policies may be implemented independently for sensitive components. Security boundaries align naturally with functional boundaries, reducing attack surfaces while improving maintainability.

Component reuse extends beyond individual robots. Common localization components may operate across autonomous mobile robots, warehouse systems, agricultural robots, outdoor inspection platforms, quadrupeds, humanoids, and collaborative manipulators. Hardware-specific drivers remain isolated while higher-level navigation, perception, planning, and AI components remain platform independent. This software reuse dramatically reduces development effort for new robotic platforms.

The architecture further supports simulation-to-real transfer. During simulation, hardware interface components communicate with virtual sensors and simulated actuators. During physical deployment, only hardware abstraction components change while perception, planning, behavior coordination, navigation, AI inference, and mission management components remain identical. This separation substantially accelerates validation while reducing deployment risks.

Future Software-Defined Robots will increasingly depend upon component-based architectures. Runtime software updates, over-the-air deployment, AI model replacement, adaptive mission configuration, cloud-assisted optimization, and autonomous software evolution all require modular software structures capable of dynamic reconfiguration. ROS2 Composition provides the underlying execution model supporting these capabilities while preserving deterministic communication and efficient resource utilization.

The emergence of Physical AI further amplifies the importance of component-based software engineering. Foundation models, Vision-Language-Action systems, world models, semantic planners, multimodal perception pipelines, digital twins, fleet intelligence, and distributed AI services naturally become independent reusable components integrated through standardized middleware interfaces. Their computational demands vary significantly, making dynamic composition essential for efficient execution across CPUs, GPUs, edge computers, and cloud infrastructure.

Ultimately, Component-Based Robot Architecture implemented through ROS2 Composition represents one of the most important advances in modern robotics software engineering. By combining modular software organization, standardized communication interfaces, efficient intra-process messaging, dynamic runtime composition, hardware abstraction, lifecycle management, and scalable deployment strategies, ROS2 Composition enables developers to build robotic systems that are maintainable, reusable, scalable, high-performance, and adaptable to rapidly evolving AI technologies. As autonomous robots continue growing in functional complexity, component-based architecture will remain a foundational principle supporting the next generation of intelligent, AI-native, and software-defined robotic systems.

현대의 로봇은 인식(Perception), 위치추정(Localization), 지도작성(Mapping), 경로 계획(Planning), 제어(Control), 인공지능 추론(AI Inference), 클라우드 연동(Cloud Integration), 진단(Diagnostics), 사람-로봇 상호작용(Human-Robot Interaction) 등 매우 다양한 기능을 동시에 수행한다. 이러한 기능을 하나의 거대한 프로그램으로 개발하면 코드가 복잡해지고 유지보수가 어려워진다.

이를 해결하기 위해 등장한 것이 **컴포넌트 기반 소프트웨어 아키텍처(Component-Based Software Architecture, CBSA)** 이다. 이 구조에서는 전체 시스템을 여러 개의 독립적인 컴포넌트(Component)로 나누어 개발하며, 각 컴포넌트는 하나의 명확한 기능만 담당한다.

컴포넌트 기반 아키텍처의 핵심 철학은 기능을 독립적인 소프트웨어 모듈(Module)로 분리하는 것이다. 각 컴포넌트는 자신의 내부 구현을 외부에 공개하지 않고, 표준화된 인터페이스(Interface)를 통해서만 다른 컴포넌트와 통신한다.

즉, 내부 알고리즘이 변경되더라도 인터페이스가 동일하다면 다른 모듈에는 영향을 주지 않는다. 이러한 구조는 유지보수성(Maintainability), 재사용성(Reusability), 확장성(Scalability)을 크게 향상시킨다.

이 개념은 하드웨어 설계와 매우 유사하다. 센서(Sensor)나 모터(Motor)를 새로운 제품으로 교체하더라도 전체 로봇을 다시 설계하지 않는 것처럼, 소프트웨어도 특정 인식 알고리즘이나 위치추정 알고리즘만 교체할 수 있어야 한다.

즉, 소프트웨어 역시 조립 가능한 부품(Building Block)처럼 설계하는 것이 컴포넌트 기반 아키텍처의 기본 개념이다.

ROS2는 이러한 구조를 지원하도록 설계된 대표적인 로봇 미들웨어(Middleware)이다. ROS1에서는 대부분 하나의 노드(Node)가 하나의 프로세스(Process)에서 실행되었지만, ROS2에서는 **컴포지션(Composition)** 이라는 기능을 통해 여러 개의 노드를 하나의 프로세스 안에서 동시에 실행할 수 있다.

이를 통해 논리적으로는 독립적인 노드를 유지하면서도 실행 효율을 크게 향상시킬 수 있다.

기존 방식에서는 노드마다 별도의 운영체제 프로세스가 생성되었다. 각각의 프로세스는 독립적인 메모리 공간(Memory Space), 스케줄러(Scheduler), 버퍼(Buffer)를 사용하므로 안정성은 높지만 운영체제의 오버헤드(Overhead)가 증가한다.

특히 대용량 영상(Image)이나 포인트 클라우드(Point Cloud)를 처리하는 경우에는 프로세스 간 통신 때문에 데이터 복사와 직렬화(Serialization)가 반복적으로 발생하여 성능이 크게 저하될 수 있다.

ROS2 Composition은 이러한 문제를 해결하기 위해 **컴포넌트 컨테이너(Component Container)** 라는 실행 환경을 제공한다. 여러 컴포넌트가 하나의 프로세스 안에서 실행되므로 메모리를 공유할 수 있고, 데이터 전달 과정도 훨씬 효율적으로 이루어진다.

즉, 운영체제는 하나의 프로세스로 인식하지만 내부적으로는 여러 개의 독립적인 ROS2 노드가 동시에 실행되는 구조이다.

ROS2에서 컴포넌트(Component)는 단순한 노드(Node)가 아니라 실행 중에 동적으로 적재(Dynamic Loading)할 수 있는 공유 라이브러리(Shared Library) 형태로 작성된다.

즉, 실행 파일을 다시 만들지 않아도 필요한 컴포넌트를 실행 중에 추가하거나 제거할 수 있으며, 다양한 로봇 플랫폼에서 동일한 컴포넌트를 재사용할 수 있다.

이러한 동적 로딩(Dynamic Loading)은 매우 높은 유연성(Flexibility)을 제공한다. 동일한 위치추정(Localization) 컴포넌트가 시뮬레이션(Simulation), 연구실 테스트(Laboratory Test), 산업 현장(Industrial Deployment), 클라우드 기반 시스템 등 다양한 환경에서 그대로 사용할 수 있다.

즉, 하나의 컴포넌트를 여러 프로젝트에서 공유할 수 있으므로 개발 효율이 크게 향상된다.

컴포넌트 컨테이너(Component Container)는 여러 컴포넌트를 실행하고 관리하는 런타임(Runtime) 환경이다. 하나의 컨테이너에는 몇 개의 컴포넌트만 포함될 수도 있고, 수십 개의 컴포넌트가 동시에 실행될 수도 있다.

필요에 따라 AI 추론 컴포넌트와 안전 제어 컴포넌트를 서로 다른 컨테이너에 배치하여 안정성과 성능을 동시에 확보할 수도 있다.

이러한 구조는 기존의 "노드(Node)"와 "전체 시스템(System)" 사이에 새로운 계층을 추가한다.

즉, 개별 컴포넌트(Component) → 컴포넌트 컨테이너(Component Container) → 전체 로봇 시스템(Robot System)이라는 계층적 구조가 형성되며, 소프트웨어를 보다 체계적으로 관리할 수 있다.

ROS2 Composition의 가장 큰 장점 가운데 하나는 **프로세스 내부 통신(Intra-Process Communication)** 최적화이다.

기존 방식에서는 노드 간 메시지를 전달할 때 데이터를 직렬화하고 다시 역직렬화(Deserialization)해야 했다. 이러한 과정은 CPU 사용량을 증가시키고 지연시간(Latency)을 발생시킨다.

그러나 동일한 컨테이너 안에서는 데이터를 포인터(Pointer) 형태로 직접 공유할 수 있다.

이를 **제로 카피(Zero-Copy)** 또는 **근접 제로 카피(Near Zero-Copy)** 라고 한다.

대용량 영상이나 LiDAR 데이터도 메모리 복사 없이 전달할 수 있으므로 CPU 부하가 감소하고 전체 처리 속도가 크게 향상된다.

예를 들어 카메라 영상 획득(Camera Capture), 영상 보정(Image Rectification), 객체 인식(Object Detection), 의미 분할(Semantic Segmentation), 객체 추적(Tracking), 시각화(Visualization)가 하나의 컨테이너 안에서 실행된다면 이미지를 반복적으로 복사하지 않고 공유할 수 있다.

결과적으로 전체 인식 파이프라인(Perception Pipeline)의 성능이 크게 향상된다.

컴포넌트 기반 구조는 시스템 자원(Resource)도 효율적으로 사용할 수 있다. 기존 방식에서는 각 노드마다 별도의 메모리와 스레드(Thread), 통신 버퍼(Buffer)가 필요했지만, Composition에서는 이러한 자원을 여러 컴포넌트가 함께 사용할 수 있다.

따라서 대규모 로봇 시스템에서도 메모리 사용량과 CPU 부하를 효과적으로 줄일 수 있다.

또 다른 장점은 실행 시점의 유연성(Runtime Flexibility)이다. 기존 방식에서는 어떤 기능을 실행할지 컴파일(Compile) 단계에서 결정해야 했지만, ROS2 Composition에서는 실행 중에도 필요한 컴포넌트를 자유롭게 추가하거나 제거할 수 있다.

예를 들어 검사 로봇은 검사 구역에 도착했을 때만 AI 검사 컴포넌트를 실행하고, 서비스 로봇은 사람과 대화할 때만 음성 인식(Speech Recognition) 컴포넌트를 활성화할 수 있다.

미래의 **소프트웨어 정의 로봇(Software-Defined Robot)** 은 이러한 동적 구성(Dynamic Composition)을 적극 활용하게 될 것이다.

AI 모델을 교체하거나 새로운 위치추정 알고리즘을 추가하거나 새로운 센서를 연결하는 작업도 전체 시스템을 다시 시작하지 않고 수행할 수 있는 방향으로 발전하고 있다.

컴포넌트 기반 구조는 객체지향(Object-Oriented) 설계 원칙과도 매우 잘 맞는다. 각 컴포넌트는 자신의 내부 데이터와 알고리즘을 캡슐화(Encapsulation)하며, 외부에서는 공개된 인터페이스(API)만 사용할 수 있다.

이러한 구조는 내부 구현을 자유롭게 변경하면서도 다른 컴포넌트와의 호환성을 유지할 수 있도록 해준다.

컴포넌트 간에는 **낮은 결합도(Low Coupling)** 가 매우 중요하다. 서로의 내부 함수를 직접 호출하기보다는 ROS2의 토픽(Topic), 서비스(Service), 액션(Action), 파라미터(Parameter)를 통해 통신하는 것이 바람직하다.

이러한 구조는 시스템 독립성을 높이고 다양한 플랫폼으로 쉽게 이식(Portability)할 수 있도록 한다.

반대로 각 컴포넌트는 **높은 응집도(High Cohesion)** 를 가져야 한다. 즉 하나의 컴포넌트는 하나의 기능만 수행해야 한다.

예를 들어 위치추정은 위치추정만 담당하고, 경로 계획은 경로 계획만 담당하며, 모터 제어는 모터 제어만 수행해야 한다. 이렇게 해야 컴포넌트의 이해와 유지보수가 쉬워진다.

컴포넌트의 크기(Granularity)도 중요하다. 하나의 컴포넌트가 너무 크면 기존의 모놀리식(Monolithic) 구조와 차이가 없어지고, 너무 작으면 컴포넌트 수가 지나치게 많아져 관리 비용이 증가한다.

따라서 인식, 위치추정, 지도작성, 내비게이션, AI 추론과 같이 기능적으로 자연스럽게 구분되는 수준에서 컴포넌트를 설계하는 것이 일반적이다.

하드웨어 추상화(Hardware Abstraction)도 컴포넌트 기반 구조와 잘 어울린다. 카메라(Camera), LiDAR, GNSS, IMU, 모터 드라이버(Motor Driver), 배터리 관리 시스템(Battery Management System), 통신 인터페이스 등은 각각 독립된 컴포넌트로 구현할 수 있다.

상위 소프트웨어는 실제 하드웨어가 무엇인지 몰라도 동일한 인터페이스를 통해 사용할 수 있다.

이러한 구조는 이기종 컴퓨팅(Heterogeneous Computing) 환경에서도 매우 효과적이다. AI 추론은 GPU에서 수행하고, 제어는 MCU나 FPGA에서 수행하며, 사용자 인터페이스는 CPU에서 실행하는 방식으로 기능을 분산할 수 있다.

컴포넌트 간 인터페이스는 동일하므로 하드웨어가 달라져도 소프트웨어 구조는 유지된다.

ROS2의 **라이프사이클 노드(Lifecycle Node)** 역시 컴포넌트 기반 구조와 잘 결합된다. 각 컴포넌트는 초기화(Unconfigured), 비활성(Inactive), 활성(Active), 종료(Finalized) 등의 상태를 가지며, 안전하게 시작하고 종료할 수 있다.

특히 산업용 로봇에서는 센서 초기화와 하드웨어 준비를 순차적으로 수행해야 하므로 이러한 기능이 매우 중요하다.

다만 하나의 프로세스 안에 여러 컴포넌트가 존재하므로 장애 격리(Fault Isolation)는 고려해야 할 요소이다.

하나의 컴포넌트에서 치명적인 오류가 발생하면 동일한 컨테이너 안의 다른 컴포넌트에도 영향을 줄 수 있다. 따라서 안전 제어(Safety Control)나 실시간 제어(Real-Time Control)는 별도의 컨테이너로 분리하는 경우가 많다.

실제 산업 현장에서는 혼합 구조(Hybrid Deployment)를 많이 사용한다. 영상 처리와 AI 추론처럼 데이터 교환량이 많은 모듈은 하나의 컨테이너에서 실행하여 성능을 높이고, 안전 제어나 사용자 인터페이스는 독립된 프로세스로 실행하여 안정성을 확보한다.

즉 성능과 안정성을 균형 있게 고려하는 것이 일반적인 설계 방식이다.

컴포넌트 기반 구조는 시험(Test)과 검증(Verification)도 매우 쉽다. 하나의 컴포넌트만 독립적으로 단위 시험(Unit Test)할 수 있으며, 실제 하드웨어 대신 모의 객체(Mock Object)를 연결하여 자동 시험도 수행할 수 있다.

또한 시뮬레이션에서는 센서 컴포넌트만 가상 장치로 교체하고 나머지 소프트웨어는 그대로 사용할 수 있다.

지속적 통합(Continuous Integration, CI)과 지속적 배포(Continuous Deployment, CD) 환경에서도 장점이 크다. 변경된 컴포넌트만 다시 컴파일하고 시험하면 되므로 개발 속도가 빨라지고 배포도 간단해진다.

또한 여러 개발팀이 동시에 작업하더라도 인터페이스만 유지된다면 서로의 작업에 영향을 거의 주지 않는다.

최근 AI 기술의 발전은 컴포넌트 기반 구조의 중요성을 더욱 높이고 있다. 객체 인식(Object Detection), 파운데이션 모델(Foundation Model), 대규모 언어 모델(LLM), 의미 지도(Semantic Mapping), 강화학습 정책(Reinforcement Learning Policy) 등은 매우 빠르게 발전한다.

이러한 AI 기능을 각각 독립된 컴포넌트로 구현하면 새로운 모델이 나올 때 해당 컴포넌트만 교체하면 되므로 전체 시스템을 수정할 필요가 없다.

ROS2 Composition에서는 각 컴포넌트가 다양한 파라미터(Parameter)를 제공한다. 알고리즘의 임계값(Threshold), 센서 보정값(Calibration), 통신 설정(Communication Setting), AI 모델 설정 등을 실행 중에도 변경할 수 있으므로 매우 유연한 시스템 운영이 가능하다.

관측성(Observability)도 뛰어나다. 각 컴포넌트는 자신의 CPU 사용률, 메모리 사용량, 실행 시간, 진단 정보(Diagnostics), 성능 지표(Metrics)를 독립적으로 제공할 수 있으며, 플릿 관리 시스템(Fleet Management System)은 이러한 정보를 종합하여 전체 로봇 상태를 분석할 수 있다.

사이버보안(Cybersecurity)도 기능별로 적용할 수 있다. 인증(Authentication), 접근 제어(Access Control), 암호화 통신(Encrypted Communication), 인증서 관리(Certificate Management)를 컴포넌트 단위로 적용하면 보다 세밀한 보안 정책을 구현할 수 있다.

컴포넌트의 재사용성은 매우 높다. 하나의 위치추정 컴포넌트는 AMR, 물류 로봇, 농업 로봇, 순찰 로봇, 휴머노이드 등 다양한 플랫폼에서 그대로 사용할 수 있으며, 하드웨어 인터페이스만 변경하면 상위 소프트웨어는 수정할 필요가 거의 없다.

시뮬레이션과 실제 로봇의 전환(Sim-to-Real)도 매우 간단하다. 센서와 모터 컴포넌트만 실제 장치로 교체하면 인식, 계획, AI, 행동 관리 소프트웨어는 동일하게 사용할 수 있으므로 개발 효율이 크게 향상된다.

미래의 **소프트웨어 정의 로봇(Software-Defined Robot)** 과 **피지컬 AI(Physical AI)** 시대에는 이러한 컴포넌트 기반 구조가 더욱 중요해질 것이다. 파운데이션 모델, 비전-언어-행동 모델(Vision-Language-Action Model, VLA), 월드 모델(World Model), 디지털 트윈(Digital Twin), 플릿 AI(Fleet AI)는 모두 독립적인 컴포넌트로 구성되어 필요에 따라 자유롭게 조합될 것으로 예상된다.

결론적으로 **컴포넌트 기반 로봇 아키텍처(Component-Based Robot Architecture)** 와 **ROS2 Composition** 은 소프트웨어를 독립적인 기능 단위로 분리하면서도 높은 실행 성능을 제공하는 현대 로봇 소프트웨어의 핵심 구조이다. 이러한 구조는 모듈성(Modularity), 재사용성(Reusability), 확장성(Scalability), 유지보수성(Maintainability), 성능(Performance)을 동시에 확보할 수 있으며, 향후 **AI 네이티브 로봇(AI-Native Robot)** 과 **소프트웨어 정의 로봇(Software-Defined Robot)** 의 핵심 소프트웨어 기반 기술로 자리 잡을 것으로 기대된다.

##  

## 06.04 Mixed Real-Time and Non-Real-Time Architecture Design

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

Modern robotic systems have evolved into highly sophisticated cyber-physical platforms that simultaneously execute deterministic control algorithms, computationally intensive artificial intelligence, distributed communication, cloud connectivity, human interaction, and long-term mission planning. These diverse functions possess fundamentally different timing requirements and computational characteristics. While low-level motor control may require deterministic execution every few hundred microseconds, AI perception algorithms often tolerate processing delays measured in tens of milliseconds, and cloud synchronization or mission scheduling may operate at intervals of several seconds or even minutes. Designing a single software architecture capable of supporting these vastly different execution models represents one of the most challenging problems in robotics software engineering. Consequently, modern robotic platforms increasingly adopt **Mixed Real-Time and Non-Real-Time Architecture**, an architectural paradigm that combines deterministic control systems with general-purpose computing while preserving both safety and computational efficiency.

The distinction between real-time and non-real-time computing extends beyond execution speed. Real-time systems are characterized not merely by fast execution but by predictable timing. A computation that consistently completes within one millisecond is often preferable to another that usually completes in one hundred microseconds but occasionally requires five milliseconds. Deterministic behavior is therefore considerably more important than average execution speed. Robot control algorithms depend upon consistent update intervals because irregular timing introduces instability, degraded tracking accuracy, oscillation, or even unsafe physical behavior.

Real-time systems are generally classified into hard real-time, firm real-time, and soft real-time categories. Hard real-time tasks possess absolute deadlines that must never be violated. Missing even a single deadline may produce catastrophic system failure. Examples include motor current regulation, actuator synchronization, braking control, collision avoidance safety circuits, emergency stop processing, and certain industrial motion controllers. These tasks frequently execute within embedded microcontrollers, programmable logic controllers, FPGA-based controllers, or real-time operating systems specifically designed to guarantee deterministic scheduling.

Firm real-time tasks tolerate occasional missed deadlines but discard computations that arrive too late to remain useful. Localization updates, obstacle tracking, sensor fusion, trajectory generation, and visual servoing frequently belong to this category. Missing isolated updates may slightly reduce performance without causing immediate system failure, provided the next computation arrives on schedule.

Soft real-time tasks possess timing objectives rather than absolute deadlines. Delayed execution may reduce system responsiveness but rarely causes catastrophic failure. Examples include AI object detection, semantic mapping, natural language processing, cloud synchronization, fleet coordination, telemetry, diagnostics, user interface updates, and mission planning. These applications benefit from low latency but generally prioritize computational throughput over deterministic timing.

A modern autonomous robot simultaneously executes applications belonging to all three timing categories. Consequently, attempting to implement every function under a single execution model inevitably produces either unnecessary computational restrictions or unacceptable timing uncertainty. Mixed real-time architecture resolves this conflict by separating software according to temporal requirements rather than functional domains alone.

The lowest architectural layer typically consists of deterministic control systems responsible for direct interaction with physical hardware. Motion controllers continuously regulate wheel velocities, joint positions, actuator torque, steering angles, balance stabilization, and sensor sampling. These controllers execute within precisely scheduled loops operating at frequencies ranging from several hundred Hertz to several kilohertz. Their execution timing remains strictly deterministic regardless of AI workload, network activity, visualization software, or user interaction.

Embedded controllers frequently employ real-time operating systems such as FreeRTOS, Zephyr, VxWorks, QNX, or PREEMPT_RT Linux to achieve deterministic scheduling. Priority-based preemption, bounded interrupt latency, predictable memory allocation, lock-free communication mechanisms, and static resource management collectively ensure timing guarantees essential for safe robot operation.

Above the deterministic control layer resides the robot middleware layer responsible for coordinating communication among higher-level software components. Middleware platforms such as ROS2 provide publish-subscribe messaging, service invocation, action interfaces, parameter management, lifecycle control, and distributed communication while supporting both real-time and non-real-time software modules. Careful middleware configuration ensures that deterministic control traffic remains isolated from computationally intensive background activities.

High-level perception, planning, and artificial intelligence generally execute within non-real-time environments running standard Linux distributions. These applications perform computationally expensive operations including deep neural network inference, semantic scene understanding, object recognition, simultaneous localization and mapping, reinforcement learning, language processing, cloud communication, digital twin synchronization, and mission optimization. Although these workloads require substantial computational resources, they rarely require strict microsecond-level timing guarantees.

Separating deterministic control from computationally intensive AI provides numerous architectural advantages. Deep neural network inference may consume hundreds of milliseconds depending upon model complexity and hardware acceleration. If motor controllers were forced to share scheduling resources directly with AI inference engines, unpredictable execution delays could compromise robot stability. By isolating deterministic controllers within dedicated real-time environments, AI workloads remain unable to interfere with safety-critical execution.

Time-scale separation naturally emerges throughout mixed real-time architectures. Motor current regulation often executes at frequencies exceeding ten kilohertz. Joint position control may execute at one kilohertz. Sensor fusion and localization typically operate between fifty and two hundred Hertz. Motion planning commonly updates several times per second. AI perception processes camera images between ten and sixty frames per second. Fleet coordination, mission scheduling, cloud synchronization, and database logging frequently execute at intervals measured in seconds or minutes. Assigning each subsystem to an execution environment matching its timing characteristics significantly improves both computational efficiency and system stability.

Communication between real-time and non-real-time domains represents one of the most important architectural challenges. Bidirectional information exchange must occur without allowing nondeterministic software to delay deterministic control loops. Various communication mechanisms address this requirement, including shared memory, lock-free queues, double buffering, ring buffers, publish-subscribe middleware, real-time message passing, and asynchronous event channels. Proper interface design minimizes blocking behavior while preserving deterministic execution.

Shared memory offers extremely low communication latency because data remain within a common address space. However, synchronization mechanisms must avoid unpredictable locking delays. Lock-free algorithms employing atomic operations frequently replace conventional mutexes within real-time software because they guarantee bounded execution time while preventing priority inversion.

Double buffering provides another widely adopted technique. One memory buffer stores newly acquired sensor data while another simultaneously serves computational algorithms. After processing completes, buffer pointers are exchanged atomically without copying large datasets. This strategy allows perception pipelines to process continuous sensor streams without interrupting deterministic acquisition tasks.

Ring buffers support high-frequency streaming applications such as camera acquisition, LiDAR processing, inertial sensing, and telemetry logging. Producers continuously insert new data while consumers retrieve information independently. Proper buffer sizing accommodates temporary computational overload without losing critical information.

Asynchronous message passing represents another common architectural approach. Real-time controllers publish state estimates, actuator feedback, and diagnostic information without waiting for receivers. Non-real-time software subscribes to these data streams independently. Because publishers never block, deterministic execution remains unaffected regardless of subscriber performance.

Scheduling strategy significantly influences mixed real-time performance. Fixed-priority scheduling assigns higher execution priority to deterministic control loops while lower-priority background tasks execute only when computational resources become available. Earliest Deadline First scheduling dynamically prioritizes tasks according to timing requirements. Many industrial systems combine multiple scheduling policies across heterogeneous processors to maximize both responsiveness and computational utilization.

Processor affinity provides additional timing isolation. Safety-critical control threads may execute exclusively on dedicated processor cores while AI inference, visualization, cloud communication, and user interfaces occupy separate cores. Modern multicore processors therefore enable strong temporal isolation without requiring physically separate computers.

Hardware heterogeneity further enhances mixed real-time architectures. Embedded microcontrollers execute deterministic motor control. Industrial PLCs supervise safety functions. Edge CPUs coordinate middleware and robot behaviors. GPUs accelerate neural network inference. FPGAs perform sensor preprocessing and high-speed signal processing. Cloud servers optimize fleet operations and long-term planning. Distributing computation according to hardware specialization improves overall efficiency while preserving deterministic control.

Artificial intelligence introduces particularly demanding architectural considerations because AI workloads possess highly variable execution times. Deep neural networks frequently exhibit input-dependent computational complexity influenced by image content, object count, network architecture, hardware scheduling, and memory bandwidth. Consequently, AI inference should rarely execute directly within deterministic control loops. Instead, inference services produce perception outputs asynchronously while control systems consume the latest available validated information.

Perception pipelines illustrate this architectural separation effectively. Cameras continuously capture images using deterministic acquisition timing. Image processing components asynchronously perform distortion correction, color conversion, feature extraction, neural network inference, semantic segmentation, and object tracking. Control algorithms subsequently utilize the most recent validated perception results without waiting for inference completion. This pipeline maintains deterministic control while supporting computationally intensive AI.

Sensor fusion similarly benefits from mixed timing models. High-frequency IMU measurements update continuously within deterministic estimation algorithms while slower camera-based visual odometry and LiDAR localization periodically refine accumulated estimates. State estimation frameworks integrate these heterogeneous data streams according to measurement availability rather than forcing uniform execution timing across all sensors.

Safety architecture remains fundamentally dependent upon temporal isolation. Emergency stop processing, collision detection, braking controllers, speed limit enforcement, watchdog supervision, hardware interlocks, and redundant monitoring systems must operate independently of high-level software. Even complete failure of AI perception, cloud communication, or mission planning must never prevent immediate execution of emergency safety mechanisms.

Watchdog systems frequently supervise communication between deterministic and nondeterministic domains. If expected messages fail to arrive within predefined intervals, watchdog controllers automatically transition the robot into safe operational states. Such fail-safe mechanisms prevent communication failures from producing hazardous physical behavior.

Mixed real-time architecture also simplifies fault containment. Failures within visualization software, cloud connectivity, language processing, or AI inference rarely propagate into deterministic controllers because execution environments remain isolated. Recovery procedures may restart failed non-real-time applications without interrupting fundamental robot control.

Energy management becomes increasingly important within heterogeneous architectures. High-performance GPU inference substantially increases power consumption and thermal generation. Dynamic workload scheduling allows robots to activate computationally intensive AI only when operationally necessary while maintaining continuous deterministic control using comparatively energy-efficient embedded processors.

Thermal management similarly benefits from architectural partitioning. When GPU temperature approaches operational limits, AI inference frequency may decrease while deterministic controllers continue operating unaffected. Such graceful degradation maintains safe operation even under constrained computational resources.

ROS2 supports mixed real-time architectures through careful separation of executors, callback groups, Quality of Service policies, intra-process communication, lifecycle management, and composition mechanisms. Real-time nodes frequently employ dedicated executors with static memory allocation and deterministic callback scheduling. Non-real-time nodes execute independently using multithreaded executors optimized for throughput rather than deterministic latency.

Quality of Service configuration plays a particularly important role. Control messages often require reliable delivery with bounded latency, whereas sensor streams may prioritize low latency over guaranteed delivery. AI perception outputs frequently utilize best-effort communication because newer information rapidly supersedes delayed messages. Appropriate QoS selection therefore aligns communication behavior with application requirements.

Containerization introduces additional deployment flexibility. Real-time software frequently executes outside conventional container environments to minimize scheduling overhead, whereas AI services, databases, cloud gateways, monitoring tools, and web interfaces readily operate within Docker or Kubernetes infrastructures. Hybrid deployment strategies combine deterministic embedded software with cloud-native application management.

Simulation and testing become considerably more manageable under mixed real-time architecture. Deterministic controllers undergo hardware-in-the-loop validation while AI perception executes using prerecorded sensor datasets. Software-in-the-loop simulation evaluates planning algorithms independently of physical hardware. Digital twins reproduce large-scale mission behavior without requiring complete real-time execution. Layered verification improves software quality while reducing debugging complexity.

Industrial robotics provides numerous practical examples of mixed real-time architecture. Collaborative manipulators execute joint control loops within dedicated servo controllers while vision systems perform object recognition on GPU-equipped industrial computers. Autonomous warehouse robots regulate wheel motion using embedded motor controllers while fleet management software optimizes transportation assignments through cloud infrastructure. Inspection robots synchronize cameras and lighting deterministically while AI defect detection executes asynchronously using edge GPUs. Autonomous agricultural machinery combines deterministic steering controllers with AI-based crop recognition and cloud-assisted field management.

Humanoid robots represent perhaps the most demanding application of mixed real-time architecture. Whole-body balance control, force regulation, and gait stabilization require deterministic execution measured in microseconds or milliseconds. Simultaneously, perception, language understanding, multimodal reasoning, manipulation planning, world modeling, and conversational AI execute within nondeterministic computing environments. Architectural separation enables sophisticated intelligence without compromising physical stability.

The emergence of Physical AI further increases the importance of mixed real-time design. Foundation models, Vision-Language-Action architectures, world models, semantic reasoning engines, and lifelong learning systems introduce unprecedented computational complexity while deterministic motor control remains fundamentally unchanged. Future robots will therefore increasingly employ heterogeneous architectures integrating real-time embedded controllers, high-performance AI accelerators, edge computing platforms, distributed cloud infrastructure, and adaptive middleware.

Software-defined robotics also depends heavily upon mixed timing architectures. Runtime software updates, dynamic AI model replacement, adaptive computational scheduling, cloud-assisted optimization, and over-the-air deployment require flexible non-real-time software while preserving deterministic safety functions that remain continuously operational throughout system reconfiguration.

Ultimately, Mixed Real-Time and Non-Real-Time Architecture Design represents one of the foundational principles of modern robotics software engineering. By separating deterministic control from computationally intensive intelligence while maintaining carefully designed communication interfaces, temporal isolation, fault containment, and hardware specialization, this architecture enables robotic systems to achieve both strict safety guarantees and advanced autonomous intelligence. As robotics continues evolving toward AI-native cyber-physical systems operating in increasingly dynamic environments, mixed real-time architectures will remain indispensable for integrating precision control, artificial intelligence, distributed computing, and long-term operational reliability into a unified and scalable software framework.

현대의 로봇은 단순히 모터를 제어하는 장치가 아니라, 인공지능(AI), 센서 융합(Sensor Fusion), 자율주행(Autonomous Navigation), 클라우드(Cloud), 사람-로봇 상호작용(Human-Robot Interaction) 등을 동시에 수행하는 **사이버-물리 시스템(Cyber-Physical System)** 으로 발전하고 있다. 이러한 기능들은 모두 서로 다른 실행 시간과 계산 특성을 가지므로 하나의 실행 방식만으로는 전체 시스템을 효율적으로 운영할 수 없다.

예를 들어 모터 제어는 수백 마이크로초에서 수 밀리초 안에 반드시 수행되어야 하지만, AI 객체 인식(Object Detection)은 수십에서 수백 밀리초 정도의 지연이 발생해도 시스템이 정상적으로 동작할 수 있다. 또한 클라우드 동기화나 데이터 저장은 수 초 단위의 실행으로도 충분하다. 따라서 현대 로봇은 **실시간(Real-Time)** 과 **비실시간(Non-Real-Time)** 기능을 함께 운용하는 혼합 아키텍처를 사용한다.

실시간 시스템(Real-Time System)의 핵심은 빠른 실행 속도가 아니라 **예측 가능한 실행 시간(Predictable Timing)** 이다. 평균적으로 매우 빠른 시스템보다 항상 일정한 시간 안에 실행을 완료하는 시스템이 로봇에서는 훨씬 중요하다. 제어 주기가 일정하지 않으면 로봇의 움직임이 불안정해지고 진동(Oscillation), 위치 오차(Position Error), 심한 경우 충돌(Collision)까지 발생할 수 있기 때문이다.

따라서 실시간 시스템은 평균 성능보다 실행 시간의 일관성과 결정성(Determinism)을 가장 중요한 목표로 삼는다.

실시간 시스템은 일반적으로 **하드 실시간(Hard Real-Time)**, **펌 실시간(Firm Real-Time)**, **소프트 실시간(Soft Real-Time)** 으로 구분된다.

하드 실시간은 정해진 마감 시간(Deadline)을 단 한 번도 초과해서는 안 되는 시스템이다. 모터 전류 제어(Motor Current Control), 브레이크 제어(Brake Control), 비상 정지(Emergency Stop), 산업용 서보 제어(Servo Control) 등이 여기에 속하며, 한 번의 시간 초과도 시스템 안전을 위협할 수 있다.

펌 실시간은 마감 시간을 놓친 결과는 폐기되지만, 다음 실행이 정상적으로 이루어지면 시스템은 계속 동작할 수 있는 구조이다. 위치추정(Localization), 장애물 추적(Object Tracking), 센서 융합(Sensor Fusion), 시각 서보(Visual Servoing) 등이 대표적인 예이다.

소프트 실시간은 실행 시간이 다소 늦어져도 전체 시스템이 즉시 실패하지는 않는 작업이다. 객체 인식(Object Recognition), 의미 지도(Semantic Mapping), 자연어 처리(Natural Language Processing), 클라우드 동기화(Cloud Synchronization), 사용자 인터페이스(User Interface), 데이터 로깅(Data Logging) 등이 이에 해당한다.

현대의 자율주행 로봇은 이 세 가지 실행 방식을 모두 동시에 사용한다. 따라서 하나의 운영체제나 하나의 실행 모델만으로 전체 시스템을 구성하는 것은 현실적으로 어렵다.

혼합 실시간 아키텍처(Mixed Real-Time Architecture)는 이러한 문제를 해결하기 위해 기능이 아니라 **시간 특성(Time Characteristics)** 을 기준으로 시스템을 분리한다.

가장 아래에는 실시간 제어 계층(Real-Time Control Layer)이 위치한다. 이 계층은 모터 제어(Motor Control), 조향(Steering), 토크 제어(Torque Control), 자세 안정화(Stabilization), 센서 샘플링(Sensor Sampling) 등을 담당한다.

이 계층의 제어 루프(Control Loop)는 일반적으로 수백 헤르츠에서 수천 헤르츠(Hz)로 실행되며, AI나 사용자 인터페이스의 부하와 관계없이 항상 동일한 주기로 실행되어야 한다.

이를 위해 FreeRTOS, Zephyr, VxWorks, QNX, PREEMPT_RT Linux 등의 **실시간 운영체제(Real-Time Operating System, RTOS)** 가 사용된다. 이러한 운영체제는 우선순위 기반 스케줄링(Priority Scheduling), 짧은 인터럽트 지연(Interrupt Latency), 정적 메모리 할당(Static Memory Allocation), 결정론적 스케줄링(Deterministic Scheduling)을 제공하여 일정한 실행 시간을 보장한다.

실시간 제어 계층 위에는 **미들웨어 계층(Middleware Layer)** 이 존재한다. ROS2와 같은 로봇 미들웨어는 토픽(Topic), 서비스(Service), 액션(Action), 파라미터(Parameter), 라이프사이클(Lifecycle) 등을 관리하며 실시간과 비실시간 소프트웨어 사이의 연결 역할을 수행한다.

미들웨어는 실시간 제어 메시지가 AI 연산이나 사용자 인터페이스 때문에 지연되지 않도록 적절하게 통신을 관리해야 한다.

상위 계층에서는 AI 추론(AI Inference), 객체 인식(Object Detection), 의미 이해(Semantic Understanding), 자연어 처리(Language Processing), 디지털 트윈(Digital Twin), 클라우드 연동(Cloud Integration), 플릿 관리(Fleet Management) 등이 수행된다.

이러한 작업은 계산량이 매우 크지만 반드시 수 밀리초 안에 끝날 필요는 없다. 따라서 일반 Linux 환경에서 GPU를 이용하여 실행되는 경우가 대부분이다.

실시간 제어와 AI를 분리하면 많은 장점이 있다. 예를 들어 딥러닝(Deep Learning) 추론은 영상의 복잡도에 따라 실행 시간이 크게 달라질 수 있다. 만약 이러한 AI 연산이 모터 제어와 같은 CPU에서 동일한 우선순위로 실행된다면 제어 주기가 흔들려 로봇이 불안정해질 수 있다.

반면 제어 계층을 독립적으로 구성하면 AI가 아무리 복잡한 계산을 수행하더라도 모터 제어에는 영향을 주지 않는다.

혼합 아키텍처에서는 기능마다 서로 다른 시간 주기를 가진다. 모터 전류 제어는 10kHz 이상으로 실행될 수 있고, 자세 제어는 1kHz 정도에서 실행된다. 센서 융합은 100Hz 내외, 경로 계획(Path Planning)은 수 Hz, AI 영상 처리는 초당 10\~60 프레임(Frame), 클라우드 동기화는 수 초 간격으로 실행된다.

이처럼 각 기능을 자신의 특성에 맞는 실행 주기로 분리하면 전체 시스템의 효율성과 안정성이 크게 향상된다.

실시간과 비실시간 시스템 사이의 통신은 가장 중요한 설계 요소 가운데 하나이다. AI에서 생성한 객체 정보를 제어기가 받아야 하고, 제어기의 상태 정보를 AI가 다시 활용해야 하기 때문이다.

하지만 비실시간 작업이 실시간 제어를 기다리게 하거나 반대로 실시간 제어가 AI의 계산이 끝날 때까지 대기해서는 안 된다.

이를 해결하기 위해 **공유 메모리(Shared Memory)**, **락 프리 큐(Lock-Free Queue)**, **더블 버퍼(Double Buffer)**, **링 버퍼(Ring Buffer)**, **비동기 메시지(Asynchronous Messaging)** 와 같은 구조가 사용된다.

공유 메모리는 매우 빠르지만 락(Lock)에 의해 실행 시간이 예측 불가능해질 수 있다. 따라서 실시간 시스템에서는 일반적인 뮤텍스(Mutex) 대신 원자 연산(Atomic Operation)을 사용하는 경우가 많다.

이러한 구조는 실시간성을 유지하면서도 높은 데이터 처리 성능을 제공한다.

더블 버퍼(Double Buffer)는 두 개의 메모리를 번갈아 사용하는 방식이다. 하나의 버퍼에는 센서가 새로운 데이터를 기록하고, 다른 버퍼에서는 AI가 데이터를 처리한다. 처리가 끝나면 버퍼의 역할만 교환하므로 데이터 복사가 거의 발생하지 않는다.

이 방식은 카메라(Camera), LiDAR, 레이더(Radar) 등의 대용량 데이터를 처리할 때 매우 효과적이다.

링 버퍼(Ring Buffer)는 센서 데이터가 지속적으로 생성되는 환경에서 많이 사용된다. 생산자(Producer)는 데이터를 계속 저장하고 소비자(Consumer)는 필요한 시점에 데이터를 읽는다.

버퍼의 크기를 적절히 설계하면 순간적으로 AI가 느려져도 중요한 데이터를 잃지 않고 계속 처리할 수 있다.

비동기 메시지(Asynchronous Message)는 실시간 제어기가 데이터를 전송한 후 상대방의 응답을 기다리지 않는 방식이다. 제어기는 항상 자신의 주기를 유지하면서 데이터를 송신하고, AI나 클라우드는 필요한 시점에 이를 수신한다.

이러한 구조는 실시간성과 독립성을 동시에 확보할 수 있는 가장 일반적인 통신 방식이다.

작업 스케줄링(Scheduling)도 매우 중요하다. 일반적으로 모터 제어와 같은 하드 실시간 작업은 가장 높은 우선순위를 가지며, AI나 사용자 인터페이스는 낮은 우선순위에서 실행된다.

멀티코어(Multi-Core) CPU에서는 특정 코어(Core)를 제어 전용으로 할당하고, 다른 코어에서 AI를 실행하는 방식도 많이 사용된다. 이를 **프로세서 친화성(Processor Affinity)** 이라고 한다.

현대 로봇은 CPU뿐 아니라 GPU, MCU, FPGA, DSP 등을 함께 사용하는 **이기종 컴퓨팅(Heterogeneous Computing)** 구조를 사용한다.

예를 들어 MCU는 모터 제어를 담당하고, FPGA는 센서 전처리를 수행하며, GPU는 AI 추론을 수행하고, 클라우드는 플릿 최적화와 장기 계획을 수행한다.

이처럼 기능을 하드웨어 특성에 맞게 분산하면 성능과 전력 효율을 동시에 향상시킬 수 있다.

AI는 실행 시간이 일정하지 않다는 특징을 가진다. 동일한 모델이라도 입력 영상의 복잡도에 따라 실행 시간이 달라질 수 있으며, GPU 부하나 메모리 사용량에 따라서도 변화한다.

따라서 AI 추론은 실시간 제어 루프 안에서 직접 실행하지 않고, 독립적인 서비스(Service) 형태로 실행한 후 결과만 제어기에 전달하는 것이 일반적이다.

예를 들어 카메라는 일정한 주기로 영상을 획득하고, AI는 이를 비동기적으로 분석한다. 제어기는 AI가 가장 최근에 생성한 결과만 활용하여 제어를 수행하므로 AI 실행 시간이 변해도 제어 주기는 일정하게 유지된다.

센서 융합(Sensor Fusion)도 동일한 방식으로 동작한다. IMU는 매우 높은 주기로 데이터를 생성하고, 카메라와 LiDAR는 상대적으로 느리게 동작한다.

상태 추정(State Estimation)은 각 센서의 실행 주기에 맞추어 데이터를 융합하므로 모든 센서를 동일한 주기로 동작시킬 필요가 없다.

안전(Safety)은 혼합 아키텍처에서 가장 중요한 요소이다. 비상 정지(Emergency Stop), 충돌 방지(Collision Avoidance), 브레이크 제어(Brake Control), 속도 제한(Speed Limitation)은 AI와 독립적으로 동작해야 한다.

AI가 오류를 일으키거나 클라우드 연결이 끊기더라도 안전 기능은 항상 정상적으로 동작해야 한다.

이를 위해 **워치독(Watchdog)** 이 사용된다. 워치독은 일정 시간 동안 제어기나 AI가 정상 신호를 보내지 않으면 자동으로 안전 정지(Safe Stop)를 수행한다.

이러한 구조는 통신 장애나 소프트웨어 오류가 발생해도 위험한 상황을 방지할 수 있도록 한다.

혼합 구조는 장애 격리(Fault Isolation)에도 유리하다. 사용자 인터페이스가 종료되거나 AI 프로그램이 충돌하더라도 제어기는 계속 동작할 수 있으며, AI만 다시 시작하면 된다.

즉 전체 시스템이 아니라 문제가 발생한 부분만 복구할 수 있으므로 신뢰성이 크게 향상된다.

전력 관리(Energy Management)와 열 관리(Thermal Management)도 혼합 구조에서 중요한 요소이다. GPU는 많은 전력을 소비하고 높은 발열을 발생시키므로 필요할 때만 AI를 실행하고, 평상시에는 저전력 MCU가 제어를 담당하도록 설계할 수 있다.

GPU 온도가 높아지면 AI의 실행 빈도만 줄이고 제어기는 계속 정상적으로 동작하도록 설계하는 것이 일반적이다.

ROS2는 이러한 혼합 구조를 지원하기 위해 실행기(Executor), 콜백 그룹(Callback Group), QoS(Quality of Service), 라이프사이클(Lifecycle), 컴포지션(Composition) 기능을 제공한다.

실시간 노드는 별도의 실행기에서 실행하고, AI 노드는 멀티스레드 실행기(Multi-Threaded Executor)에서 실행하는 방식으로 서로 간섭을 최소화할 수 있다.

QoS 설정도 매우 중요하다. 제어 메시지는 반드시 전달되어야 하지만 AI 영상은 새로운 프레임이 더 중요하므로 이전 프레임이 일부 손실되어도 문제가 없는 경우가 많다.

따라서 기능의 특성에 따라 QoS를 적절히 설정해야 전체 시스템의 효율을 높일 수 있다.

컨테이너(Container) 기반 배치도 많이 사용된다. AI 서버(Database, Monitoring, Web Interface)는 Docker나 Kubernetes에서 실행하고, 실시간 제어기는 일반적으로 컨테이너 밖에서 실행하여 결정론적인 성능을 유지한다.

실시간성과 클라우드 기술을 동시에 활용하는 대표적인 구조이다.

혼합 아키텍처는 시험(Test)과 검증(Verification)도 효율적으로 수행할 수 있다. 제어기는 **하드웨어 인 더 루프(Hardware-in-the-Loop, HIL)** 로 시험하고, AI는 저장된 데이터셋을 이용하여 평가하며, 플릿 알고리즘은 디지털 트윈(Digital Twin)에서 검증할 수 있다.

각 계층을 독립적으로 시험할 수 있으므로 개발 효율과 신뢰성이 크게 향상된다.

산업 현장에서도 이러한 구조는 널리 사용된다. 협동로봇(Collaborative Robot)은 실시간 서보 제어와 GPU 기반 비전 AI를 분리하여 운영하고, 물류 AMR은 MCU에서 바퀴 제어를 수행하는 동시에 클라우드에서는 플릿 최적화를 수행한다. 검사 로봇은 조명과 카메라를 실시간으로 동기화하면서 AI 기반 결함 검출은 엣지 GPU에서 수행한다.

휴머노이드(Humanoid)는 가장 복잡한 사례이다. 균형 제어(Balance Control), 보행(Gait Control), 힘 제어(Force Control)는 하드 실시간으로 수행되고, 언어 이해(Language Understanding), 월드 모델(World Model), 대규모 언어 모델(LLM), 행동 계획(Action Planning)은 비실시간 환경에서 수행된다. 이러한 분리를 통해 높은 수준의 AI와 안정적인 물리 제어를 동시에 구현할 수 있다.

앞으로 **피지컬 AI(Physical AI)** 시대에는 이러한 혼합 구조의 중요성이 더욱 커질 것이다. 파운데이션 모델(Foundation Model), 비전-언어-행동 모델(Vision-Language-Action Model, VLA), 월드 모델(World Model), 평생 학습(Lifelong Learning)은 매우 큰 계산량을 요구하지만, 모터 제어는 여전히 결정론적인 실시간 실행이 필요하다.

따라서 미래의 로봇은 MCU, CPU, GPU, FPGA, 엣지 컴퓨터(Edge Computer), 클라우드(Cloud)를 함께 사용하는 대규모 혼합 아키텍처를 기반으로 발전하게 될 것이다.

또한 **소프트웨어 정의 로봇(Software-Defined Robot)** 은 OTA 업데이트(Over-the-Air Update), AI 모델 교체, 동적 자원 관리(Dynamic Resource Management), 클라우드 최적화 등을 지속적으로 수행하면서도 제어 시스템은 중단 없이 안전하게 동작해야 한다. 이러한 요구사항 역시 혼합 실시간 아키텍처가 가장 효과적으로 지원한다.

결론적으로 **실시간과 비실시간 혼합 아키텍처(Mixed Real-Time and Non-Real-Time Architecture)** 는 현대 로봇 소프트웨어의 핵심 설계 원칙 가운데 하나이다. 결정론적인 제어(Control)와 고성능 인공지능(AI), 분산 컴퓨팅(Distributed Computing), 클라우드(Cloud), 엣지 컴퓨팅(Edge Computing)을 서로 독립적으로 운영하면서도 효율적으로 연결함으로써, 높은 안전성(Safety), 실시간성(Real-Time), 확장성(Scalability), 유지보수성(Maintainability), 신뢰성(Reliability)을 동시에 확보할 수 있다. 이러한 구조는 향후 **AI 네이티브 로봇(AI-Native Robot)** 과 **피지컬 AI(Physical AI)** 의 핵심 소프트웨어 아키텍처로 더욱 중요한 역할을 수행하게 될 것이다.

##  

## 06.05 State Machines and Behavior Trees in Robot Software

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

As robotic systems become increasingly autonomous, their software must manage not only individual control algorithms but also the complex sequencing of behaviors required to accomplish meaningful tasks. Modern robots continuously perceive their environments, interpret sensor information, make decisions, react to unexpected events, coordinate multiple subsystems, and recover from failures while maintaining safe and predictable operation. These activities require more than mathematical control algorithms; they require structured mechanisms for representing robot behavior and controlling execution flow. Among the numerous behavioral modeling techniques developed in robotics, **Finite State Machines (FSMs)** and **Behavior Trees (BTs)** have emerged as the two most influential architectural approaches for organizing robot decision logic. Each provides a systematic framework for representing robot behaviors, coordinating execution, and managing interactions among perception, planning, and control modules.

Behavior representation has always been one of the central challenges in robotics software engineering. While perception algorithms identify objects and localization algorithms estimate robot position, neither determines how the robot should respond when multiple events occur simultaneously. A mobile robot navigating through a warehouse, for example, must determine whether to continue moving toward its destination, avoid an obstacle, recharge its battery, wait for an automatic door to open, recover from localization failure, or respond to an emergency stop request. Such decisions require explicit behavioral models capable of coordinating multiple activities while maintaining predictable execution.

Early robotic systems relied primarily upon procedural software in which execution followed predefined sequences of conditional statements. Although sufficient for simple automation, procedural approaches rapidly became difficult to maintain as robot complexity increased. Large collections of nested conditions produced software that was difficult to understand, extend, verify, and debug. Researchers therefore began investigating formal behavior representation techniques capable of describing robot activities more systematically.

One of the earliest and most widely adopted approaches was the **Finite State Machine**. An FSM models robot behavior as a finite collection of discrete states connected through well-defined transitions. At any instant, the robot occupies exactly one active state. Events, sensor conditions, timers, operator commands, or internal decisions trigger transitions from one state to another. Because every possible transition is explicitly defined, FSMs provide highly deterministic and easily understandable behavioral models.

The simplicity of FSMs makes them attractive for many robotic applications. Consider an autonomous delivery robot operating inside a warehouse. The robot may occupy states such as Idle, Navigate, Dock, Load Cargo, Deliver, Recharge, Error Recovery, and Emergency Stop. Each state defines the robot\'s current activity, while transitions specify conditions under which execution moves between states. If battery level falls below a predefined threshold, navigation transitions to charging. If docking succeeds, cargo loading begins. If an emergency stop signal arrives, every operational state immediately transitions to the emergency state.

State transitions represent the fundamental mechanism governing FSM execution. Transitions may depend upon sensor observations, completed actions, timeout conditions, communication messages, operator input, hardware faults, AI predictions, or combinations of multiple events. Guard conditions prevent invalid transitions by ensuring that predefined logical requirements are satisfied before execution proceeds. This explicit representation significantly simplifies behavioral analysis because every possible execution path can be inspected systematically.

Determinism constitutes one of the primary advantages of finite state machines. Because transitions are explicitly defined, engineers can reason precisely about robot behavior under all expected operating conditions. Formal verification techniques further allow safety-critical FSMs to be analyzed mathematically for unreachable states, deadlocks, transition completeness, deterministic execution, and logical consistency. Consequently, industrial automation, embedded control systems, and safety-certified robotics frequently employ FSMs for mission-critical behavioral control.

Hierarchical Finite State Machines extend the traditional FSM model by allowing complex states to contain nested substates. Rather than representing every behavioral detail within one enormous state machine, hierarchy decomposes behavior into multiple abstraction levels. A high-level Navigation state, for example, may internally contain Plan Route, Follow Path, Avoid Obstacle, Replan, and Goal Arrival substates. This hierarchical organization substantially reduces visual complexity while improving software maintainability.

Orthogonal state machines introduce parallel execution by allowing multiple independent state regions to operate simultaneously. A robot may independently manage locomotion, communication, battery monitoring, manipulator operation, and perception while synchronizing selected events across these parallel state machines. Such concurrency significantly expands modeling capability but also increases synchronization complexity.

Despite their strengths, finite state machines exhibit important limitations as robotic systems grow more sophisticated. The number of transitions often increases exponentially as additional states and interactions are introduced. This phenomenon, commonly known as state explosion, rapidly transforms otherwise elegant behavioral models into difficult-to-maintain diagrams containing hundreds or thousands of transitions. Adding new behaviors frequently requires modifying existing transitions throughout the state machine, increasing maintenance effort while introducing opportunities for regression errors.

Reactive robotics further complicates FSM design because robots frequently require dynamic interruption and recovery. While navigating toward a destination, a robot may temporarily avoid obstacles, communicate with elevators, recharge its battery, yield to pedestrians, perform visual inspection, resume navigation, and eventually complete its original mission. Representing such highly dynamic execution through conventional FSM transitions often produces extremely complicated transition networks.

These limitations motivated the development of more modular behavioral modeling techniques, among which **Behavior Trees** have become the dominant solution in modern robotics. Originally developed within the computer gaming industry to organize non-player character behaviors, Behavior Trees were later adopted by robotics researchers because of their flexibility, readability, modularity, and scalability.

Unlike FSMs, which model behavior primarily through explicit state transitions, Behavior Trees organize execution as a hierarchical tree composed of reusable nodes. Execution begins at the tree root and proceeds downward through parent-child relationships. Each node returns one of three execution statuses: Success, Failure, or Running. Parent nodes determine overall execution flow according to these returned statuses.

Behavior Trees distinguish several categories of nodes. Control nodes govern execution order, decorator nodes modify execution behavior, condition nodes evaluate logical expressions, and action nodes perform actual robot operations. By combining these simple building blocks, engineers create highly expressive behavioral models capable of representing complex autonomous missions while maintaining structural clarity.

Sequence nodes execute children sequentially from left to right. If every child succeeds, the sequence itself succeeds. If any child fails, execution immediately terminates with failure. Sequence nodes therefore represent ordered task execution such as Navigate to Target, Align Robot, Activate Camera, Capture Images, Run Inspection, Generate Report, and Return Home.

Selector nodes implement fallback behavior. Children are evaluated sequentially until one succeeds. If every child fails, the selector also fails. This structure naturally represents recovery strategies. A localization selector may first attempt visual localization, then LiDAR localization, then GNSS positioning, and finally operator assistance. Only if every alternative fails does the overall localization behavior report failure.

Parallel nodes execute multiple children simultaneously according to configurable success policies. For example, obstacle monitoring, battery supervision, network communication, and environmental perception may execute concurrently while navigation continues independently. Parallel execution significantly improves behavioral flexibility compared to conventional FSMs.

Decorator nodes modify execution without introducing additional functional behaviors. Common decorators include timeout control, retry mechanisms, inversion of logical results, execution limiting, repetition, conditional execution, and rate limiting. These reusable execution modifiers eliminate the need to repeatedly implement identical behavioral patterns throughout the software.

Condition nodes evaluate logical predicates without modifying the environment. Battery sufficient, Target Visible, Localization Valid, Communication Connected, Goal Reached, and Safety Zone Clear represent typical condition nodes. Action nodes perform physical operations including Move Forward, Stop Robot, Capture Image, Open Gripper, Compute Path, Execute AI Inference, or Publish Status Message.

One of the most important characteristics of Behavior Trees is continuous reevaluation. Rather than committing permanently to previously selected execution paths, Behavior Trees repeatedly evaluate conditions during every execution cycle. If environmental conditions change, execution naturally adapts by selecting different branches without requiring explicit transition definitions. This property significantly enhances robot responsiveness within dynamic environments.

Behavior modularity represents another major advantage. Individual subtrees encapsulate reusable behavioral capabilities such as navigation, manipulation, inspection, charging, docking, localization recovery, or obstacle avoidance. These modules may be combined across multiple robotic applications without modifying internal implementation. Software reuse therefore increases substantially compared to monolithic FSM designs.

Behavior Trees also simplify debugging because execution naturally follows hierarchical structures that are easy to visualize. Modern robotics development environments display currently executing nodes, completed actions, failed branches, timing statistics, and execution history in real time. Engineers therefore understand robot decision processes more easily than when interpreting complex transition networks.

ROS2 has greatly accelerated Behavior Tree adoption through frameworks such as **BehaviorTree.CPP**, **Nav2 Behavior Trees**, and numerous open-source behavior orchestration libraries. The Navigation2 framework, one of the most widely used autonomous navigation systems in ROS2, employs Behavior Trees to coordinate path planning, controller selection, obstacle avoidance, recovery behaviors, replanning, and mission execution. Developers customize robot behavior primarily by modifying behavior trees rather than rewriting navigation software.

Behavior Trees integrate naturally with the Task-Behavior-Control layered architecture introduced previously. High-level mission planners assign objectives, Behavior Trees coordinate operational execution, and deterministic controllers execute physical motion. This separation preserves architectural modularity while allowing complex behavioral adaptation without compromising real-time control.

Artificial intelligence further enhances Behavior Tree capabilities. Machine learning algorithms increasingly determine subtree selection, optimize execution ordering, estimate action success probabilities, predict environmental changes, and personalize robot behaviors according to previous experience. Rather than replacing Behavior Trees, AI augments decision quality while preserving interpretable execution structure.

Hybrid architectures combining FSMs and Behavior Trees have become increasingly common. Finite State Machines manage high-level operational modes such as Startup, Normal Operation, Maintenance, Fault Recovery, and Shutdown because these modes possess clearly defined transitions and strict operational constraints. Within each state, Behavior Trees coordinate detailed operational behaviors such as navigation, inspection, manipulation, communication, and recovery. This combination exploits the strengths of both approaches while minimizing their respective weaknesses.

Safety-critical systems frequently continue relying upon FSMs for certified operational states because deterministic transition analysis supports regulatory verification. Meanwhile, adaptive operational intelligence, AI-assisted autonomy, mission coordination, and complex behavioral sequencing increasingly utilize Behavior Trees due to their flexibility and scalability. Consequently, industrial robots, autonomous mobile robots, collaborative robots, humanoids, quadrupeds, agricultural robots, service robots, and autonomous vehicles often implement hybrid behavioral architectures.

Mission interruption and recovery illustrate the superiority of Behavior Trees for dynamic environments. Suppose an inspection robot navigates toward an inspection station. During execution, battery level becomes critically low. Rather than requiring dozens of explicit FSM transitions, the Behavior Tree automatically interrupts inspection, navigates toward a charging station, recharges, resumes navigation, and continues inspection using reusable behavioral modules. Such adaptive execution significantly reduces software complexity.

Failure recovery similarly benefits from Behavior Trees. Navigation failure may trigger recovery behaviors including costmap clearing, localization reset, alternative path planning, reverse motion, obstacle negotiation, or operator notification. Recovery branches remain isolated within dedicated subtrees that may be reused across multiple missions without modifying primary execution logic.

Behavior representation also plays an essential role in Human-Robot Interaction. Social robots continuously evaluate user presence, speech commands, facial expressions, conversational context, gesture recognition, and safety constraints while selecting appropriate interaction behaviors. Behavior Trees naturally coordinate these concurrent perception and communication activities while maintaining structured execution.

Testing behavioral software requires specialized methodologies. Unit testing verifies individual action nodes, condition evaluation, and decorators independently. Integration testing validates interactions among multiple behavioral modules. Scenario testing reproduces realistic mission execution under diverse environmental conditions. Simulation platforms evaluate complete behavior trees before deployment to physical robots. Formal verification techniques continue supporting safety-critical FSM components.

Performance optimization focuses on minimizing unnecessary condition evaluation, reducing subtree execution overhead, limiting computational complexity, and ensuring predictable execution timing. Large Behavior Trees frequently employ lazy evaluation, event-triggered execution, subtree caching, blackboard optimization, and asynchronous actions to improve runtime efficiency.

The concept of the **Blackboard** forms another important component of many Behavior Tree implementations. The blackboard functions as shared memory where behavior nodes exchange information including robot pose, sensor observations, target locations, AI predictions, environmental models, mission parameters, and execution status. Shared information eliminates unnecessary communication while maintaining loose coupling among behavior modules.

As robots evolve toward Physical AI systems, behavior representation continues expanding beyond manually designed execution structures. Large Language Models, Vision-Language-Action architectures, world models, and foundation models increasingly generate behavioral plans dynamically according to natural language instructions and environmental understanding. Nevertheless, deterministic execution frameworks remain necessary to translate abstract AI reasoning into predictable physical actions. Behavior Trees therefore provide an ideal execution substrate for AI-generated robot behaviors because they preserve modularity, interpretability, safety, and execution monitoring.

Future robotics software will likely combine symbolic planning, Behavior Trees, reinforcement learning policies, probabilistic reasoning, and foundation models within unified behavioral architectures. High-level AI systems will determine objectives, Behavior Trees will coordinate structured execution, and deterministic controllers will guarantee safe physical interaction. Rather than competing technologies, these approaches complement one another across different abstraction levels.

Ultimately, State Machines and Behavior Trees represent two of the most important behavioral modeling techniques in modern robotics software engineering. Finite State Machines provide deterministic, analyzable, and safety-oriented execution suitable for well-defined operational modes and certified control systems. Behavior Trees offer modular, scalable, reusable, and adaptive behavioral coordination ideally suited for intelligent autonomous robots operating within dynamic environments. Together they form the behavioral foundation upon which contemporary mobile robots, industrial manipulators, collaborative robots, humanoids, autonomous vehicles, and future AI-native Physical Intelligence systems organize perception, planning, decision making, recovery, and interaction into coherent, maintainable, and reliable software architectures.

현대의 자율 로봇은 단순히 모터를 제어하는 수준을 넘어, 주변 환경을 인식하고 상황을 판단하며 적절한 행동을 선택하고, 예상치 못한 사건에 대응하면서 임무를 수행해야 한다. 이러한 복잡한 의사결정은 단순한 제어 알고리즘(Control Algorithm)만으로는 구현할 수 없으며, 로봇의 행동을 체계적으로 표현하고 관리하는 **행동 모델(Behavior Model)** 이 필요하다.

대표적인 행동 모델이 **유한 상태 머신(Finite State Machine, FSM)** 과 **행동 트리(Behavior Tree, BT)** 이다. 두 기술은 모두 로봇의 행동을 구조적으로 표현하지만, 동작 방식과 적용 분야에는 차이가 있다.

행동 표현(Behavior Representation)은 로봇 소프트웨어에서 매우 중요한 요소이다. 센서는 주변 환경을 인식하고 위치추정(Localization)은 로봇의 위치를 계산하지만, 여러 상황이 동시에 발생했을 때 어떤 행동을 우선 수행할 것인지는 별도의 행동 관리 시스템이 결정해야 한다.

예를 들어 물류 로봇은 이동 중 장애물을 회피할 수도 있고, 배터리가 부족하면 충전을 해야 하며, 자동문을 기다리거나 비상 정지(Emergency Stop)를 수행해야 할 수도 있다. 이러한 다양한 행동을 논리적으로 관리하기 위해 행동 모델이 사용된다.

초기의 로봇 소프트웨어는 대부분 절차형 프로그래밍(Procedural Programming)으로 구현되었다. 그러나 조건문과 반복문이 계속 증가하면서 프로그램 구조가 매우 복잡해졌고, 새로운 기능을 추가할수록 유지보수가 어려워졌다.

이러한 문제를 해결하기 위해 로봇의 행동을 명확한 상태(State)와 전이(Transition)로 표현하는 **유한 상태 머신(Finite State Machine, FSM)** 이 등장하였다.

FSM은 로봇의 동작을 여러 개의 상태(State)로 나누고, 특정 조건이 만족되면 다른 상태로 전환하는 구조이다. 어느 순간에도 로봇은 하나의 상태만 활성화되어 있으며, 센서 입력, 시간(Time), 사용자 명령(Command), 내부 이벤트(Event)에 따라 다음 상태로 이동한다.

모든 상태 전이가 명확하게 정의되어 있으므로 시스템의 동작을 쉽게 이해하고 분석할 수 있다는 장점이 있다.

예를 들어 창고 운반 로봇이라면 Idle, Navigate, Dock, Load Cargo, Deliver, Recharge, Error Recovery, Emergency Stop과 같은 상태를 가질 수 있다.

배터리가 부족하면 Navigate에서 Recharge로 이동하고, 도킹이 완료되면 Load Cargo로 이동하며, 비상 정지가 발생하면 어떤 상태에서든 Emergency Stop으로 즉시 전환된다.

FSM에서 가장 중요한 요소는 **상태 전이(State Transition)** 이다. 상태 전이는 센서 정보, 타이머(Timer), 사용자 명령, AI 판단, 통신 메시지(Message), 하드웨어 오류 등 다양한 조건에 의해 발생한다.

또한 가드 조건(Guard Condition)을 사용하여 특정 조건이 만족될 때만 상태 전이가 이루어지도록 제어할 수 있다.

FSM의 가장 큰 장점은 **결정론성(Determinism)** 이다. 모든 상태와 전이가 명확하게 정의되어 있으므로 로봇이 어떤 상황에서 어떻게 동작할지를 정확하게 예측할 수 있다.

또한 형식 검증(Formal Verification)을 통해 도달할 수 없는 상태(Unreachable State), 교착 상태(Deadlock), 논리 오류(Logical Error)를 분석할 수 있으므로 산업용 로봇이나 안전 인증 시스템에서 널리 사용된다.

FSM의 확장 형태로 **계층형 상태 머신(Hierarchical State Machine, HSM)** 이 있다. 하나의 상태 안에 여러 개의 하위 상태(Substate)를 포함할 수 있으므로 복잡한 시스템을 계층적으로 표현할 수 있다.

예를 들어 Navigation 상태 안에는 Path Planning, Follow Path, Obstacle Avoidance, Goal Arrival 등의 하위 상태가 존재할 수 있으며, 이러한 구조는 대형 시스템의 복잡도를 크게 줄여준다.

또 다른 확장 형태는 **직교 상태 머신(Orthogonal State Machine)** 이다. 이는 여러 개의 상태 머신을 동시에 실행하는 방식이다.

예를 들어 이동 제어(Motion Control), 배터리 관리(Battery Management), 통신(Communication), 센서 관리(Sensor Management)를 서로 독립적으로 동시에 실행할 수 있으며, 필요한 경우 특정 이벤트(Event)를 통해 서로 동기화할 수 있다.

그러나 FSM에도 한계가 존재한다. 상태가 증가할수록 상태 전이의 개수가 기하급수적으로 증가하는 **상태 폭발(State Explosion)** 문제가 발생한다.

새로운 기능을 하나 추가할 때마다 기존의 여러 상태 전이를 수정해야 하므로 유지보수성이 크게 떨어질 수 있다.

특히 자율주행 로봇처럼 매우 복잡한 행동을 수행하는 시스템에서는 이러한 문제가 더욱 심각해진다.

예를 들어 이동 중 장애물을 회피하고, 자동문을 통과하고, 엘리베이터를 이용하고, 배터리가 부족하면 충전한 뒤 다시 원래 작업을 계속해야 하는 경우를 FSM으로 표현하면 상태와 전이가 지나치게 복잡해질 수 있다.

이러한 문제를 해결하기 위해 등장한 것이 **행동 트리(Behavior Tree, BT)** 이다.

행동 트리는 원래 게임(Game) 분야에서 NPC(Non-Player Character)의 행동을 표현하기 위해 개발되었지만, 현재는 로봇 분야에서 가장 널리 사용되는 행동 관리 기법 가운데 하나가 되었다.

행동 트리는 상태 전이 대신 **트리(Tree)** 구조를 사용하여 행동을 표현한다. 실행은 루트(Root) 노드에서 시작하여 부모(Parent)와 자식(Child) 관계를 따라 아래로 진행된다.

각 노드는 실행 결과를 **성공(Success)**, **실패(Failure)**, **실행 중(Running)** 의 세 가지 상태 가운데 하나로 반환하며, 부모 노드는 이를 이용하여 다음 실행 흐름을 결정한다.

행동 트리는 크게 **제어 노드(Control Node)**, **데코레이터 노드(Decorator Node)**, **조건 노드(Condition Node)**, **행동 노드(Action Node)** 로 구성된다.

각 노드는 매우 단순하지만 이들을 조합하면 매우 복잡한 자율 행동도 쉽게 표현할 수 있다.

대표적인 제어 노드는 **시퀀스(Sequence)** 이다. Sequence는 왼쪽부터 차례대로 실행하며 모든 자식이 성공해야 전체가 성공한다.

예를 들어 Navigate → Align → Capture Image → AI Inspection → Generate Report → Return Home과 같은 작업 순서를 표현하기에 적합하다.

또 다른 대표적인 노드는 **셀렉터(Selector)** 이다. Selector는 첫 번째 자식이 실패하면 다음 자식을 실행한다.

예를 들어 Visual Localization이 실패하면 LiDAR Localization을 수행하고, 다시 실패하면 GNSS를 사용하며, 모두 실패하면 사람에게 도움을 요청하는 방식으로 복구 전략을 쉽게 표현할 수 있다.

**병렬 노드(Parallel Node)** 는 여러 행동을 동시에 수행한다.

예를 들어 이동하면서 장애물 감시, 배터리 확인, 네트워크 통신을 동시에 수행할 수 있으며, 현대의 자율주행 로봇에서는 매우 자주 사용된다.

**데코레이터 노드(Decorator Node)** 는 행동을 변경하는 역할을 수행한다.

예를 들어 시간 제한(Timeout), 반복(Retry), 성공과 실패 반전(Inverter), 반복 실행(Repeat), 조건 실행(Conditional Execution) 등을 간단히 구현할 수 있어 동일한 코드를 반복 작성할 필요가 없다.

**조건 노드(Condition Node)** 는 배터리가 충분한지, 목표물이 보이는지, 위치추정이 정상인지와 같은 논리 조건을 검사한다.

반면 **행동 노드(Action Node)** 는 실제로 이동(Move), 정지(Stop), 영상 촬영(Capture Image), AI 추론(AI Inference), 그리퍼 제어(Gripper Control) 등 실제 작업을 수행한다.

행동 트리의 가장 큰 특징은 **지속적인 재평가(Continuous Reevaluation)** 이다.

FSM은 상태에 들어가면 명시적인 전이가 발생할 때까지 해당 상태를 유지하지만, 행동 트리는 매 실행 주기마다 조건을 다시 평가한다.

따라서 환경이 바뀌면 새로운 행동을 자연스럽게 선택할 수 있으며, 매우 높은 적응성을 제공한다.

또한 행동 트리는 **모듈성(Modularity)** 이 뛰어나다.

내비게이션(Navigation), 도킹(Docking), 충전(Charging), 장애물 회피(Obstacle Avoidance), 검사(Inspection) 등을 각각 독립적인 서브트리(Subtree)로 구현하면 다른 프로젝트에서도 그대로 재사용할 수 있다.

행동 트리는 디버깅(Debugging)도 매우 쉽다.

현재 어떤 노드가 실행 중인지, 어느 노드에서 실패했는지, 실행 시간이 얼마나 걸렸는지를 시각적으로 확인할 수 있으므로 복잡한 자율주행 시스템의 동작을 쉽게 분석할 수 있다.

ROS2에서는 **BehaviorTree.CPP**, **Navigation2(Nav2)** 등의 프레임워크를 통해 행동 트리가 널리 사용되고 있다.

특히 Nav2에서는 경로 계획(Path Planning), 장애물 회피(Obstacle Avoidance), 복구 행동(Recovery Behavior), 재계획(Replanning) 등을 행동 트리로 구현하여 매우 높은 유연성을 제공한다.

행동 트리는 **3T(Task-Behavior-Control)** 아키텍처와도 매우 잘 어울린다.

작업 계층(Task Layer)은 목표를 생성하고, 행동 계층(Behavior Layer)은 행동 트리를 이용하여 실행 순서를 관리하며, 제어 계층(Control Layer)은 실제 하드웨어를 제어한다.

이러한 구조는 현대 자율주행 로봇의 대표적인 소프트웨어 구조이다.

최근에는 행동 트리에도 인공지능(AI)이 결합되고 있다.

강화학습(Reinforcement Learning), 대규모 언어 모델(Large Language Model, LLM), 대규모 행동 모델(Large Action Model, LAM)은 행동 선택이나 행동 순서를 최적화하는 데 활용되고 있다.

즉 AI가 행동 트리를 대체하는 것이 아니라, 행동 트리의 의사결정을 더욱 지능적으로 만들어 주는 역할을 수행한다.

실제 산업 현장에서는 **FSM과 행동 트리를 함께 사용하는 하이브리드(Hybrid) 구조** 가 가장 많이 사용된다.

예를 들어 Startup, Normal Operation, Maintenance, Shutdown, Emergency Mode와 같은 시스템 모드는 FSM으로 관리하고, 각 모드 안에서 Navigation, Manipulation, Inspection, Docking 등 세부 동작은 행동 트리로 관리한다.

이렇게 하면 두 기술의 장점을 모두 활용할 수 있다.

행동 트리는 특히 **복구(Recovery)** 에 강하다.

검사 로봇이 이동 중 배터리가 부족하면 충전 스테이션으로 이동하여 충전한 후 다시 원래 작업을 계속할 수 있다.

FSM에서는 이러한 동작을 표현하기 위해 많은 상태 전이가 필요하지만, 행동 트리에서는 충전 서브트리만 추가하면 매우 간단하게 구현할 수 있다.

장애 복구(Failure Recovery)도 행동 트리의 강점이다.

내비게이션이 실패하면 Costmap 초기화, 위치추정 재설정, 새로운 경로 생성, 후진, 사람 호출 등의 복구 행동을 하나의 Recovery 서브트리로 구현할 수 있으며, 다른 프로젝트에서도 그대로 사용할 수 있다.

사람-로봇 상호작용(Human-Robot Interaction, HRI)에서도 행동 트리는 매우 효과적이다.

사람의 음성 명령, 얼굴 인식, 제스처 인식, 안전 상태를 동시에 고려하면서 적절한 행동을 선택해야 하는데, 행동 트리는 이러한 복잡한 흐름을 매우 자연스럽게 표현할 수 있다.

행동 소프트웨어의 시험(Test)도 중요하다.

각 Action Node는 단위 시험(Unit Test)을 수행하고, 여러 노드를 연결한 행동은 통합 시험(Integration Test)과 시뮬레이션(Simulation)을 통해 검증한다.

안전성이 중요한 FSM은 형식 검증(Formal Verification)을 함께 수행하여 논리적 오류를 제거하는 것이 일반적이다.

행동 트리에서는 **블랙보드(Blackboard)** 가 중요한 역할을 한다.

블랙보드는 여러 노드가 공유하는 메모리 공간으로, 로봇 위치, 목표 지점, 센서 정보, AI 결과, 임무 상태 등을 저장한다.

이를 통해 노드 간 결합도를 낮추면서도 필요한 정보를 효율적으로 공유할 수 있다.

미래의 **피지컬 AI(Physical AI)** 시대에는 행동 표현 방식도 더욱 발전할 것이다.

대규모 언어 모델(LLM), 비전-언어-행동 모델(Vision-Language-Action, VLA), 월드 모델(World Model)은 자연어를 기반으로 행동 계획을 생성할 수 있지만, 실제 로봇을 안전하게 제어하기 위해서는 여전히 행동 트리와 같은 결정론적인 실행 구조가 필요하다.

따라서 AI는 목표와 계획을 생성하고, 행동 트리는 이를 안전하고 예측 가능한 실제 행동으로 변환하는 실행 엔진(Execution Engine)의 역할을 수행하게 될 것이다.

앞으로의 로봇은 심볼릭 계획(Symbolic Planning), 행동 트리(Behavior Tree), 강화학습(Reinforcement Learning), 확률적 추론(Probabilistic Reasoning), 파운데이션 모델(Foundation Model)을 함께 사용하는 복합적인 행동 구조로 발전할 것으로 예상된다.

결론적으로 **유한 상태 머신(Finite State Machine, FSM)** 과 **행동 트리(Behavior Tree, BT)** 는 현대 로봇 소프트웨어에서 가장 핵심적인 행동 모델이다. FSM은 높은 결정론성(Determinism)과 검증 가능성(Verifiability)을 제공하여 안전성과 운영 모드 관리에 적합하며, 행동 트리는 뛰어난 모듈성(Modularity), 재사용성(Reusability), 확장성(Scalability), 적응성(Adaptability)을 제공하여 복잡한 자율행동을 효과적으로 구현할 수 있다. 두 기술은 경쟁 관계가 아니라 서로를 보완하는 관계이며, 현대의 **자율이동로봇(AMR)**, **산업용 로봇**, **협동로봇(Cobot)**, **휴머노이드(Humanoid)**, **자율주행 차량**, 그리고 미래의 **AI 네이티브 로봇(AI-Native Robot)** 과 **피지컬 AI(Physical AI)** 의 핵심 행동 아키텍처를 구성하는 기반 기술로 계속 활용될 것이다.

##  

## 06.06 Sense-Plan-Act vs. Reactive Architecture

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}

The history of autonomous robotics has been shaped by one fundamental question: how should a robot decide what to do next? Every autonomous robot continuously receives sensory information, interprets environmental conditions, evaluates possible actions, and executes physical behaviors. The architectural organization of this decision-making process directly influences robot intelligence, responsiveness, computational complexity, safety, and adaptability. Throughout the evolution of robotics, two fundamentally different architectural philosophies have emerged to address this challenge. The first is the **Sense-Plan-Act (SPA) Architecture**, which emphasizes deliberate reasoning and structured planning before action. The second is the **Reactive Architecture**, which emphasizes immediate responses to environmental stimuli without relying on extensive internal planning. Although these approaches appear to represent opposite philosophies, modern robotics increasingly combines their strengths into hybrid intelligent architectures capable of both strategic reasoning and rapid environmental adaptation.

The earliest generations of autonomous robots primarily adopted deliberative reasoning models inspired by classical artificial intelligence. Researchers believed that intelligent behavior required robots to construct complete internal models of the surrounding environment, analyze every possible alternative, generate optimal plans, and then execute carefully designed action sequences. This reasoning process became known as the Sense-Plan-Act paradigm because robot operation naturally followed three sequential stages: sensing the environment, planning an appropriate response, and finally executing physical actions.

Within the Sense stage, robots acquire information about the external world using multiple sensing modalities. Cameras capture visual information, LiDAR measures geometric structures, radar estimates object motion, ultrasonic sensors detect nearby obstacles, GNSS determines global position, inertial measurement units estimate orientation, force sensors monitor physical interaction, wheel encoders estimate odometry, and numerous additional sensors contribute complementary observations. Raw sensor measurements typically contain noise, uncertainty, missing information, and measurement errors. Consequently, substantial preprocessing occurs before higher-level reasoning begins.

Perception algorithms transform raw sensor data into meaningful environmental representations. Object detection identifies vehicles, pedestrians, equipment, or obstacles. Semantic segmentation classifies regions according to environmental categories. Simultaneous Localization and Mapping constructs spatial maps while estimating robot position. Sensor fusion combines heterogeneous measurements into consistent environmental models. World modeling integrates dynamic observations into coherent internal representations that describe both static infrastructure and moving objects.

The Plan stage represents the intellectual core of the Sense-Plan-Act architecture. Based upon the internal world model constructed during perception, planning algorithms determine the sequence of actions required to accomplish mission objectives. Planning may occur at multiple abstraction levels simultaneously. Mission planners allocate long-term objectives, task planners decompose complex activities into executable subtasks, path planners generate collision-free routes, trajectory planners compute dynamically feasible motions, and motion planners determine actuator-level commands satisfying physical constraints.

Planning algorithms employ diverse computational techniques depending upon application requirements. Graph search algorithms including Dijkstra and A\* compute shortest paths through known environments. Sampling-based planners such as Rapidly-exploring Random Trees and Probabilistic Roadmaps efficiently explore high-dimensional configuration spaces. Optimization-based planners minimize energy consumption, travel time, or collision risk while satisfying kinematic and dynamic constraints. Artificial intelligence planners incorporate symbolic reasoning, knowledge graphs, semantic maps, temporal constraints, and logical inference into long-term mission planning.

The final Act stage executes planned behaviors through deterministic control systems. Motion controllers regulate actuator commands according to generated trajectories while continuously monitoring execution progress. Feedback control algorithms compensate for disturbances, sensor noise, wheel slip, environmental uncertainty, and actuator imperfections. During execution, sensors continue monitoring robot state, allowing deviations between planned and actual behavior to be detected and corrected.

The sequential organization of Sense-Plan-Act offers numerous advantages. Since planning occurs before execution, robot actions typically exhibit globally consistent behavior aligned with long-term mission objectives. Planning algorithms evaluate multiple alternatives before selecting optimal solutions, often producing highly efficient paths, reduced energy consumption, minimized travel distance, and improved mission completion times. Symbolic reasoning also enables robots to perform sophisticated cognitive tasks involving scheduling, resource allocation, semantic understanding, and long-term strategic decision making.

Complex industrial automation frequently benefits from deliberative planning. Warehouse logistics systems optimize transportation routes across entire facilities. Manufacturing robots schedule production tasks according to resource availability. Agricultural robots generate efficient coverage paths across large fields. Planetary exploration rovers carefully evaluate terrain traversability before executing costly movements. In such structured environments, deliberate planning significantly improves operational efficiency.

However, purely deliberative architectures possess important limitations. Planning requires computational time, particularly within large or highly dynamic environments. During planning, environmental conditions may already begin changing. Moving obstacles, pedestrians, vehicles, unexpected equipment, communication failures, or sensor disturbances may invalidate previously generated plans before execution completes. Frequent replanning therefore becomes necessary, increasing computational overhead while reducing responsiveness.

The dependence upon complete environmental models also creates challenges. Real-world environments rarely provide perfect information. Sensor occlusions, measurement uncertainty, changing illumination, communication delays, dynamic obstacles, and incomplete observations continuously degrade world model accuracy. Consequently, robots relying exclusively upon internal models may make incorrect decisions when environmental representations diverge from physical reality.

These challenges motivated an alternative architectural philosophy emphasizing direct interaction with the environment rather than extensive internal reasoning. This philosophy became known as **Reactive Architecture** because robot behavior emerges through immediate reactions to current sensory observations instead of detailed planning over comprehensive world models.

Reactive robotics gained widespread attention through Rodney Brooks\' **Subsumption Architecture**, which argued that intelligent behavior need not require complete symbolic world representations. Instead, complex robot behavior could emerge from multiple simple behavioral modules operating simultaneously and responding directly to environmental stimuli. Rather than constructing detailed internal maps before acting, robots continuously sensed the environment and generated immediate motor responses according to current conditions.

Within reactive architectures, perception and action become tightly coupled. Sensor observations directly trigger behavioral responses through predefined behavioral rules, neural networks, fuzzy controllers, reinforcement learning policies, or behavior arbitration mechanisms. If an obstacle appears, avoidance immediately begins. If battery level decreases below a threshold, charging behavior activates. If a target becomes visible, tracking commences without requiring comprehensive mission replanning.

Behavior-based robotics decomposes intelligence into multiple concurrent behavioral modules. Obstacle avoidance, wall following, target tracking, collision prevention, exploration, charging, docking, and communication each operate independently while competing for robot control according to behavioral priorities. Arbitration mechanisms determine which behaviors dominate under current environmental conditions. Higher-priority safety behaviors may immediately override lower-priority navigation objectives whenever necessary.

Reactive architectures demonstrate exceptional responsiveness because decision latency remains extremely low. Behavioral rules operate directly upon sensor observations without requiring computationally expensive global planning. Consequently, robots respond naturally to rapidly changing environments where immediate reactions prove more valuable than globally optimized plans.

Mobile robots operating among pedestrians illustrate the advantages of reactive behavior. Human movement remains highly unpredictable. Attempting to generate complete long-term motion plans becomes impractical because pedestrian trajectories continuously change. Instead, reactive obstacle avoidance combined with short-term local planning enables robots to navigate safely despite environmental uncertainty.

Industrial safety systems similarly depend upon reactive behavior. Emergency stop activation, collision detection, force limitation, speed reduction, and protective separation monitoring require deterministic immediate responses independent of mission planning. Waiting for deliberative reasoning before preventing hazardous collisions would produce unacceptable safety risks.

Reactive architectures also exhibit remarkable robustness. Since behavior depends primarily upon current sensor observations rather than extensive internal models, moderate mapping errors or localization uncertainty often exert limited influence upon immediate behavioral responses. Robots naturally adapt to unexpected environmental changes without requiring complete replanning.

Despite these advantages, purely reactive systems possess important limitations. Immediate responses frequently optimize local behavior while neglecting long-term mission objectives. Robots may repeatedly avoid obstacles without making meaningful progress toward destinations. Local minima frequently trap robots within environmental configurations where reactive behaviors cannot discover globally successful solutions. Without strategic planning, robots often struggle with tasks requiring long temporal reasoning, resource scheduling, symbolic manipulation, or complex sequential objectives.

Navigation through maze-like environments illustrates this limitation clearly. A purely reactive robot following obstacle avoidance rules may endlessly circulate around obstacles despite the existence of feasible routes toward the goal. Deliberative planning, by contrast, analyzes global map structure to identify successful navigation strategies.

Reactive systems also experience difficulty coordinating complex multi-stage missions. Industrial inspection may require visiting multiple stations according to optimized schedules while coordinating battery management, elevator usage, cloud communication, data acquisition, and maintenance activities. Such objectives require reasoning beyond immediate sensory reactions.

The complementary strengths and weaknesses of Sense-Plan-Act and Reactive architectures gradually led robotics researchers toward integrated hybrid systems. Modern autonomous robots rarely employ either architecture exclusively. Instead, they combine deliberative planning for strategic decision making with reactive behaviors for immediate environmental adaptation.

Hybrid architectures typically organize software into multiple hierarchical layers. High-level planners generate long-term mission objectives, task sequences, and global navigation strategies. Intermediate behavior coordinators supervise execution while monitoring environmental conditions. Low-level reactive controllers continuously perform obstacle avoidance, stabilization, collision prevention, and emergency safety responses. This layered organization combines strategic intelligence with rapid responsiveness.

The previously discussed 3T Task-Behavior-Control architecture exemplifies this hybrid philosophy. Task layers perform deliberative planning, Behavior layers coordinate adaptive execution, and Control layers implement deterministic reactive control. Each layer operates according to appropriate temporal and computational requirements while maintaining clearly defined interfaces.

Navigation provides perhaps the clearest demonstration of hybrid operation. Global planners compute efficient routes toward distant destinations using known environmental maps. Local planners react continuously to dynamic obstacles, temporary pathway blockages, pedestrian movement, and unexpected environmental changes. If local avoidance repeatedly fails, high-level planners generate revised global routes. Thus planning and reaction cooperate continuously rather than competing.

Artificial intelligence further strengthens hybrid architectures. Deep learning perception systems provide semantic understanding supporting long-term planning while reinforcement learning policies generate reactive control behaviors directly from sensory observations. Large Language Models interpret high-level user instructions, world models predict future environmental evolution, and deterministic controllers execute physically safe motions. Multiple AI paradigms therefore contribute across different architectural layers.

Autonomous vehicles illustrate sophisticated hybrid architecture implementation. Global route planning determines destination sequences across city-scale road networks. Behavioral planners coordinate lane changes, intersections, traffic regulations, and overtaking maneuvers. Reactive controllers immediately respond to pedestrians, cyclists, unexpected vehicles, or emergency situations. Safety systems continuously override planning whenever imminent collision risks appear. Each architectural level contributes complementary decision-making capabilities.

Warehouse automation similarly employs hybrid intelligence. Fleet management software optimizes transportation assignments across hundreds of robots. Individual robots generate efficient navigation plans toward assigned destinations. Local obstacle avoidance responds immediately to forklifts, workers, or temporary obstructions. Battery management reactively interrupts missions when necessary while strategic schedulers reassign incomplete tasks to alternative robots.

Humanoid robots require even richer architectural integration. Language understanding, symbolic reasoning, and social interaction rely upon deliberative planning. Balance control, locomotion stabilization, reflexive grasp adjustment, collision avoidance, and force regulation depend upon reactive control. Conversation, perception, manipulation, and whole-body coordination therefore operate simultaneously across multiple architectural levels.

Physical AI further expands the integration between planning and reaction. Foundation models construct semantic world understanding supporting high-level reasoning. Vision-Language-Action systems translate natural language into behavioral objectives. World models simulate future environmental evolution for predictive planning. Reinforcement learning policies provide adaptive reactive control. Behavior Trees coordinate execution. Deterministic controllers ensure safe physical interaction. Intelligence therefore becomes distributed across complementary architectural components rather than centralized within one reasoning mechanism.

Temporal separation plays a critical role within hybrid architectures. Deliberative planning may execute every several seconds or minutes depending upon mission complexity. Reactive obstacle avoidance updates at frequencies exceeding fifty Hertz. Motor controllers operate above one kilohertz. Safety circuits respond within microseconds. Assigning appropriate update frequencies to different architectural layers significantly improves computational efficiency while preserving deterministic safety.

Communication between planning and reactive modules requires carefully designed interfaces. Reactive behaviors continuously report environmental changes upward while planners provide updated objectives downward. Blackboard systems, publish-subscribe middleware, shared world models, behavior trees, and event-driven messaging facilitate this bidirectional information exchange while minimizing coupling.

Failure recovery further demonstrates hybrid advantages. If global planning fails due to localization uncertainty, reactive exploration behaviors temporarily gather additional environmental information until planning resumes successfully. Conversely, repeated reactive navigation failures may trigger high-level replanning that selects alternative routes or modifies mission objectives. Cooperation between strategic and reactive intelligence substantially improves overall robustness.

Testing hybrid architectures requires multilayer verification. Deliberative planners undergo scenario-based simulation evaluating mission optimization. Reactive controllers undergo deterministic hardware-in-the-loop validation ensuring timing correctness. Integrated system testing verifies interaction among planning, perception, behavior coordination, and control. Digital twins increasingly reproduce complete robot operation under realistic environmental conditions before physical deployment.

ROS2 naturally supports hybrid architecture implementation. Navigation2 combines global planning, local planning, Behavior Trees, controller servers, recovery behaviors, lifecycle management, and middleware communication within one integrated navigation framework. Developers configure planning algorithms, reactive controllers, and behavioral coordination independently while preserving standardized communication interfaces.

Future robotics will likely strengthen rather than replace this architectural integration. Foundation models will enhance planning quality through semantic reasoning. Reinforcement learning will improve reactive adaptation. World models will predict environmental evolution. Behavior Trees will coordinate execution. Deterministic controllers will preserve safety. Each architectural paradigm addresses distinct computational requirements that remain essential even as AI capabilities continue advancing.

Ultimately, Sense-Plan-Act and Reactive Architecture represent two complementary perspectives on autonomous intelligence rather than mutually exclusive alternatives. Sense-Plan-Act provides structured reasoning, global optimization, symbolic planning, and long-term mission coordination. Reactive Architecture provides immediate responsiveness, robustness, environmental adaptation, and deterministic safety. Modern robotics successfully combines these complementary strengths through layered hybrid architectures capable of strategic deliberation, adaptive behavior, and real-time physical interaction. As robots evolve toward increasingly capable AI-native cyber-physical systems operating within complex dynamic environments, the integration of planning and reactive intelligence will remain one of the fundamental architectural principles enabling safe, efficient, and truly autonomous robotic behavior.

자율 로봇의 역사는 하나의 중요한 질문에서 시작되었다. **"로봇은 다음 행동을 어떻게 결정해야 하는가?"** 로봇은 끊임없이 주변 환경을 인식하고, 정보를 해석하며, 행동을 선택하고, 실제 물리적인 동작을 수행한다. 이러한 의사결정 과정이 어떻게 구성되는지는 로봇의 지능(Intelligence), 반응성(Responsiveness), 안전성(Safety), 계산 효율(Computational Efficiency), 그리고 환경 적응성(Adaptability)을 결정하는 핵심 요소가 된다.

이러한 문제를 해결하기 위해 로봇 분야에서는 크게 두 가지 대표적인 소프트웨어 아키텍처가 발전하였다. 하나는 **센스-플랜-액트(Sense-Plan-Act, SPA)** 구조이며, 다른 하나는 **반응형 아키텍처(Reactive Architecture)** 이다. 두 구조는 서로 상반된 철학을 가지고 있지만, 현대의 로봇에서는 두 방식을 결합한 하이브리드(Hybrid) 구조가 가장 널리 사용되고 있다.

초기의 자율 로봇은 대부분 전통적인 인공지능(Classical AI)의 영향을 받아 **센스-플랜-액트(Sense-Plan-Act)** 구조를 사용하였다. 이 구조에서는 로봇이 먼저 환경을 충분히 인식한 후 내부에 세계 모델(World Model)을 구축하고, 이를 기반으로 가장 적절한 행동을 계획한 다음 마지막으로 실제 행동을 수행한다.

즉, **인식(Sense) → 계획(Plan) → 실행(Act)** 의 순차적인 단계로 동작하는 것이 가장 큰 특징이다.

첫 번째 단계인 **인식(Sense)** 에서는 로봇이 다양한 센서를 이용하여 환경 정보를 수집한다. 카메라(Camera), 라이다(LiDAR), 레이더(Radar), 초음파(Ultrasonic Sensor), GNSS, 관성측정장치(IMU), 힘 센서(Force Sensor), 엔코더(Encoder) 등이 주변 환경과 로봇 자신의 상태를 지속적으로 측정한다.

하지만 센서 데이터는 노이즈(Noise), 측정 오차(Error), 누락(Missing Data)을 포함하므로 그대로 사용할 수 없다. 따라서 필터링(Filter), 센서 융합(Sensor Fusion), 보정(Calibration) 등의 전처리 과정을 거쳐 의미 있는 정보로 변환된다.

이후 **인지(Perception)** 과정에서는 수집된 데이터를 분석하여 실제 환경을 이해한다. 객체 인식(Object Detection)은 사람이나 차량, 장애물을 찾아내고, 의미 분할(Semantic Segmentation)은 도로, 벽, 바닥 등을 구분한다.

또한 **동시적 위치추정 및 지도작성(Simultaneous Localization and Mapping, SLAM)** 은 로봇의 위치와 지도를 동시에 생성하며, 이러한 정보를 종합하여 내부의 **월드 모델(World Model)** 을 구축한다.

두 번째 단계인 **계획(Plan)** 은 센스-플랜-액트 구조의 핵심이다. 구축된 월드 모델을 기반으로 목표를 달성하기 위한 최적의 행동 순서를 계산한다.

계획은 여러 수준으로 나누어질 수 있다. 임무 계획(Mission Planning)은 전체 작업을 결정하고, 작업 계획(Task Planning)은 이를 세부 작업으로 분해하며, 경로 계획(Path Planning)은 목적지까지의 경로를 생성하고, 궤적 계획(Trajectory Planning)은 실제 이동 궤적을 계산한다.

이 과정에서는 **A**\*, **다익스트라(Dijkstra)** 와 같은 그래프 탐색 알고리즘(Graph Search Algorithm), **RRT(Rapidly-exploring Random Tree)**, **PRM(Probabilistic Roadmap)** 등의 샘플링 기반 알고리즘(Sampling-Based Algorithm), 그리고 최적화(Optimization) 기법이 사용된다.

최근에는 의미 기반 계획(Semantic Planning), 지식 그래프(Knowledge Graph), 인공지능 계획기(AI Planner)도 함께 활용되고 있다.

마지막 단계인 **실행(Act)** 에서는 생성된 계획을 실제 하드웨어(Hardware)에 적용한다. 제어기(Control)는 생성된 경로를 따라 모터(Motor), 조향(Steering), 매니퓰레이터(Manipulator)를 제어하며, 센서 피드백을 이용하여 지속적으로 오차를 보정한다.

실행 중에도 센서는 계속 동작하며 계획과 실제 움직임 사이의 차이를 줄이기 위해 피드백 제어(Feedback Control)가 수행된다.

센스-플랜-액트 구조는 여러 가지 장점을 가진다. 먼저 전체 임무를 고려하여 장기적인 최적화(Long-Term Optimization)가 가능하다.

경로가 가장 짧거나 에너지 소비가 가장 적은 계획을 생성할 수 있으며, 자원(Resource), 시간(Time), 우선순위(Priority)를 종합적으로 고려한 전략적인 의사결정도 가능하다.

따라서 공장 자동화, 물류 시스템, 농업 로봇, 우주 탐사 로버(Rover)와 같이 구조화된 환경에서는 매우 효과적이다.

그러나 순수한 센스-플랜-액트 구조에는 한계도 존재한다. 가장 큰 문제는 **계획 시간이 필요하다** 는 점이다.

계획을 수행하는 동안 환경은 계속 변화한다. 사람이 움직이고, 차량이 지나가며, 새로운 장애물이 나타나면 기존 계획은 이미 유효하지 않을 수 있다.

결국 계획을 다시 수행해야 하며, 복잡한 환경에서는 이러한 반복적인 재계획(Replanning)이 시스템의 반응성을 크게 떨어뜨릴 수 있다.

또한 내부의 월드 모델(World Model)에 크게 의존한다는 문제도 있다.

현실에서는 센서 오차, 조명 변화, 통신 장애, GPS 오류 등으로 인해 월드 모델이 실제 환경과 달라질 수 있으며, 잘못된 모델을 기반으로 계획을 세우면 잘못된 행동을 수행할 위험이 있다.

이러한 문제를 해결하기 위해 등장한 것이 **반응형 아키텍처(Reactive Architecture)** 이다.

반응형 구조는 복잡한 내부 모델과 장기 계획 대신 현재의 센서 정보를 이용하여 즉시 행동을 결정한다.

즉, "생각한 후 행동한다"가 아니라 **"환경을 보고 바로 반응한다."** 는 것이 핵심 철학이다.

반응형 로봇은 **로드니 브룩스(Rodney Brooks)** 가 제안한 **서브섬션 아키텍처(Subsumption Architecture)** 를 통해 널리 알려졌다.

브룩스는 복잡한 내부 모델 없이도 단순한 행동들이 함께 동작하면 지능적인 행동이 자연스럽게 나타날 수 있다고 주장하였다.

반응형 구조에서는 인식과 행동이 매우 밀접하게 연결된다.

센서가 장애물을 감지하면 즉시 회피 행동이 실행되고, 배터리가 부족하면 충전 행동이 시작되며, 목표물이 보이면 즉시 추적을 시작한다.

복잡한 계획 과정을 거치지 않으므로 매우 빠르게 대응할 수 있다.

반응형 아키텍처는 여러 개의 행동(Behavior)이 동시에 실행된다.

장애물 회피(Obstacle Avoidance), 벽 따라가기(Wall Following), 목표 추적(Target Tracking), 충전(Charging), 통신(Communication) 등이 각각 독립적으로 동작하며, 행동 우선순위(Behavior Priority)에 따라 최종 행동이 결정된다.

안전과 관련된 행동은 항상 가장 높은 우선순위를 가진다.

반응형 구조의 가장 큰 장점은 **반응 속도(Responsiveness)** 이다.

복잡한 계획 없이 현재의 센서 정보만으로 즉시 행동을 수행할 수 있으므로 사람이 많은 환경이나 예측하기 어려운 환경에서 매우 효과적이다.

예를 들어 사람 사이를 이동하는 서비스 로봇(Service Robot)은 장기 계획보다 순간적인 장애물 회피가 훨씬 중요하다.

산업 현장의 안전 시스템도 대부분 반응형 구조를 사용한다.

비상 정지(Emergency Stop), 충돌 감지(Collision Detection), 힘 제한(Force Limitation), 속도 제한(Speed Limitation)은 계획보다 즉각적인 반응이 중요하기 때문이다.

또한 반응형 구조는 환경 변화에 매우 강하다.

현재의 센서 정보만 이용하므로 지도 오류(Map Error)나 위치추정(Localization) 오차가 발생해도 비교적 안정적으로 동작할 수 있다.

하지만 반응형 구조에도 단점이 존재한다.

현재 상황만 고려하므로 장기적인 목표(Long-Term Goal)를 고려하지 못하는 경우가 많다.

장애물만 계속 피하다 보면 목적지에 도착하지 못하거나 같은 장소를 계속 반복해서 이동하는 **지역 최소(Local Minimum)** 문제가 발생할 수 있다.

복잡한 미로(Maze) 환경에서는 이러한 문제가 더욱 심각해진다.

반응형 구조는 복잡한 순차 작업도 어려워한다.

예를 들어 검사 로봇이 여러 검사 지점을 방문하고, 자동문을 통과하고, 엘리베이터를 이용하며, 충전까지 수행하는 복잡한 임무는 단순한 반응만으로는 처리하기 어렵다.

이러한 이유로 현대 로봇은 **하이브리드 아키텍처(Hybrid Architecture)** 를 사용한다.

상위에서는 센스-플랜-액트 방식으로 장기 계획을 수행하고, 하위에서는 반응형 제어를 이용하여 실시간으로 환경 변화에 대응한다.

즉 계획과 반응을 동시에 사용하는 구조이다.

대표적인 예가 **3T(Task-Behavior-Control)** 구조이다.

작업 계층(Task Layer)은 장기적인 목표를 계획하고, 행동 계층(Behavior Layer)은 상황에 따라 행동을 조정하며, 제어 계층(Control Layer)은 실시간 반응을 수행한다.

각 계층은 서로 다른 시간 주기로 동작하며 서로를 보완한다.

내비게이션(Navigation)은 이러한 구조를 가장 잘 보여주는 예이다.

전역 계획(Global Planner)은 목적지까지의 최적 경로를 계산하고, 지역 계획(Local Planner)은 이동 중 나타나는 사람이나 장애물을 실시간으로 회피한다.

지역 회피가 반복적으로 실패하면 다시 전역 계획을 수행하여 새로운 경로를 생성한다.

최근에는 인공지능(AI)이 두 구조를 더욱 발전시키고 있다.

딥러닝(Deep Learning)은 인식 성능을 향상시키고, 강화학습(Reinforcement Learning)은 반응형 제어를 개선하며, 대규모 언어 모델(Large Language Model, LLM)은 장기적인 임무 계획을 지원한다.

즉 AI는 센스-플랜-액트와 반응형 구조 모두에서 활용되고 있다.

자율주행 자동차(Autonomous Vehicle)는 대표적인 하이브리드 구조이다.

전역 경로 계획(Global Route Planning)은 도시 전체를 대상으로 수행되고, 행동 계획(Behavior Planning)은 차선 변경과 교차로 통과를 관리하며, 반응형 제어는 보행자나 차량에 즉시 대응한다.

안전 시스템은 언제나 가장 높은 우선순위를 가진다.

물류 AMR도 동일하다.

플릿 관리(Fleet Management)는 여러 대의 로봇에게 작업을 분배하고, 각 로봇은 전역 경로를 계산하며, 이동 중에는 장애물을 즉시 회피한다.

배터리가 부족하면 반응적으로 충전을 시작하고, 상위 계획기는 작업을 다른 로봇에게 재할당한다.

휴머노이드(Humanoid)는 더욱 복잡한 구조를 가진다.

언어 이해(Language Understanding)와 추론(Reasoning)은 계획 계층에서 수행되며, 균형 유지(Balance Control), 보행(Gait), 충돌 회피는 반응형 제어에서 수행된다.

두 구조가 동시에 동작해야 사람과 자연스럽게 상호작용할 수 있다.

최근 등장한 **피지컬 AI(Physical AI)** 는 이러한 하이브리드 구조를 더욱 강화하고 있다.

파운데이션 모델(Foundation Model)은 환경을 이해하고, 비전-언어-행동 모델(Vision-Language-Action, VLA)은 행동 목표를 생성하며, 월드 모델(World Model)은 미래를 예측한다.

반면 실제 제어는 여전히 반응형 제어기와 결정론적인 실시간 제어가 담당한다.

시간적인 분리(Time Separation)도 중요하다.

장기 계획은 수 초 또는 수 분 단위로 수행되고, 반응형 장애물 회피는 수십 헤르츠(Hz), 모터 제어는 수천 헤르츠로 실행된다.

각 기능을 적절한 시간 주기로 분리하면 계산 효율과 안정성을 동시에 확보할 수 있다.

계획과 반응 사이의 통신도 중요하다.

반응형 제어는 새로운 환경 정보를 상위 계획기로 전달하고, 계획기는 새로운 목표를 하위 제어기로 전달한다.

이를 위해 블랙보드(Blackboard), ROS2 토픽(Topic), 발행-구독(Publish-Subscribe), 이벤트(Event) 기반 통신 등이 사용된다.

장애 복구(Failure Recovery)도 하이브리드 구조의 장점이다.

계획이 실패하면 반응형 탐색(Exploration)이 새로운 정보를 수집하고, 반대로 반응형 이동이 계속 실패하면 상위 계획기가 새로운 경로를 생성한다.

두 구조가 협력함으로써 훨씬 높은 신뢰성을 얻을 수 있다.

시험과 검증(Verification and Validation)도 계층별로 수행된다.

계획기는 시뮬레이션(Simulation)에서 평가하고, 반응형 제어는 **하드웨어 인 더 루프(Hardware-in-the-Loop, HIL)** 로 검증하며, 전체 시스템은 디지털 트윈(Digital Twin)을 이용하여 통합 시험을 수행한다.

ROS2의 **Navigation2(Nav2)** 역시 이러한 하이브리드 구조를 기반으로 설계되어 있다.

전역 계획기(Global Planner), 지역 계획기(Local Planner), 행동 트리(Behavior Tree), 복구 행동(Recovery Behavior), 컨트롤러(Controller)가 서로 협력하여 높은 수준의 자율주행 기능을 제공한다.

앞으로는 파운데이션 모델, 강화학습, 월드 모델, 행동 트리(Behavior Tree), 결정론적 제어가 하나의 통합된 구조로 발전할 것으로 예상된다.

각 기술은 서로 경쟁하는 것이 아니라 서로 다른 계층에서 서로를 보완하는 역할을 수행하게 된다.

결론적으로 **센스-플랜-액트(Sense-Plan-Act)** 와 **반응형 아키텍처(Reactive Architecture)** 는 서로 대립하는 기술이 아니라 서로를 보완하는 두 가지 핵심 철학이다. 센스-플랜-액트는 장기적인 계획(Long-Term Planning), 전역 최적화(Global Optimization), 전략적 의사결정(Strategic Decision Making)에 강점을 가지며, 반응형 아키텍처는 빠른 반응(Fast Response), 환경 적응성(Environmental Adaptation), 안전성(Safety)에 강점을 가진다. 현대의 자율주행 로봇, 산업용 로봇, 협동로봇(Cobot), 휴머노이드(Humanoid), 자율주행 자동차, 그리고 미래의 **AI 네이티브 로봇(AI-Native Robot)** 과 **피지컬 AI(Physical AI)** 는 이 두 가지 방식을 계층적으로 결합한 하이브리드 아키텍처를 기반으로 더욱 지능적이고 안전한 자율 시스템으로 발전해 나갈 것이다.

##  

## 06.07 Hardware Abstraction Layer for Robot Software

![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

Modern robotic systems integrate a remarkably diverse collection of hardware components including sensors, actuators, communication interfaces, embedded controllers, edge computers, artificial intelligence accelerators, industrial networks, and cloud gateways. Cameras, LiDARs, radars, inertial measurement units, GNSS receivers, force sensors, motor drivers, robotic manipulators, battery management systems, safety controllers, and wireless communication modules all operate according to different electrical interfaces, communication protocols, timing characteristics, and software development kits. Without a systematic architectural approach, software quickly becomes tightly coupled to specific hardware implementations, making robot maintenance, platform migration, product upgrades, and large-scale deployment extremely difficult. To address these challenges, modern robotics adopts the **Hardware Abstraction Layer (HAL)**, a foundational architectural component that separates hardware-specific implementation details from higher-level robot software. Hardware abstraction enables software portability, modularity, scalability, maintainability, and long-term system evolution while significantly reducing engineering complexity.

The fundamental objective of the Hardware Abstraction Layer is to provide a consistent software interface regardless of the underlying physical hardware. High-level applications should request services such as obtaining camera images, commanding motor velocity, reading battery status, acquiring localization information, or controlling robotic manipulators without knowing the manufacturer, communication protocol, electrical interface, or operating characteristics of the actual devices. HAL therefore functions as an intermediary between hardware resources and robot software, translating standardized software requests into device-specific operations while converting raw hardware data into uniform software representations.

The importance of abstraction becomes immediately apparent when considering hardware diversity across modern robotic platforms. A warehouse robot may employ one LiDAR vendor during prototype development and another during mass production. Agricultural robots operating outdoors may require different GNSS receivers than indoor inspection robots. Industrial manipulators from different manufacturers expose proprietary programming interfaces despite providing similar functional capabilities. If navigation software directly depends upon each manufacturer\'s API, changing hardware requires extensive software modification throughout the entire robotics application. Hardware abstraction eliminates this dependency by isolating vendor-specific implementations inside dedicated abstraction modules.

The Hardware Abstraction Layer follows one of the most fundamental principles of software engineering: **program to interfaces rather than implementations**. High-level software communicates only with abstract interfaces defining required functionality. Individual hardware drivers implement these interfaces according to the capabilities of specific devices. Consequently, replacing hardware usually requires modifying only the corresponding driver implementation while leaving perception, planning, navigation, artificial intelligence, mission management, and user interface software unchanged.

This separation significantly improves software maintainability. As hardware evolves over the operational lifetime of robotic products, software investment remains protected because application logic remains independent of specific devices. New sensors, improved actuators, upgraded communication modules, or next-generation AI accelerators integrate through additional HAL implementations without disrupting existing software architecture.

Hardware abstraction naturally introduces multiple architectural layers. The lowest layer consists of physical devices connected through electrical interfaces such as Ethernet, CAN, RS-485, SPI, I2C, USB, PCIe, GPIO, industrial fieldbus systems, or wireless communication links. Device drivers communicate directly with hardware according to manufacturer specifications, implementing initialization procedures, configuration management, communication protocols, error detection, timing synchronization, and data acquisition.

Above device drivers resides the Hardware Abstraction Layer itself. Rather than exposing manufacturer-specific APIs, HAL presents standardized software interfaces representing functional capabilities. Camera interfaces publish synchronized image streams independent of camera vendor. Motor interfaces accept velocity, position, or torque commands regardless of actuator implementation. Localization interfaces provide standardized robot pose estimates independent of GNSS receivers, visual localization systems, or SLAM algorithms. Battery interfaces consistently report remaining energy, charging state, health status, and operational limits despite differing battery management hardware.

Application software occupies higher architectural layers including perception, localization, mapping, navigation, behavior coordination, mission planning, artificial intelligence inference, diagnostics, cloud communication, fleet management, and human-machine interfaces. These components interact exclusively with standardized HAL interfaces rather than hardware-specific implementations. Consequently, software functionality remains independent of underlying hardware technologies.

Sensor abstraction represents one of the most important HAL responsibilities. Modern robots employ numerous sensing modalities including cameras, LiDARs, radars, ultrasonic sensors, IMUs, GNSS receivers, encoders, force sensors, tactile arrays, microphones, thermal cameras, environmental sensors, and industrial inspection devices. Each sensor family differs significantly regarding communication protocols, sampling frequencies, calibration procedures, synchronization requirements, data formats, and configuration mechanisms.

HAL transforms these heterogeneous devices into unified software interfaces. Camera abstraction consistently provides timestamped image frames regardless of sensor manufacturer. LiDAR abstraction publishes standardized point cloud representations independent of scanning technology. IMU abstraction delivers synchronized acceleration, angular velocity, and orientation estimates despite underlying sensor differences. Application developers therefore concentrate upon perception algorithms rather than hardware communication details.

Timestamp management forms another critical abstraction responsibility. Sensor synchronization significantly influences localization accuracy, sensor fusion quality, perception reliability, and motion estimation performance. Hardware devices often employ independent internal clocks with varying precision. HAL establishes unified timestamp management through Precision Time Protocol, hardware triggering, synchronized acquisition, clock translation, or software compensation mechanisms. Higher-level software consequently receives temporally consistent sensor information regardless of individual hardware timing characteristics.

Calibration abstraction similarly simplifies robot software. Camera intrinsic parameters, LiDAR alignment, IMU biases, encoder scaling, manipulator kinematics, GNSS antenna offsets, and extrinsic transformations between sensors remain stored within hardware abstraction modules rather than application software. Perception algorithms therefore consume calibrated observations without repeatedly implementing device-specific correction procedures.

Actuator abstraction represents an equally important architectural capability. Robot mobility systems include differential drives, Ackermann steering, omnidirectional platforms, mecanum wheels, tracked vehicles, articulated manipulators, quadrupeds, humanoid joints, collaborative robots, hydraulic systems, and industrial servo mechanisms. Although physical implementations differ substantially, higher-level planning software generally requires only abstract motion commands rather than low-level motor control details.

Motor abstraction therefore provides standardized interfaces supporting velocity control, position control, torque control, acceleration limits, braking commands, emergency stopping, feedback monitoring, and diagnostic reporting. Individual motor drivers convert these abstract commands into hardware-specific communication protocols including CANopen, EtherCAT, Modbus, proprietary Ethernet protocols, industrial fieldbus systems, or embedded controller interfaces.

Manipulator abstraction extends similar concepts toward robotic arms and end-effectors. Pick-and-place applications issue high-level commands such as move to pose, open gripper, close gripper, execute trajectory, or apply force without depending upon robot manufacturer programming languages. Whether the manipulator originates from Universal Robots, ABB, FANUC, KUKA, Doosan, Franka, or custom hardware becomes largely transparent above the abstraction layer.

Communication abstraction further enhances hardware independence. Industrial robots increasingly integrate Ethernet, Wi-Fi, Bluetooth, CAN, EtherCAT, OPC UA, Modbus TCP, DDS, MQTT, serial communication, 5G networks, and cloud connectivity. Rather than embedding protocol-specific logic throughout application software, HAL encapsulates communication mechanisms behind standardized messaging interfaces. Network upgrades therefore require minimal software modification.

Safety abstraction deserves particular attention because safety hardware often differs substantially across manufacturers while supporting similar operational objectives. Emergency stop circuits, safety laser scanners, safety PLCs, protective light curtains, collision sensors, speed monitoring systems, and redundant controllers expose different proprietary interfaces. HAL standardizes safety state reporting, emergency commands, fault acknowledgment, operational mode transitions, and diagnostic information while preserving certification requirements.

Power management also benefits significantly from hardware abstraction. Modern robots employ diverse battery chemistries including lithium iron phosphate, lithium-ion, lead-acid, and hybrid energy storage systems. Battery management hardware reports voltage, current, temperature, state of charge, state of health, remaining runtime, charging status, fault conditions, and thermal limits using proprietary communication formats. HAL converts these vendor-specific data into standardized battery information accessible throughout robot software.

Artificial intelligence hardware introduces additional abstraction requirements. Contemporary robots increasingly utilize heterogeneous computing platforms including CPUs, GPUs, NPUs, TPUs, FPGAs, dedicated AI accelerators, edge computing devices, and cloud inference services. AI software should execute independently of specific computational hardware whenever possible. HAL therefore abstracts hardware acceleration capabilities through unified inference interfaces supporting dynamic hardware selection according to computational availability, thermal constraints, power consumption, latency requirements, and operational policies.

Hardware abstraction also facilitates simulation. During software development, physical devices frequently remain unavailable or impractical to operate continuously. HAL enables seamless substitution of simulated devices for physical hardware. Simulated cameras generate virtual images, simulated LiDARs produce synthetic point clouds, simulated motors execute motion models, and simulated sensors reproduce realistic environmental measurements. Since application software interacts exclusively through abstract interfaces, simulation and physical deployment require minimal software modification.

Digital twin technologies extend this capability by combining simulated and physical hardware simultaneously. Some sensors may operate physically while others remain simulated. Hardware abstraction transparently manages this mixed execution environment, enabling incremental system validation, hardware-in-the-loop testing, and progressive deployment.

Fault tolerance constitutes another major advantage of hardware abstraction. Device failures become isolated within abstraction modules, preventing error propagation throughout application software. HAL continuously monitors hardware health, communication integrity, sensor availability, calibration status, thermal conditions, power supply, and operational diagnostics. Standardized fault reporting allows higher-level behavior coordination to activate recovery procedures independent of device manufacturers.

Hot-swappable hardware further benefits from abstraction. Future modular robots will increasingly replace sensors, batteries, AI modules, communication devices, or inspection payloads during operation. HAL supports dynamic hardware discovery, automatic driver loading, capability negotiation, and runtime reconfiguration without requiring application restart. Such flexibility aligns closely with software-defined robotics and adaptive robotic platforms.

ROS2 naturally incorporates many hardware abstraction concepts through its node-based architecture. Individual drivers publish standardized sensor messages, actuator interfaces expose common command structures, lifecycle nodes manage hardware initialization, and controller frameworks separate hardware implementations from control algorithms. Packages such as ros2_control provide standardized hardware interfaces enabling identical controllers to operate diverse robotic mechanisms through interchangeable hardware plugins.

The ros2_control framework illustrates hardware abstraction particularly well. Controllers generate abstract position, velocity, effort, or trajectory commands without depending upon specific motor manufacturers. Hardware interface plugins translate controller outputs into device-specific communication protocols. Consequently, replacing motor drivers frequently requires only changing hardware plugins while retaining identical controller software.

Plugin architectures further improve HAL flexibility. Instead of statically linking hardware implementations during compilation, plugins dynamically load appropriate device drivers during runtime. Configuration files specify selected hardware modules according to deployed robot platforms. One software distribution therefore supports multiple hardware configurations without recompilation, greatly simplifying product maintenance across robot families.

Configuration management becomes critically important as hardware diversity increases. Hardware abstraction commonly employs structured configuration files describing device addresses, calibration parameters, communication interfaces, synchronization settings, operational limits, safety constraints, and diagnostic options. Separating configuration from source code enables field deployment without software modification while simplifying manufacturing customization.

Large robotic fleets particularly benefit from standardized hardware abstraction. Hundreds or thousands of robots may employ slightly different hardware revisions accumulated throughout years of production. HAL enables fleet management software to operate uniformly despite underlying hardware variation. Maintenance teams replace failed components with equivalent alternatives without requiring software redevelopment.

Cybersecurity also benefits from abstraction. Secure communication, authentication, encrypted firmware updates, hardware identity management, trusted platform modules, secure boot, certificate management, and access control mechanisms integrate within HAL rather than application software. Centralized security management simplifies compliance while reducing duplicated implementation effort.

Performance optimization requires careful HAL design because abstraction inevitably introduces additional software layers. Efficient implementations minimize unnecessary memory copying, avoid excessive virtual function overhead, employ zero-copy communication where appropriate, support asynchronous I/O, utilize hardware interrupts efficiently, and maintain deterministic execution for real-time components. Well-designed abstraction layers provide flexibility without sacrificing computational performance.

Real-time considerations significantly influence hardware abstraction architecture. Motor controllers, safety systems, encoder acquisition, and deterministic communication require bounded execution latency. HAL therefore separates deterministic real-time interfaces from non-real-time management functions such as diagnostics, configuration, logging, firmware updates, and maintenance utilities. Mixed real-time architecture integrates naturally with hardware abstraction by assigning appropriate execution environments according to timing requirements.

Industrial robotics demonstrates numerous practical examples of HAL benefits. Autonomous Mobile Robots replace LiDAR vendors while preserving navigation software. Agricultural robots switch GNSS receivers according to regional availability without modifying localization algorithms. Inspection robots integrate multiple camera manufacturers using identical vision software. Collaborative robots support interchangeable grippers, force sensors, and vision systems through standardized hardware interfaces. Medical robots accommodate evolving imaging equipment without redesigning control software.

Future Physical AI systems will depend even more heavily upon hardware abstraction because computational hardware will evolve far more rapidly than application software. AI accelerators, multimodal sensors, tactile skins, event cameras, quantum positioning systems, neuromorphic processors, distributed edge computing platforms, and future robotic hardware technologies will continue emerging throughout coming decades. Without robust abstraction layers, robotics software would require continual redevelopment following every hardware innovation.

Hardware abstraction also enables reusable robotics software ecosystems. Navigation, perception, manipulation, behavior coordination, mission planning, fleet management, cloud integration, and artificial intelligence modules become portable across diverse robotic platforms ranging from warehouse robots and autonomous vehicles to agricultural systems, humanoids, inspection robots, collaborative manipulators, and service robots. Standardized hardware interfaces therefore accelerate innovation while reducing engineering cost.

Ultimately, the Hardware Abstraction Layer represents one of the most fundamental architectural principles in modern robotics software engineering. By separating hardware-specific implementation from application logic through standardized interfaces, modular drivers, portable communication mechanisms, synchronized sensor management, configurable hardware plugins, and reusable software abstractions, HAL enables robotic systems to evolve continuously while preserving software investment. As robotics advances toward AI-native, software-defined, and highly modular autonomous systems operating across increasingly diverse hardware platforms, Hardware Abstraction Layers will remain indispensable for achieving portability, scalability, maintainability, interoperability, and long-term technological sustainability.

현대의 로봇은 카메라(Camera), 라이다(LiDAR), 레이더(Radar), 관성측정장치(IMU), GNSS, 모터 드라이버(Motor Driver), 로봇 매니퓰레이터(Manipulator), 배터리 관리 시스템(BMS), 엣지 컴퓨터(Edge Computer), AI 가속기(AI Accelerator), 산업용 네트워크(Industrial Network) 등 매우 다양한 하드웨어를 동시에 사용한다. 이들 장치는 제조사와 통신 방식, 전기적 인터페이스, 소프트웨어 개발 도구(SDK)가 모두 다르기 때문에 이를 직접 사용하는 소프트웨어는 유지보수가 매우 어려워진다.

이러한 문제를 해결하기 위해 사용하는 핵심 구조가 **하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)** 이다. HAL은 하드웨어와 상위 소프트웨어 사이에서 중간 계층 역할을 수행하며, 다양한 장치를 동일한 방식으로 사용할 수 있도록 표준 인터페이스(Standard Interface)를 제공한다.

HAL의 가장 중요한 목적은 실제 장비와 상관없이 동일한 소프트웨어 인터페이스를 제공하는 것이다. 상위 소프트웨어는 "카메라 영상을 요청한다", "모터 속도를 설정한다", "배터리 상태를 읽는다", "로봇 위치를 가져온다"와 같은 기능만 호출하면 된다.

실제 내부에서는 HAL이 제조사별 API(Application Programming Interface), 통신 프로토콜(Communication Protocol), 데이터 형식을 자동으로 변환하여 장치를 제어한다. 따라서 응용 프로그램은 어떤 하드웨어가 사용되는지 알 필요가 없다.

하드웨어 추상화의 중요성은 실제 산업 현장에서 더욱 크게 나타난다. 예를 들어 개발 단계에서는 A사의 LiDAR를 사용하고 양산 단계에서는 B사의 LiDAR를 사용할 수 있다. 또한 실내 AMR은 UWB나 SLAM을 사용하지만 실외 로봇은 GNSS RTK를 사용할 수도 있다.

만약 응용 프로그램이 특정 제조사의 API를 직접 사용한다면 하드웨어를 교체할 때마다 전체 소프트웨어를 수정해야 한다. HAL은 이러한 제조사 의존성(Vendor Dependency)을 제거하여 하드웨어 교체 비용을 크게 줄여준다.

HAL은 **구현(Implementation)이 아니라 인터페이스(Interface)를 기준으로 설계한다** 는 소프트웨어 공학의 핵심 원칙을 따른다.

상위 소프트웨어는 추상 인터페이스(Abstract Interface)만 사용하며, 실제 하드웨어 드라이버는 각 제조사 장비에 맞추어 인터페이스를 구현한다. 따라서 하드웨어가 변경되더라도 상위 소프트웨어는 거의 수정하지 않아도 된다.

이러한 구조는 유지보수성(Maintainability)을 크게 향상시킨다. 로봇은 일반적으로 수년 이상 운영되며, 그동안 센서나 컴퓨팅 장치가 여러 번 교체될 수 있다.

HAL을 적용하면 새로운 센서나 AI 가속기를 추가하더라도 해당 드라이버만 수정하면 되므로 기존 인식(Perception), 내비게이션(Navigation), AI, 사용자 인터페이스(UI) 소프트웨어는 그대로 유지할 수 있다.

HAL은 일반적으로 여러 개의 계층으로 구성된다. 가장 아래에는 실제 하드웨어(Device)가 존재하며 Ethernet, CAN, RS-485, SPI, I2C, USB, PCIe, GPIO, EtherCAT 등의 인터페이스를 통해 연결된다.

그 위에는 **장치 드라이버(Device Driver)** 가 존재하며, 제조사가 제공하는 프로토콜에 따라 초기화, 설정, 데이터 수집, 오류 처리 등을 수행한다.

그 위에 위치하는 HAL은 제조사별 API를 숨기고 표준 인터페이스만 제공한다.

예를 들어 카메라는 모두 동일한 Image 인터페이스를 제공하고, LiDAR는 Point Cloud 인터페이스를 제공하며, 모터는 Velocity나 Torque 인터페이스를 제공한다. 상위 소프트웨어는 어떤 제조사의 장비인지 전혀 알지 못한 채 동일한 인터페이스만 사용하면 된다.

최상위에는 인식(Perception), 위치추정(Localization), 지도작성(Mapping), 경로 계획(Path Planning), 행동 제어(Behavior Control), AI 추론(AI Inference), 클라우드(Cloud), 플릿 관리(Fleet Management) 등의 응용 프로그램이 위치한다.

이들 프로그램은 오직 HAL 인터페이스만 사용하므로 하드웨어와 완전히 분리된 구조를 유지할 수 있다.

센서 추상화(Sensor Abstraction)는 HAL의 가장 중요한 기능 가운데 하나이다.

현대 로봇은 RGB 카메라, Depth Camera, LiDAR, Radar, IMU, GNSS, 엔코더(Encoder), 힘 센서(Force Sensor), 열화상 카메라(Thermal Camera), 마이크(Microphone) 등 매우 다양한 센서를 사용한다.

각 센서는 데이터 형식, 통신 방식, 샘플링 속도, 초기화 과정이 모두 다르지만 HAL은 이를 동일한 인터페이스로 통합한다.

예를 들어 카메라 제조사가 달라도 HAL은 동일한 이미지(Image) 메시지를 제공한다. LiDAR 제조사가 달라도 Point Cloud 형식은 동일하게 유지된다.

따라서 객체 인식(Object Detection), SLAM, AI 추론과 같은 상위 알고리즘은 하드웨어 변경과 무관하게 그대로 사용할 수 있다.

시간 동기화(Time Synchronization)도 HAL의 중요한 역할이다.

카메라, LiDAR, IMU는 서로 다른 내부 시계를 사용하기 때문에 정확한 센서 융합(Sensor Fusion)을 위해서는 시간을 맞추어야 한다.

HAL은 **정밀 시간 프로토콜(Precision Time Protocol, PTP)**, 하드웨어 트리거(Hardware Trigger), 소프트웨어 보정(Time Compensation) 등을 이용하여 센서의 시간을 통일하고, 상위 프로그램에는 동기화된 데이터만 제공한다.

센서 보정(Calibration)도 HAL에서 관리하는 것이 일반적이다.

카메라 내부 파라미터(Intrinsic Parameter), LiDAR 위치, IMU 오프셋(Bias), 엔코더 스케일(Scale), 센서 간 외부 파라미터(Extrinsic Calibration)는 HAL 내부에서 관리되며, 상위 프로그램은 이미 보정된 데이터를 사용하게 된다.

액추에이터 추상화(Actuator Abstraction)도 매우 중요하다.

로봇은 차동 구동(Differential Drive), 아커만 조향(Ackermann Steering), 메카넘 휠(Mecanum Wheel), 스티어 드라이브(Steer Drive), 매니퓰레이터, 협동로봇(Cobot), 유압 시스템(Hydraulic System) 등 매우 다양한 구동 방식을 사용한다.

하지만 상위 소프트웨어는 단순히 "속도를 설정하라", "위치를 이동하라", "토크를 제어하라"와 같은 명령만 전달하면 된다.

실제 CANopen, EtherCAT, Modbus, Ethernet 등의 통신은 HAL이 담당한다.

매니퓰레이터(Manipulator)도 동일한 원리로 동작한다.

Move Pose, Open Gripper, Close Gripper, Execute Trajectory와 같은 명령은 제조사와 관계없이 동일하게 사용할 수 있으며, Universal Robots, ABB, FANUC, KUKA, Doosan, Franka 등 다양한 로봇팔을 동일한 응용 프로그램으로 제어할 수 있다.

통신 추상화(Communication Abstraction)는 Ethernet, Wi-Fi, Bluetooth, CAN, EtherCAT, OPC UA, MQTT, DDS, Modbus TCP 등 다양한 통신 방식을 하나의 인터페이스로 통합한다.

응용 프로그램은 메시지를 송수신할 뿐 실제 어떤 프로토콜을 사용하는지는 HAL이 처리한다.

안전(Safety) 시스템도 HAL을 통해 추상화할 수 있다.

비상 정지(Emergency Stop), 안전 PLC(Safety PLC), 안전 LiDAR(Safety LiDAR), 라이트 커튼(Light Curtain), 충돌 감지(Collision Detection) 등은 제조사마다 인터페이스가 다르지만 HAL은 동일한 안전 상태(Safety State)와 진단(Diagnostics) 인터페이스를 제공한다.

전력 관리(Power Management) 역시 HAL의 중요한 기능이다.

리튬인산철(LFP), 리튬이온(Li-ion), 납축전지(Lead Acid) 등 다양한 배터리를 사용할 수 있으며, 배터리 관리 시스템(BMS)은 전압, 전류, 온도, 충전 상태(State of Charge), 건강 상태(State of Health)를 서로 다른 형식으로 제공한다.

HAL은 이를 하나의 표준 Battery Interface로 변환하여 상위 프로그램이 쉽게 활용할 수 있도록 한다.

최근에는 AI 하드웨어도 HAL의 대상이 되고 있다.

GPU, NPU, TPU, FPGA, 전용 AI 가속기, 엣지 컴퓨터(Edge Computer), 클라우드 AI 서비스 등 다양한 연산 장치를 하나의 AI 추론 인터페이스(AI Inference Interface)로 통합하면 하드웨어가 바뀌어도 AI 응용 프로그램은 수정하지 않아도 된다.

HAL은 시뮬레이션(Simulation)과 실제 하드웨어를 연결하는 역할도 수행한다.

개발 단계에서는 실제 센서 대신 Gazebo, Isaac Sim, Webots와 같은 시뮬레이터를 사용하고, 실제 운영에서는 물리 장비를 사용한다.

상위 소프트웨어는 HAL만 사용하므로 시뮬레이션과 실제 시스템을 거의 수정 없이 전환할 수 있다.

디지털 트윈(Digital Twin) 환경에서는 일부 장치는 실제 하드웨어를 사용하고, 나머지는 시뮬레이션으로 대체하는 혼합 운영도 가능하다.

HAL은 이러한 복합 환경을 자연스럽게 지원한다.

장애 허용(Fault Tolerance)도 HAL의 중요한 기능이다.

HAL은 장치 상태, 통신 오류, 센서 이상, 과열(Thermal Overload), 전원 이상 등을 지속적으로 감시하고 표준화된 오류 정보를 상위 시스템에 전달한다.

따라서 행동 제어(Behavior Control)는 제조사와 관계없이 동일한 복구(Recovery) 절차를 수행할 수 있다.

미래에는 **핫 스와핑(Hot Swapping)** 도 중요한 기능이 될 것이다.

센서, 배터리, AI 모듈을 교체하면 HAL이 새로운 장치를 자동으로 인식하고 적절한 드라이버를 로딩하여 응용 프로그램을 다시 시작하지 않고도 운영할 수 있게 된다.

ROS2는 이러한 HAL 구조를 자연스럽게 지원한다.

각 드라이버는 ROS2 노드(Node)로 구성되며 표준 메시지(Standard Message)를 발행하고, 라이프사이클 노드(Lifecycle Node)를 통해 초기화와 종료를 관리한다. 특히 **ros2_control** 은 HAL의 대표적인 구현 사례이다. Controller는 Position, Velocity, Effort 인터페이스만 사용하고 실제 CAN, EtherCAT, Modbus 통신은 Hardware Plugin이 담당한다.

모터 제조사가 바뀌어도 Controller는 그대로 사용할 수 있다. 플러그인(Plugin) 구조는 HAL을 더욱 유연하게 만든다. 실행 중 필요한 하드웨어 드라이버만 동적으로 로딩(Dynamic Loading)할 수 있으며, 설정 파일(Configuration File)만 변경하면 동일한 프로그램으로 여러 종류의 로봇을 지원할 수 있다.

대규모 플릿(Fleet)에서도 HAL은 매우 중요한 역할을 한다. 수백 대의 로봇이 조금씩 다른 하드웨어를 사용하더라도 플릿 관리 시스템은 동일한 인터페이스를 사용하므로 전체 시스템을 하나의 플랫폼처럼 운영할 수 있다. 사이버보안(Cybersecurity)도 HAL에 통합될 수 있다.

암호화 통신(Encrypted Communication), 인증(Authentication), 보안 부팅(Secure Boot), 인증서 관리(Certificate Management), 접근 제어(Access Control)를 HAL에서 통합 관리하면 상위 소프트웨어는 보안 기능을 별도로 구현할 필요가 없다.

HAL은 성능(Performance)도 고려해야 한다. 추상화 계층이 너무 많으면 메모리 복사(Memory Copy)와 함수 호출(Function Call)이 증가하여 성능이 저하될 수 있다.

따라서 제로 카피(Zero Copy), 비동기 입출력(Asynchronous I/O), 인터럽트(Interrupt), 실시간 스케줄링(Real-Time Scheduling)을 적극 활용하여 성능 손실을 최소화해야 한다. 실시간 제어(Real-Time Control)에서는 HAL도 실시간성을 유지해야 한다.

모터 제어와 엔코더 처리는 결정론적인 실행 시간이 필요하지만, 진단(Diagnostics), 로그(Log), 펌웨어 업데이트(Firmware Update)는 비실시간으로 수행할 수 있다. 따라서 HAL도 실시간과 비실시간 기능을 적절히 분리하여 설계하는 것이 중요하다.

산업 현장에서는 HAL의 활용 사례가 매우 많다. AMR은 LiDAR 제조사가 바뀌어도 내비게이션 소프트웨어는 그대로 사용할 수 있고, 농업 로봇은 GNSS 장비만 교체하면 된다. 검사 로봇은 카메라를 변경해도 AI 비전 시스템은 그대로 유지되며, 협동로봇은 다양한 그리퍼(Gripper)와 힘 센서(Force Sensor)를 동일한 인터페이스로 제어할 수 있다.

앞으로 **피지컬 AI(Physical AI)** 시대에는 하드웨어의 변화 속도가 소프트웨어보다 훨씬 빨라질 것이다.

AI 가속기, 이벤트 카메라(Event Camera), 촉각 센서(Tactile Sensor), 뉴로모픽 프로세서(Neuromorphic Processor), 차세대 엣지 컴퓨터가 계속 등장하더라도 HAL이 존재하면 상위 AI 소프트웨어는 거의 수정 없이 새로운 하드웨어를 활용할 수 있다.

또한 HAL은 다양한 로봇 플랫폼 간 소프트웨어 재사용성을 극대화한다.

내비게이션, 인식, 행동 제어, AI, 플릿 관리, 클라우드 연동 소프트웨어는 창고 AMR, 실외 자율주행 로봇, 농업 로봇, 검사 로봇, 휴머노이드, 협동로봇 등 다양한 플랫폼에서 동일한 구조로 사용할 수 있다.

결론적으로 **하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)** 은 현대 로봇 소프트웨어에서 가장 중요한 기반 기술 가운데 하나이다. HAL은 하드웨어의 세부 구현을 응용 소프트웨어로부터 완전히 분리하고, 표준 인터페이스(Standard Interface), 모듈형 드라이버(Modular Driver), 플러그인(Plugin), 센서 동기화(Sensor Synchronization), 통신 추상화(Communication Abstraction)를 제공함으로써 높은 이식성(Portability), 확장성(Scalability), 유지보수성(Maintainability), 상호운용성(Interoperability), 재사용성(Reusability)을 동시에 확보한다. 앞으로 **AI 네이티브 로봇(AI-Native Robot)**, **소프트웨어 정의 로봇(Software-Defined Robot)**, **피지컬 AI(Physical AI)** 시대에도 HAL은 다양한 하드웨어와 지능형 소프트웨어를 연결하는 핵심 아키텍처로 지속적으로 활용될 것이다.

##  

## 06.08 Multi-Robot Distributed Architecture Principles

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

The rapid evolution of autonomous robotics has shifted the focus of robot software architecture from controlling individual robots toward coordinating large groups of intelligent robotic systems operating as cooperative distributed networks. Modern industrial facilities, warehouses, hospitals, agricultural environments, ports, airports, construction sites, and smart cities increasingly deploy dozens, hundreds, or even thousands of autonomous robots that must cooperate to accomplish shared objectives. Instead of functioning as isolated machines, these robots continuously exchange information, negotiate responsibilities, coordinate movements, share environmental knowledge, and collectively optimize mission execution. This transformation has made **Multi-Robot Distributed Architecture** one of the most important architectural paradigms in modern robotics software engineering. Distributed architectures provide the scalability, reliability, flexibility, and computational efficiency necessary for future AI-native robotic ecosystems operating across highly dynamic environments.

Traditional robotic systems generally assumed that one robot executed one mission using one onboard computer. All perception, localization, planning, control, communication, and decision making occurred within a single integrated software stack. While this architecture remains effective for many applications, it becomes increasingly limited as operational complexity grows. A single robot possesses finite sensing capability, computational resources, battery capacity, communication bandwidth, and physical workspace. Large-scale industrial operations require multiple robots working simultaneously to increase productivity, improve fault tolerance, reduce mission completion time, and maximize resource utilization.

The transition from single-robot systems to distributed robotic fleets introduces fundamentally different software engineering challenges. Individual robots must cooperate without sacrificing autonomy. Shared resources must be allocated efficiently. Communication delays, network failures, hardware diversity, environmental uncertainty, and dynamic task allocation all become central architectural concerns. Consequently, distributed robot software must be designed according to principles substantially different from conventional centralized robotic applications.

The primary objective of distributed architecture is to divide complex robotic operations into multiple cooperating computational and physical agents. Each robot performs local sensing, local control, and local decision making while simultaneously contributing to global mission objectives through coordinated communication and information sharing. Instead of concentrating all intelligence within one centralized controller, distributed systems distribute computation, perception, planning, and execution across the entire robotic fleet.

One of the defining characteristics of distributed robotics is **decentralization**. In decentralized systems, no single robot possesses complete authority over every operational decision. Individual robots maintain local autonomy while coordinating with neighboring robots or supervisory management systems. Decentralization significantly improves robustness because failure of one robot does not necessarily compromise the remainder of the system. The fleet continues operating even when individual agents experience hardware faults, communication failures, or temporary environmental isolation.

Centralized architecture represents the simplest multi-robot coordination model. A central fleet management server continuously receives status information from every robot, computes global task assignments, optimizes traffic flow, schedules charging operations, resolves resource conflicts, and distributes execution commands. Each robot primarily executes locally while relying upon centralized planning and coordination.

Centralized systems offer several advantages. Since complete fleet information resides within one management system, global optimization algorithms can generate highly efficient task allocations, minimize travel distance, balance workload, optimize battery utilization, and maximize overall productivity. Warehouse logistics systems frequently employ centralized scheduling because inventory management, transportation priorities, and production scheduling naturally depend upon facility-wide information.

However, centralized architectures also introduce several limitations. The central server becomes a computational bottleneck as fleet size increases. Communication infrastructure must support continuous bidirectional data exchange with every robot. Network latency directly influences coordination quality. Most importantly, the central controller becomes a single point of failure. Complete loss of the central management system may disable the entire robotic fleet unless appropriate redundancy mechanisms exist.

Distributed architectures eliminate many of these limitations by allowing robots to coordinate directly with one another. Instead of relying exclusively upon centralized planning, robots exchange information through peer-to-peer communication while making cooperative decisions locally. Distributed coordination naturally improves scalability because computational workload increases proportionally with fleet size rather than accumulating within one server.

Hybrid architectures combine centralized optimization with decentralized execution. Strategic planning, fleet scheduling, mission allocation, digital twin synchronization, and enterprise integration remain centralized. Local navigation, obstacle avoidance, sensor processing, behavior coordination, emergency responses, and short-term decision making execute independently within individual robots. Hybrid architecture has become the dominant approach for modern industrial robotics because it combines global optimization with local robustness.

Communication forms the foundation of every distributed robotic architecture. Robots continuously exchange localization estimates, environmental observations, task status, battery information, diagnostic reports, obstacle maps, traffic conditions, resource availability, and mission progress. Communication quality directly influences coordination effectiveness, mission reliability, and operational safety.

Publish-subscribe communication models have become particularly important within distributed robotics. Rather than establishing direct point-to-point communication among every software component, robots publish information to shared communication channels while interested subscribers receive relevant updates asynchronously. This architecture significantly reduces coupling among distributed software components while supporting scalable communication across large robotic fleets.

The Data Distribution Service (DDS), which underlies ROS2 communication, naturally supports distributed robotic systems. DDS automatically discovers participating robots, manages communication quality, distributes information efficiently, supports multicast communication, and provides configurable Quality of Service policies appropriate for diverse robotic applications. Consequently, distributed ROS2 systems scale naturally across multiple computers and multiple robots.

Quality of Service configuration becomes especially important in distributed architectures. Safety messages require reliable low-latency delivery. Video streams may prioritize bandwidth efficiency. Localization updates require consistent timing. Diagnostic messages tolerate occasional delays. Appropriate QoS policies ensure communication resources remain allocated according to operational priorities.

Task allocation represents one of the most extensively studied problems in distributed robotics. Given multiple robots and multiple tasks, the system must determine which robot should perform each assignment. Efficient task allocation minimizes mission completion time while balancing workload, reducing energy consumption, avoiding traffic congestion, and respecting operational constraints.

Numerous task allocation algorithms have been developed. Market-based methods treat robots as autonomous economic agents competing through bidding mechanisms. Contract Net Protocol allows robots to announce available tasks while neighboring robots submit proposals based upon current capabilities. Auction algorithms dynamically assign work according to estimated execution cost. Consensus algorithms enable robots to negotiate cooperative decisions without centralized control. Optimization algorithms compute globally optimal assignments using mathematical programming techniques.

Task allocation frequently considers multiple optimization criteria simultaneously. Travel distance, remaining battery capacity, robot capabilities, payload limitations, maintenance schedules, environmental accessibility, communication quality, mission urgency, and operational priorities all influence assignment decisions. Multi-objective optimization therefore plays an increasingly important role within intelligent fleet management.

Shared world models significantly enhance distributed cooperation. Rather than each robot independently constructing isolated environmental representations, distributed mapping systems continuously merge sensor observations into common global maps. LiDAR measurements, camera observations, semantic information, dynamic obstacle tracking, and localization estimates collectively contribute toward shared environmental understanding. Every robot therefore benefits from observations acquired throughout the fleet.

Collaborative Simultaneous Localization and Mapping exemplifies this principle. Individual robots independently estimate local maps while periodically exchanging observations. Map merging algorithms identify overlapping regions, estimate relative robot positions, and construct unified global environmental models. Collaborative mapping significantly improves localization accuracy while accelerating exploration of unknown environments.

Distributed perception similarly enhances robotic intelligence. Cameras installed upon multiple robots observe different viewpoints simultaneously. Object recognition algorithms combine distributed observations to improve detection accuracy. Multi-robot tracking systems follow moving objects across extensive operational environments. Distributed sensor fusion therefore creates richer environmental understanding than any individual robot could achieve independently.

Localization coordination also benefits from information sharing. Robots exchange relative observations, GNSS measurements, landmark detections, visual features, and map alignment information. Cooperative localization algorithms reduce accumulated estimation uncertainty while maintaining accurate positioning despite individual sensor limitations.

Traffic management constitutes another essential component of distributed robotic architecture. Large robotic fleets frequently share narrow corridors, intersections, charging stations, elevators, loading docks, inspection zones, and constrained workspaces. Without coordinated traffic control, robots may block one another, create deadlocks, reduce productivity, or compromise operational safety.

Centralized traffic managers compute globally consistent movement schedules preventing collisions and congestion. Alternatively, distributed traffic negotiation allows neighboring robots to resolve conflicts locally through reservation systems, virtual traffic signals, priority negotiation, reciprocal avoidance algorithms, or behavior arbitration. Hybrid traffic management frequently combines centralized route planning with local collision avoidance.

Charging coordination becomes increasingly important as fleet size grows. Large robot populations continuously compete for limited charging infrastructure. Intelligent charging schedulers allocate charging resources according to battery levels, mission priorities, estimated future workload, and charging station availability. Predictive energy management minimizes operational interruptions while preventing simultaneous charging congestion.

Fault tolerance represents one of the greatest advantages of distributed robotics. Individual robot failures rarely terminate fleet operation because remaining robots dynamically redistribute incomplete missions. Distributed monitoring continuously evaluates hardware health, communication status, localization confidence, battery condition, and operational performance. Automatic task reassignment maintains productivity despite partial system failures.

Redundancy extends beyond physical robots into computational infrastructure. Multiple fleet management servers may operate simultaneously using distributed consensus protocols. Edge computing nodes replicate critical operational data. Distributed databases preserve mission information despite network disruptions. Cloud synchronization further protects operational continuity through geographically distributed infrastructure.

Distributed computing also improves computational scalability. Computationally intensive AI workloads including object recognition, semantic mapping, optimization, reinforcement learning, digital twin simulation, and predictive maintenance may execute across multiple edge computers or cloud resources. Robots dynamically distribute workloads according to available computational capacity, communication latency, and energy constraints.

Edge computing has become particularly important for distributed robotic systems. Rather than transmitting every sensor observation to distant cloud servers, nearby edge computers perform AI inference, map processing, data fusion, fleet coordination, and mission optimization while maintaining low communication latency. Cloud infrastructure subsequently performs long-term learning, historical analysis, digital twin synchronization, and enterprise integration.

Cloud robotics further extends distributed architecture beyond physical robot fleets. Robots continuously exchange operational knowledge through cloud-based knowledge repositories. Successful navigation experiences, learned manipulation strategies, semantic maps, object recognition models, failure recovery procedures, and software updates become available throughout entire robot populations. Collective learning therefore accelerates autonomous capability development.

Cybersecurity becomes critically important within distributed systems because communication networks expose expanded attack surfaces. Authentication, encrypted communication, certificate management, secure discovery, access control, trusted execution environments, secure boot, and continuous security monitoring protect distributed robotic infrastructure from unauthorized access or malicious interference.

Time synchronization significantly influences distributed coordination quality. Cooperative localization, sensor fusion, distributed perception, synchronized manipulation, and collaborative inspection all require consistent timestamps across multiple robots. Precision Time Protocol, Network Time Protocol, hardware synchronization, and deterministic communication protocols maintain temporal consistency throughout distributed systems.

Software architecture must also address heterogeneous robot capabilities. Large fleets frequently include robots possessing different payload capacities, mobility systems, sensor configurations, computational resources, manipulators, inspection equipment, or operational specializations. Distributed coordination algorithms therefore consider robot capabilities when assigning missions rather than assuming homogeneous hardware.

ROS2 provides numerous capabilities supporting distributed robotic architectures. DDS communication, lifecycle management, composition, namespace isolation, parameter management, service discovery, Quality of Service configuration, and standardized messaging collectively simplify multi-robot software development. Individual robots execute independent ROS2 systems while communicating through shared distributed middleware infrastructure.

Namespaces play a particularly important role. Each robot maintains isolated software components using unique namespaces while sharing selected topics through fleet communication channels. This organization prevents naming conflicts while preserving standardized software architectures across identical robot platforms.

Containerization and microservice architectures increasingly complement distributed robotics. Fleet management, AI inference services, mapping servers, digital twin synchronization, mission scheduling, monitoring dashboards, databases, and cloud gateways execute independently as scalable software services. Individual services scale horizontally according to operational demand without affecting robot software itself.

Digital twins have become central components of advanced distributed robotic architectures. Virtual fleet representations continuously synchronize with physical robot states, enabling predictive maintenance, traffic optimization, mission simulation, software validation, energy forecasting, and operational analytics. Digital twins therefore bridge physical robot fleets with enterprise management systems.

Artificial Intelligence increasingly transforms distributed robotic coordination. Multi-agent reinforcement learning enables robots to learn cooperative strategies through shared experience. Graph neural networks model robot interaction networks. Foundation models support semantic task allocation. Large Language Models facilitate human-fleet interaction through natural language mission specification. World models predict cooperative system evolution under changing environmental conditions.

Swarm robotics represents the most decentralized form of distributed architecture. Large populations of relatively simple robots cooperate through local interactions rather than centralized control. Inspired by biological systems including ants, bees, birds, and fish, swarm algorithms generate sophisticated collective behavior through simple distributed behavioral rules. Although individual robots remain computationally limited, collective intelligence emerges from large-scale interaction.

Industrial applications increasingly combine swarm concepts with structured fleet management. Hundreds of warehouse robots coordinate transportation autonomously while enterprise software manages inventory. Agricultural robots collectively monitor crops, distribute harvesting responsibilities, and share environmental information. Inspection robots simultaneously evaluate large industrial facilities while exchanging defect information through distributed perception systems.

Future Physical AI systems will depend heavily upon distributed architectures. Humanoid assistants, autonomous vehicles, drones, mobile manipulators, industrial robots, inspection platforms, and cloud AI services will increasingly operate as interconnected intelligent ecosystems rather than isolated machines. Foundation models may coordinate fleet-level reasoning while edge AI provides local intelligence and deterministic controllers ensure safe physical interaction.

Software-defined robotics further reinforces distributed architecture principles. Runtime software updates, dynamic AI model deployment, cloud-assisted optimization, distributed digital twins, predictive maintenance, collaborative learning, and adaptive fleet reconfiguration all require scalable distributed software infrastructure. Modular distributed architectures therefore become essential for long-term robotic evolution.

Ultimately, Multi-Robot Distributed Architecture represents one of the foundational principles supporting the future of autonomous robotics. By distributing computation, perception, communication, planning, task allocation, fault tolerance, artificial intelligence, and operational coordination across multiple cooperating robots and computing resources, distributed architecture enables robotic systems to achieve scalability, robustness, flexibility, efficiency, and collective intelligence far beyond the capabilities of individual autonomous machines. As robotics continues progressing toward large-scale AI-native ecosystems integrating edge computing, cloud intelligence, Physical AI, and collaborative autonomous systems, distributed architecture will remain the essential software foundation enabling intelligent cooperation across the next generation of robotic platforms.

현대의 자율 로봇은 더 이상 하나의 로봇이 하나의 작업만 수행하는 구조에 머물지 않는다. 창고(Warehouse), 공장(Factory), 병원(Hospital), 농업(Agriculture), 항만(Port), 공항(Airport), 스마트 시티(Smart City) 등에서는 수십에서 수천 대의 로봇이 동시에 협력하여 하나의 목표를 수행하는 시대가 되었다. 이러한 환경에서는 개별 로봇의 성능보다 여러 대의 로봇이 얼마나 효율적으로 협력하는지가 전체 시스템의 생산성과 안정성을 결정한다.

이러한 요구를 해결하기 위한 핵심 개념이 **다중 로봇 분산 아키텍처(Multi-Robot Distributed Architecture)** 이다. 이 구조는 여러 로봇이 서로 정보를 공유하고 역할을 분담하며, 전체 시스템을 하나의 지능형 네트워크처럼 운영하도록 설계된다.

기존의 단일 로봇(Single Robot) 시스템에서는 하나의 로봇이 하나의 컴퓨터에서 모든 기능을 수행하였다. 인식(Perception), 위치추정(Localization), 경로 계획(Path Planning), 제어(Control), 통신(Communication), 인공지능(AI) 등이 모두 로봇 내부에서 처리되었다.

이러한 구조는 소규모 시스템에서는 효과적이지만, 작업 범위가 넓어지고 로봇 수가 증가하면 계산 능력, 센서 범위, 배터리 용량, 작업 속도 등의 한계가 빠르게 나타난다.

다중 로봇 시스템에서는 여러 대의 로봇이 하나의 작업을 분담하여 수행한다.

각 로봇은 자신의 센서를 이용하여 주변을 인식하고, 자체적으로 제어를 수행하면서도 다른 로봇과 지속적으로 정보를 교환한다. 즉 개별 로봇은 독립적으로 움직이지만 동시에 전체 시스템의 일부로 협력하는 구조를 가진다.

분산 아키텍처의 가장 중요한 목적은 하나의 거대한 문제를 여러 개의 작은 문제로 나누어 여러 로봇이 동시에 해결하도록 하는 것이다.

이를 통해 계산 부하를 분산시키고, 작업 시간을 단축하며, 시스템 전체의 확장성(Scalability)과 신뢰성(Reliability)을 크게 향상시킬 수 있다.

분산 아키텍처의 가장 큰 특징은 **분산성(Decentralization)** 이다.

분산 시스템에서는 하나의 중앙 제어기(Central Controller)가 모든 결정을 내리는 것이 아니라, 각 로봇이 일정 수준의 자율성(Autonomy)을 가지고 스스로 판단한다.

따라서 하나의 로봇이 고장 나더라도 나머지 로봇은 계속 작업을 수행할 수 있으며, 시스템 전체가 중단되지 않는다.

반대로 **중앙 집중형 아키텍처(Centralized Architecture)** 는 하나의 플릿 관리 서버(Fleet Management Server)가 모든 로봇을 관리한다.

각 로봇은 자신의 상태와 센서 정보를 중앙 서버로 전송하고, 서버는 전체 작업을 최적화하여 각 로봇에게 새로운 작업을 할당한다.

창고 물류 시스템에서는 이러한 방식이 매우 많이 사용된다.

중앙 집중형 구조의 장점은 전체 시스템을 한 번에 최적화할 수 있다는 점이다.

모든 로봇의 위치와 배터리 상태, 작업 현황을 동시에 고려하여 이동 거리 최소화, 충전 최적화, 작업 균형 등을 계산할 수 있다.

전체 생산성을 최대화하기에는 매우 효과적인 구조이다.

그러나 중앙 집중형에는 한계도 존재한다.

모든 계산이 하나의 서버에 집중되므로 로봇 수가 증가하면 서버의 계산 부하가 급격히 증가한다.

또한 네트워크 장애가 발생하거나 중앙 서버가 고장 나면 전체 시스템이 영향을 받을 수 있는 **단일 장애점(Single Point of Failure)** 문제가 발생한다.

이러한 문제를 해결하기 위해 **분산형 아키텍처(Distributed Architecture)** 가 등장하였다.

분산 구조에서는 로봇들이 서로 직접 통신(Peer-to-Peer Communication)을 수행하며 필요한 정보를 공유한다.

중앙 서버가 없어도 기본적인 협력이 가능하며, 계산도 여러 로봇이 나누어 수행하므로 확장성이 매우 뛰어나다.

현재 산업 현장에서는 **하이브리드(Hybrid) 구조** 가 가장 많이 사용된다.

장기적인 작업 계획(Task Planning), 플릿 관리(Fleet Management), 생산 관리(Manufacturing Execution System, MES)는 중앙 서버에서 수행하고, 장애물 회피(Obstacle Avoidance), 센서 처리(Sensor Processing), 행동 제어(Behavior Control)는 각 로봇이 독립적으로 수행한다.

이러한 구조는 중앙 최적화와 분산 자율성을 동시에 활용할 수 있다.

분산 시스템에서 가장 중요한 요소는 **통신(Communication)** 이다.

로봇은 자신의 위치, 배터리 상태, 센서 정보, 장애물 위치, 작업 진행 상황 등을 지속적으로 다른 로봇이나 중앙 서버와 공유해야 한다.

통신 품질은 전체 협력 성능을 직접 결정한다.

가장 많이 사용되는 방식은 **발행-구독(Publish-Subscribe)** 구조이다.

로봇은 필요한 정보를 토픽(Topic)에 발행(Publish)하고, 다른 로봇은 필요한 정보만 구독(Subscribe)한다.

이러한 구조는 직접 연결(Point-to-Point Connection)보다 결합도(Coupling)가 낮고 확장성이 매우 뛰어나다.

ROS2의 기반인 **DDS(Data Distribution Service)** 는 이러한 분산 구조를 위해 설계되었다.

DDS는 새로운 로봇을 자동으로 발견하고(Auto Discovery), 통신을 관리하며, QoS(Quality of Service)를 이용하여 데이터의 우선순위와 신뢰성을 조절한다.

따라서 ROS2는 다중 로봇 시스템에 매우 적합한 미들웨어이다.

QoS 설정도 매우 중요하다.

비상 정지(Emergency Stop)는 반드시 전달되어야 하지만, 영상(Video)은 일부 프레임이 손실되어도 문제가 없는 경우가 많다.

위치 정보(Localization)는 일정한 시간 간격이 중요하며, 진단 정보(Diagnostics)는 약간의 지연을 허용할 수 있다.

기능에 따라 서로 다른 QoS를 적용하면 통신 효율을 크게 높일 수 있다.

다중 로봇 시스템의 핵심 문제 가운데 하나는 **작업 할당(Task Allocation)** 이다.

여러 작업과 여러 로봇이 존재할 때 어떤 로봇이 어떤 작업을 수행할 것인지 결정해야 한다.

목표는 작업 시간을 최소화하면서 배터리 소비와 이동 거리를 줄이고 전체 시스템의 효율을 최대화하는 것이다.

작업 할당에는 다양한 알고리즘이 사용된다.

시장 기반 방식(Market-Based Method)은 로봇이 입찰(Bidding)을 수행하여 작업을 가져가고, **계약망 프로토콜(Contract Net Protocol)** 은 작업을 공고하면 로봇들이 제안서를 제출하는 방식이다.

또한 경매 알고리즘(Auction Algorithm), 합의 알고리즘(Consensus Algorithm), 수학적 최적화(Optimization) 기법도 널리 사용된다.

작업 할당은 단순히 거리가 가까운 로봇을 선택하는 것이 아니다.

배터리 잔량(Battery Level), 적재 능력(Payload), 센서 구성(Sensor Configuration), 유지보수 상태(Maintenance Status), 작업 우선순위(Priority) 등을 모두 고려하여 최적의 로봇을 선택해야 한다.

이를 위해 다목적 최적화(Multi-Objective Optimization)가 많이 활용된다.

분산 시스템에서는 **공유 월드 모델(Shared World Model)** 도 매우 중요하다.

각 로봇이 자신의 지도(Map)만 사용하는 것이 아니라 여러 로봇의 센서 정보를 통합하여 하나의 공통 지도를 생성한다.

이를 통해 모든 로봇이 동일한 환경 정보를 사용할 수 있으며 위치추정과 내비게이션의 정확도도 향상된다.

대표적인 기술이 **협업 SLAM(Collaborative SLAM)** 이다.

각 로봇은 자신의 지도를 생성하고, 서로 겹치는 영역을 발견하면 이를 자동으로 병합하여 하나의 대규모 지도를 생성한다.

여러 로봇이 동시에 탐색하므로 지도 생성 속도와 정확도가 크게 향상된다.

분산 인식(Distributed Perception)도 중요한 개념이다.

여러 로봇이 서로 다른 위치에서 카메라와 LiDAR를 이용하여 환경을 관찰하고, 객체 인식 결과를 공유하면 단일 로봇보다 훨씬 높은 인식 성능을 얻을 수 있다.

움직이는 사람이나 차량을 여러 로봇이 동시에 추적하는 것도 가능하다.

협력 위치추정(Cooperative Localization) 역시 동일한 원리이다.

GNSS, LiDAR, 카메라, 랜드마크(Landmark) 정보를 여러 로봇이 공유하면 위치추정 오차를 줄이고 더 안정적인 내비게이션을 수행할 수 있다.

대규모 플릿에서는 **교통 관리(Traffic Management)** 가 필수적이다.

여러 로봇이 좁은 통로, 교차로, 엘리베이터, 충전소를 함께 사용하므로 적절한 교통 제어가 이루어지지 않으면 교착 상태(Deadlock)가 발생할 수 있다.

중앙 서버가 전체 교통을 관리할 수도 있고, 로봇끼리 직접 우선순위를 협상(Negotiation)하여 해결할 수도 있다.

충전 관리(Charging Management)도 중요한 요소이다.

수십 대의 로봇이 동시에 충전하려고 하면 충전소가 부족해질 수 있으므로 배터리 상태와 작업 우선순위를 고려하여 충전 일정을 계획해야 한다.

예측 기반 에너지 관리(Predictive Energy Management)는 작업 중단을 최소화하는 데 중요한 역할을 한다.

분산 구조의 가장 큰 장점 가운데 하나는 **장애 허용(Fault Tolerance)** 이다.

하나의 로봇이 고장 나더라도 다른 로봇이 해당 작업을 자동으로 이어받을 수 있다.

플릿은 지속적으로 각 로봇의 상태를 감시하며 문제가 발생하면 작업을 재할당(Reallocation)하여 전체 운영을 유지한다.

이러한 원리는 서버에도 적용된다.

여러 개의 플릿 관리 서버를 운영하거나, 엣지 컴퓨터(Edge Computer)에 데이터를 복제하고, 클라우드(Cloud)에 백업을 유지하면 하나의 서버가 고장 나더라도 전체 시스템은 계속 동작할 수 있다.

분산 컴퓨팅(Distributed Computing)은 AI에서도 매우 중요하다.

객체 인식(Object Detection), 디지털 트윈(Digital Twin), 예측 유지보수(Predictive Maintenance), 강화학습(Reinforcement Learning) 등의 계산량이 큰 작업은 여러 엣지 서버나 클라우드에 분산하여 처리할 수 있다.

로봇은 필요한 계산을 가장 가까운 컴퓨팅 자원으로 전달하여 처리한다.

최근에는 **엣지 컴퓨팅(Edge Computing)** 이 중요한 역할을 수행한다.

모든 데이터를 클라우드로 보내는 대신, 가까운 엣지 서버에서 AI 추론과 지도 생성, 센서 융합을 수행하면 통신 지연(Latency)을 크게 줄일 수 있다.

클라우드는 장기 학습(Long-Term Learning), 디지털 트윈, 데이터 분석 등을 담당한다.

**클라우드 로보틱스(Cloud Robotics)** 는 분산 구조를 더욱 확장한다.

한 로봇이 학습한 내비게이션 경험, 객체 인식 모델, 장애 복구 방법을 클라우드를 통해 다른 모든 로봇과 공유할 수 있다.

즉, 개별 로봇이 아니라 전체 플릿(Fleet)이 함께 학습하는 구조이다.

사이버보안(Cybersecurity)은 분산 시스템에서 더욱 중요하다.

여러 로봇이 네트워크로 연결되어 있으므로 인증(Authentication), 암호화(Encryption), 인증서 관리(Certificate Management), 접근 제어(Access Control), 보안 부팅(Secure Boot)이 필수적으로 적용되어야 한다.

시간 동기화(Time Synchronization)도 중요한 요소이다.

협업 SLAM, 센서 융합, 다중 로봇 검사에서는 모든 로봇이 동일한 시간을 사용해야 한다.

이를 위해 **정밀 시간 프로토콜(Precision Time Protocol, PTP)** 과 **네트워크 시간 프로토콜(Network Time Protocol, NTP)** 등이 사용된다.

현실에서는 모든 로봇이 동일한 하드웨어를 사용하는 것은 아니다.

적재 능력, 센서 구성, 계산 성능, 이동 방식이 서로 다를 수 있으므로 작업 할당은 이러한 능력(Capability)을 함께 고려해야 한다.

ROS2는 DDS, 네임스페이스(Namespace), QoS, 라이프사이클(Lifecycle), 컴포지션(Composition) 등을 통해 이러한 분산 구조를 자연스럽게 지원한다.

각 로봇은 독립적인 ROS2 시스템을 가지면서도 필요한 정보만 서로 공유하는 구조를 사용할 수 있다.

최근에는 **마이크로서비스(Microservice)** 와 컨테이너(Container) 기술도 많이 사용된다.

플릿 관리, AI 추론, 지도 서버(Map Server), 디지털 트윈, 모니터링, 데이터베이스(Database)를 각각 독립적인 서비스로 운영하면 필요한 기능만 쉽게 확장할 수 있다.

디지털 트윈(Digital Twin)은 실제 로봇과 가상 모델을 실시간으로 연결하여 예측 유지보수, 교통 최적화, 시뮬레이션, 운영 분석을 수행한다.

이는 미래의 대규모 로봇 운영에서 매우 중요한 요소가 될 것이다.

최근에는 AI가 분산 협력에도 활용되고 있다.

다중 에이전트 강화학습(Multi-Agent Reinforcement Learning)은 여러 로봇이 협력 전략을 학습하도록 하며, 그래프 신경망(Graph Neural Network)은 로봇 간 관계를 모델링한다.

파운데이션 모델(Foundation Model)은 작업 할당을 지원하고, 대규모 언어 모델(Large Language Model, LLM)은 사람과 플릿 간 자연어 인터페이스를 제공한다.

가장 분산화된 형태는 **스웜 로보틱스(Swarm Robotics)** 이다.

개별 로봇은 매우 단순한 규칙만 수행하지만, 개미나 벌과 같은 자연 생태계처럼 전체적으로는 매우 복잡하고 지능적인 집단 행동이 나타난다.

이는 미래 대규모 로봇 시스템의 중요한 연구 분야이다.

산업 현장에서는 이러한 스웜 개념과 플릿 관리가 함께 사용되고 있다.

창고에서는 수백 대의 AMR이 동시에 물류를 수행하고, 농업에서는 여러 대의 로봇이 협력하여 작물을 관리하며, 검사 로봇은 넓은 공장을 동시에 검사하면서 결과를 공유한다.

앞으로의 **피지컬 AI(Physical AI)** 시대에는 휴머노이드(Humanoid), AMR, 드론(Drone), 산업용 로봇, 검사 로봇, 자율주행차가 모두 하나의 AI 네트워크로 연결될 것이다.

파운데이션 모델은 전체 플릿 수준의 의사결정을 수행하고, 엣지 AI는 각 로봇의 지역 지능(Local Intelligence)을 담당하며, 실시간 제어기는 안전한 물리 제어를 수행하는 계층형 구조가 발전할 것으로 예상된다.

또한 **소프트웨어 정의 로봇(Software-Defined Robot)** 은 OTA 업데이트(Over-the-Air Update), AI 모델 배포, 디지털 트윈, 예측 유지보수, 협업 학습 등을 지속적으로 수행해야 하므로 확장 가능한 분산 소프트웨어 구조가 필수적이다.

결론적으로 **다중 로봇 분산 아키텍처(Multi-Robot Distributed Architecture)** 는 미래 자율 로봇 시스템의 핵심 기반 기술이다. 계산(Computation), 인식(Perception), 통신(Communication), 계획(Planning), 작업 할당(Task Allocation), 인공지능(AI), 장애 복구(Fault Recovery)를 여러 로봇과 컴퓨팅 자원에 분산함으로써 높은 확장성(Scalability), 신뢰성(Reliability), 유연성(Flexibility), 효율성(Efficiency), 집단 지능(Collective Intelligence)을 실현할 수 있다. 향후 **AI 네이티브 로봇(AI-Native Robot)**, **피지컬 AI(Physical AI)**, **엣지-클라우드 협업(Edge-Cloud Collaboration)** 시대에는 이러한 분산 아키텍처가 다양한 로봇 플랫폼을 하나의 지능형 생태계(Intelligent Robotic Ecosystem)로 연결하는 핵심 소프트웨어 구조로 자리 잡게 될 것이다.

##  

## 06.09 Human-Robot Interaction Architecture Interface

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

Human-Robot Interaction (HRI) has become one of the most significant research and engineering domains in modern robotics as autonomous systems increasingly operate alongside humans in industrial facilities, hospitals, warehouses, offices, homes, public spaces, and outdoor environments. Unlike traditional industrial robots that operated inside isolated safety cages with minimal human contact, contemporary collaborative robots, autonomous mobile robots, service robots, medical robots, and humanoid platforms must continuously communicate, cooperate, and interact with people in natural, intuitive, and safe ways. Consequently, Human-Robot Interaction is no longer considered merely a user interface component but has evolved into a comprehensive software architecture that integrates perception, cognition, communication, decision making, behavior planning, safety management, artificial intelligence, and real-time control into a unified interaction framework. The Human-Robot Interaction Architecture Interface defines how humans and robots exchange information, establish mutual understanding, coordinate shared tasks, and maintain safe collaboration throughout dynamic operational environments.

The primary objective of Human-Robot Interaction architecture is to establish an effective communication channel between human users and autonomous robotic systems. Unlike conventional software interfaces where users interact through keyboards or graphical applications, robotic interaction occurs simultaneously through speech, gestures, body movement, facial expressions, touch, visual displays, wearable devices, mobile applications, environmental sensors, and increasingly through multimodal artificial intelligence systems capable of interpreting complex human intentions. An effective HRI architecture must therefore integrate multiple communication modalities while providing consistent behavioral responses independent of individual input methods.

Human interaction begins with perception. Before meaningful collaboration becomes possible, robots must first perceive the presence of nearby humans and continuously monitor their activities. Human perception utilizes numerous sensing technologies including RGB cameras, depth cameras, stereo vision systems, LiDAR, radar, thermal imaging, microphones, inertial sensors, wearable devices, force sensors, tactile sensors, pressure-sensitive flooring, proximity sensors, and environmental monitoring systems. Each sensing modality contributes complementary information describing human position, movement, posture, identity, intention, physiological condition, and environmental context.

Visual perception remains the dominant sensing modality within Human-Robot Interaction. Cameras continuously observe surrounding environments while computer vision algorithms detect human bodies, estimate skeletal pose, recognize gestures, identify facial expressions, estimate gaze direction, monitor hand movement, classify activities, and detect safety hazards. Modern deep learning models provide highly accurate human detection under varying illumination conditions while estimating complex three-dimensional body configurations suitable for collaborative manipulation and social interaction.

Depth sensing significantly improves interaction quality by providing accurate spatial information independent of visual appearance. Structured light cameras, time-of-flight sensors, stereo vision, and depth-enhanced perception enable robots to estimate human distance, body orientation, arm position, and workspace occupancy. Accurate three-dimensional perception allows safe navigation among moving people while supporting collaborative manipulation tasks requiring precise spatial coordination.

Speech recognition has become another fundamental component of modern HRI architecture. Advances in deep learning, transformer-based language processing, and large speech recognition models allow robots to understand natural spoken commands across diverse operating environments. Rather than relying upon predefined command vocabularies, contemporary robots increasingly interpret conversational language, contextual references, follow-up questions, clarification requests, and multi-turn dialogue.

Natural Language Processing transforms recognized speech into structured semantic representations suitable for robotic reasoning. Language understanding identifies user intent, extracts relevant parameters, resolves ambiguities, interprets temporal references, recognizes object names, and establishes contextual meaning. For example, the spoken instruction "Please bring me the red toolbox from the maintenance room after charging yourself" requires semantic understanding involving task sequencing, object identification, navigation planning, resource management, and temporal reasoning before execution can begin.

Large Language Models have significantly expanded robotic communication capabilities. Instead of merely recognizing isolated commands, robots increasingly participate in natural conversations, explain operational status, answer questions, clarify uncertainties, summarize completed missions, generate procedural guidance, and adapt communication according to individual user preferences. Language models therefore function as cognitive interfaces translating between natural human communication and structured robotic planning systems.

Gesture recognition provides an additional communication modality particularly valuable in noisy industrial environments where speech recognition becomes difficult. Vision-based hand tracking, body pose estimation, wearable inertial sensors, and radar-based motion analysis identify pointing gestures, stop commands, directional guidance, object selection, collaborative manipulation cues, and emergency intervention signals. Gesture interfaces often complement spoken language, allowing multimodal communication that improves interaction robustness.

Facial expression recognition further enriches social interaction. Robots increasingly estimate emotional states including happiness, confusion, frustration, fatigue, surprise, or discomfort using visual facial analysis combined with contextual reasoning. Although emotion recognition remains inherently uncertain, integrating facial cues with speech, body posture, physiological sensing, and task context improves interaction personalization while enabling robots to adjust communication strategies according to perceived user conditions.

Human identification represents another important architectural capability. Face recognition, voice biometrics, wearable authentication devices, RFID badges, mobile device pairing, access control systems, and enterprise identity management allow robots to distinguish authorized operators, maintenance personnel, visitors, supervisors, and collaborative workers. Personalized interaction becomes possible once user identity has been established. Robots recall individual preferences, authorization levels, task histories, communication styles, language preferences, accessibility requirements, and operational permissions while maintaining appropriate security controls.

Multimodal fusion forms one of the defining characteristics of advanced Human-Robot Interaction architecture. Rather than relying upon one sensing modality alone, robots integrate speech, vision, gesture, touch, localization, environmental context, historical interaction, physiological sensing, and task status into unified human understanding. Multimodal fusion significantly improves robustness because uncertainty within one communication channel may be compensated through complementary observations from others.

Context awareness substantially influences interaction quality. Identical human commands may require different interpretations depending upon environmental conditions, mission status, user identity, safety constraints, robot operational mode, and collaborative task progress. For example, the instruction "Stop" spoken during navigation differs fundamentally from "Stop" spoken during conversational explanation. HRI architecture therefore maintains contextual knowledge continuously throughout interaction rather than processing individual commands independently.

Intent recognition extends beyond direct command interpretation by estimating underlying human objectives. Rather than reacting solely to explicit instructions, intelligent robots increasingly infer probable user intentions through behavioral observation, historical interaction patterns, environmental context, gaze direction, movement prediction, and task progression. Anticipatory interaction reduces communication burden while improving collaboration efficiency.

Following perception and understanding, HRI architecture enters the interaction management stage. Interaction management coordinates dialogue flow, command validation, clarification requests, error recovery, task negotiation, permission handling, feedback generation, and behavioral adaptation. Rather than immediately executing every recognized command, robots evaluate command feasibility, operational safety, authorization requirements, environmental constraints, and mission priorities before responding appropriately.

Dialogue management coordinates conversational interaction across multiple communication turns. Robots maintain conversational context, resolve references, remember previous topics, answer clarification questions, request additional information when necessary, and manage interruptions gracefully. Modern dialogue systems increasingly integrate symbolic reasoning with large language models to combine conversational flexibility with reliable task execution.

Behavior planning transforms human intentions into executable robotic actions. High-level interaction objectives become structured mission plans coordinated through Behavior Trees, task planners, mission managers, or workflow engines. For example, a request to inspect a manufacturing cell may require navigation, localization, equipment verification, image acquisition, AI inspection, report generation, and user notification. Interaction architecture therefore connects natural communication with deterministic robotic execution.

Feedback represents one of the most essential components of successful Human-Robot Interaction. Humans naturally expect confirmation that robots have understood commands correctly and are progressing toward requested objectives. Feedback may include spoken responses, graphical displays, projected visual information, indicator lights, augmented reality overlays, mobile notifications, wearable alerts, or expressive robotic motion. Continuous feedback significantly improves user confidence while reducing misunderstanding.

Explainable Artificial Intelligence has become increasingly important within HRI architecture. Rather than simply performing actions, robots increasingly explain why particular decisions were made, which environmental observations influenced planning, why requested actions cannot safely execute, and what alternative solutions remain available. Explainability substantially improves user trust, regulatory acceptance, debugging efficiency, and collaborative decision making.

Trust constitutes one of the most fundamental human factors influencing robotic adoption. Human operators must develop appropriate confidence in robotic capabilities without becoming either excessively skeptical or dangerously overconfident. HRI architecture therefore carefully manages transparency, predictability, consistent behavior, understandable communication, safety assurance, and reliable performance to establish calibrated trust throughout long-term operation.

Safety remains the highest priority within every Human-Robot Interaction architecture. Collaborative robots operating near humans continuously monitor separation distance, relative velocity, force interaction, workspace occupancy, human posture, environmental hazards, emergency stop devices, protective zones, and operational risk levels. Safety monitoring executes independently from conversational or AI processing to ensure deterministic protective responses.

International safety standards including ISO 10218 and ISO/TS 15066 define collaborative robot safety principles supporting power and force limitation, speed and separation monitoring, safety-rated monitored stop, hand-guided operation, and collaborative workspace protection. HRI architecture integrates these safety mechanisms directly into interaction management, ensuring that communication never overrides fundamental protective behaviors.

Physical interaction introduces additional architectural complexity. Collaborative manipulation, object handover, assistive robotics, rehabilitation systems, wearable exoskeletons, and humanoid assistance require continuous force sensing, impedance control, compliance regulation, tactile feedback, grasp adaptation, and shared control strategies. Rather than commanding rigid robot motion, physical HRI emphasizes adaptive behavior responding naturally to human movement and applied forces.

Shared autonomy represents another important interaction paradigm. Instead of assigning complete control either to humans or robots, shared autonomy dynamically allocates responsibility according to task complexity, environmental uncertainty, user expertise, sensor confidence, and operational safety. Teleoperation augmented with autonomous assistance, semi-autonomous navigation, collaborative manipulation, and supervisory control all exemplify shared autonomy architectures.

User interface technologies continue expanding beyond conventional graphical displays. Tablets, wearable devices, augmented reality glasses, virtual reality environments, digital twins, voice assistants, gesture interfaces, projected interfaces, smartphone applications, haptic controllers, and spatial computing platforms increasingly function as complementary HRI components. Effective architecture integrates these diverse interfaces through unified interaction services rather than isolated communication channels.

Cloud connectivity extends Human-Robot Interaction beyond local physical environments. Remote monitoring, fleet supervision, software updates, mission scheduling, operational analytics, telepresence, remote expert assistance, predictive maintenance, and cloud-based conversational intelligence allow geographically distributed collaboration between human operators and robotic systems. Edge computing simultaneously preserves low-latency local interaction while cloud services provide computationally intensive reasoning.

ROS2 naturally supports Human-Robot Interaction through distributed communication architecture. Vision systems, speech recognition modules, dialogue managers, navigation systems, manipulation controllers, digital twins, mobile applications, cloud services, and operator interfaces exchange information using standardized publish-subscribe communication. Behavior Trees frequently coordinate interaction workflows while lifecycle management controls interface initialization and fault recovery.

Artificial intelligence increasingly transforms every component of Human-Robot Interaction. Computer vision models recognize human activities. Speech models transcribe conversations. Language models interpret intent. Reinforcement learning optimizes interaction strategies. World models predict human behavior. Foundation models integrate multimodal understanding. Graph neural networks represent social relationships. Together these technologies produce increasingly natural collaborative behavior while preserving deterministic execution through structured robot software architecture.

Human digital twins may eventually become important interaction components. Robots maintain continuously updated computational models describing user preferences, physical capabilities, cognitive workload, operational responsibilities, interaction history, accessibility requirements, and collaborative performance. Personalized assistance therefore adapts continuously according to evolving user characteristics while respecting privacy and ethical constraints.

Accessibility represents another major architectural consideration. Robots should communicate effectively with users possessing diverse physical abilities, languages, sensory limitations, cognitive characteristics, and technological experience. Multimodal interaction naturally supports accessibility by allowing users to choose among speech, touch, gesture, visual displays, wearable interfaces, or remote communication according to individual needs.

Cybersecurity and privacy become increasingly critical as robots collect large quantities of human-related information. Facial images, voice recordings, interaction history, operational preferences, medical information, industrial knowledge, location tracking, and behavioral analytics require careful protection through encryption, authentication, access control, secure storage, privacy-preserving AI, and regulatory compliance. HRI architecture must therefore integrate cybersecurity directly into interaction services rather than treating it as an independent infrastructure concern.

Future Human-Robot Interaction will increasingly evolve toward Physical AI systems combining perception, reasoning, communication, manipulation, and social intelligence into unified cognitive architectures. Vision-Language-Action models will translate natural language directly into robotic behavior. Foundation models will provide semantic world understanding. Large Language Models will support conversational reasoning. World models will predict human intentions and environmental evolution. Multimodal AI will integrate vision, speech, touch, and contextual knowledge into comprehensive interaction intelligence. Deterministic robot controllers, behavior trees, safety supervisors, and real-time control systems will continue ensuring predictable physical execution despite increasingly sophisticated cognitive capabilities.

Ultimately, Human-Robot Interaction Architecture Interface represents one of the foundational architectural disciplines enabling intelligent collaboration between humans and autonomous robotic systems. By integrating multimodal perception, natural language understanding, gesture recognition, contextual reasoning, dialogue management, behavior planning, explainable artificial intelligence, shared autonomy, safety supervision, cloud connectivity, and real-time control within a unified software architecture, HRI establishes the communication bridge through which humans and robots cooperate effectively, safely, and naturally. As robotics progresses toward AI-native collaborative ecosystems populated by humanoids, service robots, industrial assistants, autonomous mobile robots, and Physical AI platforms, Human-Robot Interaction architecture will remain an essential software foundation supporting trustworthy, adaptive, and intelligent collaboration between people and machines.

현대의 로봇은 더 이상 사람과 분리된 공간에서 독립적으로 동작하는 기계가 아니다. 협동로봇(Collaborative Robot), 자율이동로봇(AMR), 서비스 로봇(Service Robot), 의료 로봇(Medical Robot), 휴머노이드(Humanoid)는 사람과 같은 공간에서 함께 작업하고 협력해야 한다. 따라서 **인간-로봇 상호작용(Human-Robot Interaction, HRI)** 은 단순한 사용자 인터페이스(UI)를 넘어, 인식(Perception), 의사소통(Communication), 인공지능(AI), 행동 계획(Behavior Planning), 안전(Safety), 실시간 제어(Real-Time Control)를 통합하는 핵심 소프트웨어 아키텍처가 되었다.

HRI 아키텍처의 목적은 사람과 로봇이 서로의 의도를 이해하고 자연스럽게 정보를 교환하며 공동의 작업을 수행할 수 있도록 하는 것이다. 기존 컴퓨터처럼 키보드나 마우스만 사용하는 것이 아니라, 음성(Speech), 제스처(Gesture), 표정(Facial Expression), 시선(Gaze), 터치(Touch), 모바일 기기(Mobile Device), 웨어러블(Wearable Device) 등 다양한 방법을 동시에 활용하는 것이 특징이다.

효율적인 HRI 시스템은 여러 입력 방식을 통합하여 일관성 있는 상호작용을 제공해야 하며, 사용자는 특별한 교육 없이도 자연스럽게 로봇과 협력할 수 있어야 한다.

상호작용의 첫 단계는 **인간 인식(Human Perception)** 이다. 로봇은 먼저 사람의 존재를 인식하고 지속적으로 움직임과 상태를 관찰해야 한다.

이를 위해 RGB 카메라(Camera), 깊이 카메라(Depth Camera), 스테레오 비전(Stereo Vision), 라이다(LiDAR), 레이더(Radar), 열화상 카메라(Thermal Camera), 마이크(Microphone), IMU, 촉각 센서(Tactile Sensor), 압력 센서(Pressure Sensor) 등 다양한 센서를 사용한다.

각 센서는 서로 다른 정보를 제공하며, 이를 종합하여 사람의 위치, 자세, 움직임, 행동을 이해하게 된다.

영상 기반 인식(Visual Perception)은 HRI에서 가장 중요한 기술이다. 컴퓨터 비전(Computer Vision)은 사람의 몸을 검출하고, 골격 추정(Pose Estimation)을 수행하며, 손동작(Gesture), 얼굴(Face), 시선(Gaze), 활동(Activity) 등을 분석한다.

최근 딥러닝(Deep Learning)의 발전으로 다양한 조명과 환경에서도 높은 정확도로 사람을 인식할 수 있게 되었으며, 협동 작업을 위한 3차원 자세 추정도 가능해지고 있다.

깊이 센서(Depth Sensor)는 사람과 로봇 사이의 거리를 정확하게 계산하는 데 사용된다.

구조광(Structured Light), ToF(Time-of-Flight), 스테레오 비전은 사람의 위치와 자세를 3차원으로 측정하여 안전한 이동과 협동 작업을 지원한다.

이러한 공간 정보는 충돌 방지와 협업 제어에 매우 중요한 역할을 한다.

음성 인식(Speech Recognition)은 현대 HRI의 핵심 기능이다.

최신 딥러닝 기반 음성 인식 모델은 다양한 환경에서도 사람의 말을 높은 정확도로 인식할 수 있으며, 단순한 명령어뿐 아니라 자연스러운 대화도 이해할 수 있다.

이를 통해 로봇은 사람과 보다 직관적인 방식으로 상호작용할 수 있다.

음성 인식 이후에는 **자연어 처리(Natural Language Processing, NLP)** 가 수행된다.

NLP는 사용자의 의도를 분석하고 필요한 정보를 추출하며, 모호한 표현을 해석한다.

예를 들어 "충전한 후 정비실에서 빨간 공구함을 가져와."라는 명령은 충전, 이동, 물체 인식, 작업 순서 등을 모두 이해해야 실행할 수 있다.

최근에는 **대규모 언어 모델(Large Language Model, LLM)** 이 HRI에 적극 활용되고 있다.

LLM은 단순히 명령을 이해하는 것을 넘어 대화를 이어가고, 작업 상태를 설명하며, 질문에 답하고, 오류 상황을 안내하는 등 사람과 자연스러운 대화를 수행할 수 있다.

이로 인해 로봇은 단순한 기계가 아니라 협력 파트너로 발전하고 있다.

제스처 인식(Gesture Recognition)은 음성이 어려운 환경에서 매우 유용하다.

카메라, IMU, 레이더 등을 이용하여 손짓, 방향 지시, 정지 명령, 물체 선택 등을 인식할 수 있으며, 음성과 함께 사용하면 더욱 높은 신뢰성을 얻을 수 있다.

멀티모달(Multimodal) 상호작용의 중요한 구성 요소이다.

표정 인식(Facial Expression Recognition)은 사람의 감정을 이해하는 데 활용된다.

행복, 당황, 피로, 놀람, 불편함 등의 감정을 분석하여 로봇이 상황에 맞는 대응을 수행할 수 있도록 한다.

단독으로 사용하기보다는 음성, 자세, 작업 상황과 함께 종합적으로 판단하는 것이 일반적이다.

사람 식별(Human Identification)도 중요한 기능이다.

얼굴 인식(Face Recognition), 음성 인증(Voice Biometrics), RFID, 모바일 인증, 출입 관리 시스템 등을 이용하여 사용자를 구분하고, 권한(Authorization)과 개인 설정(Personal Preference)을 적용할 수 있다.

이를 통해 맞춤형 서비스와 보안 기능을 동시에 제공할 수 있다.

현대 HRI의 가장 큰 특징은 **멀티모달 융합(Multimodal Fusion)** 이다.

음성, 영상, 제스처, 위치, 센서 정보, 과거 이력 등을 통합하여 사용자의 의도를 더욱 정확하게 이해한다.

한 가지 입력 방식이 실패하더라도 다른 정보를 이용하여 안정적인 상호작용을 유지할 수 있다.

상황 인식(Context Awareness)도 매우 중요하다.

동일한 명령이라도 현재 작업, 사용자 권한, 로봇 상태, 주변 환경에 따라 다른 의미를 가질 수 있다.

예를 들어 "멈춰."라는 명령은 이동 중에는 긴급 정지를 의미하지만, 설명 중에는 대화를 잠시 중단하는 의미일 수도 있다.

따라서 HRI는 항상 현재의 문맥(Context)을 함께 고려해야 한다.

의도 추론(Intent Recognition)은 사용자가 명시적으로 말하지 않은 목적까지 추정하는 기술이다.

사용자의 행동, 시선, 과거 작업, 현재 환경을 분석하여 다음 행동을 예측함으로써 보다 자연스럽고 효율적인 협력이 가능해진다.

인간의 의도를 미리 이해하는 것이 미래 HRI의 중요한 방향이다.

사용자의 의도가 이해되면 **상호작용 관리(Interaction Management)** 단계가 시작된다.

이 단계에서는 명령 검증, 대화 흐름 관리(Dialogue Management), 오류 처리(Error Handling), 작업 협상(Task Negotiation), 권한 확인(Authorization), 피드백 제공(Feedback) 등을 수행한다.

즉시 명령을 실행하는 것이 아니라 안전성과 실행 가능성을 먼저 확인한다.

대화 관리(Dialogue Management)는 여러 차례 이어지는 대화를 관리한다.

로봇은 이전 대화 내용을 기억하고, 질문에 답하며, 필요한 경우 추가 설명을 요청하고, 대화가 중단되더라도 자연스럽게 이어갈 수 있어야 한다.

최근에는 LLM과 규칙 기반 시스템을 함께 사용하는 방식이 많이 활용되고 있다.

행동 계획(Behavior Planning)은 사람의 요구를 실제 로봇 행동으로 변환한다.

예를 들어 "검사 작업을 시작해."라는 명령은 이동, 위치 확인, 카메라 촬영, AI 검사, 보고서 작성 등의 여러 작업으로 분해되어 행동 트리(Behavior Tree)나 작업 계획기(Task Planner)를 통해 실행된다.

피드백(Feedback)은 성공적인 HRI에서 매우 중요한 요소이다.

사람은 로봇이 자신의 명령을 제대로 이해했는지 확인하고 싶어 한다.

따라서 음성 응답, 화면 표시(Display), LED, 모바일 알림, 증강현실(AR), 몸짓(Motion) 등을 이용하여 현재 상태와 진행 상황을 지속적으로 알려주는 것이 필요하다.

최근에는 **설명 가능한 인공지능(Explainable AI, XAI)** 도 중요한 역할을 한다.

로봇은 단순히 행동하는 것이 아니라 왜 그런 결정을 내렸는지, 어떤 정보를 사용했는지, 왜 작업을 수행할 수 없는지를 사용자에게 설명할 수 있어야 한다.

이는 사용자 신뢰성과 유지보수성을 크게 향상시킨다.

신뢰(Trust)는 HRI에서 가장 중요한 요소 가운데 하나이다.

사용자는 로봇을 과도하게 신뢰해서도 안 되고, 지나치게 불신해서도 안 된다.

따라서 로봇은 일관성 있는 행동, 예측 가능한 동작, 명확한 설명을 제공하여 적절한 신뢰 수준을 형성해야 한다.

안전(Safety)은 HRI에서 절대적으로 우선되는 요소이다.

협동로봇은 사람과의 거리, 속도, 힘, 작업 공간을 지속적으로 감시하며 충돌 가능성을 예측한다.

이러한 안전 기능은 AI나 대화 시스템과 독립적으로 동작하여 항상 결정론적으로 실행되어야 한다.

국제 표준인 **ISO 10218** 과 **ISO/TS 15066** 은 협동로봇의 안전 원칙을 정의하고 있다.

힘 제한(Power and Force Limitation), 속도 및 거리 감시(Speed and Separation Monitoring), 안전 정지(Safety-Rated Monitored Stop) 등의 기능은 HRI 아키텍처에 반드시 포함되어야 한다.

물리적 상호작용(Physical Interaction)은 HRI를 더욱 복잡하게 만든다.

물체 전달(Object Handover), 협동 조립(Collaborative Assembly), 재활 로봇(Rehabilitation Robot), 웨어러블 로봇(Exoskeleton)은 힘 제어(Force Control), 임피던스 제어(Impedance Control), 순응 제어(Compliance Control), 촉각 피드백(Tactile Feedback)을 이용하여 사람과 자연스럽게 협력해야 한다.

**공유 자율성(Shared Autonomy)** 은 사람과 로봇이 함께 제어를 수행하는 방식이다.

상황에 따라 사람이 직접 조작하기도 하고, 로봇이 자동으로 보조하기도 하며, 작업 난이도와 안전 수준에 따라 제어 권한을 유연하게 변경한다.

원격 조작(Teleoperation)과 자율 제어를 결합한 구조가 대표적인 예이다.

사용자 인터페이스(User Interface)도 다양해지고 있다.

태블릿(Tablet), 스마트폰, 증강현실(AR), 가상현실(VR), 디지털 트윈(Digital Twin), 웨어러블 장치 등이 모두 HRI의 일부로 사용되며, 하나의 통합 인터페이스에서 관리되는 것이 바람직하다.

클라우드(Cloud)는 HRI를 원격 환경까지 확장한다.

원격 모니터링(Remote Monitoring), 플릿 관리(Fleet Management), 원격 지원(Remote Assistance), AI 분석, 소프트웨어 업데이트 등을 통해 사람은 언제 어디서나 로봇과 상호작용할 수 있다.

엣지 컴퓨팅(Edge Computing)은 실시간 응답을 담당하고, 클라우드는 장기적인 학습과 분석을 수행한다.

ROS2는 HRI를 구현하기 위한 다양한 기능을 제공한다.

카메라, 음성 인식, 대화 시스템, 내비게이션, 행동 트리, 모바일 앱 등이 DDS 기반으로 연결되며, 라이프사이클(Lifecycle)과 QoS를 이용하여 안정적인 상호작용을 구현할 수 있다.

AI는 HRI의 모든 영역을 변화시키고 있다.

컴퓨터 비전은 사람을 인식하고, 음성 모델은 대화를 이해하며, LLM은 자연어를 처리하고, 강화학습(Reinforcement Learning)은 상호작용 전략을 최적화한다.

또한 월드 모델(World Model)은 사람의 다음 행동을 예측하고, 파운데이션 모델(Foundation Model)은 다양한 입력을 통합적으로 이해한다.

미래에는 **인간 디지털 트윈(Human Digital Twin)** 이 등장할 것으로 예상된다.

로봇은 사용자의 선호도, 작업 습관, 신체 능력, 작업 이력을 지속적으로 학습하여 개인 맞춤형 서비스를 제공하게 된다.

단, 개인정보 보호(Privacy)와 윤리(Ethics)를 반드시 함께 고려해야 한다.

접근성(Accessibility)도 매우 중요하다.

노인, 장애인, 다양한 언어 사용자 등 누구나 쉽게 로봇을 사용할 수 있도록 음성, 제스처, 터치, 화면 등 여러 인터페이스를 제공해야 한다.

다양한 입력 방식을 제공하는 것이 포용적인 HRI의 핵심이다.

사이버보안(Cybersecurity)과 개인정보 보호(Privacy)는 HRI에서 필수 요소이다.

얼굴 영상, 음성 데이터, 위치 정보, 작업 이력 등은 암호화(Encryption), 인증(Authentication), 접근 제어(Access Control), 보안 저장(Secure Storage)을 통해 안전하게 관리되어야 한다.

미래의 **피지컬 AI(Physical AI)** 시대에는 HRI도 크게 발전할 것이다.

비전-언어-행동 모델(Vision-Language-Action, VLA), 파운데이션 모델(Foundation Model), 월드 모델(World Model), 대규모 언어 모델(LLM)이 사람의 의도를 더욱 정확하게 이해하고 행동을 생성하게 될 것이다.

그러나 실제 물리적인 제어는 여전히 행동 트리(Behavior Tree), 안전 감독기(Safety Supervisor), 실시간 제어기(Real-Time Controller)가 담당하여 안전성과 결정론성을 유지하게 된다.

결론적으로 **인간-로봇 상호작용 아키텍처(Human-Robot Interaction Architecture Interface)** 는 현대 로봇 소프트웨어의 핵심 구성 요소이다. 멀티모달 인식(Multimodal Perception), 자연어 처리(NLP), 제스처 인식(Gesture Recognition), 대화 관리(Dialogue Management), 행동 계획(Behavior Planning), 설명 가능한 AI(XAI), 공유 자율성(Shared Autonomy), 안전 관리(Safety Management), 클라우드 연동(Cloud Integration), 실시간 제어(Real-Time Control)를 하나의 통합된 아키텍처로 연결함으로써 사람과 로봇이 안전하고 자연스럽게 협력할 수 있는 기반을 제공한다. 앞으로 **AI 네이티브 로봇(AI-Native Robot)**, **휴머노이드(Humanoid)**, **서비스 로봇(Service Robot)**, **협동로봇(Cobot)**, 그리고 **피지컬 AI(Physical AI)** 시대에는 HRI 아키텍처가 인간과 로봇을 연결하는 가장 중요한 소프트웨어 플랫폼으로 더욱 발전하게 될 것이다.

##  

## 06.10 Integration of Safety Functions into SW Architecture

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

As autonomous robots become increasingly integrated into factories, hospitals, warehouses, public spaces, agricultural environments, construction sites, logistics centers, and human-centered workplaces, safety has evolved from being a standalone hardware feature into a comprehensive software architecture concern. Traditional industrial robots operated inside fenced work cells where physical separation alone provided adequate protection. Modern collaborative robots, autonomous mobile robots, service robots, humanoids, autonomous vehicles, and intelligent inspection systems now share workspaces with humans while continuously making autonomous decisions. Consequently, safety can no longer rely solely on emergency stop circuits, protective barriers, or hardware interlocks. Instead, safety must be deeply integrated throughout the entire robot software architecture, influencing perception, planning, communication, behavior coordination, artificial intelligence, real-time control, cloud services, and system management. The integration of safety functions into software architecture establishes a comprehensive framework that continuously monitors operational conditions, evaluates risk, supervises system behavior, and guarantees safe operation throughout the robot\'s entire lifecycle.

The primary objective of safety-integrated software architecture is to ensure that every software component contributes to maintaining an acceptable level of operational risk. Safety should not be viewed as an isolated subsystem activated only during emergencies. Instead, safety must become a continuous supervisory layer that observes every decision generated by perception modules, planning algorithms, artificial intelligence models, navigation systems, manipulation controllers, communication services, and user interfaces. Every software action should therefore be evaluated not only according to mission objectives but also according to safety constraints before execution.

Modern robot software architectures commonly separate functional behavior from safety supervision. Functional software focuses on accomplishing assigned missions efficiently, while independent safety components continuously verify whether those actions remain within predefined operational boundaries. This architectural separation follows the principle of independent supervision, ensuring that failures within mission software cannot directly compromise safety mechanisms. Even if navigation software generates an unsafe trajectory or an AI model produces an incorrect decision, the safety supervisor remains capable of preventing hazardous execution.

Safety supervision naturally spans multiple architectural layers. At the hardware level, emergency stop circuits, safety programmable logic controllers, redundant processors, watchdog timers, power monitoring, safe motor controllers, and certified safety sensors provide deterministic protective capabilities. Above these hardware mechanisms, software continuously monitors sensor health, actuator status, communication integrity, localization confidence, computational performance, battery conditions, environmental hazards, and operational state transitions. Higher architectural layers incorporate risk assessment, behavioral validation, mission supervision, operator authorization, cybersecurity monitoring, and regulatory compliance.

Risk assessment forms one of the foundational principles of software-integrated safety. Before any robotic action is executed, the software continuously estimates the probability and severity of potential hazards. Risk evaluation considers numerous factors including robot speed, payload mass, stopping distance, human proximity, environmental complexity, sensor confidence, communication quality, actuator status, battery condition, terrain characteristics, weather conditions, and operational context. Safety decisions therefore become dynamic rather than static, adapting continuously to changing environmental conditions.

Hazard identification begins during perception. Modern robots continuously monitor their surroundings using cameras, LiDARs, radars, ultrasonic sensors, depth cameras, thermal sensors, force sensors, tactile arrays, inertial measurement units, GNSS receivers, and proximity sensors. Safety-oriented perception extends beyond obstacle detection by recognizing human presence, estimating human intention, monitoring safety zones, identifying dangerous equipment, detecting floor conditions, recognizing emergency situations, and evaluating environmental accessibility. Multiple sensing modalities are fused to increase reliability while reducing false detections.

Sensor redundancy significantly enhances safety. Multiple independent sensing technologies often observe the same operational environment simultaneously. For example, collaborative robots may combine LiDAR, depth cameras, safety laser scanners, and force sensors to monitor nearby humans. If one sensing modality temporarily fails or produces uncertain observations, complementary sensors continue providing reliable environmental awareness. Redundant perception therefore improves fault tolerance while reducing single points of failure.

Localization confidence also directly influences operational safety. Robots continuously estimate not only their position but also the uncertainty associated with localization. If confidence decreases due to poor GNSS reception, degraded visual features, LiDAR occlusion, sensor malfunction, or map inconsistency, software safety supervisors automatically reduce vehicle speed, increase safety margins, activate alternative localization methods, or request operator intervention. Safe behavior therefore adapts dynamically according to localization quality.

Safety constraints must also influence planning algorithms. Navigation software should generate trajectories satisfying both mission efficiency and safety requirements simultaneously. Planned routes consider human traffic density, obstacle clearance, vehicle dynamics, emergency stopping distance, speed limits, operational zones, restricted areas, battery limitations, environmental hazards, and regulatory requirements. Safe trajectory generation therefore becomes an optimization problem balancing productivity with risk minimization.

Behavior Trees naturally support safety integration through dedicated supervisory nodes. Safety conditions continuously evaluate robot state before permitting execution of lower-level behaviors. Emergency stop requests, localization failures, communication interruptions, sensor faults, battery depletion, collision risk, and operator commands automatically interrupt mission execution while activating predefined recovery behaviors. Safety conditions therefore remain continuously active regardless of current mission progress.

State machines similarly provide structured safety management by defining explicit operational modes. Typical robotic systems transition among initialization, standby, autonomous operation, manual control, collaborative mode, degraded operation, recovery mode, emergency stop, maintenance mode, and shutdown. Each operational state defines permitted behaviors, communication policies, motion limits, user permissions, and recovery procedures. Controlled state transitions prevent unsafe combinations of operational activities.

Real-time control represents one of the most safety-critical software components. Motor controllers continuously monitor commanded velocity, acceleration, torque, current consumption, actuator temperature, encoder consistency, communication latency, and feedback integrity. Safety supervision immediately overrides motion commands whenever operational limits are exceeded. Deterministic execution remains essential because unpredictable response times may compromise physical safety during rapidly evolving situations.

Speed and Separation Monitoring has become one of the defining safety functions within collaborative robotics. Robots continuously estimate human distance while dynamically adjusting operating speed according to separation requirements. As humans approach collaborative workspaces, robot velocity decreases progressively until complete stopping occurs if minimum separation distances are violated. Dynamic speed adaptation maximizes productivity while preserving acceptable risk levels.

Power and Force Limitation represents another fundamental collaborative safety mechanism. Instead of relying solely upon physical separation, collaborative manipulators continuously monitor joint torque, contact force, motor current, mechanical impedance, and collision energy. Physical interaction remains acceptable provided contact forces remain below medically established injury thresholds. Software therefore continuously regulates actuator behavior according to biomechanical safety models.

Safety-rated monitored stop enables robots to suspend motion while maintaining power and operational readiness. Human operators may safely enter collaborative workspaces while the robot continuously monitors environmental conditions. Once safe conditions are restored, automatic operation resumes without complete system restart. Software architecture coordinates these transitions while preserving mission continuity and operator awareness.

Emergency stop remains the final protective mechanism within every robotic architecture. Emergency stop commands immediately override all mission execution regardless of current operational state. Hardware emergency circuits guarantee deterministic power removal while software simultaneously records diagnostic information, preserves operational logs, notifies supervisory systems, activates warning indicators, and initiates controlled recovery procedures. Integration between hardware and software ensures comprehensive incident management.

Fault detection constitutes another major responsibility of safety-integrated software. Continuous diagnostic monitoring evaluates sensor availability, communication quality, actuator health, processor utilization, memory integrity, storage capacity, network connectivity, AI inference performance, localization confidence, thermal conditions, battery health, and software exceptions. Rather than waiting for catastrophic failures, predictive diagnostics identify gradual degradation before hazardous conditions develop.

Fault tolerance extends diagnostic capabilities by enabling continued operation despite component failures. Redundant sensors, backup localization algorithms, alternative communication channels, replicated computing nodes, secondary controllers, and degraded operating modes allow robotic systems to maintain essential functionality while reducing operational capability. Safe degradation frequently proves preferable to immediate system shutdown within critical industrial operations.

Watchdog supervision further improves software reliability. Independent watchdog processes continuously monitor software execution timing, communication heartbeat signals, task responsiveness, and computational health. If primary control software becomes unresponsive due to deadlock, excessive computation, memory corruption, or unexpected failure, watchdog systems automatically trigger recovery procedures or transition toward safe operational states.

Cybersecurity increasingly contributes directly to robotic safety. Unauthorized access, malicious software modification, communication spoofing, denial-of-service attacks, compromised AI models, and manipulated sensor data all represent potential physical safety hazards. Secure authentication, encrypted communication, certificate management, secure boot, trusted execution environments, firmware verification, access control, and intrusion detection therefore become essential architectural safety functions rather than optional security enhancements.

Artificial intelligence introduces unique safety challenges because machine learning models inherently produce probabilistic rather than deterministic outputs. AI perception systems may misclassify objects, language models may misunderstand instructions, reinforcement learning policies may encounter unfamiliar situations, and foundation models may generate unexpected behavior. Consequently, AI decisions should never directly control safety-critical functions without deterministic supervisory validation.

AI safety architecture commonly separates intelligent reasoning from certified execution. Perception models estimate environmental understanding while deterministic safety supervisors independently validate resulting actions before execution. If AI recommendations violate predefined safety constraints, supervisory software rejects unsafe decisions while selecting alternative behaviors. This layered architecture combines AI flexibility with deterministic protection.

Explainable Artificial Intelligence significantly enhances operational safety by providing understandable reasoning supporting autonomous decisions. Operators can verify why robots selected particular actions, rejected alternative plans, reduced operating speed, requested human intervention, or activated recovery procedures. Transparent reasoning improves trust while facilitating debugging, incident investigation, regulatory compliance, and operational training.

Human-Robot Interaction architecture integrates closely with safety supervision. Robots continuously monitor human location, posture, intention, authorization level, physiological condition, interaction history, and communication context. Safety policies dynamically adapt according to user expertise, operational role, collaborative task, environmental conditions, and current mission state. Authorized maintenance personnel, for example, may safely operate robots under conditions inappropriate for untrained visitors.

User interfaces contribute significantly toward operational safety by communicating robot intentions clearly. Visual displays, voice synthesis, warning lights, projected pathways, augmented reality overlays, wearable notifications, mobile applications, and expressive robot motion all inform nearby personnel regarding current operational state, intended movement, detected hazards, and emergency conditions. Clear communication reduces misunderstanding while improving situational awareness.

Cloud-connected robotic systems require distributed safety management. Local controllers always retain immediate responsibility for deterministic physical protection, while cloud infrastructure performs long-term analytics, predictive maintenance, fleet coordination, software updates, and digital twin synchronization. Safety-critical decisions should never depend exclusively upon remote communication because network latency and connectivity remain inherently uncertain.

Fleet management introduces additional safety considerations. Multiple robots operating simultaneously require coordinated traffic management, collision avoidance, charging supervision, resource allocation, emergency evacuation planning, and shared workspace protection. Fleet-level safety supervisors continuously monitor collective system behavior while resolving potential conflicts before hazardous interactions occur.

Digital twins increasingly support safety validation throughout robotic system development. Virtual environments simulate hardware failures, communication interruptions, sensor degradation, environmental hazards, human interaction scenarios, emergency procedures, and rare operational events difficult to reproduce physically. Safety algorithms therefore undergo extensive validation before deployment into operational systems.

Simulation similarly supports software verification by evaluating safety functions under thousands of operating conditions. Hardware-in-the-loop testing integrates real controllers with simulated environments, enabling deterministic validation of emergency stopping, collision avoidance, localization recovery, fault detection, degraded operation, and communication failures without risking physical equipment or human safety.

International safety standards strongly influence robot software architecture. ISO 10218 defines industrial robot safety requirements. ISO/TS 15066 specifies collaborative robot safety principles. IEC 61508 establishes functional safety methodologies. ISO 13849 addresses safety-related control systems. IEC 62061 defines machine safety functional requirements. Autonomous mobile robots increasingly reference ISO 3691-4, while autonomous vehicles employ additional automotive functional safety standards. Modern software architectures integrate these regulatory requirements throughout design rather than addressing compliance after implementation.

Safety lifecycle management extends beyond deployment. Operational logs, diagnostic records, incident reporting, software updates, configuration management, cybersecurity patches, AI model validation, predictive maintenance, operator training, and continuous performance monitoring all contribute toward maintaining acceptable long-term safety. Architecture therefore supports traceability, reproducibility, auditing, and regulatory documentation throughout system operation.

Future Physical AI systems will require even deeper integration between cognition and safety. Foundation models will provide semantic environmental understanding, Vision-Language-Action models will generate intelligent behavior, world models will predict future environmental evolution, reinforcement learning will optimize adaptive strategies, and multimodal reasoning will interpret complex human intentions. Despite these advances, deterministic safety supervisors, certified real-time controllers, hardware redundancy, and formally verified protective mechanisms will continue governing physical execution. Intelligent reasoning will remain subordinate to certified safety constraints whenever uncertainty arises.

Software-defined robotics will further strengthen safety integration by supporting secure over-the-air software updates, continuous cybersecurity improvement, distributed digital twin validation, runtime policy adaptation, AI model certification, predictive diagnostics, fleet-wide safety analytics, and cloud-assisted regulatory compliance. Safety architecture will therefore evolve continuously alongside operational experience rather than remaining static after initial deployment.

Ultimately, the integration of safety functions into software architecture represents one of the most fundamental principles governing modern autonomous robotics. Rather than existing as isolated emergency mechanisms, safety functions become pervasive architectural services continuously supervising perception, localization, planning, artificial intelligence, communication, behavior coordination, real-time control, cloud connectivity, fleet management, and human interaction. By combining deterministic safety supervision, continuous risk assessment, fault tolerance, redundant sensing, cybersecurity, explainable AI, international safety standards, and adaptive operational policies within a unified architectural framework, modern robot software achieves the robustness, trustworthiness, regulatory compliance, and operational reliability required for safe collaboration between autonomous machines and human society. As robotics advances toward AI-native ecosystems populated by collaborative robots, autonomous vehicles, humanoids, industrial assistants, and Physical AI platforms, integrated software safety architecture will remain the indispensable foundation enabling intelligent autonomy without compromising human safety or system reliability.

# 23_06_10 소프트웨어 아키텍처에 안전 기능 통합 (Integration of Safety Functions into Software Architecture)

현대의 자율 로봇은 공장(Factory), 물류센터(Logistics Center), 병원(Hospital), 건설 현장(Construction Site), 농업(Agriculture), 공공장소(Public Space) 등 사람과 같은 공간에서 함께 작업하는 환경으로 빠르게 확대되고 있다. 과거 산업용 로봇은 안전 펜스(Safety Fence) 안에서 동작했지만, 협동로봇(Cobot), 자율이동로봇(AMR), 서비스 로봇(Service Robot), 휴머노이드(Humanoid)는 사람과 직접 상호작용해야 한다. 따라서 안전(Safety)은 단순한 하드웨어 기능이 아니라 소프트웨어 전체를 구성하는 핵심 아키텍처 요소가 되었다.

현대 로봇에서는 인식(Perception), 계획(Planning), 인공지능(AI), 제어(Control), 통신(Communication), 클라우드(Cloud), 사용자 인터페이스(UI) 등 모든 소프트웨어 구성 요소가 안전을 고려하여 설계되어야 한다.

안전 통합 아키텍처의 가장 중요한 목적은 로봇의 모든 소프트웨어가 항상 허용 가능한 위험 수준(Acceptable Risk Level)을 유지하도록 하는 것이다.

안전은 비상 상황에서만 동작하는 별도의 기능이 아니라, 로봇이 움직이는 모든 순간에 지속적으로 위험을 감시하고 판단하는 상시 감독 시스템(Supervisory System)이다.

모든 행동은 임무 수행뿐 아니라 안전 기준을 동시에 만족해야 실제 실행된다.

현대 로봇 소프트웨어는 일반적으로 **기능(Function)** 과 **안전(Safety)** 을 분리하여 설계한다.

기능 소프트웨어는 작업을 수행하는 것이 목적이며, 독립적인 안전 감독기(Safety Supervisor)는 모든 명령이 안전한지 지속적으로 검사한다.

설령 AI가 잘못된 판단을 하거나 내비게이션이 위험한 경로를 생성하더라도 안전 감독기는 이를 차단하고 안전한 상태를 유지한다.

이러한 독립성은 기능 오류가 안전 기능까지 영향을 주지 않도록 하는 중요한 원칙이다.

안전 기능은 여러 계층(Layer)에 걸쳐 통합된다.

하드웨어 계층(Hardware Layer)에서는 비상 정지(Emergency Stop), 안전 PLC(Safety PLC), 이중 프로세서(Redundant Processor), 워치독 타이머(Watchdog Timer), 안전 모터 드라이버(Safe Motor Driver)가 기본적인 보호 기능을 수행한다.

상위 소프트웨어에서는 센서 상태, 통신 품질, 위치추정 정확도, 배터리 상태, AI 동작, 시스템 성능 등을 지속적으로 감시하며 위험 요소를 분석한다.

최상위 계층에서는 위험 평가(Risk Assessment), 사용자 권한(Authorization), 사이버보안(Cybersecurity), 규제 준수(Regulatory Compliance)까지 포함한 통합 안전 관리를 수행한다.

위험 평가(Risk Assessment)는 안전 아키텍처의 핵심이다.

로봇이 어떤 행동을 수행하기 전에 충돌 가능성, 사람과의 거리, 이동 속도, 적재 중량(Payload), 정지 거리(Stopping Distance), 센서 신뢰도(Sensor Confidence), 통신 상태, 배터리 상태 등을 종합적으로 평가한다.

안전 수준은 고정되어 있는 것이 아니라 환경 변화에 따라 실시간으로 계속 변경된다.

위험 요소(Hazard)의 탐지는 인식 단계에서 시작된다.

카메라(Camera), 라이다(LiDAR), 레이더(Radar), 초음파(Ultrasonic Sensor), 깊이 카메라(Depth Camera), 힘 센서(Force Sensor), IMU, GNSS 등을 이용하여 사람, 장애물, 위험 지역, 바닥 상태, 장비 위치 등을 지속적으로 감시한다.

단순히 장애물을 찾는 것이 아니라 사람의 위치와 이동 방향, 작업 환경의 위험 요소까지 함께 분석하는 것이 중요하다.

센서 이중화(Sensor Redundancy)는 안전성을 크게 향상시킨다.

예를 들어 사람을 감지할 때 LiDAR, 깊이 카메라, 안전 스캐너(Safety Scanner), 힘 센서를 동시에 사용하면 하나의 센서가 고장 나더라도 나머지 센서가 안전 기능을 유지할 수 있다.

이러한 다중 센서 구조는 단일 장애점(Single Point of Failure)을 줄이는 핵심 기술이다.

위치추정(Localization)의 신뢰도도 안전과 직접 연결된다.

GNSS 신호가 약하거나 SLAM의 정확도가 낮아지면 위치 오차가 증가할 수 있다.

이 경우 소프트웨어는 자동으로 이동 속도를 줄이고 안전 거리를 확대하며, 대체 위치추정(Localization Backup)을 수행하거나 운영자에게 개입을 요청한다.

즉 위치 정확도에 따라 안전 정책도 함께 변경된다.

경로 계획(Path Planning) 역시 안전을 고려해야 한다.

최단 거리만 계산하는 것이 아니라 사람이 많은 지역, 제한 구역(Restricted Area), 정지 거리, 속도 제한, 차량의 동역학(Dynamics) 등을 함께 고려하여 안전한 경로를 생성한다.

생산성과 안전을 동시에 최적화하는 것이 현대 내비게이션의 핵심 목표이다.

행동 트리(Behavior Tree)는 안전 기능을 통합하기에 매우 적합하다.

안전 노드(Safety Node)가 항상 먼저 실행되어 현재 상태를 확인한 후에만 다음 행동이 수행된다.

비상 정지, 위치추정 오류, 센서 고장, 통신 장애, 배터리 부족 등이 발생하면 현재 작업을 즉시 중단하고 복구 행동(Recovery Behavior)으로 전환한다.

상태 머신(State Machine)도 안전 관리에 널리 사용된다.

초기화(Initialization), 대기(Standby), 자율 운행(Autonomous Operation), 수동 제어(Manual Mode), 협업 모드(Collaborative Mode), 비상 정지(Emergency Stop), 유지보수(Maintenance Mode), 종료(Shutdown)와 같은 상태를 정의하고, 각 상태에서 허용되는 동작을 명확하게 구분한다.

이를 통해 잘못된 상태 전이를 방지할 수 있다.

실시간 제어(Real-Time Control)는 안전성이 가장 중요한 영역이다.

모터 속도(Velocity), 가속도(Acceleration), 토크(Torque), 전류(Current), 온도(Temperature), 엔코더(Encoder) 상태를 지속적으로 감시하며 허용 범위를 벗어나면 즉시 제어 명령을 차단한다.

실시간 제어는 반드시 결정론적(Deterministic)으로 동작해야 한다.

협동로봇에서는 **속도 및 거리 감시(Speed and Separation Monitoring)** 가 핵심 안전 기능이다.

사람과의 거리를 실시간으로 계산하여 사람이 가까워질수록 로봇 속도를 자동으로 줄이고, 최소 안전 거리를 침범하면 즉시 정지한다.

이를 통해 생산성과 안전성을 동시에 확보할 수 있다.

또 다른 핵심 기능은 **힘 및 출력 제한(Power and Force Limitation)** 이다.

협동로봇은 모터 토크, 접촉력(Contact Force), 충돌 에너지(Collision Energy)를 지속적으로 계산하여 사람이 다치지 않는 범위 내에서만 동작하도록 제어한다.

즉 물리적인 접촉이 발생해도 안전을 유지할 수 있도록 설계된다.

**안전 정지(Safety-Rated Monitored Stop)** 도 중요한 기능이다.

사람이 작업 공간 안으로 들어오면 로봇은 움직임만 멈추고 시스템은 계속 동작한다.

작업자가 안전하게 벗어나면 다시 초기화 없이 자동으로 작업을 재개할 수 있다.

이는 생산성을 높이는 중요한 기능이다.

**비상 정지(Emergency Stop)** 는 최후의 안전 장치이다.

비상 정지 신호가 들어오면 현재 수행 중인 모든 작업보다 우선하여 즉시 동작을 중단한다.

하드웨어는 전원을 차단하고, 소프트웨어는 로그(Log)를 저장하며, 경고를 출력하고, 복구 절차를 준비한다.

하드웨어와 소프트웨어가 함께 동작하는 구조이다.

고장 감지(Fault Detection)는 안전 아키텍처의 중요한 기능이다.

센서 이상, 통신 오류, CPU 과부하, 메모리 부족, 저장 공간 부족, 과열(Thermal Overload), 배터리 이상 등을 지속적으로 감시한다.

문제가 심각해지기 전에 조기에 발견하여 사고를 예방하는 것이 목적이다.

장애 허용(Fault Tolerance)은 일부 장치가 고장 나더라도 시스템이 안전하게 계속 동작하도록 한다.

예를 들어 센서가 고장 나면 다른 센서를 사용하고, 통신이 끊기면 대체 통신을 사용하며, 계산 장치가 실패하면 백업 시스템으로 전환한다.

필요하면 성능을 낮춘 상태(Degraded Mode)로 안전하게 운영할 수도 있다.

워치독(Watchdog)은 소프트웨어가 정상적으로 실행되는지 지속적으로 확인한다.

프로그램이 멈추거나 응답하지 않으면 자동으로 재시작하거나 안전 모드로 전환하여 시스템 전체의 안정성을 유지한다.

최근에는 **사이버보안(Cybersecurity)** 도 안전의 일부가 되었다.

해킹(Hacking), 위조된 센서 데이터(Spoofing), 서비스 거부 공격(Denial of Service), AI 모델 변조 등은 실제 물리적 사고를 유발할 수 있다.

따라서 인증(Authentication), 암호화(Encryption), 보안 부팅(Secure Boot), 접근 제어(Access Control), 침입 탐지(Intrusion Detection)가 필수적으로 적용되어야 한다.

인공지능(AI)은 새로운 안전 문제를 가져온다.

AI 모델은 확률적으로 판단하기 때문에 항상 올바른 결과를 보장하지 않는다.

객체를 잘못 인식하거나 명령을 잘못 이해할 가능성이 있으므로 AI의 결과를 직접 실행하지 않고 결정론적 안전 감독기에서 다시 검증해야 한다.

AI는 안전 시스템을 보조하지만 최종 안전 결정은 독립적인 안전 계층이 담당한다.

설명 가능한 AI(Explainable AI, XAI)는 안전성을 향상시킨다.

로봇은 왜 특정 행동을 선택했는지, 왜 속도를 줄였는지, 왜 작업을 거부했는지를 사용자에게 설명할 수 있어야 한다.

이는 신뢰성을 높이고 사고 분석과 유지보수에도 도움이 된다.

HRI(Human-Robot Interaction)도 안전과 밀접하게 연결된다.

로봇은 사람의 위치, 자세, 의도, 권한을 고려하여 행동을 결정하며, 작업자와 방문자에게 서로 다른 안전 정책을 적용할 수 있다.

사람 중심의 안전 설계가 중요하다.

사용자 인터페이스(UI)는 현재 상태와 위험 정보를 명확하게 전달해야 한다.

화면(Display), 음성 안내(Voice Guidance), LED, 경고음, 모바일 알림, 증강현실(AR) 등을 이용하여 로봇의 현재 상태와 다음 행동을 사용자에게 이해하기 쉽게 알려준다.

클라우드 기반 로봇에서도 안전은 반드시 로컬(Local)에서 처리되어야 한다.

클라우드는 데이터 분석과 예측 유지보수(Predictive Maintenance)를 수행하지만, 비상 정지나 충돌 방지는 네트워크 상태와 관계없이 로컬 제어기에서 즉시 실행되어야 한다.

플릿(Fleet) 환경에서는 여러 대의 로봇을 동시에 안전하게 관리해야 한다.

교통 관리(Traffic Management), 충전 관리(Charging Management), 충돌 방지(Collision Avoidance), 공동 작업 공간 관리 등을 중앙에서 조정하여 전체 시스템의 안전성을 확보한다.

디지털 트윈(Digital Twin)은 안전 검증에도 매우 중요하다.

실제 장비를 사용하지 않고도 센서 고장, 통신 장애, 충돌 상황, 비상 정지 등을 가상 환경에서 반복적으로 시험할 수 있다.

이를 통해 위험한 상황을 미리 검증하고 안전 알고리즘을 개선할 수 있다.

시뮬레이션(Simulation)과 **하드웨어 인 더 루프(Hardware-in-the-Loop, HIL)** 시험도 필수적이다.

비상 정지, 장애 복구, 통신 오류, 센서 실패 등을 수천 번 반복 시험하여 실제 환경에서 안전하게 동작하는지를 검증한다.

국제 안전 표준은 소프트웨어 아키텍처 설계의 중요한 기준이다.

**ISO 10218** 은 산업용 로봇 안전을, **ISO/TS 15066** 은 협동로봇 안전을, **IEC 61508** 은 기능 안전(Functional Safety)을, **ISO 13849** 와 **IEC 62061** 은 안전 제어 시스템을 정의한다.

AMR은 **ISO 3691-4** 와 같은 이동 로봇 안전 표준도 함께 고려해야 한다.

안전은 개발이 끝난 후 적용하는 기능이 아니라 제품의 전체 생애주기(Lifecycle) 동안 관리되어야 한다.

로그 관리(Log Management), 사고 분석(Incident Analysis), 소프트웨어 업데이트(Update), AI 모델 검증(Model Validation), 예측 유지보수, 운영자 교육(Operator Training)을 지속적으로 수행해야 한다.

미래의 **피지컬 AI(Physical AI)** 시대에는 파운데이션 모델(Foundation Model), 비전-언어-행동 모델(Vision-Language-Action, VLA), 월드 모델(World Model), 강화학습(Reinforcement Learning)이 더욱 발전하겠지만, 실제 물리적인 안전은 여전히 결정론적 실시간 제어기(Real-Time Controller), 안전 감독기(Safety Supervisor), 이중화 시스템(Redundancy)이 담당하게 된다.

AI가 아무리 발전해도 안전은 독립적이고 검증 가능한 구조를 유지해야 한다.

또한 **소프트웨어 정의 로봇(Software-Defined Robot)** 시대에는 OTA 업데이트(Over-the-Air Update), AI 모델 교체, 클라우드 기반 안전 분석, 디지털 트윈 검증, 플릿 전체 안전 정책 관리가 지속적으로 이루어질 것이다.

안전 아키텍처 역시 운영 중에도 계속 진화하는 구조가 될 것이다.

결론적으로 **소프트웨어 아키텍처에 안전 기능 통합(Integration of Safety Functions into Software Architecture)** 은 현대 자율 로봇에서 가장 중요한 설계 원칙 가운데 하나이다. 안전은 더 이상 비상 정지나 하드웨어 보호장치에만 의존하는 기능이 아니라, 인식(Perception), 위치추정(Localization), 경로 계획(Planning), 인공지능(AI), 행동 제어(Behavior Control), 통신(Communication), 클라우드(Cloud), 플릿 관리(Fleet Management), 인간-로봇 상호작용(HRI)을 모두 포함하는 통합 아키텍처로 발전하고 있다. 지속적인 위험 평가(Risk Assessment), 장애 허용(Fault Tolerance), 센서 이중화(Sensor Redundancy), 사이버보안(Cybersecurity), 설명 가능한 AI(XAI), 국제 안전 표준(International Safety Standards)을 기반으로 한 안전 통합 소프트웨어는 앞으로 **협동로봇(Cobot)**, **AMR**, **휴머노이드(Humanoid)**, **자율주행차(Autonomous Vehicle)**, 그리고 **피지컬 AI(Physical AI)** 시대에 인간과 로봇이 신뢰성 있고 안전하게 협력하기 위한 가장 핵심적인 기반 기술이 될 것이다.
