| 매일 기술적 알게 된걸 기록한다. 

| 정리 없이 순차적으로 



Spring-Data-JPA 는 JPA 쉽게 쓸 수 있게 간단 작업 

QueryDSL은 JPQL대신 사용하기 좋은 복잡한 검색에 씀 - 동적 쿼리와 타입 체크와 객체지향이 장점 

queryDSL에서 selectFrom 은  Select와 From 합쳐 놓은것 

Spring-Data-Jpa에서 쿼리 메소드 _ 써서 외래키 접근 하면 되는데 조인을 하게 된다. 그래서 조인 안 하려면 QueryDSL 쓰는게 좋은듯 . 



Docker CMD는 배포할때 ENTRYPOINT는 실행중인 컨테이너에서 실행할때? 

Docker 멀티 스테이지로 이미지 용량 줄일 수 있다. 빌드/실행 나눠서 빌드한 결과에 필요한것만 가져와 실행 



---

JWT

---

스프링 스큐리티는 다음과 같이 인터페이스에 시큐리티를 걸어 줄수도 있다.

```java
@EnableGlobalMethodSecurity(prePostEnabled = true)
public class MethodSecurityConfig {
// ...
}
public interface BankService {

@PreAuthorize("isAnonymous()")
public Account readAccount(Long id);

@PreAuthorize("isAnonymous()")
public Account[] findAccounts();

@PreAuthorize("hasAuthority('ROLE_TELLER')")
public Account post(Account account, double amount);
}
```

