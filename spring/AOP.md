# AOP
- Aspect Oriented Programming
- 관점 지향 프로그래밍
- 공통 관심 사항과 핵심 관심 사항을 분리
- 공통 관심 사항과 핵심 관심 사항인 비지니스 로직이 섞여 있으면 유지보수가 어려움
- 공통 관심 사항에 변경이 필요하면 해당 로직만 변경하면 돼서 유지보수에 용이
- AOP 를 사용하면 간편하게 공통 관심 사항을 적용할 대상을 선택할 수 있음

## AOP 의 구성
- Aspect: Advice 와 PointCut 을 결합한 것
- Advice: 핵심 로직에 적용할 공통 로직
  - Before: 메서드 실행 전에 실행
  - After: 메서드 실행 후에 실행
  - AfterReturning: 메서드가 성공적으로 실행된 후에 실행
  - AfterThrowing: 메서드 실행 중 예외가 발생한 후에 실행
  - Around: 메서드 실행 전과 후에 실행
- PointCut: 어떤 메서드에 Advice 를 적용할지 결정
- JoinPoint: AOP 대상이 되는 메서드 정보에 접근할 수 있는 개체
  - proceed: 해당 메서드를 호출하고 그 결과를 반환
  - getArgs: 메서드의 인자 목록을 반환
  - getSignature: 메서드에 대한 정보를 반환

## Pointcut 표현식
- 어노테이션 기반
```
@annotation(org.springframework.transaction.annotation.Transactional) // @Transactional 어노테이션이 붙은 메서드
```
- 빈 이름 기반
```
bean(*Controller) // 이름이 Controller 로 끝나는 빈
```
- 패키지 기반
```
within(com.example.service.*) // com.example.service 패키지에 속한 모든 클래스
```
- 메서드 기반
```
execution(* com.example.service.*.*(..)) // com.example.service 패키지에 속한 모든 메서드  
```

## 스프링에서 AOP 가 적용된 예
- 트랜잭션 관리: @Transactional 을 사용하여 메서드 호출 전후에 트랜잭션을 시작하고 종료
- 로깅: 메서드 호출 전후 또는 예외 발생 시 로그 출력
- 보안: 메서드 호출 전에 권한 확인
