# Transaction Isolation Level
## Transaction
### - Transaction의 정의
- Transaction이란, Database의 데이터를 조작하는 작업의 단위(Unit of Work)
- 예시)
  - A가 B에게 1,000원을 송금한다고 가정했을시 시나리오
    1. A의 계좌에서 1,000원을 차감
    2. B의 계좌에서 1,000원을 추가 
  - B의 계좌를 관리하는 은행에서 알 수 없는 오류로 금액이 추가가 되지 않는다면, A의 계좌에서 돈만 차감되는 장애로 연결됨
  - 이를 보장하기 위해서 모든 작업들은 트랜잭션 단위로 묶어서 처리 해야함
-----
## Isolation Level
- ACID의 원칙을 너무 타이트하게 지키면 동시성(Concurrency)에 대한 퍼포먼스가 너무 떨어짐
- Isolation Level로 차등을 두어 동시성에 대한 이점을 가지게 함
- 그러나 문제가 발생할 가능성이 커짐
  
### - ANSI/ISO SQL Standard의 Isolation Level
  1. `*READ UNCOMMITTED*`
  2. `*READ COMMITED*`
  3. `*REPEATABLE READ*`
  4. `*SERIALIZABLE*`

### - **READ UNCOMMITTED**
  - `SELECT` 쿼리 실행시에 다른 트랜잭션에서 `COMMIT` 되지 않은 데이터를 읽어올 수 있음
  - `COMMIT` 되지 않은 데이터를 읽는 현상을 **Dirty Read** 라 함
  - `INSERT` 만 진행되고 `ROLLBACK` 될 수도 있는, 한번도 `COMMIT` 되지 않은 데이터를 읽을 수 있어 유의해야함
    
    > PostgreSQL은 `*READ UNCOMMITTED*` 를 지원하지 않음

### - **READ COMMITED**
  - `COMMIT` 이 완료된 데이터만 `SELECT` 시에 보이는 수준을 보장하는 LEVEL
  - `*READ COMMITED*` 에서는 `*READ UNCOMMITTED*` 에서 발생하는 **Dirty Read** 가 발생하지 않도록 보장함
  - 대부분의 DBMS에서 `*READ COMMITED*` 를 기본으로 설정함
  - 트랜잭션에서 `COMMIT` 이전의 데이터를 보장받기 위해서는 `COMMIT` 되지 않은 쿼리를 복구하는 **Consistent Read**를 수행해야 함
  
    > **Consistent Read** </br>
    > Consistent Read란 Read(=`SELECT`) Operation을 수행할 때 **현재 DB의 값이 아닌 특정 시점의 DB Snapshot을 읽어오는 것이다.** 이 Snapshot은 `COMMIT` 된 변화만이 적용된 상태를 의미한다.
  
- `*READ COMMITTED*`의 문제는 하나의 트랜잭션 안에서 `SELECT` 를 수행 할 떄마다 데이터가 동일하다는 보장을 해주지 않음
- 다른 트랜잭션에서 해당 데이터를 `COMMIT` 했을 경우에 `COMMIT` 된 데이터를 반환해주는게 `*READ COMMITTED*` 의 특징
- 위와같은 이유로 `*READ COMMITTED*` 를 `*NON REPEATABLE READ*` 라고도 함

### - **REPREATABLE READ**
  - `*READ COMMITTED*`와 다르게 `*REPEATABLE READ*` 는 한 트랜잭션 안에서 반복해서 `SELECT`를 수행하더라고 읽어 들이는 값이 변화하지 않음을 보장함
  - 