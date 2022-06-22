# Section 1. 코틀린에서 변수와 타입, 연산자를 다루는 방법

## 1. 코틀린에서 변수를 다루는 방법
### 1-1. 변수 선언 키워드 - var과 val의 차이점
- Java에서 `long`과 `final long`의 차이
- 해당 변수는 가변인가, 불변인가(read-only)
- `var` -> Variable의 약자, 변경 가능
- `val` -> Value의 약자, 변경 불가능 (read-only)
  - `val` Collection의 경우 element를 추가할 수 있음
- Kotlin은 Type을 명시적으로 작성하지 않아도, Type이 추론됨
  - ex) `val x = 1.0` == `val x :Double = 1.0`
- **TIP) 모든 변수는 우선 `val`로 선언하고, 필요한 경우 `var`로 변경한다.**

### 1-2. Kotlin에서의 Primitive Type
- Java에서 `long`은 Primitive Type, `Long`은 Reference Type
  - 연산 작업이 필요한 경우 Reference Type은 boxing과 unboxing이 일어나면서 불필요한 객체생성이 이루어 짐
  - 따라서 연산 작업의 경우 Primitive Type을 쓰는 것이 바람직
- Kotlin에서는 `long`이 존재하지 않고, `Long`을 사용하는데 성능상의 문제는?
  - Kotlin 공식 문서에서는 **숫자, 문자, 불리언**과 같은 몇몇 타입은 내부적으로 특별한 표현을 갖고 있다고 함
  - 때문에, 코드에서는 평범한 Reference Type으로 보이지만 실행시에 Primitive Value로 표현됨
  - 즉, Kotlin은 개발자가 boxing, unboxing을 고려하지 않아도 되도록 알아서 잘해줌!

### 1-3. Kotlin에서의 nullable 변수
- Kotlin애서 null이 변수에 들어갈 수 있다면 `Type?`를 사용해야 함
  - Kotlin에서는 아예 다른 Type으로 간주됨
  - ex) `String` != `String?`

### 1-4. Kotlin에서의 객체 인스턴스화
- Kotlin에서는 객체 인스터스화를 할 때 `new`를 붙이지 않음

## 2. 코틀린에서 null을 다루는 방법
### 2-1. Kotlin에서의 null check
- Kotlin에서는 null이 가능한 Type을 기존 Type고 완전히 다르게 취급함
  - ex) `String` != `String?` 둘은 다른 Type
- `String`과 `String?`이 왜 다를까?
  - 자료형(Type) : 데이터를 식별하는 분류 (자료형이 무엇인지에 따라 행동이 달라짐)
  - 둘은 같은 문자열이지만, `String?`의 경우 `String`이 지원하는 행동을 수행하기 위해서 Safe Call 혹은 Elvis 연산자와 같은 특수한 처리를 해야함
  - 따라서, 엄밀히 말하면 같은 문자열이지만 완전 같은 Type은 아님
    - ex) 푸들의 종류에는 토이 푸들, 미디엄 푸들, 스탠다드 푸들 등 여러 종이 존재
- `Type?`가 있는 경우에만 해당 변수에 null이 존재할 수 있음

### 2-2. Safe Call과 Elvis 연산자
- Kotlin의 Safe Call은 null이 아니면 실행하고, null이면 실행하지 않음
  - ex) `val str: String? = "ABC"`, `str.length` **불가능**, `str?.length` **가능**
- Koltin의 Elvis 연산자는 앞의 연산 결과가 null이면 뒤의 값을 사용함
  - ex) `val str: String? = "ABC"`, `str?.length ?: 0` `str`이 null인 경우 0을 반환
  - ?: 를 오른쪽 90도 회전하면 엘비스 프레슬리의 머리모양과 비슷하여 Elvis 연산사라고 함..

### 2-3. null이 아님을 단언(!!)
- Nullable Type이지만, 아무리 생각해도 null이 될 수 없는 경우
- 크게 사용할 일은 없지만, null을 허용하는 Type임에도 과정상 null이 절대로 될 수 없는 경우 사용함
- 컴파일 에러를 발생시키지 않지만, 런타임 에러를 허용하게 됨 (NullPointer Exception)
  - ex) `val str: String? = "ABC"`, `str!!.length`

### 2-4. Platform Type
- Java와 Kotlin을 함께 사용할 때 사용함
- Platform Type은 Kotlin이 null 관련 정보를 알 수 없는 Type으로 런타임 에러가 발생할 수 있음
  - Java 코드를 읽으며 null 가능성을 확인하고, Kotlin으로 wrapping 해야함

