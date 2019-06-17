# WEBFLUX

- 스프링 5에서 지원 
- 스프링 리액티브 웹 초기버전 
- 논블록킹 IO 프로그래밍 



블록킹 IO 

- 소켓은 요청에 대한 응답 처리 할 때까지 쓰레드 차단 
- 동시 요청이 오면 여러개의 쓰레드로 소켓 대응 
- <u>요청이 증가 할 수록 쓰레드 관리 부담</u> 



Non-블록킹 IO 

- 요청 각각에 서버의 쓰레드를 바인딩 하지 않고 개별 버퍼를 사용해 요청에 대한 알림을 주고 받음
- 여러 연결을 하나의 쓰레드로 처리 



Tomcat : 1Request = 1 Thread 

Node.js: All Request = 1 Thread 

Netty : Many Request = 1 Thread 



스프링5의 웹플럭스는 Netty임베디드 서버가 기본 스펙 



웹애플리케이션 용도에 맞게 MVC와 웹플럭스 공존 

용도 

- 비동기-논블록킹 리액티브 개발에 사용 
- 효율적으로 동작하는 고성능 웹 애플리케이션 개발 
- 서비스간 호출이 많은 마이크로서비스 아키텍처에 적합 





![Spring MVC / WebFlux](https://docs.spring.io/spring/docs/current/spring-framework-reference/images/spring-mvc-and-webflux-venn.png)

스프링 웹플럭스 프레임워크는 두가지 방식으로 개발이 지원 

- 기존의 @어노테이션 
- 새로운 함수형 모델 방식



토비 왈: MVC와 웹플럭스를 한프로젝트에서 같이 사용하는것이 좋지 않음





