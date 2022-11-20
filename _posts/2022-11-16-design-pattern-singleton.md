---
layout: post
current: post
navigation: True
title: Design Pattern (1) - Singleton
date: 2022-11-16
tags: ["Design Pattern", "Java"]
class: post-template
subclass: 'post'
author: on-Sync
---

# Singleton

> ___한정된 자원을 아끼는___ 생성 패턴

## 단어의 뜻

- `Single` + `-ton`
- = (`단독` + `인 것`)
- = `단일 개체`

> (e.g. Singleton set : 단일 개체 집합)

## 집합의 기준

`Singleton` 은 집합의 기준에 따라 단일성을 보장하면 되는 개념이다.
하지만 일반적으로 단일 소프트웨어에서 유일성을 보장하는 것을 `Singleton` 이라 부른다.
즉, 더 작은 단위에서도 사용될 수 있으므로 `Singleton` 의 집합수준을 확인할 필요가 있다.

### Class Diagram

> 클래스(Application) 수준에서의 사용방식

#### 범용적인 설계구조

> static (non final)

![범용적인 설계구조](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuU9ApaaiBbO8pinBpqajoSzJgEPI089ge8AIpEHQ1TtCF20pBpdL2g46h48NpjNGHDMYdPvQuWdL1PIhvU9oICrB0Le60000)

#### JVM 기반에서의 효율성 강화

> Class Loader 의 Load 시점을 고려한 설계

![JVM 기반에서의 효율성 강화](https://www.plantuml.com/plantuml/png/SoWkIImgAStDuU9ApaaiBbO8pinBpqajoSzJgEPI009Tk8269bYW6gYZ93EvLa4xFRN4Cm_8oqjE1ShLOfYWvvDOLUcHdvEIMgHGZQfNDnEgqGgX76G2mdPjNLsi9d5nEQJcfG0T3000)

## Class Loader 의 Load 방식

로드시점은 다음 두가지로 구분된다.

- Load-Time Dynamic Loading
- Runtime Dynamic Loading

전자는 JVM 이 ByteCode 의 main() 부터 순차적으로 코드를 검사하면서, `참조` 하는 __클래스들을__ `Method Area` 에 적재하는 방식이다.
후자는 `Reflection` 처럼 실행 중에 대상이 지정될 때, 해당 클래스를 `Method Area` 에 적재하는 방식이다.
전자도 Dynamic 이라 불리는 이유는 클래스의 사용이 `명시적 참조`가 아닌 `암시적 참조` 는 제외되기 때문이다.
실제로 같은 main() 를 소유하는 클래스 안에 `inner 클래스` 가 하나 더 존재해도 이를 실제로 참조하고 있지 않다면,
메모리에 적재하지 않는다. 이러한 이유를 `External Class Loader` 를 생각하면 우리가 외부에서 가져온 코드들을 실제로 다 사용하지 않기에
필요한 것만 사용하겠다는 JVM 구현을 위한 요구사항에 기반한다.

그렇게 적재되는 `명시적 참조` 된 클래스 정보도, 전부 적재되지는 않는다.
내부 구조에서 변동성이 있는 정보(클래스 상태/형태)만을 적재하고,
변동성이 없는 다음 항목들은 제외된다.

- Constant : `static` + `final` 이 동시에 명시될 경우, JVM 상의 불변대상이므로 Class 구조와 무관하게 처리된다.
- Literal : Operand 인 문자 또는 숫자값, 이때 숫자값은 메모리 설정시 효율이 떨어지는 큰 수에 해당된다. (e.g, `int num = 1111111111111;` 인 경우 `1111111..` 의 할당비효율성으로 Literal 로 취급된다.)
- Static Nested(Inner) Class : `static` 이 지정된 `Inner Class` 는 클래스구조가 불변(Final)인 점을 고려하여 `Outer Class` 에 종속되지 않는다 판단하여 별도로 처리된다.

> 이후 나올 `Holder` Idiom 은 마지막 제외기준을 활용한다.

## Idiom

> 보편화된 구현틀을 말하며, 관용구라 부른다.

### `Eager Initialization`

> `non-final` 이기에 Class Load 시에 적재(할당,연결,초기화)된다.

- 장점
    - Class Loader(JVM)의 구조적인 단일보장에 책임을 전가하므로 안정성이 높다.
- 단점
    - 실재로 instance 변수에 유무와 관계없이 클래스의 사용만으로 초기화되므로 메모리 낭비(Memory Leak)가 존재한다.

```java
public class Singleton {

    private static Singleton instance = new Singleton();

    private Singleton () {

    }

    public static Singleton getInstance() {
        return instance;
    }

}
```

### `Lazy Initialization`

> getInstance() 를 호출할 때, 메모리에 할당한다.

- 장점
    - 메모리 낭비를 방지할 수 있다.
- 단점
    - `Thread-safe` 하지 않으므로 중복된 초기화를 진행할 수 있다.

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

}

