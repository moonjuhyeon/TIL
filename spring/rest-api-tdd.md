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