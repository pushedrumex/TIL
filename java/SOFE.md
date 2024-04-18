## StackOverflowError 란?
- 스택 메모리가 가득 찼을 때 발생하는 에러

### StackOverflowError 예시
1. 재귀 호출이 너무 깊게 들어가서 발생하는 StackOverflowError
```java
public static void main(String[] args) {
    recursive();
}

private static void recursive() {
    recursive();
}
```

#### [실습 코드 저장소](https://github.com/pushedrumex-labs/java/blob/main/src/error/StackOverflow.java)