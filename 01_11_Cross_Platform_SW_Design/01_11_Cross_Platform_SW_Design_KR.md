**Volume 1 Software Architecture Fundamentals**

# 11. Cross-Platform SW Design

## 11.01 Necessity and Scope of Cross-Platform Design

![](images/image1.png){width="7.268055555555556in" height="7.268055555555556in"}

소프트웨어 아키텍처(Software Architecture)에서 **크로스 플랫폼 설계(Cross-Platform Design)**는 현대 소프트웨어 공학과 로보틱스(Robotics) 분야에서 필수적인 요소가 되었다. 과거의 소프트웨어는 특정 운영체제나 특정 프로세서에서만 동작하도록 개발되는 경우가 많았지만, 오늘날의 지능형 시스템은 하나의 소프트웨어가 다양한 하드웨어와 운영체제에서 동일한 기능을 수행해야 한다. 특히 자율주행 로봇(Autonomous Robot), 임베디드 시스템(Embedded System), 클라우드(Cloud), 엣지 컴퓨팅(Edge Computing)이 함께 동작하는 환경에서는 플랫폼 독립성(Platform Independence)이 시스템의 경쟁력을 결정하는 핵심 요소가 된다.

전통적인 소프트웨어 개발 방식에서는 응용 프로그램(Application)이 특정 하드웨어와 매우 강하게 결합(Coupling)되어 있었다. 운영체제 서비스, 장치 드라이버(Device Driver), 메모리 관리(Memory Management), 인터럽트 처리(Interrupt Handling), 통신 인터페이스 등이 프로그램 내부에 직접 구현되었기 때문에 새로운 하드웨어로 이전하려면 대부분의 코드를 다시 수정해야 했다. 이러한 구조는 초기에는 높은 성능을 제공할 수 있었지만, 장기적으로는 유지보수 비용과 기술 부채(Technical Debt)를 급격하게 증가시키는 원인이 되었다.

임베디드 리눅스(Embedded Linux), 가상화(Virtualization), 컨테이너(Container), 클라우드 컴퓨팅(Cloud Computing), 그리고 하드웨어 추상화(Hardware Abstraction) 기술이 발전하면서 소프트웨어 개발 방식도 크게 변화하였다. 현대의 아키텍처는 특정 플랫폼에 종속되는 구조를 지양하고, 공통 인터페이스(Common Interface)와 추상화 계층(Abstraction Layer)을 중심으로 설계된다. 이를 통해 알고리즘과 비즈니스 로직(Business Logic)은 변경하지 않고도 새로운 하드웨어나 운영체제로 손쉽게 이전할 수 있다.

로봇 시스템에서는 이러한 필요성이 더욱 크다. 하나의 자율주행 로봇은 실시간 제어를 수행하는 마이크로컨트롤러(Microcontroller), 센서 처리를 담당하는 ARM 프로세서, 인공지능 추론(AI Inference)을 수행하는 GPU, 미션 계획(Mission Planning)을 담당하는 산업용 x86 컴퓨터, 그리고 플릿(Fleet)을 관리하는 클라우드 서버가 동시에 동작한다. 각각의 컴퓨팅 환경은 프로세서 구조, 운영체제, 메모리 구성, 실시간성, 통신 방식 등이 모두 다르므로 이를 하나의 일관된 소프트웨어 구조로 통합하는 것이 매우 중요하다.

크로스 플랫폼 설계는 단순히 동일한 소스 코드를 여러 운영체제에서 컴파일하는 기술만을 의미하지 않는다. 이는 다양한 하드웨어와 운영환경에서 동일한 기능을 제공하면서도 성능, 신뢰성(Reliability), 안전성(Safety), 유지보수성(Maintainability)을 동시에 확보하기 위한 전체적인 아키텍처 전략을 의미한다. 즉, 공통 기능은 최대한 재사용하고, 플랫폼에 따라 달라지는 부분만 최소한으로 분리하는 것이 핵심 목표이다.

가장 기본적인 범위는 프로세서 이식성(Processor Portability)이다. 현대 시스템에서는 ARM Cortex-M, ARM Cortex-A, x86, RISC-V, DSP(Digital Signal Processor), FPGA(Field Programmable Gate Array), AI 가속기(AI Accelerator) 등이 함께 사용된다. 이들은 명령어 집합(Instruction Set), 메모리 구조, 캐시(Cache), 인터럽트 처리 방식이 모두 다르다. 따라서 응용 소프트웨어는 이러한 차이를 직접 다루지 않고 공통 인터페이스를 통해 동작하도록 설계되어야 한다.

운영체제 이식성(Operating System Portability) 역시 중요한 범위에 포함된다. 하나의 프로젝트에서도 FreeRTOS, Zephyr, 임베디드 리눅스(Embedded Linux), PREEMPT_RT Linux, Ubuntu, Windows, Android, QNX 등이 혼합되어 사용될 수 있다. 운영체제마다 스케줄링(Scheduling), 동기화(Synchronization), 네트워크(Network), 파일 시스템(File System), 타이머(Timer) 등이 서로 다르기 때문에 상위 응용 프로그램은 이러한 차이를 인식하지 않도록 설계하는 것이 바람직하다.

미들웨어(Middleware)의 이식성도 중요한 요소이다. 최근의 로봇 시스템은 ROS 2, DDS(Data Distribution Service), MQTT(Message Queuing Telemetry Transport), gRPC, REST API, WebSocket 등 다양한 통신 기술을 사용한다. 프로젝트가 발전하면서 새로운 미들웨어를 도입할 수도 있기 때문에, 응용 프로그램은 특정 통신 기술에 의존하지 않고 추상화된 인터페이스를 통해 데이터를 주고받을 수 있도록 설계되어야 한다.

하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)은 크로스 플랫폼 설계의 핵심 기술 가운데 하나이다. 응용 프로그램이 GPIO, UART, SPI, CAN, 카메라(Camera), 라이다(LiDAR), IMU(Inertial Measurement Unit), 모터 드라이버(Motor Driver)를 직접 제어하지 않고 기능(Function) 중심의 인터페이스만 호출하도록 만든다. 따라서 실제 장비가 변경되더라도 상위 응용 프로그램은 수정하지 않고 그대로 사용할 수 있으며, 시뮬레이션(Simulation) 환경에서도 동일한 인터페이스를 활용할 수 있다.

개발 환경(Development Environment)의 독립성도 중요한 범위이다. 개발자는 Windows에서 코드를 작성하고, 리눅스 서버에서 지속적 통합(CI, Continuous Integration)을 수행하며, 임베디드 리눅스에서 실제 로봇을 실행하고, 클라우드에서 대규모 시뮬레이션을 수행하는 경우가 많다. 이러한 다양한 개발 환경에서도 동일한 소스 코드와 동일한 빌드(Build) 절차가 유지될 수 있어야 한다.

시뮬레이션과 실제 시스템의 호환성(Simulation Compatibility) 역시 중요한 설계 목표이다. 현대 로봇 개발에서는 디지털 트윈(Digital Twin), 소프트웨어 인 더 루프(SIL, Software-in-the-Loop), 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop), 물리 기반 시뮬레이션(Physics-Based Simulation)이 적극적으로 활용된다. 이상적인 구조에서는 동일한 소프트웨어 모듈이 시뮬레이터와 실제 로봇에서 거의 수정 없이 실행되어야 하며, 이를 통해 개발 기간과 검증 비용을 크게 줄일 수 있다.

인공지능(AI)의 확산은 크로스 플랫폼 설계의 중요성을 더욱 높이고 있다. AI 모델은 GPU, CPU, NPU(Neural Processing Unit), TPU(Tensor Processing Unit), FPGA 등 다양한 연산 장치에서 실행될 수 있으며, TensorRT, ONNX Runtime, OpenVINO와 같은 여러 추론 엔진(Inference Engine)이 존재한다. 따라서 응용 프로그램은 AI 모델 자체가 아니라 추론 인터페이스(Inference Interface)를 호출하도록 설계하여 하드웨어가 변경되어도 동일한 기능을 유지해야 한다.

성능 최적화(Performance Optimization)와 이식성은 서로 상충되는 개념으로 생각되기 쉽지만 실제로는 그렇지 않다. 대부분의 알고리즘은 플랫폼과 무관하게 공통 코드로 유지하고, 계산량이 많은 일부 핵심 연산만 플랫폼별 최적화 코드로 분리하면 된다. 이러한 계층적 최적화(Layered Optimization)는 코드 재사용성을 유지하면서도 높은 실행 성능을 확보할 수 있는 대표적인 방법이다.

크로스 플랫폼 설계는 소프트웨어 품질(Quality Assurance) 향상에도 크게 기여한다. 플랫폼마다 별도의 코드를 유지하면 기능 차이와 버그가 지속적으로 증가하지만, 공통 코드 기반(Common Code Base)을 유지하면 하나의 수정 사항이 모든 플랫폼에 동시에 적용된다. 또한 자동화된 회귀 테스트(Regression Test)와 통합 테스트(Integration Test)를 통해 동일한 동작을 지속적으로 검증할 수 있다.

유지보수성(Maintainability)은 크로스 플랫폼 설계를 도입하는 가장 큰 이유 가운데 하나이다. 산업용 로봇은 일반적으로 10년 이상 운영되며 그동안 프로세서, 운영체제, GPU, 통신 기술은 여러 차례 교체된다. 플랫폼 의존성이 높은 소프트웨어는 하드웨어가 바뀔 때마다 대규모 수정이 필요하지만, 플랫폼 독립적인 구조는 플랫폼 계층만 교체하면 상위 응용 프로그램은 거의 수정하지 않아도 된다.

확장성(Scalability) 역시 중요한 장점이다. 대부분의 기업은 하나의 로봇만 개발하지 않는다. 자율주행 이동로봇(AMR), 매니퓰레이터(Manipulator), 사족보행 로봇(Quadruped), 휴머노이드(Humanoid), 검사 로봇(Inspection Robot) 등 다양한 제품군을 개발한다. 이들 제품은 하드웨어는 다르지만 위치 추정(Localization), 경로 계획(Path Planning), 센서 처리(Sensor Processing), 플릿 관리(Fleet Management), 인공지능 추론과 같은 핵심 소프트웨어는 대부분 공통으로 사용할 수 있다.

크로스 플랫폼 설계의 범위는 단순한 코드 이식성을 넘어선다. 빌드 시스템(Build System), 의존성 관리(Dependency Management), 자동화된 빌드(Build Automation), 지속적 통합 및 배포(CI/CD), 패키지 관리(Package Management), 문서화(Document), 코딩 규칙(Coding Standard), 버전 관리(Version Management), 인터페이스 호환성 관리까지 모두 포함하는 종합적인 소프트웨어 엔지니어링 체계를 의미한다.

안전 필수 시스템(Safety-Critical System)에서는 플랫폼과 관계없이 동일한 안전 기능(Safety Function)이 보장되어야 한다. 비상 정지(Emergency Stop), 감시 기능(Monitoring), 장애 진단(Diagnostics), 워치독(Watchdog), 이중화(Redundancy), 장애 복구(Recovery)는 어떤 프로세서와 운영체제에서도 동일하게 동작해야 한다. 이를 위해 명확한 인터페이스 정의와 반복 가능한 검증 절차가 반드시 필요하다.

보안(Security) 또한 크로스 플랫폼 설계에서 매우 중요한 영역이다. 인증(Authentication), 암호화(Encryption), 보안 부팅(Secure Boot), 펌웨어 무결성(Firmware Integrity), 접근 제어(Access Control), OTA(Over-the-Air) 업데이트는 모든 플랫폼에서 동일한 보안 정책을 유지해야 한다. 특정 플랫폼만 보안 수준이 낮아지면 전체 시스템의 신뢰성이 크게 저하될 수 있다.

또 다른 중요한 목적은 소프트웨어 자산(Software Asset)의 보호이다. 기업은 위치 추정, 인식(Perception), 경로 계획, 제어 알고리즘, AI 모델 개발에 막대한 비용을 투자한다. 하드웨어는 수년마다 변경되지만 이러한 핵심 알고리즘은 오랫동안 재사용되어야 한다. 크로스 플랫폼 설계는 플랫폼 변화에도 기업의 핵심 기술과 지식재산(IP, Intellectual Property)을 지속적으로 활용할 수 있도록 지원한다.

크로스 플랫폼 아키텍처는 대규모 조직의 협업에도 매우 적합하다. 펌웨어(Firmware), 로봇 소프트웨어, 클라우드, AI, 시뮬레이션, 안전 소프트웨어를 각각 다른 팀이 개발하더라도 표준 인터페이스(Standard Interface)를 공유하면 독립적인 개발이 가능하며, 전체 시스템의 통합도 훨씬 수월해진다. 이는 개발 생산성을 높이고 프로젝트의 확장성을 향상시키는 중요한 기반이 된다.

궁극적으로 크로스 플랫폼 설계는 단순한 기술이 아니라 장기적인 소프트웨어 아키텍처 철학(Architecture Philosophy)이다. 이는 특정 하드웨어나 운영체제에 종속되지 않는 지속 가능한 소프트웨어를 구축하도록 지원하며, 변화하는 컴퓨팅 환경에서도 높은 재사용성(Reusability), 유지보수성, 확장성, 안전성, 보안성을 동시에 확보할 수 있게 한다. 앞으로의 **물리 AI(Physical AI)**와 **차세대 로봇(Next-Generation Robotics)** 시대에서는 크로스 플랫폼 설계가 미래 지향적 소프트웨어 아키텍처의 핵심 기반 기술로 자리매김할 것이다.

## 11.02 HAL Design Patterns [w/Code]

![](images/image2.png){width="7.268055555555556in" height="7.268055555555556in"}

**하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)**은 현대의 임베디드 시스템(Embedded System), 로봇(Robotics), 산업 자동화(Industrial Automation), 그리고 **물리 AI(Physical AI)** 플랫폼에서 가장 중요한 아키텍처 요소 가운데 하나이다. 다양한 프로세서, 운영체제, 센서, 액추에이터(Actuator), 통신 장치가 함께 사용되는 환경에서는 응용 소프트웨어(Application Software)가 하드웨어에 직접 의존하지 않도록 만드는 것이 필수적이다. HAL은 이러한 문제를 해결하기 위해 하드웨어와 응용 소프트웨어 사이에 표준 인터페이스(Standard Interface)를 제공하는 계층이다.

HAL의 가장 중요한 목적은 **관심사의 분리(Separation of Concerns)**이다. 실제 하드웨어는 제품 수명 동안 부품 변경, 공급망 문제, 성능 향상, 비용 절감 등의 이유로 계속 변경된다. 그러나 상위의 인공지능(AI), 자율주행(Autonomous Navigation), 경로 계획(Path Planning), 플릿 관리(Fleet Management)와 같은 응용 소프트웨어는 가능한 한 변경되지 않아야 한다. HAL은 하드웨어 변화가 상위 소프트웨어에 영향을 주지 않도록 차단하는 역할을 수행한다.

과거의 임베디드 소프트웨어는 하드웨어와 매우 강하게 결합(Coupling)되어 있었다. 응용 프로그램 내부에서 레지스터(Register)를 직접 접근하고, 인터럽트(Interrupt)를 처리하며, 통신 프로토콜과 장치 초기화를 함께 구현하는 경우가 많았다. 이러한 방식은 새로운 하드웨어가 등장할 때마다 프로그램 전체를 수정해야 했으며, 유지보수 비용과 기술 부채(Technical Debt)를 지속적으로 증가시키는 원인이 되었다.

HAL은 단순히 장치 드라이버(Device Driver)를 감싸는 래퍼(Wrapper)가 아니다. 이는 소프트웨어 계층 간의 계약(Architectural Contract)을 정의하는 아키텍처이다. 예를 들어 영상 처리 소프트웨어는 카메라 센서의 레지스터를 직접 제어하지 않고 "영상(Frame)을 가져온다"는 기능만 호출한다. 모터 제어 소프트웨어 역시 PWM(Pulse Width Modulation)이나 CAN 통신을 직접 제어하는 것이 아니라 "속도를 설정한다" 또는 "토크를 설정한다"와 같은 기능만 사용한다.

효율적인 HAL은 일반적으로 여러 개의 계층으로 구성된다. 가장 아래 계층은 실제 하드웨어와 직접 통신하는 장치 드라이버(Device Driver)이며, 그 위에는 장치별 구현을 공통 인터페이스로 변환하는 적응 계층(Adapter Layer)이 위치한다. 최상위에는 응용 프로그램이 사용하는 서비스 인터페이스(Service Interface)가 존재한다. 이러한 계층 구조는 하드웨어 의존성을 점진적으로 제거하여 소프트웨어 재사용성을 크게 향상시킨다.

HAL 설계에서 가장 중요한 요소 가운데 하나는 **인터페이스 안정성(Interface Stability)**이다. 인터페이스는 하드웨어 구현 방식이 아니라 기능(Function)을 중심으로 설계되어야 한다. 예를 들어 카메라 인터페이스는 초기화, 프레임 획득(Frame Acquisition), 설정(Configuration), 동기화(Synchronization), 진단(Diagnostics) 기능만 제공하고, 실제 USB, Ethernet, MIPI CSI 등의 차이는 내부 구현에서 처리해야 한다.

객체지향(Object-Oriented Programming)은 HAL 구현에 매우 적합한 방식이다. 추상 클래스(Abstract Class)나 인터페이스(Interface)를 이용하여 공통 기능을 정의하고, 실제 하드웨어마다 이를 상속(Inheritance)하여 구현한다. 응용 프로그램은 실제 장치가 아니라 추상 인터페이스만 참조하므로 하드웨어 변경 시 응용 프로그램을 수정할 필요가 거의 없다.

