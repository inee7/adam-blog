# Hystrix



```
하나의 컴포넌트가 느려지거나 장애가 나면 그 장애가난 컴포넌트를 호출하는 종속된 컴포넌트까지 장애가 전파되는 특성을 가지고 있다.
```



## Circuit breaker 패턴

![img](https://t1.daumcdn.net/cfile/tistory/99E6754C5AC39FAA08)

Service B가 정상적인 상황에서는 트래픽을 문제 없이 bypass 



![img](https://t1.daumcdn.net/cfile/tistory/99427C475AC39FAA08)![img](https://t1.daumcdn.net/cfile/tistory/993FD73B5AC39FAA17)

Service B가 문제가 생겼음을 Circuit breaker가 감지한 경우에는 Service B로의 호출을 강제적으로 끊어서 Service A에서 쓰레드들이 더 이상 요청을 기다리지 않도록 해서 장애가 전파하는 것을 방지 한다.

예를 들어 오늘의 추천곡을 서비스B에서 받아야 하는 상황인데 서비스B가 장애가 나면 Fallback Message로 대신 내려준다. 이로써 최소한 사용자까지 장애가 전파 되지는 않는다. 



### **소프트웨어적으로 서킷프레이커 패턴을 구현한 라이브러리가 넷플릭스의 Hystrix이다. (Spring 환경에서 쓸 수 있게 만든 것이 Spring Cloud이다. )**



```
Hystrix 프로퍼티 
feign:
 hystrix:
 enabled: true
 client:
 config:
 default:
 loggerLevel: BASIC
 # feign의 전역 timeout 설정 : 5초
 connectTimeout: 5000
 readTimeout: 5000

# hystrix 명령의 기본 timeout을 10초로 변경
hystrix.command.default.execution.isolation.thread.timeoutInMilliseconds: 10000
```