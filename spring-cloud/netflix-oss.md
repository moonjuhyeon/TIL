# Netflix OSS

- Netflix OSS는 Spring Cloud 기반의 MSA를 구현하는 오픈소스 프레임워크

    ![netflix-oss-framework.webp](./assets/netflix-oss-framework.webp)

- Netfilx OSS의 구성
  - Service Discovery : Eureka
  - Circuit Breaker : Hysrix
  - Intelligent Routing : Zuul
  - Client Side Load Balancing : Ribbon
  
- Netflix OSS의 MSA 5대 분류 특징
  1. Service Discovery : Eureka
  2. Load Balancing : ~~Ribbon~~ (Spring Cloud LoadBalancer)
  3. Dynamic Routing : ~~Zuul~~ (Spring Cloud Gateway)
  4. Tracing : Zipkin 
  5. Resiliency : ~~Hystrix~~ (Resilience4j)

- Eureka
  - Service Discovery & Resistry 담당
  - MSA 내에서 각 Service들을 Resistry에 등록하고, Resistry를 기반으로 다른 서비스를 Discovery 함
  
     ![eureka](./assets/eureka.png)
  - MSA의 Service들은 Eureka Client가 되어 자신의 hostname, ip, port 등 자신의 Meta Data를 Eureka Server에 전송하여 Resistry 됨
  - Eureka Client는 Eureka Server를 통해 다른 Client의 Meta Data를 사용 할 수 있음
  - Eureka Server는 각 Client로 부터 정해진 (기본 30초) 시간 마다 Heartbeat을 수신
  - Heartbeat이 오지 않는 Service는 죽은 것으로 판단하고, Resistry에서 제거
  
- ~~Ribbon~~ (지원 종료)
  - Client Side Load Balancer 담당
  - Ribbon은 각 Service Instance들의 Load Balancing을 수행함
  - API Gateway에서도 Ribbon을 통해 Load Balancing을 수행 할 수 있으며, Ribbon을 사용하면 API Gateway가 없이 직접 Load Balancing을 수행 할 수 있음
  - Ribbon의 L/B Rules
    1. Round Robin
        - 각 Service를 돌아가며 연결하는 방식
    1. Availability Fitering Rule
         - Service 중 가용성이 높은 것 부터 연결하는 방식
         - 3번 이상 연결이 실패하면 30초 동안 연결을 하지 않음 (Ribbon 내부의 Circuit Breaker 모듈 사용, 설정 가능)
    2. Weighted Response Time Rule
         - 응답시간이 빠른 Service 부터 연결하는 방식
  - **2018년 12월 이후 EOS 되었기 때문에, Spring Cloud LoadBalancer를 사용함**

- Spring Cloud LoadBalancer
  - Ribbon과 비교

    |구분|Ribbon|SCL|
    |:----|:----|:----|
    |지원 HttpClient|Rest Template(Blocking)|Rest Template(Blocking), Web Client(Non-Blocking)|
    |지원 LB 정책| Round Robin, Availability Fitering Rule, Weighted Response Time Rule | Round Robin, Random |


