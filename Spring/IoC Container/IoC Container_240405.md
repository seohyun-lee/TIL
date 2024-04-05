## TIL - The IoC Container

### 1. Spring IoC Container 및 Bean 소개

Spring Framework에서의 **IoC**(Inversion of Control, 제어의 역전) 원칙의 구현인 **Spring IoC 컨테이너**와 **Bean**.

**DI**(Dependency Injection, 의속성 주입)은 IoC의 특수한 형태로, 객체가 생성자 인수, factory method 인수 또는 factory method에서 가져온 후 객체 인스턴스에 설정된 속성을 통해서만 의존성(즉, 함께 작업하는 다른 객체)을 정의하는 것이다. 그런 다음 IoC 컨테이너는 이러한 의존성을 bean을 생성할 때 주입한다.
(이 과정은 bean 자체가 의존성의 인스턴스화 또는 위치를 제어하는 것-클래스의 직접 구성 또는 Service Locator 패턴과 같은 매커니즘을 통해-과 근본적으로 반대이다.)

`org.springframework.beans`와 `org.springframework.`context` 패키지는 SpringFramework의 IoC 컨테이너의 기초이다.
BeanFactory 인터페이스는 모든 유형의 객체를 관리할 수 있는 고급 구성 메커니즘을 제공한다.
ApplicationContext는 BeanFatory의 하위 인터페이스이다. ApplicationContext가 추가하는 것들:
* Spring의 AOP 기능과 더 쉽게 통합 가능
* Message resource handling (메세지 리소스 처리, internationalization에 사용)
* Event publication (이벤트 발행)
* Application-layer specific contexts such as the WebApplicationContext for use in web applications.(웹 응용 프로그램에서 사용할 응용 계층별 컨텍스트)
`BeanFactory`는 구성 프레임워크와 기본 기능을 제공하며 `ApplicationContext`는 보다 엔터프라이즈급 기능을 추가한다. `ApplicationContext`는 `BeanFactory`의 완전한 상위 집합으로 이 장에서 IoC 컨테이너 설명에 독점적으로 사용된다. // ApplicationContext를 쓰며 설명할 것이라는 의미

Spring에서는 애플리케이션의 중추를 구성하고 Spring IoC 컨테이너에 의해 관리되는 객체를 **Bean**이라고 한다. Bean은 Spring IoC 컨테이너에 의해 인스턴스화, 조립 및 관리되는 객체이다. 그렇지 않으면 Bean은 단순히 애플리케이션의 많은 객체 중 하나일 뿐이다. Bean들과 그들 사이의 의존성들은 컨테이너에서 사용하는 configuration metadata에 반영된다.

---

### 2. Container 개요
`org.springframework.context.ApplicationContext` 인터페이스는 Spring IoC 컨테이너를 나타내며 빈을 초기화하고, 구성하고, 조립하는 책임을 갖는다.
컨테이너는 구성 메타데이터를 읽어서 어떤 객체를 인스턴스화하고 구성하고 조립할지에 대한 지시를 얻는다.
구성 메타데이터는 XML, Java 어노테이션 또는 Java 코드로 표현된다.
이를 통해 애플리케이션을 구성하는 객체와 그 객체들 간의 풍부한 상호 의존성을 표현할 수 있다.

Spring에는 `ApplicationContext` 인터페이스의 여러 구현체가 제공된다. 독립 실행현 애플리케이션에서는 `ClassPathXmlApplicationContext`또는 `FileSystemXmlApplicationContext`의 인스턴스를 생성하는 것이 일반적이다.
XML은 구성 메타데이터를 정의하는 전통적인 형식이지만, XML 구성에서 Java 어노테이션 또는 코드를 메타데이터 형식으로 사용하도록 컨테이너에 지시하여 이러한 추가 메타데이터 형식을 지원할 수 있다.

대부분엔 응용 프로그램 시나리오에서 Spring IoC 컨테이너에 인스턴스를 생성하기 위해 사용자가 직접 코드를 작성할 필요 없다. Spring이 제공하는 설정 파일에 몇 줄의 설정을 추가하는 것 만으로 IoC 컨테이너를 구성할 수 있다. 간단한 XML 코드로 가능함.

* Spring이 작동하는 고수준 개요: 애플리케이션 클래스는 구성 메타데이터와 결합되어 ApplicationContext가 생성되고 초기화된 후에 완전히 구성되고 실행 가능한 시스템 또는 애플리케이션을 갖게 된다.
* Spring Container가 Configuration Metadata와 개발자의 Business Objects(POJOs)를 이용해 Fully configured system(Ready for Use)을 produce한다.

### Configuration Metadata
Spring IoC 컨테이너는 구성 메타데이터의 형태를 사용한다. 이 구성 메타데이터는 애플리케이션 개발자로서 Spring 컨테이너에게 애플리케이션의 객체를 인스턴스화하고 구성하고 조립하도록 지시하는 방법을 나타낸다.
일반적으로 XML 형식으로 제공되는데, 유일히 허용된 형식은 아니다. 요즘에는 많은 개발자가 Spring 애플리케이션에서 Java 기반 구성을 선택한다.

Spring 구선은 컨테이너가 관리해야 하는 하나 이상의 빈 정의로 구성된다.
- XML 기반 구성 메타데이터는 <beans/> 요소 내부의 <bean/> 요소로 이러한 빈을 구성한다.
- Java 구성은 @Configuration 클래스 내부의 @Bean 주석이 달린 메서드를 사용한다.
이러한 빈 정의는 애플리케이션을 구성하는 실제 객체에 해당한다.
- 서비스 계층 객체, 영속성 계층 객체(Ex. 레포지토리 또는 DAO), 표현 객체(ex. 웹 컨트롤러), 인프라스트럭처 객체(ex. JPA EntityManagerFactory)를 정의한다.
- 일반적으로, 컨테이너에서 세부 도메인 객체를 구성하지 않는다. 도메인 객체를 생성하고 로드하는 것은 보통 리포지토리와 비즈니스 로직의 책임이다.


#### Instantiating a Container Spring IoC 컨테이너 생성
#### Composing XML-based Configuration Metadata
#### Groovy Bean Definition DSL
#### Using the Container

## 레퍼런스
https://docs.spring.io/spring-framework/reference/core/beans/introduction.html
https://docs.spring.io/spring-framework/reference/core/beans/basics.html

## QnA
---
