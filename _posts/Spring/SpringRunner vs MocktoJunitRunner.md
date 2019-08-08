# MockitoJUnitRunner 와 SpringRunner 



SpringRunner는 @SpringBootTest와 함께 사용하고  Application Context를 띄우고 @Autowired를 통해 사용한다. 

MockitoJUnitRuuner는 @Mock 또는 @Spy를 사용한다. 



SpringRunner에서 @Mock쓰고 싶으면 

```java
@Rule
MockitoRule rule = MockitoJUnit.rule();

@Mock
MyService myService;
```

를 통해 사용가능 허나 @MockBean을 쓰는게 좋다 

