# Reflection
- 구체적인 클래스 타입을 알지 못하더라도 클래스의 메서드, 타입, 변수들에 접근할 수 있도록 해주는 자바 API
- private 필드나 메서드에 접근할 수 있음
- 생성자의 매개변수에 대한 정보에는 접근이 불가능하여 기본생성자로 객체를 생성
- JVM 이 실행되면 클래스 로더에 의해 바이트 코드가 로딩되고, 클래스 메타데이터가 Method(Static) Area 에 저장
- Method Area 에 저장된 정보를 통해 런타임 시점에 동적으로 특정 클래스의 정보를 추출

## Reflection API 예시
```java
public class ReflectionExample {
    public static void main(String[] args) throws Exception {
        Class<?> aClass = Class.forName("com.example.demo.ReflectionExample"); // 클래스 이름으로 클래스 정보를 가져옴
        Constructor<?> constructor = aClass.getConstructor(); // 기본 생성자를 가져옴
        Object instance = constructor.newInstance(); // 생성자를 통해 객체를 생성
        Method method = aClass.getMethod("method1"); // 메서드 이름으로 메서드 정보를 가져옴
        method.invoke(instance); // 메서드를 실행
    }

    public void method1() {
    }
}
```
## Spring Reflection API
스프링은 Reflection API 를 기반으로 동작
- Bean Factory
  어플리케이션이 실행되고 런타임에 객체가 호출 될 때 동적으로 객체의 인스턴스를 생성하는데 이 때 BeanFactory 는 Reflection API 를 사용하여 클래스의 정보를 추출하고 인스턴스를 생성
- Jpa Entity
  JPA 는 지연 로딩을 위해 Reflection API 를 사용하여 프록시 객체를 생성하기 때문에 프록시 객체가 상속받는 엔티티 객체에 기본 생성자가 필요
