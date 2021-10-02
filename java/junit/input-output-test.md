# InputOutput Test (사용자 입출력에 대한 테스트)

## Scanner Input Test
```java
	@Test
	void inputNumberTest() {
		// given 
		String input = "123";
		// when
		InputStream inputStream = 
            new ByteArrayInputStream(input.getBytes());
		System.setIn(inputStream);
		// then
		assertThat(PlayerInputView.inputNumbers()).isEqualTo(input);
	}
```

* `InputStream in = new ByteArrayInputStream(input.getBytes());
System.setIn(in);` 을 통해 input 문자열을 시스템의 사용자입력으로 입력시킨다.

* assertThat()을 통해 입력된 값에 대한 검증을 실시한다.

### 테스트가 가능한 이유
> java.lang.System.setOut() method reassigns the "standard" output stream.

* System.setOut() 메서드는 사용자 표준 출력 스트림을 재지정할 수 있게 해준다.
* 콘솔 출력을 생성한 나의 아웃풋 스트림 객체에 지정함으로써 출력값을 해당객체에 저장할 수 있게 된다.
* 사용자 입력 또한 같다.

<br>

## System.out.println() Output Test

```java
	private final ByteArrayOutputStream byteArrayOutputStream = 
		new ByteArrayOutputStream();

	@BeforeEach
	void setUp() {
		System.setOut(new PrintStream(byteArrayOutputStream));
	}

	@DisplayName("printResultOutput 테스트")
	@Test
	void printResultOutputTest() {
		// given
		String result = "3스트라이크";
		// when
		PlayerOutputView.printResultOutput(result);
		// then
		assertThat(
			byteArrayOutputStream.toString().trim())
				.isEqualTo(result.trim()
		);
	}
```

<br>


-----


#### 출처 : https://choichumji.tistory.com/118, https://eblo.tistory.com/123