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

<br/>

-----
## Isolation Level
- ACID의 원칙을 너무 타이트하게 지키면 **동시성(Concurrency)** 에 대한 퍼포먼스가 너무 떨어짐
- Isolation Level로 차등을 두어 동시성에 대한 이점을 가지게 함
- 그러나 문제가 발생할 가능성이 커짐

<br/>

### - ANSI/ISO SQL Standard의 Isolation Level
  1. `*READ UNCOMMITTED*`
  2. `*READ COMMITTED*`
  3. `*REPEATABLE READ*`
  4. `*SERIALIZABLE*`

<br/>

### - **READ UNCOMMITTED**
  - `SELECT` 쿼리 실행시에 다른 트랜잭션에서 `COMMIT` 되지 않은 데이터를 읽어올 수 있음
  - `COMMIT` 되지 않은 데이터를 읽는 현상을 **Dirty Read** 라 함
  - `INSERT` 만 진행되고 `ROLLBACK` 될 수도 있는, 한번도 `COMMIT` 되지 않은 데이터를 읽을 수 있어 유의해야함
    
    > PostgreSQL은 `*READ UNCOMMITTED*` 를 지원하지 않음

<br/>

### - **READ COMMITTED**
  - `COMMIT` 이 완료된 데이터만 `SELECT` 시에 보일 수 있도록 보장하는 LEVEL
  - `*READ COMMITTED*` 에서는 `*READ UNCOMMITTED*` 에서 발생하는 **Dirty Read** 가 발생하지 않도록 보장함
  - 대부분의 DBMS에서 `*READ COMMITTED*` 를 기본으로 설정함
  - 트랜잭션에서 `COMMIT` 이전의 데이터를 보장받기 위해서는 `COMMIT` 되지 않은 쿼리를 복구하는 **Consistent Read**를 수행해야 함
  
  
  
    > **Consistent Read** </br>
    > Consistent Read란 Read(=`SELECT`) Operation을 수행할 때 **현재 DB의 값이 아닌 특정 시점의 DB *Snapshot*을 읽어오는 것이다.** 이 *Snapshot*은 `COMMIT` 된 변화만이 적용된 상태를 의미한다.
  
- `*READ COMMITTED*`의 문제는 하나의 트랜잭션 안에서 `SELECT` 를 수행 할 떄마다 데이터가 동일하다는 보장을 해주지 않음
- 다른 트랜잭션에서 해당 데이터를 `COMMIT` 했을 경우에 `COMMIT` 된 데이터를 반환해주는게 `*READ COMMITTED*` 의 특징
- 위와같은 이유로 `*READ COMMITTED*` 를 `*NON REPEATABLE READ*` 라고도 함

<br/>

### - **REPEATABLE READ**
  - `*READ COMMITTED*`와 다르게 `*REPEATABLE READ*` 는 한 트랜잭션 안에서 반복해서 `SELECT`를 수행하더라도 읽어 들이는 값이 변화하지 않음을 보장함
  - 처음으로 `SELECT` 를 수행한 시간을 기록하고, 그 이후에는 모든 `SELECT` 마다 해당 시점을 기준으로 **Consistent Read** 를 수행함
  - 그러므로 트랜잭션 도중 다른 트랜잭션이 `COMMIT` 되더라도 첫 `SELECT` 시에 생성된 *Snapshot*을 읽기 때문에, **새로** `COMMIT` **된 데이터는 보이지 않음**

<br/>

