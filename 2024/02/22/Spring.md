# Spring

## Spring vs Spring Boot
- Spring: java 기반의 오픈 소스 프레임워크로, 웹 어플리케이션 개발을 위한 다양한 모듈 제공
- Spring Boot: Spring 프레임워크 기반의 웹 개발 도구로, Spring 기반의 웹 어플리케이션을 빠르고 쉽게 만들 수 있도록 도와줌. 기본적인 설정 파일을 자동으로 처리해주고 내장형 서버인 톰켓 서버를 제공해줌

## Spring Bean
스프링 컨테이너에서 생성되고 관리되는 객체

###  컴포넌트 스캔
자바 실행 파일 (Application)이 존재하는 패키지 하위에 @Component 를 가진 클래스들을 찾아서 Bean으로 등록
- @Controller, @Service, @Repository 은 @Component 를 포함하고 있어 컴포넌트 스캔의 대
1. @Controller : 스프링 MVC 컨트롤러로 인식
2. @Service : 스프링 비지니스 로직에서 사용
특별한 처리가 따로 없고 비지니스 계층을 인식하는데 도움이 됨
3. @Repository : 스프링 데이터 접근 계층에서 사용
데이터 계층에서 발생하는 예외를 스프링이 추상화하여 서비스 계층에서 추상화된 예외에 의존하도록 해줌

### @Configuration
@Bean 을 사용하여 직접 Bean으로 등록할 객체들을 설정

## DI
객체들 간의 의존관계를 설정하는것으로 3가지 방법(필드 주입, setter 주임 생성자 주입)이 존재
1. 필드 주입
가장 짧은 코드로 구현이 가능. DI 프레임워크에 의존
```java
@Autowired
private MemberService memberService;
```
2. setter 주입
Setter주입 방법의 가장 큰 단점은 public이 의무화되는 메서드에서 개발자가 임의로 호출할 수 있기 때문에 보안상의 문제와 실수로 인한 임의적인 변경 가능성이 있기 때문에 권장되지 않는다.
```java
@Autowired
public void setMemberService(MemberService memberService) {
		this.memberService = memberService;
}
```
3. 생성자 주입
- Null을 주입하지 않는한 NullPointException이 발생하지 않는다.
- final 키워드를 사용하여 객체의 불변을 유지할 수 있다.
- 개발자의 실수로 빈의 주입이 이루어지지 않았을 경우 final 키워드를 사용하면 컴파일 시점에서 오류를 잡아낼 수 있다.

```java
@Autowired
public void setMemberService(MemberService memberService) {
		this.memberService = memberService;
}
```

## IoC
제어의 역전을 의미하며, 개발자가 직접 객체를 생성하고 관리하느 것이 아닌 Ioc 컨테이너가 객체를 생성하고 관리

#### @Autowired
스프링 컨테이너는 컴포넌트 스캔을 통해 스프링 빈들을 생성. 이 과정에서 @Autowired 애노테이션이 있는 부분을 탐색하여, 스프링 컨테이너에서 해당 타입의 빈을 찾아 의존관계를 주입
- 생성자에 @Autowired를 사용하면, 객체 생성 시점에 스프링 컨테이너에서 해당 스프링 빈을 찾아서 주입한다. 생성자가 1개만 있으면 @Autowired는 생략 가능하다. 

## SOLID
좋은 객체지향 설계의 5가지 원칙
1. SRP(Single Responsibility Principle): 단일 책임 원칙
한 클래스는 하나의 책임만 가져야한다
2. OCP(Open/Closed Principle): 개방-폐쇄 원칙
확장에는 열려있고 변경에는 닫혀있어야한다
3. LSP(Liskov Substitution Principle): 리스코프 치환 원칙
하위타입은 상위 타입을 대체할 수 있어야한다
4. ISP(Interface Segregation Principle): 인터페이스 분리 원칙
인터페이스는 그 인터페이스를 사용하는 클라이언트 기준으로 분리해야한다
5. DIP(Dependency Inversion Principle): 의존관계 역전 원칙
추상화에 의존해야지 구체화에 의존하면 안된다

## BeanFactory vs ApplicationContext

### BeanFactory

- 스프링 컨테이너의 최상위 인터페이스
- 스프링 빈을 관리하고 조회하는 역할
- getBean()제공

### ApplicationContext

- BeanFactory를 상속
- BeanFactory기능 외의 부가기능 제공

## @Configuration
- Config 클래스를 상속하는 프록시 클래스를 만들어서 스프링 빈으로 등록
- @Bean이 붙은 메서드 마다 스프링 빈이 존재하는지 확인하여 스프링 빈이 존재한다면 반환하고 존재하지 않는다면 스프링 빈을 생성하여 등록하고 반환하여 싱글톤 보장

## Bean Scope
빈이 존재할 수 있는 범위를 뜻하며, 스프링 빈은 기본적으로 싱글톤 스코프로 생성

- 싱글톤 : 기본 스코프로, 스프링 컨테이너의 시작과 종료까지 유지
- 프로토타입 : 스프링 컨테이너는 빈의 생성과 의존관계 주입까지만 관여하고 더는 관리하지 않음
프로토타입 스코프 빈을 스프링 컨테이너에 조회하면 스프링 컨테이너는 항상 새로운 인스턴스를 생성해서 반환
- 웹 관련 스코프
    - request : 웹 요청이 들어오고 나갈때 까지 유지
    - session : 웹 세션이 생성되고 종료될때까지 유지
    - application : 웹의 서블릿 컨텍스트와 같은 범위로 유지