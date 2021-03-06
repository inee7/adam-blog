## 1. Overview mockito

mockito는 유닛 테스트를 위한 Java mocking framework 입니다. mockito를 사용하면 대부분의 로직을 검증 할 수 있습니다. 몇가지 특수한 경우를 제외하면 말이죠. 공식 홈페이지는 http://mockito.org/입니다. 자세한 내용은 홈페이지를 참조하시기 바랍니다. 이번 포스팅은 mockito의 사용법에 대해 주로 다룹니다.

## 2. Declare Maven Dependency

mockito는 Maven과 저장소를 지원합니다. Maven은 아래와 같은 의존성을 pom.xml에 선언하면 사용할 수 있습니다. 포스트 작성 시점에 2.X 버전대는 beta 딱지가 붙어 있네요. 안정적인 버전은 1.10.19 버전이 최종입니다. 이것으로 사용법을 알아봅니다.

```xml
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>1.10.19</version>
</dependency>
```

mockito의 사용법을 위해 Junit도 pom.xml에 추가 해줍니다.

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
```

## 3. Exmaple

이제부터 예제를 하나씩 살펴봅시다.

### 3.1. mock()

mock() 메소드는 목mock 객체를 만들어서 반환합니다. 예를 들어 아래와 같은 커스텀 클래스를 하나 만들었다고 가정합니다.

```java
public class Person {
    private String name;
    private int age;

    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    public int getAge() { return age; }
    public void setAge(int age) { this.age = age; }
}
```

이제 mock() 메소드를 사용해봅시다.

```java
@Test
public void example(){
    Person p = mock(Person.class);
    assertTrue( p != null );
}
```

mock()을 사용하면 손쉽게 목 객체를 생성해낼 수 있습니다.

### 3.2. @Mock

mock() 메소드 외에도 목 객체를 만들기 위해 **@Mock** 어노테이션을 선언하는 방법도 있습니다. 이 방법은 다음처럼 사용합니다.

```java
@Mock
Person p;

