# Springfox

REST api 문서를 자동화 해주는 오픈소스인 Swagger를 Spring로 구현한것이 SpringFox 

~~버전마다 여우 이미지가 다름~~ 

깃헙 → https://github.com/springfox/springfox

doc → https://springfox.github.io/springfox/docs/current/#springfox-swagger-ui



제약조건 유효성은 문서화 하기 위해 JSR303 Annotation 사용 

[→ https://dzone.com/articles/bean-validation-made-simple](https://dzone.com/articles/bean-validation-made-simple)

```groovy
implementation("io.springfox:springfox-swagger2:2.9.2")
implementation("io.springfox:springfox-swagger-ui:2.9.2")
implementation("io.springfox:springfox-bean-validators:2.9.2")
```





```kotlin
@EnableSwagger2
@Import(value = [springfox.bean.validators.configuration.BeanValidatorPluginsConfiguration::class])
@Configuration
class SwaggerConfig {
    @Bean
    fun api(): Docket {
        return Docket(DocumentationType.SWAGGER_2)
            .groupName("ts-api-v1")
            .protocols(setOf(SwaggerDefinition.Scheme.HTTP.name, SwaggerDefinition.Scheme.HTTPS.name))
            .useDefaultResponseMessages(false)
            .select()
            .apis(RequestHandlerSelectors.basePackage("kakao.tesseract"))
            .paths(PathSelectors.ant("/v1/**"))
            .build()
            .apiInfo(getApiInfo())
    }

    private fun getApiInfo(): ApiInfo {
        return ApiInfo(
            "삼성뮤직 API",
            "삼성뮤직 앱을 위한 API 문서입니다.",
            "1.0",
            "",
            Contact("카카오 S TF", "", ""),
            "",
            "",
            Collections.emptyList()
        )
    }
}
```