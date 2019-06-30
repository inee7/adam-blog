# kotlin-springboot

##gradle 필수 플러그인


###allOpen 

spring이나 jpa등 많은 프레임웤에서 어노테이션은 final클래스면 적용이 안된다. 

따라서 open class를 써야 코틀린 클래스로 스프링 조작을 할 수 있는데 

build.gradle 에 

```groovy
buildscript {
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-allopen:$kotlin_version"
    }
}

apply plugin: "kotlin-allopen"
```

As an alternative, you can enable it using the `plugins` block:

```groovy
plugins {
  id "org.jetbrains.kotlin.plugin.allopen" version "1.3.21"
}
```

Then specify the list of annotations that will make classes open:

```groovy
allOpen {
    annotation("com.my.Annotation")
    // annotations("com.another.Annotation", "com.third.Annotation")
}
```

> 사실 start.spring.io통해서 프로젝트 생성하면 kotlin-spring 플러그인 의존 되어 있는데 이것이 스프링의 어노테이션들에 모두 open 해준다. 

As with all-open, add the plugin to the buildscript dependencies:

```groovy
buildscript {
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-allopen:$kotlin_version"
    }
}

apply plugin: "kotlin-spring" // instead of "kotlin-allopen"
```

Or using the Gradle plugins DSL:

```groovy
plugins {
  id "org.jetbrains.kotlin.plugin.spring" version "1.3.21"
}
```

> 물론 둘 다 쓸 수 있다 `kotlin-allopen` and `kotlin-spring`



###noArg

리플렉션 쓰는 프레임웤 보면 무인자 생성자가 꼭 있어야 될 경우가 있다 

buildscript

```groovy
buildscript {
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-noarg:$kotlin_version"
    }
}

apply plugin: "kotlin-noarg"
```

plugin dsl

```groovy
plugins {
  id "org.jetbrains.kotlin.plugin.noarg" version "1.3.21"
}
```

then 

```groovy
noArg {
    annotation("com.my.Annotation")
}
```



As with the *kotlin-spring* plugin, *kotlin-jpa* is a wrapped on top of *no-arg*. The plugin specifies [`@Entity`](http://docs.oracle.com/javaee/7/api/javax/persistence/Entity.html), [`@Embeddable`](http://docs.oracle.com/javaee/7/api/javax/persistence/Embeddable.html) and [`@MappedSuperclass`](https://docs.oracle.com/javaee/7/api/javax/persistence/MappedSuperclass.html) *no-arg* annotations automatically.

That's how you add the plugin in Gradle:

```groovy
buildscript {
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-noarg:$kotlin_version"
    }
}

apply plugin: "kotlin-jpa"
```

Or using the Gradle plugins DSL:

```groovy
plugins {
  id "org.jetbrains.kotlin.plugin.jpa" version "1.3.21"
}
```

In Maven, enable the `jpa` plugin:

```groovy
<compilerPlugins>
    <plugin>jpa</plugin>
</compilerPlugins>
```



### 결론

allOpen, spring, jpa 하면 된다 



## dependencies

test 위해 

```groovy
testImplementation "org.jetbrains.kotlin:kotlin-test"
testImplementation "org.jetbrains.kotlin:kotlin-test-junit"
```

## querydsl 의존성

00







[gradle-컴파일러플러그인](https://kotlinlang.org/docs/reference/compiler-plugins.html)



