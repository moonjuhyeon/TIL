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
    <details>
    <summary>Answer</summary>
    - @Transactional 방식의 선언적 트랜잭션으로 프록시 객체를 사용 <br/>
    - 프록시 객체는 PlatformTransactionManager를 사용하여 트랜잭션을 시작하고, 정상 여부에 따라 Commit 또는 Rollback <br/>
    - 기본 프록시 모드에서는 클래스의 메소드에서 동일 클래스의 @Transactional 걸린 메소드를 호출하면 트랜잭션이 무시 <br/>
    - 트랜잭션을 올바로 적용하려면 현재 클래스의 메소드가 아닌 다른 클래스의 메소드에 트랜잭션을 걸어야만 함 <br/>
    - 트랜잭션을 걸지 않으면 모든 SELECT 쿼리마다 commit을 하기 때문에 성능이 떨어짐. 명시적으로 트랜잭션을 걸어주면 마지막에 명시적으로 commit을 해주면 되며, commit 횟수가 줄어서 성능이 좋아짐
    </details>
    
  - JVM기반의 Tunning (g1gc)
    - Thread Dump
      <details>
      <summary>Answer</summary>
      - Thread Dump를 통해 모든 Thread가 무슨 일을 하는지 알 수 있음 <br/>
      - 애플리케이션의 Thread 상에서 나타나는 문제는 대부분 Lock으로 인해 발생 <br/>
      - 장애가 났을 때의 Heap 상태를 기록으로 남겨 그 당시에 어떤 Java 객체들이 많이 만들어졌는지 분석 <br/>
      - jstack, VisualVM, Arthas 을 사용하여 Thread Dump를 얻을 수 있음 <br/>
      - Thread 이름, 식별자, 우선순위(prio), Thread가 점유하는 메모리 주소를 의미하는 Thread ID(tid), OS에서 관리하는 Thread ID (nid), Thread 상태 (NEW | RUNNABLE | BLOCKED | WAITING | TIMED_WAITING | TERMINATED) 등의 정보를 확인 가능 <br/>
      - RUNNABLE 상태면서 지속시간이 긴 Thread가 없는지, Lock 처리가 제대로 되지 않아 문제가 발생하고 있지는 않은지 확인
      </details>
    - Heap Dump
      <details>
      <summary>Answer</summary>
      - Heap의 사용량이 순간적으로 증가하면  GC(Garbage Collection)가 과도하게 일어나면서 어플리케이션의 성능이 저해되거나, 심한 경우에는 OOM(Out Of Memory)이 발생하여 어플리케이션이 다운됨 <br/>
      - jmap을 사용하여 Heap Dump를 얻을 수 있음
      </details>
    
  - Bean Scope
    - Singleton (Default)
      <details>
      <summary>Answer</summary>
      - 애플리케이션에서 Bean 등록 시 singleton scope로 등록 <br/>
      - Spring IoC 컨테이너 당 한 개의 인스턴스만 생성 <br/>
      - 컨테이너가 Bean 가져다 주입할 때 항상 같은 객체 사용 <br/>
      - 메모리나 성능 최적화에 유리 <br/>
      </details>
    - Prototype
      <details>
      <summary>Answer</summary>
      - 컨테이너에서 Bean을 가져다 쓸 때 항상 다른 인스턴스를 사용 <br/>
      - 모든 요청에서 새로운 객체 생성 <br/>
      - gc에 의해 Bean 제거 <br/>
      </details>
    - Request
      <details>
      <summary>Answer</summary>
      - Bean 등록 시 하나의 HTTP Request 생명주기 안에 단 하나의 Bean만 존재 <br/>
      - 각각의 HTTP 요청은 고유 Bean 객체 보유 <br/>
      - Spring MVC Web Application에서 사용 <br/>
      </details>
    - Session
      <details>
      <summary>Answer</summary>
      - 하나의 HTTP Session 생명주기 안에 단 하나의 Bean만 존재 <br/>
      - Spring MVC Web Application에서 사용 <br/>
      </details>
    - Global Session
      <details>
      <summary>Answer</summary>
      - 하나의 HTTP Session 생명주기 안에 단 하나의 Bean만 존재 <br/>
      - Spring MVC Web Application에서 사용 <br/>
      </details>
    - Application
      <details>
      <summary>Answer</summary>
      - Servlet Context 안에 단 하나의 Bean만 존재 <br/>
      - Spring MVC Web Application에서 사용 <br/>
      </details>

  - Filter Vs Interceptor Vs AOP
    <details>
    <summary>Answer</summary>
    - 셋의 적용 시점이 다름 <br/>
    - filter, interceptor, aop의 순서로 적용됨 <br/>
    - filter, interceptor, aop의 순서로 적용됨 <br/>
    </details>
    - Filter
      <details>
      <summary>Answer</summary>
      - 인증, URL 필터링 등 요청(Request) 수준에서 처리할 때 사용 <br/>
      - Servlet 단위에서 실행됨 <br/>
      </details>
    - Interceptor
      <details>
      <summary>Answer</summary>
      - 요청이 이루어진 HTTP 프로토콜 수준에서 처리할 때 사용 <br/>
      - Servlet 단위에서 실행됨 <br/>
      </details>
    - AOP
      <details>
      <summary>Answer</summary>
      - 비즈니스 로직 수준에서 Logging, Transaction 등 공통 모듈을 처리할 때 사용 <br/>
      - application 메서드 단위에서 실행됨 <br/>
      </details>
    - Spring AOP
      <details>
      <summary>Answer</summary>
      - Spring 환경에서 AOP를 구현할 수 있도록하는 프록시 패턴 기반의 AOP 구현체.용 <br/>
      - 타겟 객체를 프록시로 만들어서 제공하며 프록시가 객체의 호출을 가로챈다음 부가기능 로직을 수행하고 타겟의 로직을 호출함 <br/>
      - 공통모듈을 
      </details>

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
