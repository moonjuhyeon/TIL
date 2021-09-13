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
   