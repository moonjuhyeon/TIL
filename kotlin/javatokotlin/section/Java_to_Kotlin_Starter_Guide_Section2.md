# Section 2. 코틀린에서 코드를 제어하는 방법

## 1. 코틀린에서 제어문을 다루는 방법
### 1-1. if 문
- Java의 `if` / `if-else` / `if-else if-else` 와 동일함

### 1-2. Expression과 Statement
- Java의 `if-else`는 Statement
  - Statement : 프로그램의 문장, 하나의 값으로 도출 되지 않음
  - Java의 3항 연산자는 Expression이면서 Statement임
- Kotlin의 `if-else`는 Expression
  - Expression : 프로그램의 문장이지만, 하나의 값으로 도출 됨
  - Kotlin에서는 `if-else`를 Expresion할 수 있기 때문에 3항 연산자가 없음
- Kotlin은 범위를 다룰 때 `in` 과 `..` 을 사용함
  - ex) Java `if(0 <= score && score <= 50)` == Kotlin `if(score in 0..50)`

### 1-3. switch와 when
- Java의 `switch`는 Kotlin에서 `when`으로 대체 되었음
- Kotlin의 `when`에는 어떤 Expression도 포함할 수 있음
- Kotlin의 `when`은 값을 포함하지 않고 early return 처럼 사용할 수 있음

## 2. 코틀린에서 반목문을 다루는 방법
### 2-1. for-each 문
- Kotlin의 for-each 문은 Java의 `:` 대신 `in`을 사용함
    - ex) Java `for(number : numbers)` == Kotlin `for(number in numbers)`
- Kotlin의 for-each 문은 Java와 동일하게 Iterable이 구현된 타입 모두 들어갈 수 있음

### 2-2. 전통적인 for 문
- Kotlin의 for문은 for-each과 범위 구문 `..`을 사용함
  - ex) Java `for(int i = 0; i<=5; i++)` == Kotlin `for(i in 0..5)`
  - 만약 반대로 값이 내려 가는 경우는?
    - `downTo`를 사용함
    - ex) Java `for(int i = 5; i>=0; i--)` == Kotlin `for(i in 5 downTo 0)`
  - 만약 1이 아닌 2, 3과 같은 숫자로 올라가는 경우는?
    - `step`을 사용함
    - ex) Java `for(int i = 0; i<=6; i+=2)` == Kotlin `for(i in 0..6 step 2)`

### 2-3. Progression과 Range
- Kotlin의 `..` 연산자는 범위를 나타내는 연산자
- Kotlin의 범위는 실제 Range라는 클래스가 존재
  - Range 클래스는 Progression이라는 등차수열 클래스를 상속 받음
  - Progression 클래스는 시작 값, 끝 값, 공차의 매개 변수가 필요
  - `..` 연산자는 사실 등차수열을 만들어 주는 만들어 주는 것
  - ex) `3 downTo 1` : 시작 값 3, 끝 값 1, 공차가 -1인 등차 수열
- Kotlin의 `downTo`, `step`도 사실은 함수임 (중위 호출 함수)
  - 기존의 `변수.함수명(arg)` 대신 `변수 함수명 arg` 를 사용한 것
  - 만약 큰 범위를 사용하는 반복문일 때 `downTo`, `step`은 스택 오버 플로우를 발생시키지 않을까?
    - 결론은 **발생시키지 않음**
    - `step`, `downTo`는 `for()` 소괄호 사이에 존재하기 때문에, 1번만 호출되기 떄문
    - 무한 루프가 존재하지 않는 이상, 스택 오버 플로우는 발생하지 않음

### 2-4. while 문
- Kotlin의 while 문은 Java의 while 문과 완전 동일함 (do-while 도 동일)

## 3. 코틀린에서 예외를 다루는 방법
### 3-1. try-catch-finally 구문
- Kotlin의 try-catch-finally 구문은 Java와 완전히 동일함
  - throw에 new를 붙이지 않는 것, Expression을 사용하는 것이 다른 점

### 3-2. Checked Exception과 Unchecked Exception
- Kotlin에서는 Checked Exception과 Unchecked Exception을 구분하지 않음
  - Kotlin에서는 모두 Unchecked Exception 임

### 3-3. try-with-resource 구문
- Kotlin에는 try-with-resource 구문이 없음
  - 대신 Kotlin에서는 `use` 라는 inline 확장 함수를 사용함

## 4. 코틀린에서 함수를 다루는 방법
### 4-1. 함수 선언 문법
 - Kotlin에서 함수 선언 시 접근 지시어 `public`은 생략 가능
 - Kotlin의 함수가 하나의 결과 값을 보이면 block 대신 `=` 사용 가능
   - ex) `fun max(a: Int, b: Int) = if(a > b) a else b`
 - block { } 을 사용하는 경우에는 반환 타입이 Unit(void)이 아니면, 명시적으로 작성해야 함
 - Kotlin의 함수는 클래스 안에 있을 수도, 파일 최상단에 있을 수도 있음 그리고 한 파일에 여러 함수들이 있을 수 있음

### 4-2. default parameter
 - Kotlin의 default parameter는 외부에서 parameter 주입을 하지 않을 때 인자의 기본 값을 사용함

### 4-3. named argument (parameter)
 - Kotlin은 함수를 사용할 떄 매개변수의 순서를 무시하고, 매개변수 이름 = 값 을 통해 직접 지정 가능함
   - ex) `plusAnB(a = 10L, b = 20L)`
 - 또한, 매개변수로 지정되지 않은 값은 default parameter를 사용
 - Kotlin은 named argument를 통해 bulider를 직접 만들지 않고 builder의 장점을 가짐

### 4-4. 같은 타입의 여러 파라미터 받기 (가변 인자)
 - Kotlin의 가변인자는 Java의 `...` 대신 `vararg` 를 사용함
   - ex) Java `public void printAll(String... strings)` == Kotlin `fun printAll(vararg strings : String)`
 - Koltin의 가변인자의 매개변수는 Java와 같이 배열, 혹은 콤마(,)로 인자를 구분하고 넣을 수 있음
 - Koltin에서 배열을 가변인자로 사용할 때 스프레드 연산자(*)를 붙여주어야 함
   - ex) `val arr = arrayOf("A", "B", "C")  printAll(*arr)`