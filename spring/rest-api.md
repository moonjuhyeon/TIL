# REST API
## REST
* REpresentational State Transfer
* 분산 하이퍼미디어시스템(ex, WEB)을 위한 아키텍처 스타일
## API
* Aplication Programming Interface
## REST를 구성하는 아키텍처 스타일
    1. Client-Server
    2. Statelss
    3. Cache
    4. Uniform Interface
        - 가장 만족하지 못하고 있음
    5. Layered System
    6. Code-On-Demand(Optional)
        - 실행가능한 코드를 전송하는 것 (JS)
### Uniform Interface
    1. Identification of Resource
    2. Manipulation of Resource Through Represenations
    3. Self-Descrive Messages
    4. Hypermedia as The Engine of Application State (HATEOAS)
### Self-Descriptive Message
* 메시시는 스스로를 설명해야한다.
* 서버가 변해서 메시지가 변해도 클라이언트는 그 메시지를 보고 언제나 해석이 가능하다.
    
    ### ** 해결방법 **
    * Media-Type을 정의하여 IANA에 등록하고, 그 Media-Type을 Response Header에 Contene-Type으로 사용한다.
      * 단점 : 매번 Media-Type을 정의해야 함
    * 데이터의 명세를 작성하고 Response Header에 Profile Relation으로 해당 명세를 링크한다.
      * 단점 : 브라우저들의 스펙 지원이 미비함, Contents Negotiaion 불가

### HATEOAS
* 어플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다.
* 서버와 클라이언트가 각각 독립적으로 진화한다.
* 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.
* 전이는 미리 결정되지 않는다. 어떤 상태로 전이가 완료된 후, 그 다음 전이될 수 있는 상태가 결정된다. (링크는 동적으로 변경될 수 있다.)
  
    ### ** 해결방법 **
    * Data에 다양한 방법으로 하이퍼링크를 표현한다.
      * 단점 : 링크를 표현하는 방법을 직접 정의해야한다.
      * -> JSON으로 하이퍼링크를 표현하는 방법을 정의한 명세들을 활용
        * (JSON API, HAL, UBER, SIREN, Collection+json, etc..)
    * Response Headerd에 Link, Location등의 헤더로 링크를 표현한다.
      * 단점 : 정의된 Relation만 활용한다면 표현에 한계가 존재한다.

### 정리
    * 오늘날의 대부분의 "REST API"는 사실 REST를 따르지 않고 있다.
    * REST의 제약조건 중에서 특히 Self-Descriptive와 HATEOAS를 만족하지 못한다.
    * REST는 긴 시간에 걸쳐 진화하는 웹 어플리케이션을 위한 것이다.
    * REST를 따를 것인지는 API를 설계하는 이들이 스스로 판단하여 결정해야한다.
    * REST를 따르겠다면, Self-Descriptive와 HATEOAS를 만족시켜야한다
      - Self-Descriptives는 Custom Media-Type이나 Profile Link Relation 등으로 만족시킬 수 있다.
      - HATEOAS는 HTTP 헤더나 본문에 링크를 담아 만족시킬 수 있다.
    * REST를 따르지 않겠다면, "REST를 만족하지 않는 REST API"를 뭐라고 부를지 결정해야 한다.
      - HTTP API..
      - 그냥 이대로 REST API라고 부를 수도 있다. (로이 필딩이 싫어함..)
