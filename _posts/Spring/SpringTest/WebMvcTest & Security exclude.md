# WebMvcTest 할때 스프링시큐리티 무시하고 필터도 무시하고 싶을때



```kotlin
@WebMvcTest(
    controllers = [AdminSystemController::class],
    excludeFilters = [ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = [JwtFilter::class])],
    secure = false
)
```



secure = false 이 부분은 deprecated 됐다고 하는데 일단 작동은 한다. 

대체 방법을 오랫동안 찾았는데 도저히 찾지 못함. 

excludeFilters를 통해 토큰 필터 제외 시켰다. 

