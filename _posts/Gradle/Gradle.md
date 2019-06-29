# Gradle

핵심 기능은 플러그인으로 하기에 사실은 확장 가능한 자동화 툴
앤트의 방대한 스크립트 운영에 힘들고 메이븐이 어려울때 딱 좋음 

gradle 쓰려면 Java가 필요하다 

근데 `Gradle Wrapper`를 쓰면 실행 환경에 따라 알아서 java와 gradle을 선택해서 실행한다

gradlew.bat는 윈도우를 위한 배치 스크립트 



##JAVA용 프로젝트 설정 

```groovy
apply plugin: 'java'        // 'java'라는 Gradle 플러그인 적용 
sourceCompatibility = 1.8   // Java 호환 버전을 1.8로 설정
dependencies {
    compile 'ch.qos.logback:logback-classic:1.2.3'
    compile 'ch.qos.logback:logback-core:1.2.3'
    testCompile 'junit:junit:4.12' <- 그룹:이름:버전 명시할 수도 있음 group: name: version:
}	
```
java 파일 만들고 `./gradlew build`하면 정상적으로 빌드 될것이다 

apply plugin: 'java'하면 자바 프로젝트 규칙과 자바 빌드에 필요한 태스크가 추가됨 

##의존성 관리 
```groovy
allprojects {
    repositories {
        mavenCentral()
        jcenter()
        maven {
            url "http://repo.mycompany.com/maven2"
        }
        ivy {
            url "../local-repo"
        }
    }
    
    dependencies {
        // 로컬 jar 파일의 의존성 설정
        compile fileTree(dir: 'libs', include: '*.jar')
        // 로컬 프로젝트간 의존성 설정
        compile project(':shared')
        // 컴파일 타임에 의존성을 받아옴 
        compile 'com.google.guava', name: 'guava:23.0'
        // 테스트시만 의존성을 받아옴 
        // 마이너 버전을 '+'로 설정해서 항상 4점대 최신 버전을 사용
        testCompile group: 'junit', name: 'junit', version: '4.+'
        // 컴파일할때는 사용하고, 아티팩트를 만들때는 포함하지 않음
        compileOnly 'org.projectlombok:lombok:1.16.18'
        // 실행할때 의존성을 받아옴(기본적으로 컴파일을 모두 포함)
        runtime('org.hibernate:hibernate:3.0.5')
    }
}
```

## Gradle 플러그인 
checkstyle 
cobertura //테스트커버리지


##Gradle스크립트
DSL

[ 참조](https://medium.com/@goinhacker/%EC%9A%B4%EC%98%81-%EC%9E%90%EB%8F%99%ED%99%94-1-%EB%B9%8C%EB%93%9C-%EC%9E%90%EB%8F%99%ED%99%94-by-gradle-7630c0993d09)







## 그루비 문법

작은 따움표 ' '  : 문자열 

큰따움표 "" : 문자열 내부에 \${project.name} $name

메소드 호출시 괄호 생략 가능 

def로 타입 생략 , 객체의 메서드나 속성도 참조 가능

클로저 지원 

내장 태스크  