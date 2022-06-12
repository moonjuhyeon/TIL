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

## 4. 코틀린에서 연산자를 다루는 방법