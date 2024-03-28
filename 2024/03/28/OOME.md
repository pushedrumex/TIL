## OutOfMemoryError 란?
- JVM이 할당된 메모리를 모두 사용하고 더 이상 메모리를 할당할 수 없을 때 발생하는 에러

### OutOfMemoryError 예시
1. 배열의 사이즈가 JVM 이 할당할 수 있는 최대 메모리보다 큰 경우 발생
```java
String[] strings = new String[Integer.MAX_VALUE];
```
2. Heap 영역이 부족할 때 발생
```java
ArrayList<String> list = new ArrayList();
while (true) {
    list.add(new String());
}
```

#### [실습 코드 저장소](https://github.com/pushedrumex/Laboratory/tree/main/exception/oome)