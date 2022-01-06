- Spring Framework
  - DI
    <details>
    <summary>Answer</summary>
    - 의존성 주입 (Dependency Injection) <br/>
    - 객체를 직접 생성하지 않고 외부에서 주입하는 방식 <br/>
    - 외부(IoC 컨테이너)에 생성된 Bean을 주입함
    </details>
  - IoC
    <details>
    <summary>Answer</summary>
    - 제어의 역전 (Inversion of Control) <br/>
    - 프로그램의 제어를 개발자가 직접하는 것이 아니라 Framework에서 결정 <br/>
    - 의존을 역전 시켜 객체 간의 결합도를 줄이고 유연한 코드를 작성 가
    </details>
  - AOP
    <details>
    <summary>Answer</summary>
    - 관점 지향 프로그래밍 (Aspect Oriented Programming) <br/>
    - 공통 모듈을 코드 밖에서 필요한 시점에 비즈니스 로직에 삽입하여 실행 <br/>
    - Spring AOP는 프록시 패턴 기반의 구현체로 타겟 객체를 프록시로 만들어서 제공하며 프록시가 객체의 호출을 가로챈 다음 공통 모듈을 수행하고 타겟의 로직을 호출함 (반대로 가능) <br/>
    - Logging과 Transaction과 같이 중복적으로 발생하는 코드의 재사용과 효율적인 유지보수 가능
    </details>
  - Transaction
  - JVM기반의 Tunning (g1gc)
    - Thread Dump
    - Heap Dump
  - Bean Scope
  - Filter Vs Interceptor Vs AOP
  
- OOP
  - SOLID

- Design Pattern
  - Singleton
  - Strategy
    - SOLID?

- ORM
  - Hibernate
  - JPA
    - N+1

- Spring Security
  - OAuth2
    - OAuth1 Vs OAuth2
    - 인증방식 4가지
  - SSO

- Spring Cloud
  - MSA
    - 장점
    - 단점
  - Component
    - Config
    - API Gateway
    - Eureka
    - LoadBalancer
    - Resilience4j
  
- Spring Batch

- HTTP
  - 401 Vs 403

- CQRS
  - Event-Driven

- Cache
  - Global Cache Vs Local Cache
  - Spring Cache Vs Redis
  - Hit Up 방안
  - Redis
    - MemCached
    - Sub/Pub
  - Spring Cache
  - Scale In Vs Scale Out

- Database
  - Indexing
  - Tuning
  - Transaction
    - Level
    - Distributed Transaction

- System Architecture
  - failures
    - 대비책
  - Data 중심 Architecture
    - Write -> Queue
    - Read -> Cache
  - Security

- AWS
  - VPC 망분리
    - Subnet
    - Routing
    - Security Group  
    - Bastion, Public, Private

- ATDD
  - ATDD Vs TDD
  - 장점

- EFK
  - ELK Vs EFK
  - Why EFK APM?

- Reverse Proxy
  - Nginx Reverse Proxy 개선 방안
    - 정적데이터
    - HTTP/2
      - HTTP/1.1 Vs HTTP/2
      - Async?  
      - keep-alive ?
  - HTTP Caching

- CI/CD
  - How?

### 작은 것이라도 경험에 빗대어 말하는 습관 가지기