**의존성 역전 원칙(Dependency Inversion Principle, DIP)** 역시 HAL의 핵심 원칙이다. 상위 응용 프로그램은 하위 장치 드라이버를 직접 참조하지 않고, 두 계층 모두 공통 추상 인터페이스에 의존하도록 설계한다. 이러한 구조는 모듈성(Modularity), 테스트 용이성(Testability), 유지보수성을 크게 향상시키며 장기적으로 기술 부채를 감소시킨다.

HAL은 추상화 수준과 성능 사이의 균형도 고려해야 한다. 지나친 추상화는 함수 호출(Function Call), 메모리 사용량, 실행 지연(Latency)을 증가시킬 수 있으며, 반대로 추상화가 부족하면 하드웨어 의존성이 응용 프로그램 전체로 확산된다. 따라서 공통 기능은 추상화하고, 성능이 중요한 연산은 플랫폼별 최적화(Platform Optimization)를 적용하는 계층적 구조가 가장 이상적이다.

실시간 제어(Real-Time Control)가 필요한 로봇에서는 HAL도 결정성(Determinism)을 유지해야 한다. 모터 제어, 엔코더(Encoder), 센서 동기화, 비상 정지(Emergency Stop)와 같은 기능에서는 동적 메모리 할당(Dynamic Allocation), 과도한 가상 함수(Virtual Function), 블로킹(Blocking) 동기화 기법을 최소화하여 일정한 응답 시간을 보장해야 한다.

HAL의 가장 큰 장점 가운데 하나는 **프로세서 이식성(Processor Portability)**이다. 동일한 소프트웨어가 ARM Cortex-M, ARM Cortex-A, x86, RISC-V, GPU 플랫폼에서 실행될 수 있도록 프로세서별 차이를 추상화한다. 프로세서가 변경되어도 응용 프로그램은 수정하지 않고, HAL 내부 구현만 변경하면 새로운 플랫폼을 지원할 수 있다.

운영체제 추상화(Operating System Abstraction)도 HAL의 중요한 역할이다. FreeRTOS, Zephyr, Embedded Linux, PREEMPT_RT Linux, Ubuntu, Windows, Android, QNX 등은 모두 스레드(Thread), 타이머(Timer), 파일 시스템(File System), 네트워크(Network), 동기화(Synchronization) 방식이 다르다. HAL은 이러한 차이를 하나의 공통 인터페이스로 통합하여 응용 프로그램이 운영체제에 의존하지 않도록 만든다.

통신 추상화(Communication Abstraction)는 CAN, CAN FD, EtherCAT, Modbus, Ethernet/IP, OPC UA, DDS(Data Distribution Service), MQTT(Message Queuing Telemetry Transport), REST API, WebSocket 등 다양한 통신 기술을 하나의 공통 인터페이스로 제공한다. 응용 프로그램은 데이터 송수신 기능만 호출하고 실제 통신 방식은 HAL이 내부적으로 처리한다.

센서 추상화(Sensor Abstraction)는 HAL의 대표적인 활용 사례이다. 카메라(Camera), 라이다(LiDAR), 레이더(Radar), IMU(Inertial Measurement Unit), GNSS(Global Navigation Satellite System), 엔코더, 초음파(Ultrasonic) 센서는 모두 인터페이스와 데이터 형식이 다르다. HAL은 이러한 다양한 센서를 공통 데이터 구조와 표준 API(Application Programming Interface)로 변환하여 상위의 인식(Perception) 알고리즘이 동일한 방식으로 사용할 수 있도록 한다.

액추에이터 추상화(Actuator Abstraction) 역시 매우 중요하다. 이동로봇, 산업용 로봇, 협동로봇(Collaborative Robot), 휴머노이드(Humanoid)는 서로 다른 제조사의 모터와 드라이버를 사용한다. HAL은 위치(Position), 속도(Velocity), 토크(Torque), 운전 모드(Operation Mode), 진단 정보(Diagnostics)와 같은 공통 인터페이스를 제공하여 제어 소프트웨어가 제조사에 종속되지 않도록 만든다.

HAL은 시뮬레이션(Simulation)과 실제 시스템의 통합에도 핵심적인 역할을 한다. 소프트웨어 인 더 루프(SIL, Software-in-the-Loop), 하드웨어 인 더 루프(HIL, Hardware-in-the-Loop), 디지털 트윈(Digital Twin) 환경에서는 실제 센서 대신 가상 센서(Virtual Sensor)를 HAL 아래에 연결할 수 있다. 응용 프로그램은 실제 장비인지 시뮬레이션인지 구분하지 못하므로 동일한 코드를 그대로 사용할 수 있다.

인공지능(AI) 추론도 HAL을 통해 추상화할 수 있다. TensorRT, ONNX Runtime, OpenVINO, CUDA, CPU, GPU, NPU(Neural Processing Unit), TPU(Tensor Processing Unit)는 모두 서로 다른 실행 환경을 제공하지만, HAL은 공통 추론 인터페이스(Inference Interface)를 정의하여 응용 프로그램이 실행 환경에 관계없이 동일한 방식으로 AI 모델을 사용할 수 있도록 한다.

HAL은 자원 관리(Resource Management)도 담당한다. 메모리(Memory), 장치 초기화(Device Initialization), 전원 관리(Power Management), 하드웨어 리셋(Hardware Reset), 장애 복구(Fault Recovery), 시스템 종료(Shutdown)를 중앙에서 관리함으로써 전체 시스템의 일관성과 안정성을 향상시킨다.

오류 처리(Error Handling) 역시 HAL의 중요한 기능이다. 센서 고장, 통신 오류, 과열(Overheating), 전원 이상, 보정(Calibration) 실패 등이 발생하면 장치마다 다른 오류 코드를 사용하는 대신 HAL이 이를 공통 진단 모델(Diagnostic Model)로 변환한다. 상위 응용 프로그램은 제조사별 오류를 이해할 필요 없이 동일한 방식으로 장애를 처리할 수 있다.

구성 관리(Configuration Management)는 하드웨어별 차이를 외부 설정 파일(Configuration File)로 관리하는 방법이다. 통신 주소(Address), 보정값(Calibration Parameter), 타이밍(Timing), 안전 한계(Safety Limit)를 코드에 직접 작성하지 않고 YAML, JSON, XML과 같은 설정 파일에 저장하면 동일한 프로그램으로 여러 하드웨어를 지원할 수 있다.

HAL은 제품군(Product Family)의 확장성(Scalability)을 크게 향상시킨다. 기업은 하나의 로봇만 개발하는 것이 아니라 AMR(Autonomous Mobile Robot), 실외 자율주행 로봇, 검사 로봇, 협동로봇, 휴머노이드 등 다양한 플랫폼을 개발한다. 이들 제품은 하드웨어는 다르지만 위치 추정(Localization), 내비게이션(Navigation), 플릿 관리, AI 추론, 진단 기능은 대부분 공유할 수 있으므로 HAL을 통해 공통 소프트웨어 자산을 효과적으로 재사용할 수 있다.

테스트(Test) 역시 HAL을 통해 크게 개선된다. 실제 장비 대신 모의 객체(Mock Object), 가상 장치(Virtual Device), 시뮬레이터(Simulator)를 사용할 수 있으므로 단위 테스트(Unit Test), 통합 테스트(Integration Test), 자동 회귀 테스트(Regression Test)를 실물 장비 없이도 수행할 수 있다. 이는 지속적 통합(CI, Continuous Integration)과 자동화 테스트 환경 구축에 매우 큰 장점을 제공한다.

안전 필수 시스템(Safety-Critical System)에서는 HAL이 더욱 중요한 역할을 수행한다. 비상 정지, 워치독(Watchdog), 이중화(Redundancy), 안전 진단(Safety Diagnostics), 안전 종료(Safe Shutdown)는 플랫폼에 관계없이 항상 동일한 동작을 보장해야 한다. HAL은 이러한 안전 기능을 표준화하여 인증(Certification)과 검증(Verification)을 더욱 용이하게 만든다.

보안(Security)도 HAL 설계의 중요한 영역이다. 보안 부팅(Secure Boot), 암호화(Encryption), 신뢰 플랫폼 모듈(TPM, Trusted Platform Module), OTA(Over-the-Air) 업데이트, 인증(Authentication), 무결성 검증(Integrity Verification)은 플랫폼마다 구현 방식이 다르지만 HAL은 이를 공통 인터페이스로 제공하여 일관된 보안 정책을 유지하도록 지원한다.

HAL 구현에서는 여러 디자인 패턴(Design Pattern)이 함께 활용된다. 어댑터 패턴(Adapter Pattern)은 제조사별 인터페이스를 공통 인터페이스로 변환하고, 팩토리 패턴(Factory Pattern)은 플랫폼에 맞는 장치를 생성하며, 전략 패턴(Strategy Pattern)은 알고리즘을 선택적으로 변경한다. 브리지 패턴(Bridge Pattern)은 추상화와 구현을 분리하고, 프록시 패턴(Proxy Pattern)은 원격 장치를 동일한 방식으로 사용할 수 있도록 한다. 이러한 패턴을 조합하면 매우 유연하고 확장성이 높은 HAL을 구축할 수 있다.

현대의 소프트웨어 엔지니어링에서는 HAL이 인터페이스 정의 언어(IDL, Interface Definition Language), 서비스 지향 아키텍처(Service-Oriented Architecture), 자동 코드 생성(Code Generation), 지속적 통합(CI), 자동 배포(CD, Continuous Deployment)와 결합되어 활용되고 있다. 앞으로 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **AI 네이티브 아키텍처(AI-Native Architecture)**가 확산될수록 HAL은 더욱 중요한 기반 기술이 될 것이다.

궁극적으로 **하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)**은 단순한 프로그래밍 기법이 아니라 장기적인 소프트웨어 아키텍처 전략이다. 이는 하드웨어 교체와 기술 발전으로부터 핵심 소프트웨어 자산을 보호하고, 개발 기간 단축, 유지보수 비용 절감, 테스트 자동화, 플랫폼 확장성 확보, 그리고 차세대 **물리 AI(Physical AI)** 및 로봇 시스템 구축을 가능하게 하는 핵심 기반 기술이라 할 수 있다.

## 11.03 Platform Build Systems: CMake / Bazel / Cross-Compilation

![](images/image3.png){width="7.268055555555556in" height="7.268055555555556in"}

플랫폼 빌드 시스템(Platform Build System)은 단순히 소스 코드를 컴파일하는 도구가 아니라, 하나의 소프트웨어를 다양한 운영체제와 하드웨어에서 실행 가능한 형태로 자동 생성하는 전체 개발 인프라를 의미한다. **하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)**을 통해 플랫폼 의존성을 줄였다면, 빌드 시스템(Build System)은 동일한 소스 코드(Source Code)를 여러 플랫폼에 맞게 효율적으로 생성하는 역할을 담당한다. 따라서 크로스 플랫폼(Cross-Platform) 소프트웨어 개발에서는 HAL과 빌드 시스템이 서로 보완적인 관계를 형성한다.

현대의 로봇 소프트웨어는 단일 컴퓨터에서만 실행되지 않는다. 하나의 프로젝트 안에서도 마이크로컨트롤러(Microcontroller)용 펌웨어(Firmware), ARM 기반 임베디드 리눅스(Embedded Linux) 애플리케이션, Windows 기반 개발 도구, Ubuntu 시뮬레이터, 클라우드 서비스(Cloud Service), GPU 기반 AI 추론 프로그램이 동시에 존재한다. 이러한 다양한 실행 환경을 수동으로 관리하는 것은 현실적으로 매우 어렵기 때문에 플랫폼 독립적인 빌드 시스템이 반드시 필요하다.

빌드 시스템(Build System)은 컴파일(Compilation)만 수행하는 것이 아니다. 소스 코드 분석, 의존성 관리(Dependency Management), 컴파일러 실행, 링커(Linker) 설정, 테스트(Test), 패키징(Packaging), 설치(Installation), 문서 생성(Document Generation), 아티팩트 배포(Artifact Publishing), 지속적 통합(CI, Continuous Integration)과의 연계까지 모두 자동으로 수행하는 종합적인 자동화 시스템이다. 이러한 자동화는 개발 생산성과 유지보수성을 크게 향상시킨다.

과거에는 GNU Make와 수작업 Makefile이 가장 널리 사용되었다. 하지만 프로젝트 규모가 커질수록 플랫폼별 컴파일 옵션, 라이브러리 경로, 중복 규칙이 증가하면서 관리가 매우 어려워졌다. 특히 동일한 프로젝트를 Windows, Linux, ARM, x86에서 동시에 관리해야 하는 경우에는 Makefile 유지보수가 매우 복잡해졌고, 이러한 문제를 해결하기 위해 상위 수준의 빌드 시스템(Build System)이 등장하게 되었다.

**CMake(Cross Platform Make)**는 현재 가장 널리 사용되는 메타 빌드 시스템(Meta Build System)이다. CMake 자체가 컴파일러는 아니며, 프로젝트의 구조와 의존성을 기술하면 Linux에서는 Makefile이나 Ninja를 생성하고, Windows에서는 Visual Studio 프로젝트를 생성하며, macOS에서는 Xcode 프로젝트를 생성한다. 즉 하나의 CMake 설정 파일만으로 여러 개발 환경을 동시에 지원할 수 있다.

CMake는 특히 로보틱스(Robotics) 분야에서 사실상의 표준으로 자리 잡고 있다. ROS 2(Robot Operating System 2), OpenCV(Open Source Computer Vision Library), PCL(Point Cloud Library), Eigen, TensorRT 등 대부분의 오픈소스 로봇 라이브러리가 CMake를 기반으로 구축되어 있다. 따라서 자율주행 로봇, 산업용 로봇, 휴머노이드(Humanoid), 물리 AI(Physical AI)를 개발하는 기업에서는 CMake 사용이 매우 자연스러운 선택이 되고 있다.

CMake는 파일(File) 중심이 아니라 **타깃(Target)** 중심으로 프로젝트를 관리한다. 실행 파일(Executable), 정적 라이브러리(Static Library), 공유 라이브러리(Shared Library), 인터페이스 라이브러리(Interface Library)를 각각 하나의 독립적인 타깃으로 정의하고, 타깃 간 의존성만 선언하면 필요한 컴파일 옵션과 라이브러리가 자동으로 전달된다. 이러한 구조는 코드의 재사용성과 유지보수성을 크게 향상시킨다.

현대의 CMake는 선언형(Declarative) 방식으로 프로젝트를 정의한다. 개발자는 어떤 기능이 필요한지만 선언하고, 컴파일러 종류나 운영체제에 따라 필요한 세부 옵션은 CMake가 자동으로 처리한다. 따라서 설정 파일이 훨씬 간결해지고, 프로젝트가 성장하더라도 관리가 쉬워진다.

**Bazel(Bazel Build System)**은 Google이 초대형 소프트웨어 프로젝트를 위해 개발한 빌드 시스템이다. Bazel은 재현 가능성(Reproducibility), 확장성(Scalability), 결정성(Determinism), 원격 캐시(Remote Cache), 분산 빌드(Distributed Build)를 가장 중요한 목표로 한다. 수백만 줄 이상의 코드와 수천 개의 모듈을 관리하는 환경에서도 매우 빠른 빌드 성능을 제공한다.

Bazel의 가장 큰 특징은 **Hermetic Build**이다. 모든 빌드는 명시적으로 선언된 의존성만 사용할 수 있으며 운영체제에 설치된 라이브러리나 환경 변수(Environment Variable)에 의존하지 않는다. 따라서 어떤 개발자가 어디에서 빌드를 수행하더라도 항상 동일한 결과가 생성된다. 이러한 특성은 "내 컴퓨터에서는 동작하는데 다른 환경에서는 실패하는" 문제를 크게 줄여준다.

또한 Bazel은 매우 정교한 의존성 분석(Dependency Analysis)을 수행한다. 변경된 파일과 직접 관련된 모듈만 다시 컴파일하기 때문에 대규모 프로젝트에서도 빌드 시간이 매우 짧다. 원격 캐시(Remote Cache)와 분산 빌드 기능을 함께 사용하면 여러 개발자가 동일한 빌드 결과를 공유할 수 있어 개발 효율이 크게 향상된다.

최근 대형 로봇 기업에서도 Bazel 도입이 증가하고 있다. 자율주행, AI 추론, 클라우드 서비스, 시뮬레이션, 제어 소프트웨어를 하나의 거대한 저장소(Monorepo)에서 관리할 경우 Bazel의 확장성과 재현성이 매우 큰 장점을 제공한다. 다만 CMake보다 학습 난이도가 높고 아키텍처 설계가 더욱 엄격하게 요구된다.

CMake와 Bazel은 구현 방식은 다르지만 목표는 동일하다. 두 시스템 모두 수작업 빌드를 제거하고, 의존성을 자동 관리하며, 여러 플랫폼을 동시에 지원하고, 지속적 통합(CI) 및 자동 테스트(Test Automation)를 지원하는 것을 목표로 한다. 일반적인 로봇 프로젝트에서는 CMake가 더 많이 사용되며, 초대형 프로젝트에서는 Bazel이 선택되는 경우가 많다.

크로스 컴파일(Cross Compilation)은 현대 임베디드 소프트웨어 개발의 핵심 기술이다. 개발자는 일반적으로 x86 기반 Windows나 Linux 컴퓨터에서 개발하지만 실제 실행 대상(Target)은 ARM Cortex-M, ARM Cortex-A, NVIDIA Jetson, RISC-V와 같은 임베디드 플랫폼이다. 따라서 개발 환경과 실행 환경이 서로 다른 경우에는 반드시 크로스 컴파일이 필요하다.

