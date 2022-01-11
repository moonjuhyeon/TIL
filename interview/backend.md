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
    - Logging과 Transaction과 같이 중복적으로 발생하는 코드의 재사용과 효율적인 유지보수 가능 <br/>
    - 비즈니스 로직에서 공통적으로 사용하는 모듈을 관점 지향으로 사용하는 것
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

- OOP
  - SOLID
    <details>
    <summary>Answer</summary>
    - SRP(Single Responsibility Principle) : 단일 책임 원칙 <br/>
    &nbsp&nbsp&nbsp * 클래스는 단 하나의 책임을 가져야 하며 클래스를 변경하는 이유는 단 하나의 이유여야 함 <br/>
    - OCP(Open-Close Principle) : 개방 폐쇄 원칙 <br/>
    &nbsp&nbsp&nbsp * 확장에는 열려 있어야 하고 변경에는 닫혀 있어야 함 <br/>
    - LSP(Liskov Substitution Principle) : 리스코프 치환 원칙 <br/>
    &nbsp&nbsp&nbsp * 상위 타입의 객체를 하위 타입의 객체로 치환해도 상위 타입을 사용하는 프로그램은 정상적으로 동작해야 함 <br/>
    - ISP(Interface Segregation Principle) : 인터페이스 분리 원칙 <br/>
    &nbsp&nbsp&nbsp * 인터페이스는 그 인터페이스를 사용하는 클라이언트를 기준으로 분리해야 함 <br/>
    - DIP(Dependency Inversion Principle) : 의존 역전 원칙 <br/>
    &nbsp&nbsp&nbsp * 고수준 모듈은 저수준 모듈의 구현에 의존해서는 안됨 <br/>
    </details>

- Design Pattern
  - Singleton
    <details>
    <summary>Answer</summary>
    - 전역 변수를 사용하지 않고 객체를 하나만 생성하도록 하며, 생성된 객체를 어디에서든지 참조할 수 있도록 하는 디자인 패턴 <br/>
    - 생성(Creational) 패턴 중 하나로, 객체의 생성과 조합을 캡슐화해 특정 객체가 생성되거나 변경되어도 프로그램 구조에 영향을 크게 받지 않는 유연형 제공 <br/>
    - private 생성자를 사용하여 상속이 불가능하고, 다중 스레드 환경에서 인스턴스가 1개 이상 생성되는 경우가 발생할 수 있음 <br/>
    </details>

  - Strategy
    <details>
    <summary>Answer</summary>
    - 같은 문제를 해결하는 여러 알고리즘이 클래스별로 캡슐화되어 있고 이들이 필요할 때 교체할 수 있도록 함으로써 동일한 문제를 다른 알고리즘으로 해결할 수 있게 하는 디자인 패턴 <br/>
    - 행위(Behavioral) 패턴 중 하나로,한 객체가 혼자 수행할 수 없는 작업을 여러 개의 객체로 어떻게 분배하는지, 또 그렇게 하면서도 객체 사이의 결합도를 최소화하는 것에 중점 <br/>
    - 기존 코드의 변경을 최소화 하면서 기능을 추가할 수 있기 때문에 개방 폐쇄 원칙 (OCP)을 만족함
    </details>

- ORM
  - Hibernate
    <details>
    <summary>Answer</summary>
    - ORM 기술에 대한 명세인 JPA(Java Persistence API)의 구현체의 한 종류로 JPA 인터페이스를 구현하며, 내부적으로 JDBC API를 사용 <br/>
    - JPA의 SessionFactory, Session, Transaction으로 상속받고 각각 Impl로 구현함 <br/>
    - JPA는 추상화된 데이터 접근 계층을 제공하기 때문에 특정 벤더에 종속적이지 않음
    </details>

  - Spring Data JPA
    <details>
    <summary>Answer</summary>
    - Spring에서 제공하는 모듈로 JPA를 한 단계 추상화시킨 Repository라는 인터페이스를 제공함 <br/>
    - 사용자가 Repository 인터페이스에 정해진 규칙대로 메소드를 입력하면, Spring이 알아서 해당 메소드 이름에 적합한 쿼리를 날리는 구현체를 만들어서 Bean으로 등록함 <br/>
    - 공통 메소드가 아닐 경우에도 스프링 데이터 JPA가 메소드 이름을 분석해서 JPQL을 실행
    </details>
    
    - N+1
      <details>
      <summary>Answer</summary>
      - 원인 <br/>
      &nbsp&nbsp&nbsp - 두 개의 엔티티가 1:N의 관계를 가지며 JPQL로 조회할 때 <br/>
      &nbsp&nbsp&nbsp - EAGER 전략으로 데이터를 가져오는 경우 <br/>
      &nbsp&nbsp&nbsp - LAZY 전략으로 데이터를 가져온 이후에 가져온 데이터에서 하위 엔티티를 다시 조회하는 경우 <br/>
      - 해결방법 <br/>
      &nbsp&nbsp&nbsp - fetch join <br/>
      &nbsp&nbsp&nbsp - batch size <br/>
      &nbsp&nbsp&nbsp - entity graph <br/>
      </details>

- Spring Security
  - OAuth2
    - OAuth1 Vs OAuth2
      <details>
      <summary>Answer</summary>
      - 가장 큰 차이점은 Request Token이 Refresh Token으로 대체되어 토큰의 유효기간이 생겼다는 점이라고 생각, 또한 OAuth2는 HTTPS 기반의 서명을 지원함
      </details>
      
    - 인증방식 4가지
      <details>
      <summary>Answer</summary>
      - 권한 부여 코드 승인 타입 (Authorization Code Grant Type) <br/>
      &nbsp&nbsp&nbsp * 소셜 미디어들이 웹 서버 형태의 클라이언트를 지원하는데 사용하는 방식 <br/>
      &nbsp&nbsp&nbsp * 웹 서버에서 장기 액세스 토큰(long-lived access token)을 사용하여 사용자 인증을 처리 <br/>
      - 암시적 승인 타입 (Implicit Grant Type) <br/>
      &nbsp&nbsp&nbsp * 권한 부여 코드 승인 타입과 다르게 권한 부여 코드 없이 사용자 자격 증명을 교환하는 방식 <br/>
      &nbsp&nbsp&nbsp * 원래는 JavaScript에서 사용하기 위해 만들어 졌지만, 특정 상황에서만 권장 <br/>
      - 리소스 소유자 암호 자격 증명 승인 타입 (Resource Owner Password Credentials Grant Type) <br/>
      &nbsp&nbsp&nbsp * 클라이언트가 암호를 사용해 엑세스 토큰에 대한 사용자의 자격 증명을 교환하는 방식 <br/>
      &nbsp&nbsp&nbsp * Id, Password를 이용해 자격 증명을 클라이언트에게 인증 요청 <br/>
      &nbsp&nbsp&nbsp * Access Token을 이용해 리소스 서버와 통신 <br/>
      - 클라이언트 자격 증명 승인 타입 (Client Credentials Grant Type) <br/>
      &nbsp&nbsp&nbsp * 클라이언트가 컨텍스트 외부에서 액세스 토큰을 얻어 특정 리로스에 접근을 요청할때 사용 <br/>
      &nbsp&nbsp&nbsp * 사용자가 앱인 경우에 활용 <br/>
      </details>

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