```

### `Lazy Initialization` + `Synchronized(Thread-safe)`

> `synchronized` 를 추가하여 `Thread-safe` 를 제공한다.

- 장점
    - 중복 초기화가 발생하지 않는다.
- 단점
    - `synchronized` 는 `Class` 기준의 `Lock` 이 적용되므로, `Queue` 방식의 구조적인 속도 정체(비용)가 발생한다.

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {

    }

    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }

}

```

### `Lazy Initialization` + `Synchronized(Thread-safe)` + `Double-Checked Locking`

> `synchronized bracket` 을 사용하여 사전 체크를 제공한다.

- 장점
    - `Thread-safe` 를 제공함과 동시에 `Lock` 을 최소화한다.
- 단점
    - JVM 요구사항에 따라 `메모리 할당` > `연결` > `초기화` 순서로 객체를 생성하게 되는데, null 의 유무는 `메모리 할당` 에 따라 판별된다.
    - 즉, A 가 `초기화`를 완료하기 전인 찰나의 순간에 B 가 접근할 경우 재 초기화를 시도하는 __하드웨어적인 결함__ 이 있다.

```java
public class Singleton {

    private static Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

}
```

### `Lazy Initialization` + `Synchronized(Thread-safe)` + `Double-Checked Locking` + `Without CPU Cache`

> `volatile` 을 지정하여 `Thread` 들이 데이터를 메모리에서 접근하여 동기화 문제를 줄인다.

- 장점
    - `Cache` 를 위한 처리시간이 사라지므로 `Thread-safe` 를 위한 찰나의 비동기화 시간을 줄인다.
- 단점
    - `volatile` 가 메모리를 직접접근하므로 `Multi-Write` 에 안정성을 보장하지 않는다.
        - 찰나의 순간에 들어온 처리가 있다면 후순위 대상의 처리로 `Overwrite` 된다.
    - `volatile` 는 `Non-Cache` 이므로 매번 읽는 시간(비용)에 더 소요된다.
    - 또한, 이러한 구조를 위해서 제공되는 키워드가 아니기에 기능의 탄생배경과 맞지 않아 위험성이 크다.

```java
public class Singleton {

    private static volatile Singleton instance;

    private Singleton() {

    }

    public static Singleton getInstance() {
        if (instance == null) {
            synchronized (Singleton.class) {
                if (instance == null) {
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }

}
```

### `Lazy Initialization` + `Nested Static Class Holder`

> Class Loader(JVM) 의 처리방식에 안정성을 위임한다.

- 장점
    - 소스코드가 아닌 JVM 의 요구사항에 맞게 보장되므로, 가장 안정성이 높은 방식이다.
    - __가장 최신의 방법으로 권장되고 있다.__
- 단점
    - JVM 구현체에 따라 예기치 못한 문제가 발생할 수 있다.
        - 하지만 이는 개인의 코드가 아닌 전체적인 개발자 생태계에 의해 검증되기에 개인이 걱정할 문제는 아니다.

```java
public class Singleton {

    private Singleton() {

    }

    public static Singleton getInstance() {
        return Holder.instance;
    }

    private static class Holder  {
        private static final Singleton instance = new Singleton();
    }
}
```

## `JAVA API` 의 `Singleton`

> `JAVA API` 에서의 `Singleton` 는 주로 단일 소프트웨어가 아닌 `Collection` 기준의 유일성을 말한다.

### 호출방법

- `List.of(E e)` : Java 9 부터 지원, 정확히는 Immutable 기반 Collection 을 활용한 Overload Method
- `Collections.singletonList(E e)` : Collection 수준의 Singleton 를 목표로 제공된 기능

### 구현구조

- Collection 수준에서 구현하는 방법은 크게 1) `final` 제한, 2)`Exception` 처리 두 가지로 나뉜다.
- 구현체에 따라 중복되서 사용되는 경우도 존재한다.

```java

private static class SingletonList<E> {

  private final E element;
  
  @Override
  public boolean removeIf(Predicate<? super E> filter) {
    throw new UnsupportedOperationException();
  }
  
  @Override
  public void replaceAll(UnaryOperator<E> operator) {
    throw new UnsupportedOperationException();
  }
}

```

## `Spring` 의 `Singleton`

