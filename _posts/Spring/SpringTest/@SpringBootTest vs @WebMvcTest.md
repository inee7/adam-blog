# Spring @WebMvcTest와 @SpringBootTest 차이 (Spring Security 관련 설정)

2018년 4월 13일 by [YongPwi](http://blog.devenjoy.com/?author=1)	[Leave a Comment](http://blog.devenjoy.com/?p=524#respond)

Spring Boot(1.4X)에서 Unit Test를 적용하다 어마무시하게 삽질을 하였음

Spring Boot에서는 Unit Test를 위해서 여러가지 annotation을 제공하고 있다.

그중에서 내가 삽질한 @WebMvcTest와 @SpringBootTest 두가지에 대해서 정리

먼저 @SpringBootTest의 경우 일반적인 테스트로 slicing을 전혀 사용하지 않기 때문에 전체 응용 프로그램 컨텍스트를 시작한다.

그렇기 때문에 전체 응용 프로그램을 로드하여 모든 bean을 주입하기 때문에 속도가 느리다.

@WebMvcTest의 경우는 Controller layer를 테스트하고 모의 객체를 사용하기 때문에 나머지 필요한 bean을 직접 세팅해줘야 한다.

그렇기 때문에 가볍고 빠르게 테스트 가능하다.

문제는 여기서부터 발생했다.

@WebMvcTest를 사용하여 테스트 시도시 spring security를 사용하는 경우 기본 spring security 설정을 로드한다.

WebSecurityConfigurerAdapter 추상클래스를 extends하여 customWebSecurityConfiguration(클래스명은 만들기 나름)를 사용하는 경우

customWebSecurityConfiguration에 선언된 url filter 부분을 등록 시켜주어야 한다.

이부분 때문에 몇일을 삽질했다.

```java
@WebMvcTest(controllers = {UserController.class}, secure = false)
public class UserControllerTest {}
```

물론 상위 code와 같이 @WebMvcTest에 secure = false 옵션을 주면 spring security 인증이 타지 않고 테스트를 진행한다.

하지만 이 방식은 내가 원하던 방식이 아니였다.

해당 부분에 대해서 spring boot project에서도 이슈로 논의되어 2.0 에서는 먼가 개선 예정이 있는것 같다.

https://github.com/spring-projects/spring-boot/issues/6514

관련 내용은 상위 이슈 링크를 확인 하시길,,,

그래서 해결을 어찌했냐면,,,

```java
@RunWith(SpringRunner.class)
// 요 부분이 key!!!
@WebMvcTest(controllers = {UserController.class}, includeFilters = @ComponentScan.Filter(classes = {EnableWebSecurity.class}))
public class UserControllerTest {

    @Autowired
    private MockMvc mvc;

    @MockBean
    LoggedUserManager loggedUserManager;

    @MockBean
    UserService userService;

    @MockBean
    UserDetailsService userDetailsService;

    @Autowired
    private ObjectMapper objectMapper;

    @Test
    @WithMockUser(username = "yongpwi",authorities = {"USER","ADMIN"})
    public void create() throws Exception {
        given(loggedUserManager.getUser()).willReturn(normalUser());
        String userJson = objectMapper.writeValueAsString(new User( "yongpwi@devenjoy.com", "yongpwi", "1111"));

        // when-then
        mvc.perform(
            MockMvcRequestBuilders.post("/user/createUser")
                .contentType(MediaType.APPLICATION_FORM_URLENCODED)
                .content(userJson))
            .andExpect(MockMvcResultMatchers.status().isOk())
            .andExpect(MockMvcResultMatchers.content().contentType(MediaType.APPLICATION_JSON_UTF8))
            .andExpect(MockMvcResultMatchers.jsonPath("$.resultCode").value(1))
            .andExpect(MockMvcResultMatchers.jsonPath("$.comment").value("SUCCESS"));
    }

    @Test
    @WithMockUser(username = "yongpwi",authorities = {"USER","ADMIN"})
    public void list() throws Exception {
        List<User> userList = asList(
                new User( "yongpwi@devenjoy.com", "yongpwi", "1111")
        );
        given(userService.search(any(UserDto.Response.class))).willReturn(userList);

        mvc.perform(MockMvcRequestBuilders.get("/user/list")
            .contentType(MediaType.APPLICATION_FORM_URLENCODED)
            .param("userEmail", "yongpwi@devenjoy.com"))
        .andExpect(MockMvcResultMatchers.status().isOk())
        .andExpect(MockMvcResultMatchers.view().name("user/list"));
    }

    private User normalUser() {
        User user = new User();
        user.setRole(Role.USER);
        user.setUserName("yongpwi");
        return user;
    }
}
```



상위 코드와 같이해서 테스트 환경 설정을 하였다.

```java
@WebMvcTest(controllers = {UserController.class}, includeFilters = @ComponentScan.Filter(classes = {EnableWebSecurity.class})
```



요 부분인데

includeFilters = @ComponentScan.Filter(classes = {EnableWebSecurity.class}

sprig security 필터 부분을 추가해주면 된다.

EnableWebSecurity.class를 지정하게 되면 @EnableWebSecurity annotation 선언한 class를 load한다.

그리고 또 한 부분 챙겨할 부분이 customWebSecurityConfiguration를 구현할때 사용한 외부 bean들은 해당 test시 하단과 같이 @MockBean으로 선언해주어야 customWebSecurityConfiguration 설정할때 오류가 나지 않는다.

```java
/ customWebSecurityConfiguration에서 @Autowired 선언한 class들 선언

@MockBean
UserService userService;

@MockBean
UserDetailsService userDetailsService;
```



unit test 관련된 내용은 앞으로 꾸준히 정리 예정!

Posted in: [Java](http://blog.devenjoy.com/?cat=4), [Programing](http://blog.devenjoy.com/?cat=3), [Talk](http://blog.devenjoy.com/?cat=2)	Tagged: [Spring boot](http://blog.devenjoy.com/?tag=spring-boot), [Spring Security](http://blog.devenjoy.com/?tag=spring-security), [SpringBootTest](http://blog.devenjoy.com/?tag=springboottest), [unit test](http://blog.devenjoy.com/?tag=unit-test), [WebMvcTest](http://blog.devenjoy.com/?tag=webmvctest)