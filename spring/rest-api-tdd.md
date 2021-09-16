# REST-API-TDD

### 단축키
    - command + shift + t => 테스트클래스 생성
    - ctrl + option + o => 필요없는 import 제거

### 구현순서
- Entity 작성
- Entity Test 작성 (Builder, JavaBean)
  - Lombok 활용
- Controller Test 작성
  - mockMvc 활용
  - mockMvc.perform( {httpMethod}("uri") )
  - .andDo(print()) 진행상황 콘솔로그 출력
  - .andExpect(status().{httpResultCode}) 코드 검증
  - .andExpect(jsonPath("value")).isExist() "value" 값 검증
- Repository 작성
  - Mockito.when()으로 repository의 return 값 주입
- ReqDto 작성
  - ModelMapper 의존성 주입 및 bean 생성
  - ReqDto로 입력 필드 제한 -> ModelMapper를 통한 Entity 변환
  - Test 클래스에 @Mock @SpringBootTest, @AutoConfigureMockMvc 주입 -> @Mockbean 제거
  - Entity -> Dto 변환 후 통합테스트 진행
  - spring.jackson.deserialization.fail-on-unknown-properties=true 설정 추가
  - ReqDto에 존재하지 않는 값 BadRequest(400 Error) 테스트 추가
- ReqDto의 필드 값 검증을 위한 Validator 활용
  - String -> @NotEmpty, Object -> @NotNull, Integer -> @Min or @Max 활용
  - Controller의 ReqDto에 @Valid 적용
  - Input 값이 Null or Empty인 Test 작성
  - Input 값이 Validation에 올바르지 않는 값이 들어오는 Test 작성
  - Validator Class 작성 -> 허용하지 않는 값에 대한 조건을 작성하고 error를 유발시킴
- Error 검증을 위한 ErrorSerializer 작성
  - Errors 객체 -> json 형식으로 변환
  - Controller build(errors) -> body(errors)로 json 형식의 errors 반환
  - $[0].objectName 과 같은 json 배열형태의 error 값 테스트 진행
- 비즈니스 로직 작성 (Entity)
  - Entity의 비즈니스로직을 검증하는 유닛테스트를 BDD 형식으로 진행
  - 각 테스트를 통과할 수 있는 비즈니스 로직 작성
  - @ParameterizedTest로 테스트 리팩토링 수행