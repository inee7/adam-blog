# JPA 벌크 연산

EntityManager()의 remove(),persist() 

등은 하나 씩 밖에 처리 하지 못한다. 



JPQL 사용

em.createQuery(query).setParameter("stockAmount",10).==excuteUpdate()==

다만 이건 영속성컨텍스트 거치지 않고 바로 DB에 sql을 실행 시키기 때문에 주의 필요. 



- em.refresh(엔티티) 로 영속화컨텍스트 새로고침 
- 벌크 연산 먼저 실행 
- 벌크 연산 후 영속성컨텍스트 초기화 



QueryDsl과 spring data jpa에서는?