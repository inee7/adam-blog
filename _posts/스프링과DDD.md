# 스프링과DDD

스프링2.0 발표 컨퍼런스에서 DDD에 관한 얘기가 시작됐다. 이 시점은 무려 2005년 말이다 …. 현재 2019년..

##DDD특징 

1. 도메인 그 자체와 도메인 로직에 초점을 맞춘다 

   데이터중심의 접근법을 탈피해서 순수한 도메인의 모델과 로직에 집중 

2. 유비쿼터스 언어의 사용 

   도메인 전문가와 소프트웨어 개발자 간의 커뮤니케이션 문제를 없애고 상호가 이해할 수 있고 모든 문서와 코드에 이르기까지 동일한 표현과 단어로 단일화된 언어체계 구축 

3. 소프트웨어 엔티티와 도메인 컨셉트를 가능한 가장 가까이 일치 

   분석한 모델과 구현 모델이 다르지 않고 함께 가는것이 핵심 



그동안 getter/setter만 있는 빈약한 도메인 모델을 가지고 한 메소드에 각 업무 트랜잭션을 구성하는 패턴으로  개발했다. 

그에 따라 서비스레이어는 거대해졌고 코드의 중복과 오브젝트의 재활용성이 극히 떨어뜨렸다. 

객체지향언어의 장점은 오브젝트에 상태와 행위를 함께 구성하여 코드의 중복과 재활용성을 높이는 것인데...

이러한 데이터중심의 설계와 개발에 익숙한 개발자들은 적절한 도메인모델을 이용해서 개발하는데 경험이 없거나 익숙하지 않기 때문에 이런 방식을 선호하거나 별 문제의식 없이 사용하는 경우가 많다. 

전형적인 3-tier구조의 J2EE 아키텍처인 [ 프레젠테이션레이어 - 서비스레이어 - DA레이어 ] 는 도메인 로직이 서비스레이어에 집중되어 있고 도메인모델 오브젝트는 단지 DTO 역할을 하는 데이터홀더로만 사용되는 형대다. 

이런 개발을 표준으로 인식하고 있는건 심각했던 관례 

풍성한 도메인 모델이나 지능적인 도메인 모델 형태로 전환을 시도 하면 서비스 레이어에서 코드 중복이 사라지고 일관성 있게 도메인 오브젝트에 처리를 맞길 수 있는 형태로 발전하게 된다. 



## DDD아키텍처

풍성한 도메인 모델을 사용하는 것만으로도 도메인 모델 중심의 구현과 일치가 어느 정도 가능해졌다. 하지만 DDD에서 지향 하는 도메인 레이어라는 개념은 이보다 더 발전된 형태의 아키텍처이다.

DDD아키텍처는 [프레젠테이션레이어- 서비스레이어 - 도메인레이어 - DA레이어 ] 이며 도메인 레리어는 비지니스 애플리케이션의 심장과 같은 역할을 한다. 

모든 도메인 <u>정보와 기능과 로직과 룰</u>이 이 레이어에 집중되고 관리된다. 

이렇게 되면 도메인 전문가와 소프트웨어 개발자들이 지속적으로 함께 살펴보며 다듬어 나갈 수 있기 쉬워진다. 

서비스 레이어는 기존 비지니스를 담당했던것과 달리 도메인 레이어와의 협력을 통해서 처리할 수 있는 코디네이터 역할을 담당하게 된다. 

그리고 repository or DA레이어와의 연동을 서비스레이어에서 하지 않고 도메인 레이어가 직접하게 된다. 

위에서 말한 풍성한 도메인 모델에서는 도메인모델에게 넘겨줄 필요한 도메인 객체의 생성을 서비스레이어가 담당했다. 

```java
class CustomerService { 
    CustomerDao customerDao;
    public void addPoints() {
        List<Customer> customers = customerDao.getAllCustomers();
        ...
```

