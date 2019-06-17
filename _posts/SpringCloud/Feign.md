REST 기반 서비스 호출을 추상화한 Spring Cloud Netflix 라이브러리

선언적 방식

인터페이스를 통해 클라이언트 측 프로그램 작성

Spring이 런타임에 구현체를 제공



------



@FeignClient

- name : 서비스ID 혹은 논리적인 이름, spring-cloud의 eureka, ribbon에 사용
- url : 실제 호출할 서비스의 URL, eureka, ribbon을 사용하지 않고서도 동작
- decode404 : 404응답이 올 때 `FeignExeption`을 발생시킬지, 아니면 응답을 decode할 지 여부
- configuration : feign configuration class 지정
- fallback : hystrix fallback class 지정
- fallbackFactory : hystrix fallbak factory 지정



Feign property

```yml
feign:
 client:
 config: 
 feignName: # @FeignClient에서 name 값, 전역으로 설정하려면 default
 connectTimeout: 5000
 readTimeout: 5000
 loggerLevel: full
 errorDecoder: com.example.SimpleErrorDecoder
 retryer: com.example.SimpleRetryer
 requestInterceptors:
 - com.example.FooRequestInterceptor
 - com.example.BarRequestInterceptor
 decode404: false
 encoder: com.example.SimpleEncoder
 decoder: com.example.SimpleDecoder
 contract: com.example.SimpleContract
```



- connectionTimeout, readTimeout : hystrix의 timeout 설정이 더 짧으면, hystirx 옵션을 따라감
- loggerLevel : NONE, BASIC, HEADER, FULL을 지정할 수 있음
  - NONE : default, 로그를 남기지 않음
  - BASIC : Request Method, URL과 응답 코드, 실행 시간을 남김
  - HEADERS : `BASIC`의 정보를 포함하여, 요청, 응답의 헤더를 남김
  - FULL : 요청, 응답의 header, body, metadata를 남김
  - `logging.level.com.example.demo.PostClient: debug` 등의 debug logger 설정이 되어 있어야함
- encoder, decode : body의 내용을 Object로 변경하는 class 지정, 각각
- retryer : 요청이 실패했을 때 재시도에 대한 정책



### 의존성 추가 org.springframework.cloud:spring-cloud-starter-openfeign

