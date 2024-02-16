## 스프링 트랜잭션 추상화

- 데이터 접근 기술(JDBC, JPA 등)은 트랜잭션을 처리하는 방식이 다름
- 스프링은 이런 문제를 해결하기 위해 트랜잭션 추상화를 제공

```java
public interface PlatformTransactionManager extends TransactionManager {

    TransactionStatus getTransaction(@Nullable TransactionDefinition definition) throws TransactionException;

    void commit(TransactionStatus status) throws TransactionException;
    void rollback(TransactionStatus status) throws TransactionException;
}
```

### 트랜잭션 매니저 구현체
- JDBC: DataSourceTransactionManager
- JPA: JpaTransactionManager

## 트랜잭션과 AOP

- @Transactional 을 사용하면 프록시 방식의 AOP 가 적용
- 프록시를 통해 트랜잭션을 처리하는 객체와 비지니스 로직을 처리하는 서비스 객체를 명확하게 분리
- 프록시 객체가 실제 객체를 상속
- @Transactional 애노테이션이 특정 클래스나 메소드에 하나라도 있으면 트랜잭션 AOP 는 프록시를 만들어 컨테이너에 등록
- 실제 객체 대신 프록시 객체를 빈으로 등록
- 프록시 객체는 내부에 실제 객체를 참조
- public 메서드에만 트랜잭션을 적용하도록 기본 설정 되어있음

## 트랜잭션 프록시 동작 방식

1. 클라이언트가 프록시 객체의 메서드를 호출
2. 프록시 객체의 메서드가 호출
3. 프록시는 해당 메서드가 트랜잭션을 사용할 수 있는지 확인
4. @Transactional 이 붙어있다면 트랜잭션을 시작
5. 실제 객체의 메서드 호출
6. 프록시로 제어가(리턴) 돌아오면 프록시는 트랜잭션을 커밋하거나 롤백

## 프록시 내부 호출

- 트랜잭션을 적용하려면 항상 프록시를 통해서 대상 객체(Target)을 호출해야한다. 이렇게 해야 프록시에서 먼저 트랜잭션을 적용하고 이후에 대상 객체를 호출하게된다.
- 프록시를 거치지 않고 대상 객체를 직접 호출하면 AOP 가 적용되지 않고 트랜잭션도 적용되지 않는다.
- 대상 객체의 내부에서 메서드 호출이 발생하면 프록시를 거치지 않고 대상 객체를 직접 호출하게 되어서 @Transactional 이 있어도 트랜잭션이 적용되지 않는다. → 내부호출

### 내부 호출 시 트랜잭션이 적용되지 않는 이유
- 자바 언어에서 메서드 앞에 별도의 참조가 없을 경우 this 라는 뜻으로 자기 자신의 인스턴스를 가리킨다. 결과적으로 this 는 자기 자신을 가리키는 실제 대상 객체(target) 의 인스턴스를 뜻하므로 프록시를 거치지 않아 트랜잭션을 적용할 수 없다.
- 해결 방법: 별도의 클래스로 분리