`Spring` 의 `Singleton` 은 DI 를 위한 IoC 에서 활용된다.
IoC 는 `POJO` 를 `Bean` 이라는 단위로 저장하여 사용하고 `Bean`의 기본 관리 수준은 소프트웨서 상의 단일 개체이며 이를 `SingletonScope` 라고 부른다.
물론 다른 수준의 `Singleton` 을 제공하는데, 크게 두가지로 `ConfigurableBeanFactory`, `WebApplicationContext` 로 나뉜다.
`ConfigurableBeanFactory` 는 기본적인 `Spring Framework` 의 IoC 이고, `WebApplicationContext` 는 `Spring MVC` 기반의 IoC 이다.
기본형은 다시 Default 인 `SingletonScope` 과 항시 재생성되는 `PrototypeScope` 으로 나뉘며 주로 Status 기반의 Object 에 활용된다.
MVC 는 `HTTP` 기준의 요청으로 수준을 나누는데 WAS(`Servlet`) 수준인 `ApplicationScope`, 연결 수준인 `SessionScope`, 요청 수준인 `ReqeustScope` 로 나뉜다.
즉 `Spring` 내에서도 여러 수준을 위한 `Singleton` 이 프레임워크에 의해 관리되고 있다.

### Scope 지정방법

#### `SingletonScope`

```java
import org.springframework.beans.factory.config.ConfigurableBeanFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class SingletonBean {

    @Bean
    public Object singleton1() {
        return new Object();
    }

    @Bean
    @Scope("singleton")
    public Object singleton2() {
        return new Object();
    }

    @Bean
    @Scope(ConfigurableBeanFactory.SCOPE_SINGLETON)
    public Object singleton3() {
        return new Object();
    }
}

```

#### `PrototypeScope`

```java
import org.springframework.beans.factory.config.ConfigurableBeanFactory;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;

@Configuration
public class PrototypeBean {

    @Bean
    public Object prototype1() {
        return new Object();
    }

    @Bean
    @Scope("prototype")
    public Object prototype2() {
        return new Object();
    }

    @Bean
    @Scope(ConfigurableBeanFactory.SCOPE_PROTOTYPE)
    public Object prototype3() {
        return new Object();
    }
}
```

#### `RequestScope`

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.annotation.RequestScope;

@Configuration
public class RequestBean {

    @Bean
    public Object request1() {
        return new Object();
    }

    @Bean
    @Scope("request")
    public Object request2() {
        return new Object();
    }

    @Bean
    @Scope(WebApplicationContext.SCOPE_REQUEST)
    public Object request3() {
        return new Object();
    }

    @Bean
    @RequestScope
    public Object request4() {
        return new Object();
    }
}

```

#### `SessionScope`

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.annotation.SessionScope;

@Configuration
public class SessionBean {

    @Bean
    public Object session1() {
        return new Object();
    }

    @Bean
    @Scope("session")
    public Object session2() {
        return new Object();
    }

    @Bean
    @Scope(WebApplicationContext.SCOPE_SESSION)
    public Object session3() {
        return new Object();
    }

    @Bean
    @SessionScope
    public Object session4() {
        return new Object();
    }
}
```

#### `ApplicationScope`

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Scope;
import org.springframework.web.context.WebApplicationContext;
import org.springframework.web.context.annotation.ApplicationScope;

@Configuration
public class ApplicationBean {

    @Bean
    public Object application1() {
        return new Object();
    }

    @Bean
    @Scope("application")
    public Object application2() {
        return new Object();
    }

    @Bean
    @Scope(WebApplicationContext.SCOPE_APPLICATION)
    public Object application3() {
        return new Object();
    }

    @Bean
    @ApplicationScope
    public Object application4() {
        return new Object();
    }
}
```

## 패턴의 조합

### `Delegator` 패턴에서 사용될 수 있는 `Singelton` Idiom

> 아래 예시는 `Eager Initialization` 으로 Delegator 패턴을 제공한다.

```java

class Delegators {
    
    private static final Map delegators;
    
    static {
      delegators = Maps.of(
              "algorithm1", new Delegator1(),
              "algorithm2", new Delegator2(),
              "algorithm3", new Delegator3()
      );
    }
    
    public <T extends Delegator> T get(String algorithm) {
        return delegators.get(algorithm);
    }
    
}

```

## 주의사항

class 는 Method Area 를 사용한다. 소프트웨어 수준의 `Sinlgeton` 는 class 기준의 load 를 활용하므로
많이 사용할 수록 JVM 의 메모리관리에서 해당 영억의 확보를 고려해야한다.

## 그 외

`Singleton` 의 구현방식 중 하니인 `Immutable` 은 `external class` 를 활용할 경우에 Parameter 의 __불변성을 확보__ 할 수 있다.
개인이 집적코드를 작성하여 __method 에서 parameter 를 `final` 로 지정__ 할 수도 있지만, 그 외의 __사용자 관점에서 오류를 예방하거나 Mocking 에서 많이 사용된다.__