크로스 컴파일에서는 **호스트(Host)**와 **타깃(Target)**을 명확히 구분한다. 호스트는 컴파일을 수행하는 컴퓨터이며, 타깃은 생성된 프로그램이 실제 실행되는 장치이다. 두 환경은 CPU 구조, 운영체제, 라이브러리, 메모리 구조가 모두 다를 수 있으므로 빌드 시스템은 이러한 차이를 정확하게 관리해야 한다.

이를 위해 **툴체인(Toolchain)**이 사용된다. 툴체인은 컴파일러(Compiler), 어셈블러(Assembler), 링커(Linker), 디버거(Debugger), 런타임 라이브러리(Runtime Library) 등을 하나로 묶은 개발 도구 집합이다. ARM GNU Toolchain, LLVM Clang, GCC, CUDA Toolkit 등이 대표적인 예이며, 빌드 시스템은 이러한 툴체인을 선택하여 목표 플랫폼에 맞는 실행 파일을 생성한다.

CMake에서는 **Toolchain File**을 이용하여 크로스 컴파일 환경을 정의한다. 이 파일에는 컴파일러 위치, 대상 프로세서, 시스템 루트(System Root), 라이브러리 검색 경로, 링커 옵션 등이 기록된다. 개발자는 동일한 프로젝트를 유지하면서 Toolchain File만 변경하면 Windows, Ubuntu, ARM Linux, Jetson 등 다양한 플랫폼으로 쉽게 빌드할 수 있다.

크로스 컴파일에서는 호스트에서 실행되는 프로그램과 타깃에서 실행되는 프로그램을 구분해야 한다. 예를 들어 코드 생성기(Code Generator), 인터페이스 생성기(Interface Generator), 프로토콜 컴파일러(Protocol Compiler)는 호스트에서 실행되지만, 생성된 결과물은 ARM이나 Jetson에서 실행된다. 빌드 시스템은 이러한 차이를 자동으로 관리해야 한다.

의존성 관리(Dependency Management)는 크로스 컴파일에서 더욱 중요하다. 컴파일에 필요한 라이브러리와 실행 시 필요한 라이브러리가 서로 다를 수 있으며, ABI(Application Binary Interface), SDK 버전, 헤더(Header), 바이너리(Binary)의 호환성을 지속적으로 유지해야 한다. 현대의 빌드 시스템은 이러한 의존성을 자동으로 분석하고 관리하는 기능을 제공한다.

이를 지원하기 위해 **패키지 관리자(Package Manager)**도 함께 사용된다. Conan, vcpkg, Hunter, CPM 등은 외부 라이브러리를 자동으로 다운로드하고 컴파일하며 프로젝트에 연결한다. 개발자는 개별 라이브러리를 직접 설치할 필요가 없으며, 모든 개발자가 동일한 버전의 라이브러리를 사용할 수 있으므로 프로젝트의 재현성이 크게 향상된다.

현대의 빌드 시스템은 **지속적 통합 및 지속적 배포(CI/CD, Continuous Integration/Continuous Deployment)**의 핵심 요소이기도 하다. 소스 코드가 변경되면 자동으로 컴파일하고, 단위 테스트(Unit Test), 정적 분석(Static Analysis), 보안 검사(Security Scan), 코드 품질 분석(Code Quality Analysis), 패키징, 배포까지 모두 자동으로 수행한다. 따라서 빌드 시스템은 단순한 개발 도구가 아니라 소프트웨어 개발 생명주기(Software Development Lifecycle)의 핵심 인프라가 된다.

다중 플랫폼 빌드(Multi-Target Build)도 중요한 기능이다. 하나의 저장소에서 시뮬레이터, 실제 로봇, 테스트 프로그램, 개발 도구, 클라우드 서비스를 모두 생성해야 하는 경우가 많다. 빌드 시스템은 빌드 옵션(Build Option), 기능 플래그(Feature Flag), 최적화 프로파일(Optimization Profile), 대상 플랫폼(Target Platform)을 선택하여 필요한 결과물을 자동으로 생성한다.

