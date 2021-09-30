# Builder Pattern

## 설명
- Builder Pattern은 디자인 패턴 중 하나로써 의도는 생성과 표현의 분리
- Factory Pattern 또는 Abstract Factory Pattern과 매우 비슷함
- Factory Pattern과 Abstract Factory Pattern에는 3가지 중대한 문제점 존재
  1. 수 많은 파라미터들이 클라이언트 클래스로 부터 전달 되는데 이것은 에러를 발생시키는 경우가 많음
  2. 파파라미터들은 값을 보낼때 선택적인데 factory pattern에서는 모든 인자를 전송해야 함
  3. 생성시키는 객체가 파라미터가 많은 경우 만들기가 매우 복잡해지며, 이런 factory pattern의 복잡성으로 인해 결국 혼란스러움
- **Builder Pattern은 선택적인 파라미터가 많을 경우 제공 상태를 일관성 있게 해주고, 객체를 생성시킬때 step-by-step으로 만들 수 있도록 제공해주며 최종에는 만들어진 객체를 반환함**

<br/>

## 예제
<br/>

```java
public class Ball {
  private int number;
  private int index;

  public Ball(BallBuilder ballBuilder){
    this.number = ballBuilder.number;
    this.index = ballBuilder.index;
  }

  public static class BallBuilder{
    private int number;
    private int index;

    public BallBuilder number(int number){
      this.number = number;
      return this;
    }

    public BallBuilder index(int index){
      this.index = index;
      return this;
    }

    public Ball build(){
      return new Ball(this);
    }
  }
} 
```
- 사용시

```java
    Ball ball = new Ball.BallBuilder()
        .number(1)
        .index(1)
        .build();
```