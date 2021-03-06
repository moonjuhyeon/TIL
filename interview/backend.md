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
    
    
  - Bean Scope
    - Singleton (Default)
      <details>
      <summary>Answer</summary>
      - 애플리케이션에서 Bean 등록 시 singleton scope로 등록 <br/>
      - Spring IoC 컨테이너 당 한 개의 인스턴스만 생성 <br/>
      - 컨테이너가 Bean 가져다 주입할 때 항상 같은 객체 사용 <br/>
      - Thread Safety를 자동으로 보장 <br/>
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

  - Java
    - Reflection
      <details>
        <summary>Answer</summary>
        - 구체적인 클래스 타입을 알지 못해도 그 클래스의 정보(메서드, 타입, 변수 등등)에 접근할 수 있게 해주는 자바 API <br/>
        - 자바에서는 JVM이 실행되면 사용자가 작성한 자바 코드가 컴파일러를 거쳐 바이트 코드로 변환되어 static 영역에 저장되며 Reflection API는 이 정보를 활용함 <br/>
        - Bean은 애플리케이션이 실행한 후 런타임에 객체가 호출될 때 동적으로 객체의 인스턴스를 생성하는데 이때 Spring Container의 BeanFactory에서 리플렉션을 사용함 <br/>
        - Reflection API로 가져올 수 없는 정보 중 하나가 생성자의 인자 정보, Entity에 기본 생성자가 필요한 이유도 동적으로 객체 생성 시 Reflection API를 활용함 <br/>
        </details>

    - GC
      - Stop-The-World
        <details>
        <summary>Answer</summary>
        - Full GC가 발생하면 JVM은 어플리캐이션 실행을 멈추고 GC를 실행하는 쓰레드만 작동함 <br/>
        - Full GC가 발생하면 서비스는 중단될 것이고 서비스가 중단 된 동안 각종 Time Out이 발생할 것이고 미뤄진 작업들이 누적되어 또 다른 Full GC를 발생시켜 장시간 장애가 발생함 <br/>
        - 어떤 GC알고리즘을 사용하더라도 Full GC와 STW는 발생함 <br/>
        - 대개의 경우 GC 튜닝이란 이 stop-the-world 시간을 줄이는 것 <br/>
        </details>
        
      - Parallel GC (8 Default)
        <details>
        <summary>Answer</summary>
        - Serial GC가 하나의 스레드로 Mark-Sweep-Compaction을 수행한다면, Parallel GC는 여러 개의 스레드로 Mark-Sweep-Compaction을 수행함 <br/>
        </details>
        
      - Parallel Old GC
        <details>
        <summary>Answer</summary>
        - Parallel GC와 비슷하나, Mark-Sweep-Compaction 알고리즘 대신 Mark-Summary-Compaction 알고리즘을 사용함 <br/>
        - Summary 작업은 Sweep 작업에 살아있는 객체를 식별하는 작업이 추가된 것 <br/>
        </details>    

      - CMS(Concurrent Mark Sweep) GC
        <details>
        <summary>Answer</summary>
        - GC 대상 객체를 최대한 자세히 파악한 후, Stop-The-World 가 발생하는 Sweep 시간을 최소화 하는데 초점, Low Latency GC라고도 부름 <br/>
        - 단점으로는 하는 일이 많다보니 CPU 부하가 커진다는 것 <br/>
        - Compaction이 기본적으로 제공되지 않고 필요할 때만 일어나는데, 이때의 Stop-The-World 시간이 다른 GC보다 더 길게 걸릴 수도 있음 <br/>
        </details>

      - G1(Garbage First) GC (9+ Default)
        <details>
        <summary>Answer</summary>
        - 큰 Heap 메모리에서 짧은 GC 시간을 보장하는데 그 목적을 둠 <br/>
        - 전체 Heap 영역을 Region이라는 특정한 크기로 나눠서, 각 Region의 상태에 따라 역할(Eden, Survivor, Old)이 동적으로 부여함 <br/>
        - 2048개의 Region으로 나뉠수 있으며, 옵션을 통해 1MB~32MB 사이로 지정할 수 있음 <br/>
        - Young 영역에서 GC가 수행되면 Stop-The-World 현상이 발생하며, 이 시간을 줄이기 위해 멀티 스레드로 GC를 수행함 <br/>
        </details> 

      - Z GC (11)
        <details>
        <summary>Answer</summary>
        - 정지 시간이 최대 10ms를 초과하지 않음, Heap의 크기가 증가하더라도 정지 시간이 증가하지 않음, 8MB~16TB에 이르는 다양한 범위의 Heap 처리 가능 <br/>
        - 위 세가지의 목표를 충족하기 위해 설계된 확장 가능하고 낮은 지연율(low latency)을 가진 GC <br/>
        - ZGC는 ZPages라는 G1 GC의 Region과 비슷한 영역의 개념을 사용하지만, Region은 고정된 크기인 것에 반해 ZPages는 크기가 2MB의 배수로 동적으로 생성 및 삭제될 수 있음 <br/>
        - Young 영역에서 GC가 수행되면 Stop-The-World 현상이 발생하며, 이 시간을 줄이기 위해 멀티 스레드로 GC를 수행함 <br/>
        </details>
        
      - Epsilon GC (11)
        <details>
        <summary>Answer</summary>
        - Epsilon은 메모리 할당은 처리하지만 사용되지 않는 영역에 대해 재활용하지 않음 <br/>
        - Epsilon의 경우 Java Heap 영역을 모두 소진하게 되면 JVM이 Shut down <br/>
        - Epsilon의 목적은 제한된 영역의 메모리 할당을 허용함으로써 최대한 latency overhead를 줄이는 것 <br/>
        </details>
        
      - Shenandoah GC (12)
        <details>
        <summary>Answer</summary>
        - low-pause GC라고 불리며 '큰 GC 작업을 적은 횟 수로 수행하는 것 보다 작은 GC 작업을 여러번 수행하는 게 낫다'라는 컨셉 <br/>
        - Shenandoah는 ZGC와 비슷하게 대량의 메모리 처리에 우수한 퍼포먼스를 내지만 좀 더 많은 옵션을 제공한다는 장점 <br/>
        - Shenandoah는 OpenJdk 12에 등장하여 jdk11, jdk8까지 backporting 지원 <br/>
        </details>      
  
    - JVM 기반의 Tuning (g1gc)
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
      
    - Iterable
      <details>
      <summary>Answer</summary>
      - Iterable은 순회할 수 있는 컬렉션을 나타냄 <br/>
      - Iterable 인터페이스를 implement하면 객체는 for-each loop를 사용할 수 있게 해줌 <br/>
      - Iterable은 어떠한 iteration 상태 값을 가지지 못함 <br/>
      - Iterable은 iterator() 메소드가 호출이 될 때마다 Iterator의 새로운 instance를 생성함 <br/>
      </details>
      
    - Iterator
      <details>
      <summary>Answer</summary>
      - Iterator 인터페이스는 다른 객체, 다른 종류의 컬렉션을 순회하게 해줌 <br/>
      - Iterator 인스턴스는 iteration 상태를 모아둔 것 <br/>
      - Iterator는 컬렉션 내의 현재 위치를 기억 <br/>
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
    - 기존 코드의 변경을 최소화 하면서 기능을 추가할 수 있기 때문에 개방 폐쇄 원칙 (OCP)을 만족함 <br/>
    </details>
  
  - Proxy
    <details>
    <summary>Answer</summary>
    - 실제 기능을 수행하는 객체(Real Object) 대신 가상의 객체(Proxy Object)를 사용해 로직의 흐름을 제어하는 디자인 패턴 <br/>
    - 구조(Structural) 패턴 중 하나로, 원래 하려던 기능을 수행하며 그외의 부가적인 작업(로깅, 인증, 네트워크 통신 등)을 수행 <br/>
    - 비용이 많이 드는 연산(DB 쿼리, 대용량 텍스트 파일 등)을 실제로 필요한 시점에 수행
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
    <details>
    <summary>Answer</summary>
    - 401 : 로그인이 되지 않은 상태에서 요청을 하는 경우 발생 <br/>
    - 403 : 권한이 없는 요청을 하는 경우 발생 <br/>
    </details>