@Test
public void example1(){
    MockitoAnnotations.initMocks(this);
    assertTrue(p != null);
}
```

MockitoAnnotations.initMocks(this)를 이용하면 Mockito 어노테이션이 선언된 변수들은 객체를 만들어냅니다.

### 3.3. when()

특정 목 객체를 만들었다면 이 객체로부터 특정 조건을 지정할 수 있습니다. 이 때 사용하는 것이 when() 메소드입니다.

#### 3.3.1. Usage 1

아주 간단한 when() 메소드 사용법입니다.

```java
@Test
public void example(){
    Person p = mock(Person.class);
    when(p.getName()).thenReturn("JDM");
    when(p.getAge()).thenReturn(20);
    assertTrue("JDM".equals(p.getName()));
    assertTrue(20 == p.getAge());
}
```

위의 코드에서처럼 지정 메소드에 대해 반환해줄 값을 설정 할 수 있습니다.

#### 3.3.2. Usage 2

조금 복잡한 경우를 생각해봅시다. 다음과 같은 메소드가 있다고 가정을 해봅니다.

```java
public List<String> getList(String name, int age){ // do something code }
```

위와 같은 메소드가 있다면 어떻게 조건을 걸어줘야 할까요. 다음처럼 사용합니다.

```java
when(mockIns.getList(anyString(), anyInt()))
    .thenReturn(
        new ArrayList<String>(){
            { this.add("JDM"); this.add("BLOG"); }
        }
    );
```

매개변수가 어떤 값이라도 관계 없다면 any...로 시작하는 메소드를 사용합니다. 만약 특정 값을 넣어야 한다면 eq() 메소드를 활용하세요.

```java
when(mockIns.getList(eq("JDM"), anyInt()))
```

위의 코드처럼 사용하면 됩니다.

### 3.4. doThrow()

만약 예외를 던지고 싶으면 어떻게 할까요? 이럴때는 doThrow() 메소드를 활용합시다.

```java
@Test(expected = IllegalArgumentException.class)
public void example(){
    Person p = mock(Person.class);
    doThrow(new IllegalArgumentException()).when(p).setName(eq("JDM"));
    String name = "JDM";
    p.setName(name);
}
```

### 3.5. doNothing()

간혹 void로 선언된 메소드에 when()를 걸고 싶을때가 있습니다. 이럴때 doNothing()을 사용합시다.

```java
@Test
public void example(){
    Person p = mock(Person.class);
    doNothing().when(p).setAge(anyInt());
    p.setAge(20);
    verify(p).setAge(anyInt());
}
```

verify() 메소드는 다음절에서 설명합니다.

### 3.6. verify()

verifiy()는 해당 구문이 호출 되었는지를 체크합니다. 단순한 호출뿐만 아니라 횟수나 타임아웃 시간까지 지정해서 체크해 볼 수 있습니다.

```java
@Test
public void example(){
    Person p = mock(Person.class);
    String name = "JDM";
    p.setName(name);
    // n번 호출했는지 체크
    verify(p, times(1)).setName(any(String.class)); // success
    // 호출 안했는지 체크
    verify(p, never()).getName(); // success
    verify(p, never()).setName(eq("ETC")); // success
    verify(p, never()).setName(eq("JDM")); // fail
    // 최소한 1번 이상 호출했는지 체크
    verify(p, atLeastOnce()).setName(any(String.class)); // success
    // 2번 이하 호출 했는지 체크
    verify(p, atMost(2)).setName(any(String.class)); // success
    // 2번 이상 호출 했는지 체크
    verify(p, atLeast(2)).setName(any(String.class)); // fail
    // 지정된 시간(millis)안으로 메소드를 호출 했는지 체크
    verify(p, timeout(100)).setName(any(String.class)); // success
    // 지정된 시간(millis)안으로 1번 이상 메소드를 호출 했는지 체크
    verify(p, timeout(100).atLeast(1)).setName(any(String.class)); // success
}
```

각 코드에 대한 설명은 주석으로 대체 했습니다.

### 3.7. @InjectMocks

만약 클래스 내부에 다른 클래스를 포함하는 경우엔 어떻게 테스트를 해야 할까요? 그리고 이 "다른 클래스"로 로직을 점검해야 한다면 외부에서 주입할 수 있도록 Setter 메소드나 생성자를 구현해야 할까요?

mockito에서는 이런 경우등을 위해 **@InjectMocks** 어노테이션을 제공합니다. 이 어노테이션은 **@Mock**이나 **@Spy** 어노테이션이 붙은 목 객체를 자신의 멤버 클래스와 일치하면 주입시킵니다.

예를 들어 다음과 같은 클래스들이 있다고 가정합니다.

```java
public class AuthService{
    private AuthDao dao;
    // some code...
    public boolean isLogin(String id){
        boolean isLogin = dao.isLogin(id);
        if( isLogin ){
            // some code...
        }
        return isLogin;
    }
}
public class AuthDao {
    public boolean isLogin(String id){ //some code ... }
}
```

테스트 해보고 싶은 것은 AuthService의 isLogin() 메소드입니다. 이 메소드에서는 AuthDao.isLogin() 반환값에 따라서 추가 작업을 더 하고 있습니다. 따라서 이 메소드를 테스트 해보고 싶다면 AuthDao의 값도 조작을 해야 하는 상황입니다.

다음의 코드는 해당 상황을 mockito로 처리한 것입니다.

```java
@Mock
AuthDao dao;

@InjectMocks
AuthService service;

@Test
public void example(){
    MockitoAnnotations.initMocks(this);
    when(dao.isLogin(eq("JDM"))).thenReturn(true);
    assertTrue(service.isLogin("JDM") == true);
    assertTrue(service.isLogin("ETC") == false);
}
```

### 3.8. @Spy

위에서 잠시 언급했지만 **@Spy**로 선언된 목 객체는 목 메소드stub를 별도로 만들지 않는다면 실제 메소드가 호출됩니다. 또한 spy()로도 같은 효과를 냅니다.

```java
Person p = spy(Person.class);
or
Person p = spy(new Person());
or
@Spy Person p;
```

 Java