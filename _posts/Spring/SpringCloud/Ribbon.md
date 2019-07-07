# Ribbon

- Client Side Load Balancer 
  -  H/W가 필요 없이 S/W로만 가능 (비용**↓**, 유연성**↑**)
  - 서버 목록의 동적 변경이 자유로움
  - Load Balancing Scheme이 마음대로 구성 가능



- ServerList - Load Balancing의 대상이될 Server 목록 결정 
  - Configuration을 통해 직접 목록 설정 
  - Discovery 기반으로 Runtime에 획득한 서버 목록 사용 
- IRule - 트래픽을 보낼 서버를 선택
  - Simple Round Robbin
  -  Response Time Weighted - 서버별 응답 시간에 따라 트래픽 양 조절 
  -  Availability Filter  최근 3번 연속 에러를 발생시킨 서버를 Load Balancing 대상에서 일정 시간 동안 제외 시킴 (최초 5초, 4번째 실패시 10초. 2배씩 증가되며 최대 값이 기본 값 설정 가능) 

- IPing - 주기적으로 호출하여 해당 서버의 Aliveness를 검사
  - 특정 URL 호출
  - Discovery 기반 - Discovery 서버로 부터 가져온 목록에 있는지 확인
  - Ping에 실패한 서버는 임시로 Load Balancing 대상에서 제외



- 선택된 대상으로의 호출 실패 시 정해진 횟수 만큼의 Retry 설정 가능

  - 같은 서버 재시도 횟수, 다른 서버 재시도 횟수 설정

  - Exception 발생 경우 뿐만 아니라 재시도 할 HTTP Status 지정 가능

  - ```groovy
    spring:
      application:
        name: display
    #  cloud:
    #    loadbalancer:
    #      retry:
    #        enabled: true   # true as default in Dalston
    ```



- 사용법 
  - Spring `RestTemplate` @LoadBalanced RestTemplate 
  - Spring Cloud Feign Client - Declarative Http Client - Ribbon이 내장되어 있음 
  -  Zuul API Gateway - Ribbon이 내장되어 있음 



- yml 

  - ```yml
    product:
      ribbon:
    #    eureka:
    #      enabled: false
    #    listOfServers: localhost:8082,localhost:7777
        MaxAutoRetries: 0  # default 0
        MaxAutoRetriesNextServer: 1 # default 1
    ```

    

- dependency 

  - ```groovy
    compile('org.springframework.retry:spring-retry:1.2.0.RELEASE')
    compile('org.springframework.cloud:spring-cloud-starter-netflix-ribbon')
    ```