- CQRS
  <details>
  <summary>Answer</summary>
  - Command And Query Responsibility Segregation, 명령과 조회의 책임을 분리함 <br/>
  - 명령 도메인(내부 관리용 데이터)과 조회 도메인(정책, 외부 주입 데이터)을 분리함 <br/>
  - 생성, 저장과 조회 시점을 나누어 조회 도메인의 비정규화된 데이터를 그대로 DB에 저장함 (이때 NoSQL을 많이 씀) <br/>
  - 명령 도메인에 몰린 데이터 생성의 책임을 이벤트 소싱 패턴을 통해 조회 모델을 생성, 저장하는 새로운 어플리케이션으로 책임을 넘길 수 있음 <br/>
  </details>
  
  - Event-Driven

- Cache
  - Local Cache
    <details>
    <summary>Answer</summary>
    - 서버마다 Cache를 따로 저장함 <br/>
    - 다른 서버의 캐시 참조가 어려움 <br/>
    - 로컬 서버의 Resource(Memory, Disk)를 이용함<br/>
    - 캐시에 변경이 있는 경우 모든 peer에 변경 사항을 전달해야 함 <br/>
    - ex) Spring Cache <br/>
    </details>

  - Global Cache
    <details>
    <summary>Answer</summary>
    - 여러 서버에서 Cache 서버에 접근 가능 <br/>
    - 별도의 Cache 서버를 이용하기 때문에 서버 간 Cache 데이터 공유가 쉬움 <br/>
    - 네트워크 트래픽을 사용하기 떄문에 Local Cache 보다는 느림 <br/>
    - 데이터를 분산 저장 가능 <br/>
    - 캐시에 변경이 있는 경우 추가적인 작업이 필요 없음 <br/>
    - ex) Redis <br/>
    </details>

  - Hit Up 방안
  
  - Redis
    - MemCached
    - Sub/Pub
  - Spring Cache
  - Scale Up Vs Scale Out
    <details>
    <summary>Answer</summary>
    - Scale Up : Cache 인스턴스의 크기를 사이즈를 크게  <br/>
    - Scale out : Replica를 Cache 클러스터에 추가  <br/>
    </details>

