#gradle plugins

gradle은 모든것을 플러그인으로 확장한다 

플러그인 타입은 script plugins 와 binary plugins가 있다 

## script plugins

스크립트 플러그인은 빌드를 추가로 구성하고 일반적으로 빌드를 조작하는 선언적 방법을 구현하는 추가 빌드 스크립트입니다.

원격 접근도 한다 



## binary plugins

플러그인 인터페이스를 구현하고 빌드를 조작하는 프로그래밍 방식을 채택하는 클래스

빌드 스크립트 내 or 프로젝트 계층 내 or 외부의 플러그인 jar에 상주



처음에는 script plugins로 시작한다 쉽기 때문에. 여러 프로젝트 간에 공유 해야할때 binary plugins로 마이그레이션 하면 됨 

//그냥 바로 이거 해도 되겠는데??



DSL로 플러그인 선택하고 적용할 수 있다 



script plugin 쓰면 apply()

binary plugins 쓰면 id와 name 로 쓴다 plugins DSL과 buildscript block 중 어디에 쓰냐에 따라 다르다 

핵심 플러그인은 잘 제공되어 있음 (javaPlugin)

비 핵심 바이너리 플러그인은 여러가지 방법으로 해결 

1. [gradle plugin portal](https://plugins.gradle.org/) 로 해결 

   core plugin은 

   ```groovy
   plugins {
       id 'java'
   } //groovy
   
   plugins {
       java
   }//kotlin
   ```

   포탈에서 커뮤니티 플러그인은 좀더 상세히 

   ```groovy
   plugins {
       id 'com.jfrog.bintray' version '0.4.1'
   }//groovy
   plugins {
      id("com.jfrog.bintray") version "0.4.1"
   }//kotlin
   ```

2. buildscript block으로 

   ```groovy
   buildscript {
       repositories {
           jcenter()
       }
       dependencies {
           classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:0.4.1'
       }
   }
   
   apply plugin: 'com.jfrog.bintray'
   ```

