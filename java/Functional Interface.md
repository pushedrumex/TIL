# Functional Interface
- 추상 메소드를 하나만 가지고 있는 인터페이스
- @FunctionalInterface 애노테이션을 사용하여 함수형 인터페이스임을 명시할 수 있음
```java
@FunctionalInterface
public interface DoSomething {
    void dodo();
}
```
- 추상 메서드가 1개 있으면 조건에 만족. static, default 메서드는 없거나 여러개 있을 수 있음

## Functional Interface 사용 하기
1. 익명 내부 클래스 사용
```java
DoSomething doSomething = new DoSomething() {
    @Override
    public void dodo() {
        System.out.println("dodo");
    }
};
```
2. 람다 표현식 사용 (java 8 이상)
```java
DoSomething doSomething = () -> System.out.println("dodo");
```

## Java 가 제공하는 함수형 인터페이스
java.util.function 패키지에 여러 함수형 인터페이스가 제공됨
1. Predicate<T>
```java
@FunctionalInterface
public interface Predicate<T> {
    boolean java.test.Test(T t);
}
```
- T 타입을 받아서 boolean을 리턴하는 함수형 인터페이스
- 주로 조건을 검사하는데 사용
```java
Predicate<Integer> isPositive = i -> i > 0;
isPositive.java.test.Test(5); // true
isPositive.java.test.Test(-5); // false
```

2. Consumer<T>
```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```
- T 타입을 받아서 리턴하지 않는 함수형 인터페이스
- 주로 값을 소비하는데 사용
```java
Consumer<String> print = s -> System.out.println(s);
print.accept("hello"); // hello
```

3. Comparator<T>
```java
@FunctionalInterface
public interface Comparator<T> {
    int compare(T o1, T o2);
}
```
- T 타입을 받아서 int를 리턴하는 함수형 인터페이스
- 주로 정렬에 사용
```java
Comparator<Integer> comparator = (a, b) -> a - b;
comparator.compare(1, 2); // -1
comparator.compare(2, 1); // 1
```

4. Supplier<T>
```java
@FunctionalInterface
public interface Supplier<T> {
    T get();
}
```
- T 타입을 리턴하는 함수형 인터페이스
- 주로 값을 제공하는데 사용
```java
Supplier<String> supplier = () -> "hello";
supplier.get(); // hello
```
