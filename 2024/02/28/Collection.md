## List
- ArrayList
  - 배열과 유사
  - 초기 용량 디폴트 값은 10
  - 용량이 꽉 찼을 경우 더 큰 배열을 생성하여 기존 배열의 내용을 복사하는 방식으로 용량을 늘림
  - 인덱스로 원소에 접근하기 때문에 시간복잡도는 O(1)
  - 배열의 크기가 크면 배열을 옮기는 과정에서 시간이 많이 소요됨

- LinkedList
  - 양방향 연결 리스트
  - 참조하려는 원소에 따라 순방향 또는 역방향으로 순회 가능
  - Head 에서부터 찾아야 하므로 시간복잡도는 O(n)

## Map
- HashMap
  - key, value 쌍으로 이루어진 데이터를 저장
  - null key, null value 를 허용
  - 해시 충돌 해결하는 방법? linked list -> tree ?

- HashTable
  - HashMap 과 동일한 내부구조를 갖음
  - null key, null value 를 허용하지 않음
  - HashMap 과 달리 synchronized 된 메소드로 구성되어 있어 thread-safe

- ConcurrentHashMap
  - HashTable 의 성능을 개선한 버전
  - 어떤 Entry를 조작하는 경우에 해당 Entry에 대해서만 락
  - 동시성을 지원하여 thread-safe
  - CAS 연산을 사용하여 동시성을 지원

- TreeMap
  - 이진 트리 구조
  - 레드 블랙 트리 높이 재정렬 logN 유지
  - key 와 value 값이 저장된 Entry 객체를 저장
  - key 기준 자동 정렬

- LinkedHashMap
  - 삽입 순서를 보장

## Object equals() & hashCode()
- equals(): 두 객체의 메모리 주소가 동일한지 비교
```java
public boolean equals(Object obj) {
    return (this == obj);
}

```
- hashCode(): Heap 에 저장된 객체의 메모리 주소를 반환
  - hashCode() 와 equals() 는 HashMap 같은 자료구조를 사용할 때 사용됨
```java
@IntrinsicCandidate
public native int hashCode();
```
- 동일한 객체는 동일한 hashCode 를 반환해야함
  - equals() 를 오버라이드 한다면 hashCode() 도 오버라이드 해야함
  - 재정의하는 방법