- Database
  - Indexing
    <details>
    <summary>Answer</summary>
    - SELECT 조건에는 부등호 연산(<, >)을 사용하기 떄문에 Index는 Hash Table이 아닌 B+Tree 사용 <br/>
    - B+Tree의 검색은 루트노드에서 어떤 리프 노드에 이르는 한 개의 경로만 검색하면 되므로 매우 효율적 <br/>
    - Index는 이진트리를 사용하기 때문에 기본적으로 정렬되어 있기 때문에 검색과 조회의 속도를 향상시킬 수 있지만 잦은 데이터의 변경(삽입, 수정 삭제)가 된다면 인덱스 데이블을 변경과 정렬에 드는 오버헤드 때문에 오히려 성능 저하됨 <br/>
    - 다중 컬럼 인덱싱할 때 카디널리티가 높은 컬럼->낮은 컬럼 순으로 인덱싱해야 효율적 <br/>
    </details>
    
  - Tuning
  - Transaction
    - Level
      - READ UNCOMMITTED
      - READ COMMITTED
      - REPEATABLE READ
      - SERIALIZABLE
    - Distributed Transaction
      - Two-Phase Commit
      - Saga Pattern
    - Exclusive Lock
      <details>
      <summary>Answer</summary>
      - 배타적 잠금으로 쓰기 잠금 (Write Lock)이라고도 불림  <br/>
      - 어떤 트랜잭션에서 데이터를 쓰거나 변경할 때 해당 트랜잭션이 완료될 때 까지 해당 테이블 혹은 레코드(row)를 다른 트랜잭션에서 읽거나 쓰지 못함 <br/>
      - Exclusive Lock 에 걸리면 Shared Lock 을 걸 수 없음 <br/>
      - Exclusive Lock 에 걸린 테이블, 레코드 등의 자원에 대해 다른 트랜잭션이 Exclusive Lock 을 걸 수 없음 <br/>
      </details>

    - Shared Lock
      <details>
      <summary>Answer</summary>
      - 공유 잠금으로 읽기 잠금 (Read Lock)이라고도 불림  <br/>
      - 어떤 트랜잭션에서 데이터를 읽고자 할 때 다른 Shared Lock은 허용, Exclusive Lock은 불가함<br/>
      - 리소스를 다른 사용자가 동시에 읽을 수 있게 하되, 변경은 불가하게 만드는 것 <br/>
      - 어떤 자원에 Shared Lock 이 동시에 여러 개 적용될 수 있음 <br/>
      - 어떤 자원에 SHared LOck 이 하나라도 걸려있으면 Exclusive Lock을 걸 수 없음 <br/>
      </details>

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
    - HTTP/3
      <details>
      <summary>Answer</summary>
      - HTTP/3는 QUIC(Quick UDP Internet Connection)이라는 프로토콜 위에서 돌아가는 HTTP <br/>
      - 기존 TCP 방식의 HTTP는 3Way Handshake와 HOLB(Head of Line Blocking) 문제로 UDP에 비해 속도가 느림 <br/>
      - 연결 설정에 필요한 정보와 함께 데이터도 보내버리는 방법으로 연결 설정 시 레이턴시가 감소함 <br/>
      - 패킷 손실 감지에 걸리는 시간이 감축됨 <br/>
      - 단일 연결에 대한 멀티 플렉싱을 지원 <br/>
      - Connection ID를 사용하여 클라이언트 IP가 바뀌어도 연결이 유지됨 <br/>
      
      </details>
      
  - HTTP Caching

- CI/CD
  - How?

### 작은 것이라도 경험에 빗대어 말하는 습관 가지기
