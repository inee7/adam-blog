Not Only SQL (SQL없이도 가능하다) 

정규화의 어려움  

비정형데이타의 증가 (문장) 

컬럼갯수 어떻게 될지 모를때  

ACID 속성 유연하게 적용  

수평적 확장  

복제, 샤딩  

스키마를 애플리케이션이 관리 ->schema Free  

스키마 변경됐을때 신속히 클라이언트에서 변경하여 대응 가능  

NoSQL과 RDB 공존해야한다  

CAP이론 (P: 분산시스템에도 잘 작동) 

둘중 하나 선택해서  

RDB - CA (일관성,가용성)  

NoSQL - CP : HBase(무조건 일관성), MongoDB, Redis , AP : 카산드라 (cp일수도 있다 ) 

다만 중심일뿐 유연하게 조절가능 

JPA 객체지향데이타  

RDB의 문제  

MyBatis 데이타구조지향 RDB에 의존 , 결국 정규화 필수  

클러스터  

RDB는 클러스터 기반에서 실행되도록 설계되지 않았음  

기본베이스는 비정규화인데 필요하면 정규화 가능  