### - **SERIALIZABLE**
  - `*SERIALIZABLE*` 은 모든 작업을 하나의 트랜잭션에서 처리하는 것과 같은 가장 높은 고립수준을 제공함
    - *위에서 설명한 `*READ COMMITTED*`, `*REPEATABLE READ*` 두 개의 공통적인 이슈는 **Phantom Read**가 발생하는 것*
  
    > **Phantom Read**<br/>
    > 하나의 트랜잭션에서 `UPDATE` 명령이 유실되거나 덮어써질 수 있는 상황 즉, `UPDATE` 후 `COMMIT` 을 했을 때 다시 조회를 하면 예상과는 다른 값이 보이거나 데이터가 유실된 경우를 **Phantom Read** 라고 한다.

  - `*SERIALIZBLE*` 에서는 `SELECT` 쿼리가 전부 `SELECT ... FOR SHARE` 로 자동으로 변경되어 `*REPEATABLE READ*` 에서 막을 수 없는 몇 가지 상황을 방지할 수 있음

    #### * 예시) 두개의 트랜잭션이 서로 업데이트를 하는 상황

    ```SQL
    (A-1) SELECT state FROM account WHERE id = 1;
    (B-1) SELECT state FROM account WHERE id = 1;
    (B-2) UPDATE account SET state = 'rich', money = money * 1000 WHERE id = 1;
    (B-3) COMMIT;
    (A-2) UPDATE account SET state = 'rich', money = money * 1000 WHERE id = 1;
    (A-3) COMMIT;
    ```
  
    1. *(A-1)* `SELECT` 쿼리가 `SELECT ... FROM SHARE` 로 바뀌면서 id=1 인 row에 **S Lock**을 겁니다.
    2. *(B-1)* `SELECT` 쿼리 역시 id=1 인 row에 **S Lock** 을 겁니다.
    3. A와 B가 각각 *(B-2)*, *(A-2)* `UPDATE` 쿼리를 실행하려고 하면 row에 **X Lock** 을 시도합니다.
    4. 이미 해당 row에는 **S Lock** 이 걸려있어, *`DEADLOCK`* 상황에 빠집니다.
    5. 두 트랜잭션 모두 *`DEADLOCK`* 으로 인한 Timeout이 발생하여 실패하고, 데이터는 변경되지 않고 원래대로 남아있게 됩니다.
   
  <br/>

  - `*SERIALIZABLE*` 은 데이터를 안전하게 보호할 수 있지만, 굉장히 쉽게 *`DEADLOCK`* 에 걸릴 수 있음
  - `*SERIALIZABLE*` 은 *`DEADLOCK`* 이 걸리지 않도록 신중하게 사용해야 함
  
  > **S Lock** : ( *Shared Lock*, 공유 잠금 ) <br/>
  > 읽기 잠금(*Read Lock*)이라고도 불린다. <br/>
  > 어떤 트랜잭션에서 데이터를 읽고자 할 때 다른 *Shared Lock*은 허용이 되지만, *Exclusive Lock*은 불가하다. <br/>
  > 쉽게 말해서 리소스를 다른 사용자가 동시에 읽을 수 있게 하되, 변경은 불가하게 만드는 것이다. <br/>
  >
  > -> 어떤 자원에 *Shared Lock* 이 동시에 여러 개 적용될 수 있다. <br/>
  > -> 어떤 자원에 *SHared LOck* 이 하나라도 걸려있으면 Exclusive Lock을 걸 수 없다. <br/>
  > </br>
  > **X Lock** : ( *Exclusive Lock*, 배타적 잠금 ) <br/>
  > 쓰기 잠금(*Write Lock*)이라고도 불린다. <br/>
  > 어떤 트랜잭션에서 데이터를 쓰거나 변경할 때  해당 트랜잭션이 완료될 때 까지 해당 테이블 혹은 레코드(row)를 다른 트랜잭션에서 읽거나 쓰지 못하게 하기 위해 *Exclusive Lock* 을 걸고 트랜잭션을 진행시키는 것이다. <br/>
  > -> *Exclusive Lock* 에 걸리면 *Shared Lock* 을 걸 수 없다. <br/>
  > -> *Exclusive Lock* 에 걸린 테이블, 레코드 등의 자원에 대해 다른 트랜잭션이 *Exclusive Lock* 을 걸 수 없다.
  
  <br/>

  ---

  ## CRUD에 적합한 ISOLATION LEVEL
  - **`READ`** 는 `*REPEATABLE READ*`
  - **`CREATE`, `UPDATE`, `DELETE`** 는 `*SERIALIZABLE*`

 > **참고사항** <br/>
 > **`UPDATE, DELETE`** 는 **Consistant Read** 의 적용을 받지 않기 때문에 <br/>
 > 같은 `WHERE` 조건을 사용하더라도, **내가 수정하려고** `SELECT` **쿼리로 읽어온 row와, 해당 row를 수정하기 위해 **`UPDATE`** 쿼리를 날렸을 때 실제로 수정되는 row가 다를 수 있습니다.** <br/>
 
 <br/>

- 표) 트랜잭션 격리성 수준과 비일관성 현상

  |LEVEL|Diry Read|Non-Repeatable Read|Phantom Read|
  |--|--|--|--| 
  |`*READ UNCOMMITTED*`|가능|가능|가능|
  |`*READ COMMITTED*`|불가능|가능|가능|
  |`*REPEATABLE READ*`|불가능|불가능|가능|
  |`*SERIALIZABLE READ*`|불가능|불가능|불가능|

<br/>

-------
<br/>

> ## 출처 <br/>
> https://velog.io/@lsb156/Transaction-Isolation-Level#read-committed