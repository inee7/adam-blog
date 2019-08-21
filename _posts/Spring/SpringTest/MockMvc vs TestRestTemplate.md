#MockMvc 테스트와 TestRestTemplate 테스트 비교

- 서블릿 컨테이너의 사용여부가 가장 큰차이
- 입장의 차이
  - **MockMvc**는 ==서버== 입장에서 구현한 API를 통해 비지니스 로직의 문제를 테스트
  - **TestRestTemplate**은 ==클라이언트== 입장에서 RestTemplate 사용하듯이 테스트
- 트랜잭션의 차이
  - spring-boot-test는 spring-test의 확장이라 `@Transaction`시 끝나고 롤백
  - TestRestTemplate 테스트의 실제 서버는 별도의 쓰레드 수행 -> 롤백안됨