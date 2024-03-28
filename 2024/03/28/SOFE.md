## OutOfMemoryError 란?
- JVM이 할당된 메모리를 모두 사용하고 더 이상 메모리를 할당할 수 없을 때 발생하는 에러

### OutOfMemoryError 예시
- Heap 영역이 부족할 때 발생하는 OutOfMemoryError
```java
String[] strings = new String[Integer.MAX_VALUE];
```

## StackOverflowError 란?
- 스택 메모리가 가득 찼을 때 발생하는 에러

### StackOverflowError 예시
- 재귀 호출이 너무 깊게 들어가서 발생하는 StackOverflowError
```java
public static void main(String[] args) {
    recursive();
}

private static void recursive() {
    recursive();
}
```

#### [실습 코드 저장소](https://github.com/pushedrumex/Laboratory/tree/main/Exception/oome)