따라서 모든 레이어간의 연동이 서비스 레이어에 집중되는 한계를 가질 수밖에 없었다. 

하지만!! DDD는 도메인 모델이 자신이 필요로 하는 퍼시스턴스 관리를 위한 repository또는 인프라스트럭처 서비스와 직접 연동한다. 

도메인오브젝터에서 직접 repository에 요청해서 필요한것 받아서 사용하기때문에 서비스 레이어 도움 없이 독립된 레이어로 구성가능하게 된것이다. 

```java
class Customer {
	PointRuleRepository pointRuleRepository;
	
    Integer customerId ...
    Date lastVisited; 
    Date registered;

    public booean isVipCustomer(PointRule pointRule) {
		return this.customerLevel > pointRule.getVipLevel(this.registered)&& isValidRegiststedCustomer();
	}
    
	public voiddoVipPointUpgrade() {
		PointRule pointRule = pointRuleRepository.getCurrentPointRule();
		if (isVipCustomer(pointRule))
			customer.setPoint(customer.getPoint() + VIP_POINTS); 
   		}else {
			customer.setPoint(customer.getPoint() + MEMBER_POINTS); 
        }
	}
}
```



 ## DI/AOP와 DDD??

그러면! DDD의 아키텍처 구조와 스프링은 무슨 관계???????

스프링은 매우 범용화된 자바엔터프라이즈 개발 프레임워크일 뿐이다 사실. 

그러면서도 DDD아키텍처를 적용할 때 꼭 필요한 기능을 제공한다. 



### 도메인모델과 DI

위에서 도메인오브젝트에서 repository를 사용했다. 

그렇다면 도메인오브젝트에서 어떻게 repository를 쓸 수 있을까

보통 스프링 싱글톤 레지스트리에 있는 빈을 DI로 주입하는것을 생각 할 수 있다만...

도메인 오브젝트는 수시로 값이 변경되고 서로 다른 상태값을 가지는 굉장히 유동적이며 생성과 소멸이 뒤죽박죽이기에 싱글톤으로 절대 관리 할 수 없다. 

따라서 굳이 빈으로 등록할 이유가 없고 new키워드로 생성하면된다. 

하지만 DDD에서 요구되는 도메인 모델이라면 new가 아니라 DI가 필요하다. 

만약 하이버네이트와 같이 써드파티프레임워크로 만들어진 오브젝트면 스프링에 해당 오브젝트의 라이프사이클을 위임할 수 없어서 빈으로 쓰는것이 부적절하다. 

스프링 2.0에서 이러한 문제를 해결하기 위해 DDD 지원기능이 만들어졌다. 

#### @Configurable

@Configurable이라는 새롭게 도입된 어노테이션과 AspectJ의 도움으로 스프링이 직접 생성하지 않는 도메인 오브젝트에도 의
존삽입이 가능하게 해주는 것이다.

AspectJ가 지원하는 LTW(Load Time Weaving)기능을 이용해서 오브젝트가 생성되는 시점의 조인 포인트에서 오브젝트에 자동결합(autowiring) 방식으로 의존성을 삽입해주는 것이다.

> **Load-time weaving (LTW)** : 
>
> Class Loader가 클래스를 Loading할 때 Weaving 작업을 진행한다. Weaving Agent가 필요하다.

AspectJ 5의 LTW를 이용하면 별도의 AOP를 위한 컴파일 작업 없이도 클래스가 로딩되는 시점을 이용해서 어느 곳에서 생성
되는지와 상관없이 자동으로 의존삽입을 시킬 수 있다. 따라서 @Configurable이 적용되어 있는 모든 도메인 오브젝트에 적절
한 외부 의존성을 삽입시켜줄 수 있다.

```java
@Configurable 
class Customer {
	PointRuleRepository pointRuleRepository;
	public void setPointRuleRepository(PointRuleRepository pointRuleRepository) {
		this.pointRuleRepository = pointRuleRepository;
	}
```