## 3. 코틀린에서 Type을 다루는 방법
### 3-1. 기본타입
- Kotlin은 선언된 기본 값으로 타입을 추론함
- Java의 기본 타입간 변화는 **암시적으로** 이루어질 수 있음
  - ex) `int number1 = 5 -> long number2 = number1`
- Kotlin의 기본 타입간 변화는 **명시적으로** 이루어져야 함
  - ex) `val number1 : Int = 5 -> val number2 : Long = number1.toLong`
  - Kotlin은 `to변환타입()`을 사용해야 함
- kotlin에서 nullable한 경우 처리를 해주어야 함, Safe Call or Elvis

### 3-2. 타입 캐스팅
- Kotlin의 타입 비교
  - `value is Type` -> Java의 `instanceof` 
  - Type이 true인 경우 자동으로 타입 캐스팅이 됨 (스마트캐스팅)
- Kotlin의 타입 캐스팅
  - `value as Type` -> Java의 타입 캐스팅 ex) `(Type) value`
  - nullable 처리는 `value ?as Type` -> `true`인 경우를 제외하고 `null`을 반환
  - Kotlin은 `is`, `!is`, `as`, `as?` 를 이용해 타입을 확인하고 캐스팅함

### 3-3. Kotlin의 3가지 특이한 타입
- Any
  - Java의 Object와 동일한 역할 (모든 객체의 최상위 타입)
  - 모든 Primitive Type의 최상위 타입
    - Java의 Object는 Primitive Type의 최상위 타입이 아님
  - Any 자체로는 null을 포함할 수 없어, null을 포함하고 싶아면 Any?로 표현
  - Any에 `equals` / `hashCode` / `toString` 존재
- Unit
  - Java의 void와 동일한 역할
  - Java의 void와 다르게 Unit은 그 자체로 타입 인자로 사용 가능
    - Java에서 void를 Generic에 사용하기 위해선 Void 클래스를 사용
    - Kotlin은 그냥 Unit을 사용하면 됨
  - 함수형 프로그래밍에서 Unit은 단 하나의 인스턴스만 갖는 타입을 의미
    - 즉, Kotlin의 Unit은 실제 존재하는 타입이라는 것을 표현
- Nothing
  - Nothing은 함수가 정상적으로 끝나지 않았다는 사실을 표현하는 역할
    - 무조건 예외를 반환하는 함수 또는 무한 루프 함수 등
    - ex) `fun fail(msg : String) : Nothing () = throw IllegalArgumentException(msg)`

### 3-4. String Interpolation, String indexing
- `${변수명}` 사용으로 문자열에 해당 변수의 값을 넣는 것이 가능
- `""" """` 큰 따옴표 3개를 붙여 쓰면 엔터를 인식하여 긴 문자열을 작성할 수 있음
- `변수명[index]`로 배열처럼 문자열 인덱스에 해당 하는 char 값을 가져올 수 있음

## 4. 코틀린에서 연산자를 다루는 방법
### 4-1. 단항 연산자 / 산술 연산자
- 단항 연산자, 산술 연산자, 산술대입 연산자 모두 Java와 완전 동일함

### 4-2. 비교 연산자와 동등성, 동일성
- Kotlin의 비교 연산자는 값을 비교할 때 Java와 완전 동일함
- Kotlin의 비교 연산자는 객체간의 비교시 자동으로 `compareTo`를 호출함
- 동등성(Equality) : 두 객체의 값이 같음
  - Java는 `==` 을 사용
  - Kotlin은 `===` 을 사용
- 동일성(Identity) : 두 객체가 완전히 같은 객체임 (주소가 같음)
  - Java는 `equals` 를 사용
  - Kotlin은 `==` 을 사용 (`==` 을 사용하면 `equals` 를 호출함)

### 4-3. 논리 연산자 / Kotlin의 특이한 연산자
- Kotlin의 논리 연산자(`&&,` `||,` `!`)는 Java와 완전 동일하고, Lazy 연산 수행
- Koltin의 특이한 연산자
  - `in`, `!in` : 컬렉션이나 범위에 포함되어 있음, 포함되어 있지 않음
  - `a..b` : a부터 b까지 범위 객체 생성
  - `a[i]` : a에서 특정 인덱스 i 값을 가져움

### 4-4. 연산자 오버로딩
- Koltin에서는 객체마다 연산자를 직접 정의할 수 있음
  - ex) `val money1 = Money(1_000L), val money2 = Money(2_000L), money1 + money2 = 3_000L` 
  