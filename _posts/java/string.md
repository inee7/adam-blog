### **String **

* immutable
* concat 인스턴스 생성이 많다

### **StringBuilder**

* Thread-unsafe

### **StringBuffer**

* Thread-safe
* 멀티쓰레드인 웹, 소켓 환경에서 써야함. 

JDK1.5 버전 이후에는 컴파일 단계에서 String 객체를 사용하더라도 StringBuilder로 컴파일 되도록 변경

하지만 반복 루프를 사용해서 문자열을 더할 때에는 객체를 계속 추가한다는 사실에는 변함이 없음

### 

### StringTokenizer



