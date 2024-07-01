# Proxy
- 타겟을 대신해서 요청을 받는 대리자 역할을 하는 객체
- 타겟은 프록시를 통해 최종적으로 요청을 받아 처리
- 타겟은 자신의 기능에만 집중하고 부가 기능은 프록시에게 위임
- 스프링의 AOP 는 기본적으로 프록시 방식으로 동작

## JDK Dynamic Proxy
- 자동 프록시 생성기를 통해 직접 프록시 객체 생성
- 프록시를 마치 실제 빈인 것처럼 포장하여 사용
- 구체 클래스는 빈으로 등록할 수 없으며 인터페이스로만 빈을 주입받아야함
- 프록시와 실제 빈은 모두 동일한 인터페이스를 구현
- 인터페이스 기반 프록시 생성에 사용하며 인터페이스를 구현한 클래스가 아니면 프록시 객체를 생성할 수 없음
- 프록시를 적용하기 위해선 반드시 인터페이스를 생성
- 구체 클래스는 빈으로 등록되지 않아 주입받을 수 없으며 인터페이스로만 빈을 주입받아야함
- 인터페이스 없는 구체 클래스는 프록시를 사용할 수 없음 -> CGLIB 등장

## CGLIB Proxy
- 클래스 상속을 기반으로 프록시 구현
- SpringBoot 는 CGLIB 라는 바이트 코드 조작 라이브러리를 사용하여 프록시 구현
- 인터페이스 없이 구체 클래스에 프록시를 적용할 수 있도록 해주는 라이브러리
- 스프링 AOP 는 기본적으로 CGLIB 를 사용하여 프록시를 생성
- 상속을 이용하므로 기본 생성자를 필요로 함
  - 만약 구체 클래스의 생성자가 매개변수를 가지고 있다면 기본 생성자를 명시적으로 선언해야함 
  - 자식 클래스로 객체를 생성할 때 부모클래스의 기본 생성자 호출
  - 자바에서 상속은 부모 클래스가 재정의한 생성자는 자식 클래스에게 상속되지 않음