조건부 컴파일(Conditional Compilation)은 최소한으로 사용하는 것이 바람직하다. 소스 코드 곳곳에 \`#ifdef\`를 많이 사용하는 대신 HAL이나 추상화 계층에서 플랫폼 차이를 분리하고, 빌드 시스템이 적절한 구현을 선택하도록 하는 것이 현대적인 아키텍처 설계 방식이다. 이러한 접근은 코드 가독성과 유지보수성을 크게 향상시킨다.

빌드 시스템은 컴파일러 기능 관리도 담당한다. C++ 표준(C++ Standard), 최적화 옵션(Optimization), 경고 수준(Warning Level), 디버깅(Debugging), 프로파일링(Profiling), 메모리 검사(Sanitizer) 등을 선언적으로 관리하며 GCC, Clang, MSVC와 같은 여러 컴파일러에 맞는 옵션을 자동으로 생성한다.

테스트(Test) 역시 빌드 시스템과 긴밀하게 통합된다. 단위 테스트(Unit Test), 통합 테스트(Integration Test), 시뮬레이션 검증(Simulation Validation), HIL(Hardware-in-the-Loop), 코드 커버리지(Code Coverage), 회귀 테스트(Regression Test)를 빌드 과정에서 자동 실행함으로써 소프트웨어 품질을 지속적으로 유지할 수 있다.

최근에는 소프트웨어 공급망 보안(Software Supply Chain Security)도 빌드 시스템의 중요한 역할이 되었다. 외부 라이브러리의 취약점 검사(Vulnerability Scan), 라이선스 검증(License Compliance), 재현 가능한 빌드(Reproducible Build), 디지털 서명(Digital Signature), SBOM(Software Bill of Materials) 생성 등이 빌드 과정에 통합되고 있다.

컨테이너(Container) 기술 역시 빌드 시스템과 함께 사용된다. Docker와 같은 컨테이너에는 컴파일러, 툴체인, 패키지 관리자, 라이브러리 등이 모두 포함되므로 개발자 환경에 관계없이 항상 동일한 빌드 환경을 제공할 수 있다. 이는 재현성과 개발 효율을 크게 향상시키는 중요한 방법이다.

로봇 소프트웨어는 임베디드 제어, AI, 시뮬레이션, 클라우드, 시각화(Visualization), 사용자 인터페이스(User Interface)를 모두 포함하는 매우 복잡한 시스템이다. 따라서 빌드 시스템도 단순한 컴파일 도구가 아니라 이러한 다양한 구성 요소를 통합 관리하는 아키텍처 인프라로 발전하고 있다.

앞으로 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)** 시대에는 빌드 시스템의 중요성이 더욱 커질 것이다. CMake는 폭넓은 생태계와 호환성을 제공하고, Bazel은 대규모 프로젝트에서 뛰어난 확장성과 재현성을 제공하며, 크로스 컴파일은 다양한 하드웨어에서 동일한 소프트웨어를 실행할 수 있도록 지원한다. 이 세 가지 기술은 미래 크로스 플랫폼 소프트웨어 개발의 핵심 기반이 될 것이다.

궁극적으로 플랫폼 빌드 시스템(Build System)은 **크로스 플랫폼 소프트웨어 엔지니어링(Cross-Platform Software Engineering)**의 운영 기반이다. HAL이 플랫폼 독립적인 소프트웨어 구조를 만든다면, 빌드 시스템은 그 구조를 실제 실행 가능한 프로그램으로 변환한다. 체계적인 빌드 구성(Build Configuration), 자동화된 의존성 관리, 크로스 컴파일, 테스트 자동화, 그리고 CI/CD 통합을 통해 현대의 로봇 소프트웨어는 높은 품질(Quality), 이식성(Portability), 유지보수성(Maintainability), 확장성(Scalability)을 지속적으로 확보할 수 있다.

## 11.04 Portability Design for ARM / x86 / RISC-V

![](images/image4.png){width="7.268055555555556in" height="7.268055555555556in"}

프로세서 이식성(Processor Portability)은 현대 로보틱스(Robotics)와 **물리 AI(Physical AI)** 소프트웨어 아키텍처에서 가장 중요한 설계 원칙 가운데 하나이다. 과거에는 하나의 제품이 특정 프로세서만을 대상으로 개발되었지만, 오늘날의 로봇은 개발 단계에서는 ARM Cortex-M을 사용하고, 상용 제품에서는 ARM Cortex-A 기반 임베디드 리눅스(Embedded Linux)를 사용하며, 고성능 AI 처리를 위해 x86 산업용 PC를 추가하고, 미래에는 RISC-V 기반 프로세서로 이전하는 경우도 많다. 따라서 특정 CPU에 종속되지 않는 소프트웨어 구조가 필수적인 경쟁력이 되고 있다.

프로세서 이식성은 단순히 동일한 소스 코드를 다른 컴파일러(Compiler)로 다시 컴파일하는 것을 의미하지 않는다. 진정한 이식성은 소프트웨어 아키텍처(Software Architecture), 데이터 표현(Data Representation), 메모리 구조(Memory Organization), 동기화(Synchronization), 하드웨어 추상화(Hardware Abstraction), 빌드 시스템(Build System), 테스트(Test), 배포(Deployment), 유지보수(Maintainability)를 모두 포함하는 종합적인 설계 철학이다. 목표는 프로세서가 변경되어도 응용 소프트웨어(Application Software)는 거의 수정하지 않는 것이다.

**ARM(Advanced RISC Machine)** 아키텍처는 전력 효율(Power Efficiency)과 성능(Performance)의 균형이 뛰어나 현재 임베디드 시스템과 로봇 분야에서 가장 널리 사용되는 프로세서이다. ARM은 Cortex-M, Cortex-R, Cortex-A, Neoverse 등 다양한 제품군을 제공하며, 초소형 마이크로컨트롤러부터 클라우드 서버까지 동일한 아키텍처 계열을 확장할 수 있다는 장점을 가지고 있다.

**ARM Cortex-M** 계열은 실시간 제어(Real-Time Control)를 위한 마이크로컨트롤러이다. 모터 제어(Motor Control), 배터리 관리(Battery Management), 안전 제어(Safety Control), 센서 데이터 수집(Sensor Acquisition), 통신 게이트웨이(Communication Gateway) 등에 주로 사용되며, FreeRTOS, Zephyr 또는 Bare Metal 환경에서 실행된다. 메모리와 연산 자원이 제한적이므로 정적 메모리(Static Memory), 결정성(Determinism), 저전력 설계가 매우 중요하다.

**ARM Cortex-A** 계열은 임베디드 리눅스(Embedded Linux)를 실행하는 고성능 응용 프로세서(Application Processor)이다. 가상 메모리(Virtual Memory), 멀티코어(Multi-Core), 캐시(Cache), 부동소수점(Floating Point), 멀티미디어 처리를 지원하며, 자율주행 로봇에서는 위치 추정(Localization), 지도 작성(Mapping), 경로 계획(Path Planning), 인공지능(AI), ROS 2(Robot Operating System 2) 등의 실행 환경으로 많이 사용된다.

**x86** 프로세서는 산업용 PC(Industrial PC)와 고성능 엣지 컴퓨팅(Edge Computing)의 대표적인 플랫폼이다. 높은 연산 성능과 대용량 메모리 지원, 성숙한 개발 도구(Development Tool), 다양한 GPU(Graphics Processing Unit) 연결 기능을 제공하므로 AI 추론(AI Inference), 디지털 트윈(Digital Twin), 플릿 관리(Fleet Management), 시뮬레이션(Simulation), 클라우드 게이트웨이(Cloud Gateway)에 매우 적합하다. 산업용 자율주행 로봇에서는 x86과 GPU를 함께 사용하는 사례가 많다.

**RISC-V**는 최근 빠르게 성장하고 있는 개방형(Open ISA, Open Instruction Set Architecture) 프로세서이다. ARM과 달리 명령어 집합(Instruction Set)을 자유롭게 확장할 수 있어 기업이 자체 AI 가속기(AI Accelerator)나 특수 목적 프로세서를 설계하기 쉽다. 현재는 ARM과 x86에 비해 생태계(Ecosystem)가 작지만, 산업 자동화와 로보틱스 분야에서 장기적인 성장 가능성이 매우 높게 평가되고 있다.

ARM, x86, RISC-V는 단순히 명령어만 다른 것이 아니다. 메모리 구조(Memory Model), 인터럽트(Interrupt), 캐시(Cache), 원자 연산(Atomic Operation), 벡터 연산(Vector Processing), 부동소수점(Floating Point), 디버깅(Debugging), 부트(Boot Process), 실행 파일(Binary Format) 등이 모두 다르다. 이러한 차이를 응용 프로그램이 직접 처리하면 유지보수가 매우 어려워지므로 아키텍처 수준에서 분리해야 한다.

이를 위한 핵심 기술이 **하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)**이다. 응용 프로그램은 CPU 레지스터(Register), 인터럽트 컨트롤러(Interrupt Controller), 타이머(Timer), 캐시(Cache)를 직접 제어하지 않고 HAL이 제공하는 표준 인터페이스(Standard Interface)만 사용한다. 프로세서가 변경되더라도 HAL 내부 구현만 수정하면 상위 소프트웨어는 그대로 유지할 수 있다.

운영체제 추상화(Operating System Abstraction)도 매우 중요하다. ARM Cortex-M은 FreeRTOS나 Zephyr를 사용하고, Cortex-A와 x86은 Linux를 사용하며, 일부 시스템은 Windows나 QNX를 사용하기도 한다. 스레드(Thread), 타이머, 파일 시스템(File System), 네트워크(Network), 동기화 방식이 모두 다르므로 운영체제 API(Application Programming Interface)를 직접 사용하는 대신 공통 인터페이스를 사용하는 것이 바람직하다.

데이터 표현(Data Representation)도 이식성에 큰 영향을 준다. ARM, x86, RISC-V는 대부분 리틀 엔디언(Little Endian)을 사용하지만, 정수(Integer) 크기, 포인터(Pointer) 크기, 메모리 정렬(Memory Alignment), 구조체(Structure) 배치 방식은 컴파일러에 따라 달라질 수 있다. 따라서 고정 크기 정수(Fixed Width Integer), 표준 직렬화(Serialization), 명확한 통신 프로토콜을 사용하는 것이 중요하다.

메모리 관리(Memory Management)는 프로세서마다 크게 다르다. Cortex-M은 메모리 관리 장치(MMU, Memory Management Unit)가 없는 경우가 많아 정적 메모리 사용이 일반적이다. 반면 Cortex-A와 x86은 가상 메모리와 동적 메모리 할당(Dynamic Allocation)을 적극적으로 활용한다. 따라서 메모리 정책을 응용 프로그램이 아닌 플랫폼 계층에서 관리하도록 설계해야 한다.

동시성(Concurrency) 구조도 프로세서에 따라 차이가 크다. x86 기반 산업용 PC는 수십 개의 스레드를 동시에 실행할 수 있지만, Cortex-M은 몇 개의 실시간 태스크(Task)만 사용하는 경우가 많다. 또한 원자 연산, 캐시 일관성(Cache Coherency), 메모리 일관성(Memory Consistency)도 다르므로 공통 동기화 인터페이스를 사용하는 것이 중요하다.

컴파일러 이식성(Compiler Portability)도 고려해야 한다. GCC(GNU Compiler Collection), Clang, MSVC(Microsoft Visual C++), ARM Compiler, IAR Embedded Workbench는 최적화(Optimization), 경고(Warning), 언어 확장(Language Extension), 인라인 어셈블리(Inline Assembly)를 서로 다르게 처리한다. 따라서 특정 컴파일러에 의존하는 기능을 최소화하고 표준 C++ 기능을 중심으로 개발하는 것이 바람직하다.

플랫폼 이식성을 위해서는 빌드 시스템(Build System)도 중요한 역할을 한다. CMake, Bazel, 크로스 컴파일(Cross Compilation)을 활용하면 동일한 저장소(Repository)에서 ARM Cortex-M 펌웨어(Firmware), ARM Linux 프로그램, x86 응용 프로그램, RISC-V 실행 파일을 동시에 생성할 수 있다. 플랫폼 차이는 빌드 설정(Build Configuration)에서 관리하고, 소스 코드는 최대한 공통으로 유지하는 것이 이상적이다.

조건부 컴파일(Conditional Compilation)은 가능한 최소한으로 사용하는 것이 좋다. \`#ifdef\`가 많아질수록 코드 가독성과 유지보수성이 크게 떨어진다. 대신 HAL과 인터페이스 계층에서 플랫폼별 구현을 분리하고, 빌드 시스템이 적절한 구현체를 선택하도록 설계하는 것이 현대적인 크로스 플랫폼 개발 방식이다.

성능 이식성(Performance Portability)은 가장 어려운 설계 요소 중 하나이다. ARM은 저전력에 최적화되어 있고, x86은 고성능 연산에 강하며, RISC-V는 구현에 따라 성능 특성이 매우 다양하다. 따라서 알고리즘은 공통으로 유지하되, 성능이 중요한 부분만 플랫폼별 최적화를 수행하는 계층적 구조가 가장 효율적이다.

인공지능(AI) 추론은 이러한 구조를 잘 보여주는 사례이다. 동일한 AI 모델이라도 ARM CPU, x86 CPU, NVIDIA GPU, NPU(Neural Processing Unit), FPGA(Field Programmable Gate Array)에서 실행될 수 있다. 응용 프로그램은 공통 추론 인터페이스(Inference Interface)를 사용하고, 내부에서는 TensorRT, ONNX Runtime, OpenVINO 등 플랫폼에 맞는 실행 엔진이 선택되도록 설계하는 것이 이상적이다.

시뮬레이션(Simulation)과 실제 로봇의 호환성도 중요하다. 개발자는 x86 기반 워크스테이션에서 Gazebo, Isaac Sim, Webots, Unity 기반 디지털 트윈을 사용하지만 실제 로봇은 ARM에서 실행된다. 프로세서 독립적인 아키텍처를 적용하면 동일한 응용 프로그램을 시뮬레이터와 실제 로봇에서 거의 수정 없이 사용할 수 있다.

통신 인터페이스(Communication Interface) 역시 추상화해야 한다. CAN, SPI, I2C, UART, Ethernet, PCI Express, USB 등의 물리 인터페이스는 프로세서마다 구현 방식이 다르다. 하지만 응용 프로그램은 메시지(Message)만 처리하도록 하고, 실제 통신은 플랫폼 계층에서 수행하면 프로세서 변경 시에도 응용 프로그램은 그대로 유지된다.

프로세서 이식성을 확보하려면 테스트(Test)가 매우 중요하다. ARM, x86, RISC-V 각각에서 자동으로 컴파일하고, 단위 테스트(Unit Test), 정적 분석(Static Analysis), 시뮬레이션 검증(Simulation Validation), HIL(Hardware-in-the-Loop), 회귀 테스트(Regression Test)를 수행해야 한다. 이를 통해 플랫폼 차이로 인한 문제를 조기에 발견할 수 있다.

안전 필수 시스템(Safety-Critical System)에서는 프로세서가 달라도 동일한 안전 기능(Safety Function)이 반드시 보장되어야 한다. 비상 정지(Emergency Stop), 워치독(Watchdog), 장애 감시(Fault Monitoring), 안전 진단(Safety Diagnostics)은 CPU가 변경되어도 동일한 기능을 유지해야 하며, 이를 위해 표준 인터페이스와 검증 절차가 필요하다.

보안(Security)도 프로세서 독립적으로 설계해야 한다. 보안 부팅(Secure Boot), 암호화(Encryption), 무결성 검증(Integrity Verification), 인증(Authentication)은 ARM, x86, RISC-V마다 구현 방식이 다르지만, 응용 프로그램은 동일한 보안 인터페이스를 사용하도록 설계하는 것이 유지보수와 확장성 측면에서 유리하다.

프로세서 선택은 전력 소비(Power Consumption)와도 밀접한 관련이 있다. ARM은 배터리 기반 자율주행 로봇에 적합하고, x86은 AI와 고성능 연산이 필요한 산업용 엣지 컴퓨터에 적합하다. RISC-V는 맞춤형 프로세서(Custom Processor)를 설계할 수 있다는 장점이 있어 향후 AI 전용 칩 개발에 매우 유리하다. 프로세서 독립적인 소프트웨어는 이러한 하드웨어 선택의 자유도를 크게 높여준다.

확장성(Scalability) 역시 중요한 장점이다. 기업은 교육용 로봇, AMR(Autonomous Mobile Robot), 검사 로봇, 협동로봇(Collaborative Robot), 휴머노이드(Humanoid) 등 다양한 제품을 개발한다. 하드웨어는 서로 다르지만 위치 추정(Localization), 내비게이션(Navigation), AI 추론, 플릿 관리 등 핵심 소프트웨어는 동일하게 재사용할 수 있으며, 이는 개발 비용 절감과 제품 경쟁력 향상으로 이어진다.

앞으로 AI 가속기, 뉴로모픽 프로세서(Neuromorphic Processor), 칩렛(Chiplet), 양자 컴퓨팅(Quantum Computing)과 같은 새로운 컴퓨팅 기술이 지속적으로 등장할 것이다. 특정 프로세서에 종속된 소프트웨어는 빠르게 노후화되지만, 프로세서 독립적인 아키텍처는 새로운 하드웨어에도 쉽게 적응할 수 있다.

결국 **ARM, x86, RISC-V를 위한 이식성 설계(Portability Design)**는 단순히 여러 CPU를 지원하는 기술이 아니라 장기적인 소프트웨어 엔지니어링 전략이다. 안정적인 인터페이스(Stable Interface), HAL, 운영체제 추상화, 표준 프로그래밍, 자동화된 빌드(Build Automation), 지속적인 테스트(Test Automation)를 기반으로 구축된 소프트웨어는 하드웨어 변화에도 지속적으로 발전할 수 있으며, **소프트웨어 정의 로봇(Software-Defined Robot)**과 **차세대 물리 AI(Next-Generation Physical AI)** 시대의 핵심 기반 기술이 될 것이다.

## 11.05 Common Interface Design for Embedded Linux and RTOS

![](images/image5.png){width="7.268055555555556in" height="7.268055555555556in"}

현대 로봇 시스템(Robotics System)은 하나의 운영체제(Operating System)만으로 구성되지 않는다. 일반적으로 모터 제어(Motor Control), 배터리 관리(Battery Management), 안전 제어(Safety Control), 센서 데이터 수집(Sensor Acquisition)과 같은 실시간 기능은 **실시간 운영체제(RTOS, Real-Time Operating System)**에서 실행되고, 인공지능(AI), 위치 추정(Localization), 지도 작성(Mapping), 경로 계획(Path Planning), 사용자 인터페이스(User Interface)는 **임베디드 리눅스(Embedded Linux)**에서 실행된다. 따라서 서로 다른 운영체제 환경에서도 동일한 응용 소프트웨어를 사용할 수 있도록 하는 **공통 인터페이스(Common Interface)** 설계가 매우 중요하다.

임베디드 리눅스와 RTOS는 설계 철학 자체가 다르다. 임베디드 리눅스는 가상 메모리(Virtual Memory), 프로세스(Process), 네트워크(Network), 파일 시스템(File System), 다양한 미들웨어(Middleware)를 제공하는 범용 운영체제이다. 반면 RTOS는 결정성(Determinism), 짧은 응답 시간(Response Time), 낮은 메모리 사용량, 실시간 스케줄링(Real-Time Scheduling)을 최우선 목표로 한다. 이러한 차이 때문에 동일한 API(Application Programming Interface)를 직접 사용하는 것은 어렵지만, 공통 인터페이스를 설계하면 두 환경을 하나의 소프트웨어 구조로 통합할 수 있다.

공통 인터페이스의 핵심 목표는 운영체제의 구현 방식이 아니라 **기능(Function)** 중심으로 소프트웨어를 설계하는 것이다. 응용 프로그램은 타이머(Timer), 메시지(Message), 파일(File), 네트워크(Network), 메모리(Memory) 등을 직접 제어하지 않고 "데이터 저장", "주기 실행", "메시지 전송"과 같은 기능만 호출한다. 실제 구현은 Linux에서는 POSIX API를 사용하고 RTOS에서는 커널(Kernel) 기능을 사용하지만, 상위 응용 프로그램은 이러한 차이를 전혀 알 필요가 없다.

가장 중요한 설계 원칙은 **운영체제 의존성(Operating System Dependency)**과 응용 소프트웨어(Application Software)를 분리하는 것이다. 비즈니스 로직(Business Logic), 제어 알고리즘(Control Algorithm), AI, 위치 추정, 경로 계획은 Linux 시스템 호출(System Call)이나 RTOS 커널 API를 직접 사용하지 않는다. 대신 운영체제 추상화 계층(OS Abstraction Layer)을 통해 필요한 기능만 호출하며, 실제 구현은 운영체제별로 독립적으로 제공된다.

스레드(Thread) 관리가 대표적인 예이다. Linux는 POSIX Thread(Pthread)를 사용하여 다양한 스레드 생성과 우선순위(Priority), CPU Affinity 등을 지원한다. RTOS는 Task 중심의 실시간 스케줄러를 사용하며 메모리 사용량과 실행 시간이 매우 제한적이다. 공통 인터페이스에서는 "작업 생성(Create Task)", "작업 시작(Start Task)", "작업 종료(Stop Task)"와 같은 기능만 정의하고, Linux와 RTOS가 각각 내부적으로 적절한 방식으로 구현하도록 한다.

동기화(Synchronization)도 운영체제마다 구현 방식이 다르다. Linux는 Mutex, Semaphore, Condition Variable, Read-Write Lock 등을 제공하며, RTOS는 보다 단순하고 결정적인 동기화 메커니즘을 제공한다. 공통 인터페이스에서는 "잠금(Lock)", "해제(Unlock)", "대기(Wait)", "신호(Signal)"와 같은 기능만 정의하여 응용 프로그램이 운영체제에 의존하지 않도록 설계한다.

시간 관리(Time Management)는 특히 중요한 요소이다. Linux는 POSIX Timer, High Resolution Timer, Monotonic Clock 등 다양한 시간 서비스를 제공하지만 RTOS는 하드웨어 타이머(Hardware Timer)를 기반으로 매우 결정적인 주기 실행을 수행한다. 공통 인터페이스는 "주기 실행(Periodic Execution)", "지연(Delay)", "타임아웃(Timeout)", "타임스탬프(Timestamp)"와 같은 기능만 정의하고, 실제 구현은 운영체제에 따라 달라지도록 한다.

메모리 관리(Memory Management)는 Linux와 RTOS의 가장 큰 차이 가운데 하나이다. Linux는 가상 메모리와 동적 메모리 할당(Dynamic Allocation)을 자유롭게 사용할 수 있지만, RTOS는 MMU(Memory Management Unit)가 없는 경우가 많아 정적 메모리(Static Memory)와 메모리 풀(Memory Pool)을 주로 사용한다. 따라서 메모리 정책은 응용 프로그램이 아니라 메모리 추상화 계층(Memory Abstraction Layer)에서 관리하는 것이 바람직하다.

동적 메모리(Dynamic Memory)는 RTOS 환경에서 특히 주의해야 한다. Linux에서는 Heap 메모리를 자유롭게 사용할 수 있지만, RTOS에서는 메모리 단편화(Fragmentation)와 예측 불가능한 할당 시간이 실시간 성능을 저하시킬 수 있다. 따라서 공통 메모리 인터페이스를 정의하고 Linux에서는 Heap을, RTOS에서는 정적 메모리나 고정 크기 메모리 풀을 사용하는 것이 일반적인 설계 방식이다.

파일 시스템(File System) 역시 운영체제마다 매우 다르다. Linux는 계층형 파일 시스템(Hierarchical File System)을 제공하지만, RTOS는 간단한 플래시 파일 시스템(Flash File System)만 지원하거나 파일 시스템 자체가 없는 경우도 있다. 따라서 응용 프로그램은 파일을 직접 다루지 않고 "설정 저장(Configuration Save)", "로그 저장(Log Save)", "데이터 읽기(Data Read)"와 같은 공통 저장 인터페이스(Storage Interface)를 사용하는 것이 바람직하다.

로그(Log) 시스템도 공통 인터페이스를 사용하는 것이 좋다. Linux에서는 텍스트 기반 로그를 파일이나 네트워크에 저장하지만, RTOS에서는 메모리 사용량이 제한되므로 바이너리(Binary) 로그나 실시간 UART 출력만 사용하는 경우가 많다. 응용 프로그램은 동일한 로그 API를 호출하고 실제 출력 방식은 운영체제가 결정하도록 설계한다.

통신(Communication)은 공통 인터페이스 설계에서 가장 중요한 요소 가운데 하나이다. Linux에서는 TCP/IP, DDS(Data Distribution Service), MQTT(Message Queuing Telemetry Transport), REST API, WebSocket 등을 사용하지만, RTOS에서는 CAN, UART, SPI, I2C와 같은 경량 프로토콜을 주로 사용한다. 응용 프로그램은 메시지(Message)만 송수신하고 실제 통신 방식은 플랫폼 계층에서 처리하는 것이 이상적인 구조이다.

장치 관리(Device Management)도 공통 인터페이스를 사용해야 한다. Linux에서는 Device File과 Kernel Driver를 통해 장치를 제어하지만 RTOS에서는 초기화 단계에서 장치를 직접 등록하는 경우가 많다. 공통 인터페이스에서는 초기화(Initialization), 설정(Configuration), 상태 조회(Status), 진단(Diagnostics), 종료(Shutdown) 기능만 정의하고, 운영체제가 내부 구현을 담당하도록 한다.

설정 관리(Configuration Management)는 플랫폼 차이를 줄이는 데 매우 효과적이다. Linux에서는 YAML, JSON, 환경 변수(Environment Variable), 데이터베이스(Database)를 사용할 수 있지만 RTOS에서는 Flash Memory나 컴파일 시 설정(Compile-Time Configuration)을 사용한다. 응용 프로그램은 설정 API만 호출하고 실제 저장 방식은 운영체제가 결정하도록 설계해야 한다.

오류 처리(Error Handling)는 반드시 표준화(Standardization)되어야 한다. Linux는 POSIX Error Code를 사용하고 RTOS는 제조사별 오류 코드를 사용하는 경우가 많다. 공통 인터페이스에서는 초기화 오류(Initialization Error), 통신 오류(Communication Error), 자원 부족(Resource Exhaustion), 타임아웃(Timeout), 하드웨어 오류(Hardware Fault)와 같은 공통 오류 모델을 정의하여 응용 프로그램이 운영체제에 관계없이 동일한 방식으로 오류를 처리할 수 있도록 한다.

상태 관리(State Management) 역시 중요한 요소이다. Linux에서는 프로세스가 종료되거나 다시 시작될 수 있지만 RTOS는 일반적으로 부팅 이후 계속 실행된다. 따라서 초기화(Initialization), 시작(Start), 운영(Operation), 장애(Degraded), 복구(Recovery), 종료(Shutdown)와 같은 공통 생명주기(Lifecycle)를 정의하면 운영체제가 달라도 동일한 상태 관리가 가능하다.

전력 관리(Power Management)는 두 운영체제에서 매우 다르게 구현된다. Linux는 CPU 주파수 조절(Frequency Scaling), 절전 모드(Suspend), 열 관리(Thermal Management)를 지원하지만 RTOS는 단순한 Sleep Mode나 저전력 모드(Low Power Mode)를 사용하는 경우가 많다. 공통 인터페이스는 원하는 동작 상태만 정의하고 실제 전력 제어는 플랫폼 계층에서 수행하는 것이 이상적이다.

안전 필수 시스템(Safety-Critical System)에서는 공통 인터페이스의 중요성이 더욱 커진다. 비상 정지(Emergency Stop), 워치독(Watchdog), 장애 감시(Fault Monitoring), 안전 종료(Safe Shutdown)는 Linux와 RTOS에서 구현 방식은 달라도 기능은 반드시 동일해야 한다. 이를 위해 표준화된 안전 인터페이스(Safety Interface)를 정의하고 검증 가능한 구조를 유지해야 한다.

인공지능(AI) 시스템도 공통 인터페이스를 활용할 수 있다. AI 추론은 Linux에서 GPU를 이용해 수행하는 경우가 많고, RTOS는 센서 수집과 제어를 담당한다. 공통 추론 인터페이스(Inference Interface)를 정의하면 RTOS와 Linux가 자연스럽게 협력하여 하나의 AI 시스템을 구성할 수 있다.

미들웨어(Middleware) 역시 공통 인터페이스를 통해 통합된다. ROS 2(Robot Operating System 2)는 Linux에서 실행되는 경우가 대부분이지만, micro-ROS는 RTOS에서도 사용할 수 있다. 응용 프로그램은 미들웨어를 직접 사용하지 않고 공통 인터페이스를 사용하며, Linux에서는 ROS 2를, RTOS에서는 micro-ROS나 경량 프로토콜을 내부적으로 사용하도록 설계하는 것이 이상적이다.

공통 인터페이스는 테스트(Test) 환경 구축에도 매우 유리하다. 운영체제 기능을 Mock Object, Simulator, Virtual Device로 쉽게 대체할 수 있기 때문에 Unit Test, Integration Test, Hardware-in-the-Loop(HIL), Software-in-the-Loop(SIL)를 동일한 응용 프로그램으로 수행할 수 있다. 이는 CI/CD(Continuous Integration/Continuous Deployment) 자동화에도 큰 장점을 제공한다.

장기 유지보수(Maintenance) 측면에서도 공통 인터페이스는 매우 중요하다. 제품이 Linux에서 RTOS로, 또는 RTOS에서 Linux로 이전되더라도 운영체제 계층만 수정하면 응용 프로그램은 그대로 유지할 수 있다. 이는 제품 수명이 긴 산업용 로봇에서 매우 큰 비용 절감 효과를 제공한다.

공통 인터페이스는 성능(Performance)도 고려해야 한다. 지나친 추상화는 함수 호출(Function Call)과 메모리 사용량을 증가시킬 수 있으므로 인터페이스는 가능한 단순하면서도 기능 중심으로 설계해야 한다. 플랫폼별 최적화는 내부 구현에서 수행하고 외부 인터페이스는 항상 동일하게 유지하는 것이 바람직하다.

보안(Security) 역시 공통 인터페이스를 통해 관리할 수 있다. 인증(Authentication), 암호화(Encryption), 보안 저장(Secure Storage), 인증서 관리(Certificate Management), OTA(Over-the-Air) 업데이트는 Linux와 RTOS에서 구현 방식이 다르지만, 공통 보안 인터페이스(Security Interface)를 정의하면 응용 프로그램은 동일한 방식으로 보안 기능을 사용할 수 있다.

결국 **임베디드 리눅스(Embedded Linux)**와 **실시간 운영체제(RTOS)**를 위한 **공통 인터페이스 설계(Common Interface Design)**는 단순히 API를 통일하는 작업이 아니다. 이는 운영체제 차이를 숨기고 기능 중심의 추상화(Abstraction)를 제공하여 응용 소프트웨어의 재사용성(Reusability), 유지보수성(Maintainability), 확장성(Scalability), 안전성(Safety), 이식성(Portability)을 동시에 확보하는 핵심 소프트웨어 아키텍처 전략이다. 앞으로 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)** 시대에는 Linux와 RTOS를 하나의 통합 플랫폼으로 연결하는 공통 인터페이스가 차세대 로봇 소프트웨어의 핵심 기반 기술이 될 것이다.

## 11.06 ROS2 Package Cross-Platform Build Strategy [w/Code]

![](images/image6.png){width="7.268055555555556in" height="7.268055555555556in"}![](images/image7.png){width="7.268055555555556in" height="7.268055555555556in"}

**ROS 2(Robot Operating System 2)**는 현대 로보틱스(Robotics)의 사실상 표준 미들웨어(Middleware)로 자리 잡고 있다. ROS 2는 분산 컴퓨팅(Distributed Computing), 표준 통신(Standard Communication), 모듈화(Modularization), 그리고 뛰어난 확장성(Scalability)을 제공하여 자율주행 로봇, 산업용 로봇, 협동로봇(Collaborative Robot), 물리 AI(Physical AI) 플랫폼의 핵심 소프트웨어 기반이 되고 있다. 그러나 ROS 2 자체가 크로스 플랫폼(Cross-Platform)을 지원한다고 해서 자동으로 이식성(Portability)이 확보되는 것은 아니다. 다양한 프로세서와 운영체제를 고려한 패키지(Package) 구조와 빌드 전략(Build Strategy)이 함께 설계되어야 한다.

현대의 로봇 개발 환경에서는 Ubuntu 기반 x86 워크스테이션(Workstation)에서 개발을 수행하고, ARM 기반 임베디드 리눅스(Embedded Linux)에 실제 로봇을 배포하며, 산업용 PC에서는 GPU(Graphics Processing Unit)를 활용한 AI 추론(AI Inference)을 수행하고, 시뮬레이션(Simulation)은 별도의 개발 환경에서 수행하는 경우가 일반적이다. 따라서 하나의 소스 코드(Source Code)를 유지하면서 여러 플랫폼에서 동일한 기능을 수행할 수 있도록 설계하는 것이 매우 중요하다.

ROS 2 크로스 플랫폼 전략의 가장 중요한 원칙은 **플랫폼 독립적인 로직(Platform-Independent Logic)**과 **플랫폼 의존적인 구현(Platform-Specific Implementation)**을 명확하게 분리하는 것이다. 내비게이션(Navigation), 인식(Perception), 경로 계획(Path Planning), 미션 관리(Mission Management), AI 알고리즘은 CPU나 운영체제와 관계없이 동일하게 유지하고, 하드웨어 드라이버(Device Driver), GPU 가속(GPU Acceleration), 운영체제 API(Application Programming Interface)는 별도의 추상화 계층(Abstraction Layer)으로 분리해야 한다.

ROS 2 패키지(Package)의 구조는 이식성에 가장 큰 영향을 주는 요소이다. 하나의 패키지 안에 하드웨어 드라이버, 알고리즘, 사용자 인터페이스(User Interface), 시뮬레이션 코드, 테스트(Test)를 모두 포함시키는 것은 바람직하지 않다. 대신 메시지(Message), 서비스(Service), 액션(Action)를 정의하는 인터페이스 패키지(Interface Package), 센서와 액추에이터를 제어하는 하드웨어 패키지(Hardware Package), AI와 제어 알고리즘을 포함하는 코어(Core) 패키지, 그리고 전체 로봇을 제어하는 애플리케이션(Application) 패키지로 계층화하는 것이 가장 이상적인 구조이다.

인터페이스 패키지(Interface Package)는 ROS 2 시스템에서 매우 중요한 역할을 수행한다. 메시지, 서비스, 액션은 하드웨어가 아니라 로봇의 기능(Function)을 중심으로 정의되어야 한다. 예를 들어 카메라(Camera)의 USB 통신 방식이 아니라 "영상(Image)"이라는 개념을 전달하고, 모터 드라이버(Motor Driver)가 아니라 "속도(Velocity)"와 "위치(Position)"라는 논리적인 인터페이스를 제공해야 한다. 이러한 구조는 하드웨어 교체 시에도 응용 프로그램을 수정하지 않도록 해준다.

빌드 시스템(Build System)은 ROS 2 크로스 플랫폼 전략의 핵심 기반이다. ROS 2는 **CMake(Cross Platform Make)**와 **ament** 빌드 시스템(Build System)을 사용하며 Linux, Windows, macOS뿐 아니라 ARM 기반 임베디드 시스템에서도 동일한 방식으로 패키지를 빌드할 수 있다. 개발자는 플랫폼별 프로젝트를 따로 만들 필요 없이 하나의 CMake 설정만으로 여러 플랫폼을 동시에 지원할 수 있다.

의존성 관리(Dependency Management)도 매우 중요하다. 하드웨어 드라이버가 상위 응용 프로그램을 참조하거나, 알고리즘이 사용자 인터페이스를 직접 사용하는 구조는 유지보수를 어렵게 만든다. 센서 드라이버는 인터페이스만 제공하고, 알고리즘은 인터페이스만 참조하며, 응용 프로그램이 이를 조합하는 계층적 구조(Layered Architecture)를 유지해야 한다. 이러한 구조는 패키지 간 결합도(Coupling)를 크게 줄여준다.

플랫폼별 조건부 컴파일(Conditional Compilation)은 최소한으로 사용하는 것이 좋다. \`#ifdef\`가 소스 코드 전체에 퍼지면 가독성과 유지보수성이 급격히 떨어진다. 대신 **의존성 역전 원칙(Dependency Inversion Principle, DIP)**과 **플러그인(Plugin)** 구조를 이용하여 플랫폼별 구현체를 선택하는 것이 현대적인 ROS 2 설계 방식이다.

플러그인 아키텍처(Plugin Architecture)는 ROS 2에서 매우 효과적인 방법이다. 라이다(LiDAR), 카메라(Camera), AI 추론 엔진(Inference Engine), 위치 추정(Localization), 경로 계획(Path Planning) 알고리즘을 모두 동일한 인터페이스로 정의하고, 실제 구현만 플러그인으로 교체한다. 새로운 센서나 알고리즘을 추가하더라도 응용 프로그램은 수정할 필요가 없으며 플러그인만 추가하면 된다.

ROS 2의 **컴포넌트(Component)** 구조도 크로스 플랫폼 설계에 매우 적합하다. 하나의 프로세스(Process) 안에서 여러 노드(Node)를 실행할 수도 있고, 각각을 독립적인 프로세스로 실행할 수도 있다. ARM과 같은 저사양 플랫폼에서는 하나의 프로세스로 통합하여 메모리를 절약하고, x86에서는 여러 프로세스로 분리하여 성능과 안정성을 높일 수 있다.

런치 시스템(Launch System)은 실행 환경을 플랫폼과 분리하는 중요한 기능이다. ROS 2 Launch는 실행할 노드(Node), 파라미터(Parameter), 네임스페이스(Namespace), 통신 구조(Communication Topology)를 별도의 Launch 파일에서 정의한다. 동일한 프로그램이라도 시뮬레이션, 실제 로봇, 테스트 환경마다 다른 Launch 파일만 사용하면 되므로 응용 프로그램은 변경하지 않아도 된다.

파라미터(Parameter)는 반드시 외부에서 관리해야 한다. 센서 보정값(Calibration), 모터 제어 이득(Control Gain), AI 모델(Model) 경로, 통신 주소(Address), 하드웨어 설정(Configuration)을 코드에 직접 작성하면 플랫폼 변경 시마다 재컴파일해야 한다. 대신 YAML(Parameter File) 등을 이용하여 외부에서 관리하면 동일한 바이너리(Binary)를 여러 플랫폼에서 그대로 사용할 수 있다.

하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)은 ROS 2에서도 매우 중요한 역할을 수행한다. 카메라, IMU(Inertial Measurement Unit), LiDAR, GNSS(Global Navigation Satellite System), 배터리(Battery), 모터(Motor)는 모두 HAL을 통해 접근하고, ROS 2 알고리즘은 HAL 인터페이스만 사용하도록 설계해야 한다. 실제 장비가 변경되더라도 HAL 내부만 수정하면 상위 패키지는 그대로 유지된다.

크로스 컴파일(Cross Compilation)은 ROS 2 배포에서 필수적인 기술이다. 일반적으로 x86 Ubuntu에서 개발한 후 ARM 기반 Jetson이나 산업용 ARM 컴퓨터에 배포한다. Toolchain File, Sysroot, 크로스 컴파일 라이브러리(Cross-Compiled Library)를 이용하면 동일한 소스 코드에서 ARM용 실행 파일을 자동 생성할 수 있다.

워크스페이스(Workspace) 구성도 유지보수성을 크게 좌우한다. 재사용 가능한 라이브러리(Library), 외부 오픈소스(Open Source), 플랫폼 전용 코드, 고객 전용 기능을 서로 다른 Workspace로 분리하면 전체 시스템을 다시 빌드하지 않아도 된다. Overlay Workspace 구조를 사용하면 플랫폼별 기능만 추가하면서도 공통 코드는 그대로 유지할 수 있다.

테스트(Test)는 모든 플랫폼에서 동일하게 수행되어야 한다. 단위 테스트(Unit Test)는 알고리즘을 검증하고, 통합 테스트(Integration Test)는 노드 간 통신을 검증하며, 시뮬레이션(Simulation)은 전체 기능을 검증한다. 또한 HIL(Hardware-in-the-Loop) 테스트와 자동 회귀 테스트(Regression Test)를 CI/CD(Continuous Integration/Continuous Deployment)에 연결하면 플랫폼 차이로 인한 문제를 조기에 발견할 수 있다.

시뮬레이션(Simulation)은 ROS 2 크로스 플랫폼 전략에서 매우 중요한 역할을 한다. Gazebo, Isaac Sim, Webots, Unity 기반 디지털 트윈(Digital Twin)은 대부분 x86 Linux에서 실행되지만 실제 로봇은 ARM 기반이다. 하드웨어 추상화와 공통 인터페이스를 적용하면 동일한 ROS 2 패키지를 시뮬레이터와 실제 로봇에서 거의 수정 없이 사용할 수 있다.

인공지능(AI) 추론도 플랫폼 독립적으로 설계해야 한다. ARM CPU, NVIDIA GPU, TensorRT, ONNX Runtime, OpenVINO 등 다양한 실행 환경이 존재하지만, 응용 프로그램은 공통 AI 인터페이스만 사용하고 내부에서 플랫폼에 맞는 추론 엔진을 선택하도록 설계해야 한다. 이렇게 하면 AI 하드웨어가 변경되어도 응용 프로그램은 그대로 유지된다.

ROS 2는 분산 시스템(Distributed System) 구조에도 적합하다. 클라우드(Cloud), 엣지 컴퓨터(Edge Computer), 자율주행 로봇, 산업용 제어기, 시뮬레이터가 각각 필요한 패키지만 실행하면서도 동일한 저장소(Repository)를 공유할 수 있다. 플랫폼에 따라 필요한 패키지만 선택적으로 배포하는 구조가 가능하다.

CI/CD는 현대 ROS 2 개발에서 반드시 필요한 요소이다. 코드가 변경되면 자동으로 컴파일하고, 인터페이스 생성, 단위 테스트, 정적 분석(Static Analysis), 시뮬레이션 검증, 문서 생성(Document Generation), 패키징(Packaging), 배포(Deployment)를 수행해야 한다. 이러한 자동화는 플랫폼이 많아질수록 더욱 큰 효과를 발휘한다.

ROS 2 버전 관리(Version Management)도 중요하다. Humble, Iron, Jazzy 등 서로 다른 ROS 2 배포판(Distribution)을 사용하는 프로젝트가 동시에 존재할 수 있다. 따라서 특정 버전에 종속되는 기능을 최소화하고, 인터페이스 중심으로 설계하면 ROS 2 버전이 변경되더라도 상위 응용 프로그램은 큰 수정 없이 유지할 수 있다.

실시간 제어(Real-Time Control)는 일반적으로 RTOS에서 수행되고 ROS 2는 Linux에서 실행된다. 따라서 ROS 2는 제어 명령(Command)과 상태(State)를 RTOS와 교환하는 역할을 수행하며, 실제 모터 제어는 RTOS에서 담당하는 것이 일반적인 구조이다. 이러한 구조는 Linux의 유연성과 RTOS의 결정성을 동시에 활용할 수 있는 장점을 가진다.

보안(Security) 역시 플랫폼 독립적으로 설계해야 한다. 인증(Authentication), 암호화(Encryption), OTA(Over-the-Air) 업데이트, 인증서(Certificate) 관리, 무결성 검증(Integrity Verification)은 응용 프로그램 내부가 아니라 공통 보안 계층(Security Layer)에서 처리하는 것이 바람직하다. 이를 통해 다양한 플랫폼에서도 일관된 보안 정책을 유지할 수 있다.

확장성(Scalability)은 ROS 2 패키지 전략의 가장 큰 장점이다. 교육용 로봇, AMR(Autonomous Mobile Robot), 산업용 로봇, 검사 로봇, 실외 자율주행 로봇, 휴머노이드(Humanoid)는 하드웨어는 다르지만 내비게이션, AI, 위치 추정, 플릿 관리(Fleet Management), 진단(Diagnostics)은 대부분 공유할 수 있다. 동일한 패키지를 재사용함으로써 개발 비용을 크게 줄일 수 있다.

앞으로의 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)** 시대에는 AI 가속기(AI Accelerator), 디지털 트윈(Digital Twin), 클라우드 로보틱스(Cloud Robotics), 엣지 AI(Edge AI)가 함께 사용될 것이다. 이러한 환경에서는 플랫폼 독립적인 ROS 2 패키지 구조가 더욱 중요한 경쟁력이 된다.

결국 **ROS 2 패키지 크로스 플랫폼 빌드 전략(ROS 2 Package Cross-Platform Build Strategy)**은 단순한 빌드 기술이 아니라 장기적인 소프트웨어 아키텍처 전략이다. 플랫폼 독립적인 로직과 플랫폼 의존적인 구현을 분리하고, HAL, 플러그인 아키텍처, CMake, ament, 자동화된 테스트와 CI/CD를 활용하면 하나의 소프트웨어 생태계로 ARM, x86, RISC-V, 시뮬레이션, 클라우드, 미래의 새로운 하드웨어까지 모두 지원할 수 있다. 이러한 전략은 높은 재사용성(Reusability), 유지보수성(Maintainability), 확장성(Scalability), 이식성(Portability)을 보장하는 차세대 로봇 소프트웨어의 핵심 기반이 된다.

## 11.07 Common SW Components for AMR / Manipulator / Quadruped / Humanoid

![](images/image8.png){width="7.268055555555556in" height="7.268055555555556in"}

현대의 로봇 소프트웨어(Robotics Software)는 더 이상 로봇 종류별로 각각 개발하는 방식이 아니다. 과거에는 **자율이동로봇(AMR, Autonomous Mobile Robot)**, 매니퓰레이터(Manipulator), 사족보행 로봇(Quadruped), 휴머노이드(Humanoid)가 모두 독립적인 소프트웨어 스택(Software Stack)을 사용하였다. 그러나 최근에는 **소프트웨어 정의 로봇(Software-Defined Robot)** 개념이 확산되면서 동일한 소프트웨어 자산을 여러 로봇 플랫폼에서 재사용하는 방향으로 발전하고 있다. 이는 개발 비용을 줄이고 유지보수성을 높이는 핵심 전략이 되고 있다.

크로스 플랫폼(Cross-Platform) 소프트웨어 아키텍처는 하드웨어(Hardware)가 아니라 기능(Function)을 중심으로 설계된다. 바퀴를 사용하는 AMR과 다리를 사용하는 사족보행 로봇은 이동 방식은 다르지만, 주변 환경을 인식하고 위치를 추정하며 경로를 계획하고 임무를 수행하는 과정은 매우 유사하다. 따라서 기구 구조(Mechanical Structure)는 달라도 상위 소프트웨어(Application Software)는 대부분 동일하게 사용할 수 있도록 설계하는 것이 현대적인 접근 방식이다.

공통 소프트웨어 컴포넌트(Common Software Component)의 개념은 "모든 로봇은 비슷한 계산(Computation)을 수행한다."는 사실에서 출발한다. 모든 로봇은 센서(Sensor)를 통해 환경을 인식하고, 자신의 위치를 계산하며, 목표를 계획하고, 모터를 제어하며, 시스템 상태를 감시하고, 외부와 통신하며, 최근에는 인공지능(AI) 추론까지 수행한다. 실제로 기계적인 구조는 다르지만 계산 알고리즘은 매우 높은 수준으로 공유될 수 있다.

인식(Perception)은 가장 대표적인 공통 소프트웨어 영역이다. 카메라(Camera), 라이다(LiDAR), 레이더(Radar), 깊이 카메라(Depth Camera), IMU(Inertial Measurement Unit), GNSS(Global Navigation Satellite System), 초음파(Ultrasonic), 열화상(Thermal Camera)은 로봇 종류와 관계없이 동일한 방식으로 데이터를 생성한다. AMR은 장애물 회피를 위해 사용하고, 매니퓰레이터는 물체 인식을 위해 사용하며, 사족보행 로봇은 지형 인식을 위해 활용하고, 휴머노이드는 사람과 환경을 이해하는 데 활용하지만, 영상 처리(Image Processing), 센서 융합(Sensor Fusion), 객체 인식(Object Detection), 의미론적 분할(Semantic Segmentation) 알고리즘은 대부분 공통으로 사용할 수 있다.

위치 추정(Localization) 역시 매우 높은 재사용성을 가진다. AMR은 실내 지도를 기준으로 자신의 위치를 추정하고, 매니퓰레이터는 작업 대상과의 상대 위치를 계산하며, 사족보행 로봇은 불규칙한 지형에서 자신의 자세를 계산하고, 휴머노이드는 전신 자세(Whole Body Pose)를 지속적으로 추정한다. 하지만 칼만 필터(Kalman Filter), 그래프 최적화(Graph Optimization), 비주얼 오도메트리(Visual Odometry), SLAM(Simultaneous Localization and Mapping), 확률 기반 위치 추정(Probabilistic Localization)은 모두 동일한 수학적 원리를 사용한다.

맵(Map) 생성도 공통 소프트웨어의 대표적인 예이다. AMR은 점유 격자 지도(Occupancy Grid Map)를 생성하고, 사족보행 로봇은 지형 지도(Terrain Map)를 생성하며, 휴머노이드는 의미 지도(Semantic Map)를 구축한다. 하지만 포인트 클라우드(Point Cloud) 처리, 메쉬(Mesh) 생성, 장애물 분류(Obstacle Classification), 환경 갱신(Environment Update), 지도 저장(Map Persistence) 알고리즘은 거의 동일하게 사용할 수 있다.

계획(Planning)은 로봇 종류와 관계없이 동일한 구조를 가진다. AMR은 이동 경로(Path)를 계산하고, 매니퓰레이터는 충돌 없는 관절 경로(Joint Trajectory)를 생성하며, 사족보행 로봇은 보행 계획(Gait Planning)을 수행하고, 휴머노이드는 전신 동작(Whole Body Motion)을 생성한다. 그래프 탐색(Graph Search), 최적화(Optimization), 행동 트리(Behavior Tree), 상태 기계(State Machine), 임무 계획(Mission Planning)과 같은 상위 알고리즘은 대부분 공통으로 사용할 수 있다.

모션 제어(Motion Control)는 하드웨어 차이가 가장 큰 영역이지만, 추상화(Abstraction)를 통해 공통 인터페이스를 구축할 수 있다. AMR은 속도(Velocity)를 제어하고, 매니퓰레이터는 관절(Joint)을 제어하며, 사족보행 로봇은 다리(Leg)를 제어하고, 휴머노이드는 전신 균형(Balance)을 유지해야 한다. 그러나 상위 응용 프로그램은 "목표 위치(Target Position)" 또는 "목표 속도(Target Velocity)"만 전달하고, 실제 모터 제어는 플랫폼별 컨트롤러(Controller)가 수행하도록 설계하는 것이 이상적이다.

미션 관리(Mission Management)는 거의 모든 자율 로봇이 공통으로 사용하는 기능이다. 물류 운송(Logistics), 산업 조립(Industrial Assembly), 시설 점검(Inspection), 순찰(Patrol), 서비스(Service) 등 응용 분야는 다르지만, 작업(Task) 생성, 일정 관리(Scheduling), 자원 할당(Resource Allocation), 예외 처리(Exception Handling), 작업 완료(Completion) 과정은 거의 동일하다. 따라서 미션 오케스트레이션(Mission Orchestration)은 공통 소프트웨어로 구현하는 것이 가장 효율적이다.

인공지능(AI)은 최근 공통 소프트웨어의 핵심이 되고 있다. 객체 인식(Object Recognition), 의미론적 분할(Semantic Segmentation), 이상 탐지(Anomaly Detection), 예지 보전(Predictive Maintenance), 자연어 이해(Natural Language Understanding), 강화학습(Reinforcement Learning), 멀티모달 AI(Multimodal AI)는 로봇 종류와 무관하게 사용할 수 있다. 따라서 AI 추론 엔진(Inference Engine), 모델 관리(Model Management), AI 배포(AI Deployment)는 하나의 공통 플랫폼으로 구축하는 것이 바람직하다.

통신(Communication) 역시 로봇 종류와 무관한 공통 기능이다. 모든 로봇은 플릿 관리(Fleet Management), 클라우드(Cloud), 디지털 트윈(Digital Twin), 운영자(Operator), 다른 로봇과 데이터를 교환한다. DDS(Data Distribution Service), MQTT(Message Queuing Telemetry Transport), REST API, WebSocket, gRPC 등은 특정 로봇에 종속되지 않으므로 표준 통신 인터페이스(Standard Communication Interface)로 구성하는 것이 효율적이다.

진단(Diagnostics)은 모든 로봇에서 반드시 필요한 기능이다. CPU 사용률(CPU Utilization), 메모리(Memory), 센서 상태(Sensor Status), 모터 상태(Motor Status), 배터리(Battery), 통신(Network), 온도(Thermal), 소프트웨어 예외(Exception)를 지속적으로 감시해야 한다. 임계값(Threshold)은 플랫폼마다 다르지만, 진단 구조(Diagnostic Architecture)는 동일하게 사용할 수 있다.

안전(Safety) 역시 공통 컴포넌트로 구현해야 한다. 비상 정지(Emergency Stop), 워치독(Watchdog), 장애 감시(Fault Monitoring), 안전 종료(Safe Shutdown), 이중화(Redundancy)는 모든 로봇에 공통적으로 필요하다. 따라서 안전 인터페이스(Safety Interface)를 표준화하면 AMR, 매니퓰레이터, 사족보행, 휴머노이드 모두 동일한 안전 구조를 사용할 수 있다.

설정 관리(Configuration Management)는 플랫폼 독립성을 높이는 핵심 기술이다. 센서 보정(Calibration), 통신 주소(Address), 제어 이득(Control Gain), AI 모델(Model), 운영 모드(Operation Mode)를 코드에 직접 작성하지 않고 YAML, JSON과 같은 설정 파일(Configuration File)로 관리하면 동일한 프로그램을 다양한 로봇에서 그대로 사용할 수 있다.

사용자 인터페이스(User Interface)도 대부분 공통으로 사용할 수 있다. 운영자는 로봇 종류와 관계없이 상태(Status), 미션(Mission), 지도(Map), 진단(Diagnostics), 로그(Log), 경보(Alarm)를 확인해야 한다. 따라서 대시보드(Dashboard), 웹 인터페이스(Web Interface), 모바일 앱(Mobile App)은 공통 플랫폼으로 개발하고, 로봇별 데이터만 변경하는 구조가 효율적이다.

클라우드 로보틱스(Cloud Robotics)는 공통 소프트웨어의 활용 범위를 더욱 확대한다. 플릿 관리, 원격 진단(Remote Diagnostics), OTA(Over-the-Air) 업데이트, AI 모델 배포(Model Deployment), 디지털 트윈 동기화(Digital Twin Synchronization), 운영 데이터 분석(Data Analytics)은 모든 로봇이 공통적으로 필요로 하는 기능이다. 따라서 클라우드 서비스는 제품군 전체가 공유하는 플랫폼으로 구축하는 것이 바람직하다.

시뮬레이션(Simulation)도 대표적인 공통 컴포넌트이다. Gazebo, Isaac Sim, Webots, Unity 기반 디지털 트윈은 로봇 종류와 관계없이 동일한 소프트웨어를 실행할 수 있다. 하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)을 적용하면 실제 센서 대신 가상 센서(Virtual Sensor)를 연결하여 동일한 응용 프로그램을 시뮬레이션과 실제 환경에서 동시에 사용할 수 있다.

HAL은 공통 소프트웨어를 구현하는 핵심 기반 기술이다. 센서, 액추에이터(Actuator), 저장 장치(Storage), 통신 장치(Communication Device), 타이머(Timer)는 모두 HAL을 통해 접근하고, 상위 응용 프로그램은 HAL 인터페이스만 사용한다. 바퀴를 다리로 바꾸거나, 카메라를 다른 제조사 제품으로 교체해도 상위 알고리즘은 수정할 필요가 없다.

ROS 2(Robot Operating System 2)는 이러한 공통 컴포넌트 구조를 가장 잘 지원하는 미들웨어이다. 인터페이스 패키지(Interface Package)는 메시지와 서비스를 정의하고, 코어(Core) 패키지는 알고리즘을 구현하며, 하드웨어 패키지는 센서와 액추에이터를 연결하고, 애플리케이션(Application) 패키지는 이들을 조합하여 하나의 로봇을 구성한다. 이러한 계층 구조는 매우 높은 재사용성을 제공한다.

플러그인 아키텍처(Plugin Architecture)는 유연성을 더욱 향상시킨다. 위치 추정(Localization), 내비게이션(Navigation), AI 추론, 보행 제어(Locomotion), 조작 제어(Manipulation)는 모두 동일한 인터페이스를 구현하는 플러그인으로 구성할 수 있다. 따라서 AMR에서는 이동 플러그인을, 매니퓰레이터에서는 역기구학(Inverse Kinematics) 플러그인을, 휴머노이드에서는 전신 제어(Whole Body Control) 플러그인을 선택적으로 사용할 수 있다.

컴포넌트(Component) 기반 아키텍처는 확장성을 크게 높인다. 인식(Perception), 위치 추정(Localization), 계획(Planning), 통신(Communication), AI, 진단(Diagnostics), 미션 관리(Mission Management)는 각각 독립적인 컴포넌트로 개발된다. 소형 교육용 로봇은 필요한 컴포넌트만 사용하고, 대형 산업용 로봇은 동일한 컴포넌트를 여러 프로세서와 클라우드에 분산 배치할 수 있다.

테스트(Test) 역시 공통 구조를 활용할 수 있다. 단위 테스트(Unit Test), 시뮬레이션(Simulation), HIL(Hardware-in-the-Loop), 회귀 테스트(Regression Test)는 로봇 종류와 관계없이 동일한 테스트 프레임워크(Test Framework)를 사용할 수 있다. 실제 하드웨어와 관련된 부분만 별도로 검증하면 되므로 개발 효율이 크게 향상된다.

보안(Security)도 공통 플랫폼으로 구축하는 것이 효과적이다. 인증(Authentication), 암호화(Encryption), 인증서 관리(Certificate Management), OTA 업데이트, 무결성 검증(Integrity Verification), 접근 제어(Access Control)는 특정 로봇에 종속되지 않으므로 하나의 보안 프레임워크(Security Framework)로 통합할 수 있다.

공통 소프트웨어 컴포넌트의 가장 큰 장점은 **확장성(Scalability)**이다. 기업은 교육용 로봇, AMR, 검사 로봇, 산업용 매니퓰레이터, 사족보행 로봇, 휴머노이드, 서비스 로봇 등 다양한 제품을 개발한다. 하드웨어는 다르지만 내비게이션, AI, 통신, 진단, 클라우드 연동은 대부분 동일하므로, 하나의 소프트웨어 플랫폼을 기반으로 여러 제품을 개발하면 개발 비용과 유지보수 비용을 크게 절감할 수 있다.

미래의 로봇은 이동(Mobility), 조작(Manipulation), 인공지능(AI), 클라우드(Cloud), 사람과의 협업(Human Collaboration)을 하나의 시스템으로 통합하는 방향으로 발전할 것이다. 따라서 AMR, 매니퓰레이터, 사족보행, 휴머노이드의 구분은 점차 줄어들고, 동일한 소프트웨어 플랫폼 위에서 다양한 하드웨어가 동작하는 형태가 될 가능성이 매우 높다.

결국 **AMR, 매니퓰레이터, 사족보행, 휴머노이드를 위한 공통 소프트웨어 컴포넌트(Common Software Components)**는 차세대 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)**를 구현하기 위한 핵심 아키텍처 전략이다. 인식, 위치 추정, 계획, AI, 통신, 진단, 안전, 클라우드, 미션 관리와 같은 기능을 공통 플랫폼으로 구축하면 소프트웨어를 기업의 핵심 자산으로 축적할 수 있으며, 이를 통해 개발 기간 단축, 유지보수 비용 절감, 높은 재사용성(Reusability), 뛰어난 확장성(Scalability), 그리고 지속 가능한 로봇 소프트웨어 생태계(Software Ecosystem)를 구축할 수 있다.

## 11.08 Multi-Target Build Configuration in CI/CD [w/Code]

![](images/image9.png){width="7.268055555555556in" height="7.268055555555556in"}

**다중 대상 빌드 구성(Multi-Target Build Configuration)**은 하나의 소스 코드(Source Code)로 다양한 프로세서와 운영체제에 맞는 실행 파일을 자동 생성하는 개발 전략이다. 현대의 로봇 소프트웨어는 하나의 플랫폼만 지원하지 않는다. 개발은 x86 기반 Ubuntu에서 수행하고, 실제 자율주행은 ARM 기반 임베디드 리눅스(Embedded Linux)에서 실행하며, 인공지능(AI)은 GPU(Graphics Processing Unit)를 사용하는 산업용 PC에서 수행하고, 시뮬레이션(Simulation)은 별도의 개발 환경에서 실행되는 것이 일반적이다. 따라서 이러한 다양한 플랫폼을 자동으로 지원하는 **CI/CD(Continuous Integration / Continuous Deployment)** 환경이 필수적인 요소가 되었다.

**지속적 통합(CI, Continuous Integration)**은 단순히 코드를 자동으로 컴파일하는 기술이 아니다. 개발자가 코드를 저장소(Repository)에 반영하면 즉시 전체 프로젝트를 컴파일하고, 의존성(Dependency)을 확인하며, 인터페이스를 생성하고, 정적 분석(Static Analysis), 단위 테스트(Unit Test), 통합 테스트(Integration Test), 시뮬레이션 검증(Simulation Validation), 문서 생성(Document Generation), 패키징(Packaging)까지 자동으로 수행하는 개발 방식이다. 이를 통해 문제를 초기에 발견하고 소프트웨어 품질을 지속적으로 유지할 수 있다.

**지속적 배포(CD, Continuous Deployment)**는 검증이 완료된 소프트웨어를 자동으로 배포하는 기술이다. 과거에는 개발자가 직접 실행 파일을 복사하여 설치했지만, 현대의 CI/CD는 ROS 2 패키지(ROS 2 Package), Docker 이미지(Image), 펌웨어(Firmware), 클라우드 서비스(Cloud Service), OTA(Over-the-Air) 업데이트 파일을 자동 생성하고 배포한다. 이러한 자동화는 사람의 실수를 줄이고 배포 시간을 크게 단축한다.

현대의 로봇 시스템은 매우 다양한 실행 환경을 가진다. Ubuntu 기반 x86 개발 PC, ARM 기반 Jetson, 산업용 x86 Edge Computer, RTOS 기반 마이크로컨트롤러(Microcontroller), 클라우드 서버(Cloud Server), 디지털 트윈(Digital Twin), AI 서버(AI Server)가 하나의 프로젝트 안에서 함께 사용된다. 따라서 플랫폼마다 별도의 빌드(Build)를 수행하는 방식은 유지보수가 매우 어려워지므로, 하나의 CI/CD 파이프라인(Pipeline)에서 모든 플랫폼을 자동으로 처리하는 것이 현대적인 개발 방식이다.

다중 대상 빌드의 가장 중요한 원칙은 **하나의 저장소(One Source Repository)**를 유지하는 것이다. 플랫폼마다 소스 코드를 복사하거나 별도로 관리하지 않고, 동일한 저장소에서 ARM, x86, RISC-V, Windows, Linux, Simulation용 실행 파일을 모두 생성한다. 플랫폼 차이는 빌드 설정(Build Configuration), Toolchain, HAL(Hardware Abstraction Layer), 설정 파일(Configuration File)에서만 관리한다.

버전 관리 시스템(Version Control System)은 CI/CD의 기반이다. 모든 소스 코드(Source Code), 설정(Configuration), 라이브러리(Library), 문서(Document), 빌드 스크립트(Build Script)는 Git과 같은 저장소에서 관리된다. 브랜치(Branch), 병합(Merge Request), 코드 리뷰(Code Review), 태그(Tag), 버전 관리(Versioning)를 통해 어떤 실행 파일이 어떤 소스 코드에서 생성되었는지를 정확하게 추적할 수 있다. 이는 산업용 로봇과 안전 필수 시스템(Safety-Critical System)에서 매우 중요한 요소이다.

최근에는 하나의 대형 저장소(Monorepo)를 사용하는 경우가 증가하고 있다. 공통 라이브러리(Common Library), 하드웨어 드라이버(Device Driver), ROS 2 패키지, AI 모델(AI Model), 시뮬레이션, 테스트, 클라우드 서비스를 하나의 저장소에서 관리하면 공통 코드를 쉽게 재사용할 수 있으며, 변경 사항이 다른 프로젝트에 미치는 영향도 자동으로 분석할 수 있다.

빌드 시스템(Build System)은 CI/CD의 핵심 엔진이다. CMake(Cross Platform Make), Bazel(Build System), ament, colcon을 사용하면 ARM, x86, RISC-V, Windows, Linux, ROS 2, Simulation용 실행 파일을 하나의 프로젝트에서 자동 생성할 수 있다. 새로운 하드웨어가 추가되더라도 소스 코드를 수정하는 것이 아니라 Toolchain과 빌드 설정만 추가하면 된다.

CI/CD의 첫 번째 단계는 코드 검증(Source Validation)이다. 코드 스타일(Code Style), 포맷(Formatting), 의존성 검사(Dependency Check), 라이선스(License) 확인, 설정 파일 검증, 정적 분석을 수행하여 컴파일 이전에 문제를 발견한다. 이를 통해 불필요한 빌드 시간을 줄이고 개발자에게 빠른 피드백(Feedback)을 제공할 수 있다.

다음 단계는 다중 플랫폼 컴파일(Multi-Platform Compilation)이다. Linux x86, Windows, ARM, Simulation, GPU 플랫폼을 동시에 컴파일하여 모든 플랫폼에서 코드가 정상적으로 동작하는지 확인한다. 병렬 컴파일(Parallel Build)을 사용하면 여러 플랫폼을 동시에 빌드할 수 있으므로 전체 빌드 시간을 크게 줄일 수 있다.

크로스 컴파일(Cross Compilation)은 다중 대상 빌드의 핵심 기술이다. 개발자는 x86 Ubuntu에서 작업하지만 실제 실행 대상은 ARM 기반 Jetson이나 산업용 ARM 컴퓨터인 경우가 대부분이다. Toolchain, Sysroot, Cross Compiler를 이용하면 ARM 장비가 없어도 ARM용 실행 파일을 자동으로 생성할 수 있다.

컨테이너(Container)는 빌드 환경을 동일하게 유지하는 데 매우 중요한 역할을 한다. Docker(Container) 안에는 컴파일러(Compiler), 라이브러리(Library), SDK, ROS 2, CUDA 등이 모두 포함되므로 개발자의 컴퓨터와 CI 서버(Server)가 동일한 환경에서 빌드를 수행할 수 있다. 이는 "내 PC에서는 동작하지만 서버에서는 실패한다."는 문제를 거의 제거할 수 있다.

의존성 관리(Dependency Management)는 플랫폼별 라이브러리 차이를 자동으로 해결한다. ARM에서는 ARM 라이브러리를 사용하고, x86에서는 x86 라이브러리를 사용하며, GPU가 있는 경우에는 CUDA나 TensorRT를 자동으로 선택한다. 이러한 처리는 빌드 시스템이 담당하고 응용 프로그램(Application Software)은 동일한 코드를 유지한다.

테스트(Test)는 CI/CD에서 가장 중요한 단계 중 하나이다. 단위 테스트(Unit Test)는 개별 알고리즘을 검증하고, 통합 테스트(Integration Test)는 패키지 간 통신을 확인하며, 회귀 테스트(Regression Test)는 기존 기능이 정상적으로 유지되는지 검사한다. 이러한 테스트는 ARM, x86, Simulation 등 여러 플랫폼에서 동시에 수행될 수 있다.

시뮬레이션(Simulation)은 실제 로봇 없이도 대부분의 기능을 검증할 수 있는 중요한 도구이다. Gazebo, Isaac Sim, Webots, Unity 기반 디지털 트윈을 활용하면 내비게이션(Navigation), 인식(Perception), 경로 계획(Path Planning), 조작(Manipulation), 미션 수행(Mission Execution)을 자동으로 테스트할 수 있다. 실제 장비를 항상 사용할 필요가 없으므로 개발 속도가 크게 향상된다.

정적 분석(Static Analysis)은 프로그램을 실행하지 않고 오류를 찾는 기술이다. 메모리 누수(Memory Leak), 정의되지 않은 동작(Undefined Behavior), 동시성 문제(Concurrency Issue), 보안 취약점(Security Vulnerability), 코딩 규칙 위반(Coding Standard Violation)을 자동으로 분석하여 초기 단계에서 문제를 제거할 수 있다.

보안(Security)은 현대 CI/CD에서 매우 중요한 요소이다. 외부 라이브러리(Open Source Library)의 취약점을 검사하고, 소프트웨어 공급망 보안(Software Supply Chain Security)을 확인하며, 디지털 서명(Digital Signature), SBOM(Software Bill of Materials), 라이선스 검증(License Compliance)을 자동으로 수행한다. 이를 통해 안전하고 신뢰할 수 있는 소프트웨어를 배포할 수 있다.

인공지능(AI) 모델도 CI/CD에서 자동으로 관리된다. 새로운 AI 모델이 등록되면 자동으로 정확도(Accuracy), 추론 속도(Inference Speed), GPU 사용률, 메모리 사용량을 평가하고, TensorRT, ONNX Runtime 등 플랫폼에 맞게 최적화하여 배포한다. AI 모델 역시 일반 소프트웨어와 동일한 개발 생명주기(Lifecycle)를 갖게 된다.

아티팩트 관리(Artifact Management)는 빌드 결과를 체계적으로 관리하는 기술이다. 실행 파일(Binary), 라이브러리, ROS 2 패키지, Docker 이미지, AI 모델, 문서, 테스트 결과를 모두 버전별로 저장하여 언제든지 이전 버전을 다시 사용할 수 있도록 한다. 이는 제품 유지보수와 인증(Certification)에 매우 중요한 역할을 한다.

자동 배포(Deployment Automation)는 검증이 완료된 소프트웨어를 실제 시스템에 자동 설치한다. OTA 업데이트를 통해 로봇의 펌웨어를 갱신하고, Docker 이미지를 클라우드에 배포하며, ROS 2 패키지를 Edge Computer에 설치하고, 디지털 트윈 환경도 함께 갱신할 수 있다. 이러한 자동화는 운영 효율을 크게 향상시킨다.

롤백(Rollback)은 자동 배포만큼 중요한 기능이다. 새로운 소프트웨어에서 문제가 발생하면 이전에 검증된 버전으로 즉시 복구할 수 있어야 한다. CI/CD는 모든 버전의 실행 파일과 설정을 저장하고 있으므로 안정적인 이전 버전으로 자동 복귀하는 기능을 제공할 수 있다.

설정 관리(Configuration Management)는 실행 환경마다 다른 파라미터(Parameter)를 관리한다. 센서 보정(Calibration), 통신 주소(Network Address), 제어 이득(Control Gain), AI 모델(Model), 안전 한계(Safety Limit)는 소스 코드가 아니라 설정 파일에서 관리한다. 따라서 동일한 실행 파일을 여러 로봇에서 그대로 사용할 수 있다.

ROS 2(Robot Operating System 2)는 CI/CD와 매우 잘 결합된다. colcon Build, ament, 메시지(Message) 생성, Launch 파일 검증, 파라미터(Parameter) 확인, 시뮬레이션 테스트를 모두 자동 수행할 수 있다. 수백 개의 ROS 2 패키지를 관리하는 대규모 프로젝트에서도 높은 생산성을 유지할 수 있다.

기업이 여러 제품을 동시에 개발할 경우 CI/CD의 중요성은 더욱 커진다. 교육용 로봇, AMR(Autonomous Mobile Robot), 산업용 매니퓰레이터(Manipulator), 사족보행 로봇(Quadruped), 휴머노이드(Humanoid), 검사 로봇(Inspection Robot)은 공통 소프트웨어를 공유하면서도 서로 다른 하드웨어를 사용한다. CI/CD는 제품별 빌드와 테스트를 자동으로 수행하여 개발 효율을 크게 향상시킨다.

성능 분석(Performance Analysis)도 자동화된다. 실행 속도(Execution Time), 메모리 사용량(Memory Usage), GPU 사용률, CPU 부하(CPU Load), 응답 시간(Response Time), 전력 소비(Power Consumption)를 지속적으로 측정하여 성능 저하를 조기에 발견할 수 있다.

품질 지표(Quality Metric)는 프로젝트 상태를 수치로 관리한다. 테스트 성공률(Test Success Rate), 코드 커버리지(Code Coverage), 결함 밀도(Defect Density), 코드 복잡도(Code Complexity), 빌드 성공률(Build Success Rate), 보안 상태(Security Status)를 지속적으로 분석하여 소프트웨어 품질을 객관적으로 관리할 수 있다.

최근에는 클라우드 기반 CI/CD가 널리 사용되고 있다. 클라우드 서버에서 대규모 병렬 빌드와 시뮬레이션을 수행하고, AI 학습 서버와 연계하며, 중앙 저장소에서 모든 빌드 결과를 관리한다. 이를 통해 글로벌 개발팀도 동일한 개발 환경을 공유할 수 있다.

앞으로 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)** 시대에는 AI 가속기(AI Accelerator), 디지털 트윈(Digital Twin), 클라우드 로보틱스(Cloud Robotics), 엣지 컴퓨팅(Edge Computing)이 함께 사용될 것이다. 이러한 환경에서는 다중 대상 CI/CD가 모든 플랫폼을 자동으로 연결하는 핵심 인프라가 된다.

결국 **CI/CD 기반 다중 대상 빌드 구성(Multi-Target Build Configuration in CI/CD)**은 단순한 자동 빌드 기술이 아니라 현대 로봇 소프트웨어 개발의 핵심 아키텍처이다. 하나의 저장소에서 ARM, x86, RISC-V, RTOS, Linux, Simulation, Cloud를 동시에 지원하고, 자동 빌드(Build), 테스트(Test), 검증(Validation), 배포(Deployment), 롤백(Rollback), 성능 분석(Performance Analysis), 보안(Security), AI 모델 관리(AI Model Management)를 통합함으로써 높은 품질(Quality), 뛰어난 이식성(Portability), 유지보수성(Maintainability), 확장성(Scalability)을 확보할 수 있으며, 차세대 로봇 플랫폼 개발의 핵심 기반 기술로 자리잡게 될 것이다.

## 11.09 HIL Cross-Platform Test Environment

![](images/image10.png){width="7.268055555555556in" height="7.268055555555556in"}

**HIL(Hardware-in-the-Loop) 교차 플랫폼(Cross-Platform) 테스트 환경(Test Environment)**은 크로스 플랫폼 소프트웨어(Cross-Platform Software) 설계의 마지막 검증 단계이다. 하드웨어 추상화(Hardware Abstraction), 크로스 플랫폼 빌드(Build), 프로세서 이식성(Processor Portability), 공통 소프트웨어 컴포넌트(Common Software Component), CI/CD(Continuous Integration / Continuous Deployment)를 모두 구축하더라도 실제 하드웨어에서 동일하게 동작하는지 검증하지 않으면 완전한 시스템이라 할 수 없다. HIL은 실제 하드웨어와 시뮬레이션(Simulation)을 결합하여 다양한 플랫폼에서 동일한 소프트웨어가 정상적으로 동작하는지를 검증하는 핵심 기술이다.

현대의 로봇 시스템은 단일 하드웨어만 사용하는 시대를 넘어섰다. **자율이동로봇(AMR, Autonomous Mobile Robot)**, 매니퓰레이터(Manipulator), 사족보행 로봇(Quadruped), 휴머노이드(Humanoid), 검사 로봇(Inspection Robot), 농업 로봇(Agricultural Robot), 협동로봇(Collaborative Robot)은 서로 다른 프로세서와 운영체제, 센서(Sensor), GPU(Graphics Processing Unit), RTOS(Real-Time Operating System), 클라우드(Cloud)를 사용한다. 따라서 하나의 플랫폼에서만 테스트하는 것은 충분하지 않으며, 여러 하드웨어를 동시에 검증할 수 있는 HIL 환경이 필요하다.

HIL은 일반적인 소프트웨어 테스트와는 근본적으로 다르다. 단위 테스트(Unit Test)는 함수(Function)를 검증하고, 통합 테스트(Integration Test)는 모듈(Module) 간 통신을 검증하며, 시뮬레이션은 알고리즘을 가상 환경에서 확인한다. 반면 HIL은 실제 모터 제어기(Motor Controller), 센서, 통신 장치, 임베디드 보드(Embedded Board)와 연결하여 실제 전기 신호(Electrical Signal)를 사용하면서 나머지 환경은 가상으로 구성한다. 따라서 실제 장비를 모두 준비하지 않아도 현실과 매우 유사한 환경에서 테스트를 수행할 수 있다.

HIL의 가장 중요한 목적은 실제 로봇을 제작하기 전에 소프트웨어의 안정성을 검증하는 것이다. 타이밍(Timing) 오류, 통신(Communication) 문제, 센서 인터페이스(Sensor Interface), 제어 알고리즘(Control Algorithm), 실시간 성능(Real-Time Performance)을 연구실 환경에서 미리 확인할 수 있다. 이를 통해 현장(Field)에서 발생할 수 있는 문제를 크게 줄이고 개발 기간을 단축할 수 있다.

크로스 플랫폼 HIL은 ARM Cortex-M, ARM Cortex-A, x86 산업용 PC, GPU 서버(Server), RISC-V 프로세서 등 다양한 하드웨어를 하나의 테스트 환경으로 통합한다. 동일한 응용 프로그램(Application Software)을 각각의 플랫폼에서 실행하고 결과를 비교함으로써 플랫폼 독립성(Platform Independence)을 지속적으로 검증할 수 있다.

HIL은 하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)의 검증에도 매우 효과적이다. 응용 프로그램은 HAL 인터페이스만 사용하므로 실제 카메라(Camera) 대신 가상 카메라(Virtual Camera)를 연결하거나, 실제 LiDAR 대신 시뮬레이션 데이터를 입력할 수 있다. 응용 프로그램은 이를 구분하지 못하기 때문에 실제 환경과 거의 동일한 조건에서 검증이 가능하다.

프로세서(Processor)마다 성능과 특성이 다르기 때문에 HIL은 플랫폼별 차이를 정량적으로 분석할 수 있다. ARM은 저전력(Power Efficiency)에 최적화되어 있고, x86은 높은 연산 성능을 제공하며, GPU는 AI 추론(AI Inference)에 적합하다. HIL은 동일한 기능을 여러 플랫폼에서 실행하여 실행 시간(Execution Time), 지연 시간(Latency), CPU 사용률(CPU Utilization), 메모리 사용량(Memory Usage)을 비교할 수 있도록 한다.

HIL 시스템은 여러 계층(Layer)으로 구성된다. 가장 아래에는 실제 임베디드 하드웨어(Embedded Hardware), 모터 제어기, 배터리 관리 시스템(Battery Management System), 안전 제어기(Safety Controller), 센서 인터페이스가 위치한다. 그 위에는 시뮬레이터(Simulator)가 가상의 환경(Environment), 센서 데이터, 차량 동역학(Vehicle Dynamics), 디지털 트윈(Digital Twin)을 생성한다. 상위 계층에서는 테스트 실행(Test Execution), 데이터 기록(Data Logging), 성능 분석(Performance Analysis), 오류 주입(Fault Injection)을 수행하며, 클라우드는 전체 테스트를 관리한다.

시간 동기화(Time Synchronization)는 HIL에서 가장 중요한 요소 중 하나이다. 로봇 제어는 센서 데이터, 제어 주기(Control Cycle), 타임스탬프(Timestamp), 통신 지연(Communication Delay)에 매우 민감하다. 따라서 HIL 환경에서는 PTP(Precision Time Protocol), 하드웨어 타이머(Hardware Timer), 공통 시계(Common Clock)를 이용하여 모든 장치를 동일한 시간 기준으로 동작시킨다.

센서 시뮬레이션(Sensor Simulation)은 HIL의 핵심 기능이다. 카메라, LiDAR, IMU(Inertial Measurement Unit), GNSS(Global Navigation Satellite System), 레이더(Radar), 초음파(Ultrasonic), 엔코더(Encoder), 힘 센서(Force Sensor), 열화상(Thermal Camera)를 수학적으로 모델링하여 실제 센서와 동일한 신호를 생성한다. 이를 이용하면 악천후(Bad Weather), 저조도(Low Light), 센서 고장(Sensor Failure), 전자파 간섭(Electromagnetic Interference)과 같은 다양한 상황을 반복적으로 시험할 수 있다.

액추에이터 시뮬레이션(Actuator Simulation)은 실제 기계(Mechanical System)를 대신하여 모터, 관절(Joint), 바퀴(Wheel), 다리(Leg)의 동작을 수학적으로 계산한다. 실제 제어기(Control Unit)는 물리적인 장비가 연결되어 있다고 인식하지만 실제로는 가상의 기계 모델과 연결되어 있으므로 위험 없이 제어 알고리즘을 검증할 수 있다.

인공지능(AI) 역시 HIL 환경에서 중요한 검증 대상이다. AI 추론은 GPU 기반 산업용 PC에서 실행되고, 실시간 제어는 RTOS 기반 MCU에서 수행되는 경우가 많다. HIL은 AI 추론 시간(Inference Time), 통신 지연, 제어 응답(Response Time), GPU 사용률, CPU 부하를 동시에 측정하여 AI와 제어 시스템이 함께 안정적으로 동작하는지를 확인한다.

통신(Communication) 검증도 HIL의 주요 역할이다. CAN, CAN FD, EtherCAT, Ethernet, DDS(Data Distribution Service), MQTT(Message Queuing Telemetry Transport), UART, SPI, I2C 등을 실제 하드웨어와 연결하여 통신 지연, 패킷 손실(Packet Loss), 대역폭(Bandwidth), 오류 복구(Error Recovery)를 반복적으로 시험할 수 있다.

ROS 2(Robot Operating System 2)는 HIL 환경과 매우 잘 결합된다. 실제 하드웨어 인터페이스 노드(Node)는 실제 센서와 연결되고, 일부 노드는 시뮬레이션 데이터를 사용하며, 내비게이션(Navigation), 인식(Perception), AI, 진단(Diagnostics)은 그대로 실행된다. 동일한 ROS 2 프로그램을 실제 로봇과 HIL에서 동시에 사용할 수 있다는 점이 큰 장점이다.

오류 주입(Fault Injection)은 HIL만의 강력한 기능이다. 센서 고장, 통신 단절, 모터 이상, 배터리 저전압(Low Voltage), CPU 과부하, 메모리 부족(Memory Exhaustion), 네트워크 지연(Network Delay) 등을 인위적으로 발생시켜 소프트웨어가 얼마나 안정적으로 복구하는지를 반복적으로 검증할 수 있다.

안전(Safety) 검증은 HIL이 가장 많이 활용되는 분야이다. 비상 정지(Emergency Stop), 워치독(Watchdog), 이중화(Redundancy), 안전 제어(Safe Motion), 충돌 감지(Collision Detection), 안전 종료(Safe Shutdown)를 반복적으로 시험할 수 있으며, 실제 사람이나 장비를 위험에 노출시키지 않고도 안전 기능을 충분히 검증할 수 있다.

성능 분석(Performance Analysis)은 HIL을 통해 매우 정확하게 수행된다. CPU 사용률, 메모리 사용량, AI 추론 시간, 통신 지연, 센서 처리 속도, 모터 응답 시간, 전력 소비(Power Consumption), 온도(Thermal Behavior)를 실제 환경과 거의 동일한 조건에서 측정할 수 있다.

자동화된 테스트(Automated Test)는 HIL의 중요한 특징이다. 사람이 직접 테스트하지 않아도 테스트 시나리오(Test Scenario)를 실행하고, 데이터를 수집하며, 결과를 비교하고, 보고서를 생성할 수 있다. 이러한 자동화는 CI/CD와 결합되어 소스 코드가 변경될 때마다 HIL 테스트를 자동 수행할 수 있게 한다.

CI/CD와 HIL을 연계하면 소프트웨어가 컴파일된 직후 자동으로 HIL 장비에 배포되고, 시뮬레이션과 실제 하드웨어를 동시에 실행하여 결과를 비교한다. 테스트가 실패하면 배포를 중단하고, 성공한 경우에만 실제 로봇으로 배포하는 품질 게이트(Quality Gate)를 구축할 수 있다.

디지털 트윈(Digital Twin)은 HIL의 성능을 더욱 향상시킨다. 실제 로봇과 동일한 가상 모델을 유지하면서 센서 보정(Calibration), 환경 변화(Environment Change), AI 모델(Model), 유지보수(Maintenance) 정보를 지속적으로 동기화한다. 따라서 HIL은 단순한 시뮬레이션이 아니라 실제 로봇과 동일한 상태를 유지하는 가상 복제 시스템이 된다.

클라우드(Cloud)는 여러 지역에 있는 HIL 장비를 하나의 플랫폼으로 통합할 수 있다. 개발자는 원격(Remote)으로 테스트를 예약하고, 실행하며, 결과를 분석하고, 품질 지표(Quality Metric)를 관리할 수 있다. 이를 통해 글로벌 개발 조직에서도 동일한 HIL 환경을 공유할 수 있다.

데이터 기록(Data Logging)과 재생(Replay)은 문제 분석에 매우 유용하다. 센서 데이터, 제어 명령(Control Command), AI 결과, 통신 메시지, 오류 로그(Log)를 모두 저장하였다가 동일한 상황을 반복 재현할 수 있다. 이는 버그 수정(Debugging), 회귀 테스트(Regression Test), AI 데이터셋 생성에도 활용된다.

확장성(Scalability)은 HIL 설계에서 매우 중요한 요소이다. 교육용 로봇, AMR, 매니퓰레이터, 사족보행 로봇, 휴머노이드, 검사 로봇, 농업 로봇 모두 동일한 HIL 프레임워크를 사용할 수 있어야 한다. 이를 위해 모듈화(Modularization), HAL, 공통 인터페이스(Common Interface), 파라미터 기반 설정(Parameter-Based Configuration)을 적용하는 것이 바람직하다.

보안(Security) 검증도 HIL에서 수행할 수 있다. 인증(Authentication), 암호화(Encryption), OTA 업데이트, 접근 제어(Access Control), 보안 부팅(Secure Boot), 펌웨어 무결성(Firmware Integrity), 사이버 공격(Cyber Attack) 시나리오를 실제 환경과 유사하게 재현하여 시스템의 보안성을 검증할 수 있다.

앞으로의 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)** 시대에는 임베디드 제어기(Embedded Controller), AI 가속기(AI Accelerator), 클라우드, 디지털 트윈, 엣지 컴퓨팅(Edge Computing)이 하나의 시스템으로 통합될 것이다. 이러한 복잡한 환경에서는 HIL 기반의 크로스 플랫폼 테스트가 실제 배포 이전에 시스템을 검증하는 가장 중요한 기술이 될 것이다.

결국 **HIL 교차 플랫폼 테스트 환경(HIL Cross-Platform Test Environment)**은 단순한 테스트 도구가 아니라 현대 로봇 소프트웨어 개발의 최종 검증 플랫폼이다. **HAL(Hardware Abstraction Layer)**, **ROS 2**, **크로스 플랫폼 빌드(Cross-Platform Build)**, **CI/CD**, **디지털 트윈(Digital Twin)**, **AI**, **자동화 테스트(Automated Test)**를 하나의 통합 검증 체계로 연결하여 실제 하드웨어와 가상 환경을 동시에 활용함으로써, 높은 소프트웨어 품질(Quality), 안전성(Safety), 이식성(Portability), 유지보수성(Maintainability), 그리고 신뢰성(Reliability)을 확보하는 차세대 로봇 개발의 핵심 기반 기술이 된다.

## 11.10 Cross-Platform SW Maintenance Strategy

![](images/image11.png){width="7.268055555555556in" height="7.268055555555556in"}

**크로스 플랫폼 소프트웨어 유지보수 전략(Cross-Platform Software Maintenance Strategy)**은 크로스 플랫폼 소프트웨어 설계(Cross-Platform Software Design)의 마지막 단계이자 장기적인 소프트웨어 생명주기(Lifecycle)를 위한 핵심 전략이다. 하드웨어 추상화(Hardware Abstraction), 프로세서 이식성(Processor Portability), ROS 2 패키지 설계, 공통 소프트웨어 컴포넌트(Common Software Component), CI/CD, HIL(Hardware-in-the-Loop) 검증까지 모두 구축하더라도, 시간이 지나면서 소프트웨어를 안정적으로 유지하고 발전시키지 못한다면 초기 아키텍처의 가치는 크게 감소한다. 따라서 유지보수는 개발 이후의 작업이 아니라 처음부터 함께 고려해야 하는 핵심 설계 요소이다.

현대의 로봇 시스템은 일반적으로 10년에서 20년 이상 운영된다. 이 기간 동안 프로세서(Processor)는 ARM에서 새로운 AI 가속기(AI Accelerator)로 변경될 수 있고, 운영체제(Operating System)는 새로운 버전으로 업그레이드되며, 통신 프로토콜(Communication Protocol), AI 프레임워크(AI Framework), 센서(Sensor), GPU(Graphics Processing Unit)도 지속적으로 발전한다. 따라서 특정 기술에 종속되지 않는 구조를 설계해야 장기간 안정적으로 유지할 수 있다.

과거에는 로봇 제품마다 독립적인 소프트웨어를 개발하는 경우가 많았다. ARM 플랫폼용 소프트웨어, x86 플랫폼용 소프트웨어, 고객 전용(Custom) 버전이 각각 별도로 유지되면서 동일한 기능이 여러 프로젝트에 중복 구현되었다. 시간이 지날수록 버전 차이가 커지고 수정 사항을 모든 프로젝트에 반영하기 어려워졌으며, 결국 새로운 기능 개발보다 유지보수에 더 많은 비용이 투입되는 문제가 발생하였다.

현대의 유지보수 전략은 소프트웨어를 단순한 프로젝트 결과물이 아니라 기업의 장기적인 자산(Software Asset)으로 관리하는 것을 목표로 한다. 새로운 하드웨어나 운영체제가 등장하더라도 전체 시스템을 다시 개발하는 것이 아니라 필요한 부분만 수정하여 지속적으로 발전시킬 수 있도록 설계해야 한다.

가장 중요한 원칙은 **안정적인 인터페이스(Stable Interface)**와 **변화하는 구현(Implementation)**을 분리하는 것이다. 미션 관리(Mission Management), 내비게이션(Navigation), 인공지능(AI), 안전(Safety), 통신(Communication)과 같은 상위 기능은 장기간 유지되지만, 운영체제 API(Application Programming Interface), 컴파일러(Compiler), 드라이버(Driver), AI 라이브러리(Library)는 계속 변경된다. 따라서 이러한 변화는 추상화 계층(Abstraction Layer) 내부에서만 처리하고, 응용 프로그램(Application Software)은 그대로 유지하는 것이 이상적인 구조이다.

하드웨어 추상화 계층(Hardware Abstraction Layer, HAL)은 장기 유지보수의 핵심이다. 카메라(Camera), LiDAR, IMU(Inertial Measurement Unit), GNSS(Global Navigation Satellite System), 모터(Motor), 저장 장치(Storage), AI 가속기 등을 HAL을 통해 접근하도록 설계하면 새로운 장비로 교체하더라도 HAL 내부만 수정하면 된다. 상위 소프트웨어는 하드웨어 변경을 거의 인식하지 않는다.

운영체제 추상화(Operating System Abstraction)도 매우 중요하다. Linux, RTOS(Real-Time Operating System), Windows, 미래의 새로운 운영체제가 등장하더라도 스레드(Thread), 타이머(Timer), 동기화(Synchronization), 파일(File), 통신(Network), 로그(Log)는 동일한 인터페이스를 통해 접근하도록 설계한다. 운영체제가 변경되어도 응용 프로그램은 거의 수정하지 않아도 된다.

모듈화(Modularization)는 유지보수성을 크게 향상시킨다. 대형 모놀리식(Monolithic) 프로그램은 수정 범위가 매우 넓어지지만, 인식(Perception), 위치 추정(Localization), 지도(Map), 계획(Planning), AI, 통신, 진단(Diagnostics), 안전, 미션 관리 등을 독립적인 모듈(Module)로 구성하면 필요한 부분만 수정하거나 교체할 수 있다. 이는 유지보수 비용을 크게 줄여준다.

인터페이스(Interface)는 구현 세부사항이 아니라 기능(Function)을 중심으로 설계해야 한다. 예를 들어 센서 인터페이스는 레지스터(Register)가 아니라 "영상(Image)"이나 "거리(Point Cloud)"를 전달해야 하며, 이동 제어 인터페이스는 모터 PWM이 아니라 "목표 속도(Target Velocity)"를 전달해야 한다. 이렇게 하면 내부 구현이 변경되어도 외부 프로그램은 그대로 유지된다.

의존성 관리(Dependency Management)는 장기 유지보수의 핵심 요소이다. 모듈 간 의존성을 최소화하고, 인터페이스를 통해 연결하며, 플러그인(Plugin) 구조를 사용하면 특정 모듈을 수정하더라도 다른 모듈에는 영향을 거의 주지 않는다. 이는 대규모 프로젝트에서 유지보수 비용을 크게 줄이는 효과를 가져온다.

버전 관리(Version Management)는 여러 제품을 동시에 유지하기 위해 반드시 필요하다. 고객마다 서로 다른 소프트웨어 버전을 사용할 수 있으므로 인터페이스, 프로토콜, 설정(Configuration), AI 모델(Model), 패키지(Package)를 체계적으로 관리해야 한다. Semantic Versioning과 장기 지원(Long-Term Support) 정책을 적용하면 버전 간 호환성을 유지하기가 훨씬 쉬워진다.

하위 호환성(Backward Compatibility)은 유지보수 전략의 중요한 목표이다. 기존 고객이 사용하는 시스템은 새로운 소프트웨어가 출시되어도 정상적으로 동작해야 한다. 메시지(Message), API, 설정 파일(Configuration File), 데이터 형식(Data Format)은 가능하면 기존 버전과 호환되도록 유지하고, 구조 변경이 필요한 경우에는 변환 도구(Migration Tool)를 함께 제공하는 것이 바람직하다.

설정 관리(Configuration Management)는 실행 환경의 차이를 코드가 아니라 설정 파일에서 처리하도록 한다. 센서 보정(Calibration), 제어 이득(Control Gain), 통신 주소(Network Address), AI 모델, 안전 한계(Safety Limit)는 모두 YAML, JSON 등의 설정 파일에서 관리한다. 이를 통해 동일한 실행 파일(Binary)을 여러 로봇에서 사용할 수 있으며 유지보수도 쉬워진다.

문서화(Documentation)는 유지보수에서 매우 중요한 역할을 한다. 아키텍처 설명, API 문서, 설정 가이드, 테스트 절차, 배포 방법, 유지보수 매뉴얼은 소스 코드와 함께 지속적으로 업데이트되어야 한다. 오래된 문서는 없는 것보다 더 큰 문제를 일으킬 수 있으므로 CI/CD와 연계하여 자동 생성하는 것이 이상적이다.

자동 테스트(Automated Test)는 유지보수 품질을 보장하는 핵심 기술이다. 단위 테스트(Unit Test), 통합 테스트(Integration Test), 시뮬레이션(Simulation), HIL(Hardware-in-the-Loop), 성능 테스트(Performance Test), 보안 테스트(Security Test)를 자동 수행하면 새로운 기능을 추가하더라도 기존 기능이 정상적으로 유지되는지 지속적으로 확인할 수 있다.

CI/CD는 유지보수 과정에서도 필수적이다. 새로운 코드가 저장소(Repository)에 반영되면 자동으로 ARM, x86, RISC-V 플랫폼을 컴파일하고, 테스트를 수행하며, 문서를 생성하고, 패키지를 배포한다. 이를 통해 유지보수 과정에서 발생할 수 있는 오류를 초기에 발견할 수 있다.

재현 가능한 빌드(Reproducible Build)는 장기 유지보수에서 매우 중요하다. 몇 년이 지난 후에도 동일한 실행 파일을 다시 생성할 수 있어야 한다. 이를 위해 Docker(Container), Toolchain Version, Dependency Repository를 모두 버전 관리하여 언제든 동일한 환경에서 다시 빌드할 수 있도록 해야 한다.

AI 시스템은 일반 소프트웨어보다 유지보수가 더욱 어렵다. AI 모델(Model)은 지속적으로 재학습(Retraining), 최적화(Optimization), 검증(Validation), 배포(Deployment)가 필요하다. 따라서 데이터셋(Dataset), 모델 버전(Model Version), 추론 성능(Inference Performance), 롤백(Rollback)을 함께 관리하는 AI 생명주기(AI Lifecycle) 관리 체계를 구축해야 한다.

보안(Security)은 장기 유지보수에서 점점 더 중요해지고 있다. 운영체제 보안 패치(Security Patch), 암호화 라이브러리(Cryptographic Library), 인증(Authentication), 인증서(Certificate), Secure Boot, OTA 업데이트를 지속적으로 관리해야 한다. 또한 외부 오픈소스(Open Source)의 취약점도 지속적으로 확인하고 업데이트해야 한다.

관측성(Observability)은 유지보수를 효율적으로 만드는 핵심 기술이다. 로그(Log), 추적(Tracing), 시스템 상태(Health Monitoring), CPU와 메모리 사용률, AI 추론 시간, 통신 상태를 지속적으로 수집하면 장애가 발생하기 전에 이상 징후를 발견할 수 있다. 이러한 데이터는 예지 보전(Predictive Maintenance)에도 활용될 수 있다.

고장 진단(Fault Diagnosis)은 공통 인터페이스를 통해 구현하는 것이 바람직하다. AMR(Autonomous Mobile Robot), 매니퓰레이터(Manipulator), 사족보행 로봇(Quadruped), 휴머노이드(Humanoid)는 하드웨어는 다르지만 진단 구조는 동일하게 사용할 수 있다. 오류 코드(Error Code), 진단 메시지(Diagnostic Message), 복구 절차(Recovery Procedure)를 표준화하면 유지보수가 훨씬 쉬워진다.

디지털 트윈(Digital Twin)은 유지보수 전략을 한 단계 발전시킨다. 실제 로봇과 동일한 가상 모델을 유지하면서 소프트웨어 업데이트(Update), 환경 변화(Environment Change), AI 모델 변경을 먼저 검증할 수 있다. 문제가 없음을 확인한 후 실제 로봇에 적용하면 운영 위험을 크게 줄일 수 있다.

플릿 관리(Fleet Management)는 다수의 로봇을 운영하는 기업에서 매우 중요한 유지보수 기술이다. 수백 대의 로봇에 동시에 OTA 업데이트를 수행하고, 버전 관리, 상태 모니터링, 장애 복구, 설정 변경을 중앙에서 수행할 수 있어야 한다. 이를 통해 운영 비용을 크게 절감할 수 있다.

기업이 교육용 로봇, AMR, 산업용 로봇, 검사 로봇, 사족보행 로봇, 휴머노이드 등 다양한 제품군을 운영할 경우 공통 소프트웨어 플랫폼(Common Software Platform)을 기반으로 유지보수를 수행하면 개발 비용과 유지보수 비용을 동시에 줄일 수 있다.

기술적인 요소뿐 아니라 조직 문화도 중요하다. 코딩 규칙(Coding Standard), 아키텍처 리뷰(Architecture Review), 코드 리뷰(Code Review), 테스트 정책(Test Policy), 릴리스 관리(Release Management), 개발자 교육(Developer Education)을 지속적으로 운영해야 장기간 안정적인 유지보수가 가능하다.

앞으로 **소프트웨어 정의 로봇(Software-Defined Robot)**과 **물리 AI(Physical AI)** 시대에는 AI 가속기, 클라우드, 디지털 트윈, 분산 컴퓨팅(Distributed Computing), 새로운 프로세서가 계속 등장할 것이다. 따라서 특정 기술에 종속되지 않는 유지보수 전략이 기업 경쟁력을 결정하는 중요한 요소가 될 것이다.

결국 **크로스 플랫폼 소프트웨어 유지보수 전략(Cross-Platform Software Maintenance Strategy)**은 단순히 버그를 수정하는 작업이 아니라 소프트웨어의 장기적인 생명주기를 관리하는 종합적인 아키텍처 전략이다. **HAL(Hardware Abstraction Layer)**, 운영체제 추상화(OS Abstraction), 모듈화(Modularization), 안정적인 인터페이스(Stable Interface), 자동 테스트(Automated Test), **CI/CD**, 재현 가능한 빌드(Reproducible Build), 디지털 트윈(Digital Twin), AI 생명주기(AI Lifecycle), 보안(Security), 플릿 관리(Fleet Management)를 통합함으로써, 기업은 하나의 소프트웨어 플랫폼으로 여러 세대의 로봇 제품을 지속적으로 발전시키고 유지할 수 있으며, 높은 유지보수성(Maintainability), 이식성(Portability), 확장성(Scalability), 신뢰성(Reliability)을 갖춘 차세대 로봇 소프트웨어 생태계(Software Ecosystem)를 구축할 수 있다.
