# String
- 참조형 변수
- Heap 영역에서 문자열 데이터가 생성되고 관리됨
- 불변(Immutable) 객체로 한번 생성된 문자열은 변경 불가능
  - 문자열을 캐싱하여 메모리 공간 절약
  - thread-safe
  - 보안 측면: 주소값의 문자열이 변경이 가능하다면 보안에 취약
- JVM 은 String Constant Pool 이라는 공간에 문자열들을 상수화하여 다른 객체들과 공유

## new String() vs ""
- new String(): 새로운 객체 생성하고 Heap 영역에 저장
- "": String Constant Pool 의 리터럴 문자열을 사용

## String vs StringBuffer vs StringBuilder
- String: 불변 객체, 문자열 변경 시 새로운 객체 생성
- StringBuffer: 가변 객체, thread-safe
- StringBuilder: 가변 객체, thread-unsafe

## String equals() vs ==
- equals(): 문자열의 내용이 같은지 비교
```java
public boolean equals(Object anObject) {
    if (this == anObject) {
        return true;
    }
    return (anObject instanceof String aString)
            && (!COMPACT_STRINGS || this.coder == aString.coder)
            && StringLatin1.equals(value, aString.value);
}
```
- ==: 문자열의 주소값이 같은지 비교
