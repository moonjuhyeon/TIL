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
### HATEOAS
* 어플리케이션의 상태는 Hyperlink를 이용해 전이되어야한다.
* 서버와 클라이언트가 각각 독립적으로 진화한다.
* 서버의 기능이 변경되어도 클라이언트를 업데이트할 필요가 없다.
* 전이는 미리 결정되지 않는다. 어떤 상태로 전이가 완료된 후, 그 다음 전이될 수 있는 상태가 결정된다. (링크는 동적으로 변경될 수 있다.)