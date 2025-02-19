## List
- ArrayList
  - 배열과 유사
  - 초기 용량 디폴트 값은 10
  - 용량이 꽉 찼을 경우 더 큰 배열을 생성하여 기존 배열의 내용을 복사하는 방식으로 용량을 늘림
  - 인덱스로 원소에 접근하기 때문에 시간복잡도는 O(1)
  - 배열의 크기가 크면 배열을 옮기는 과정에서 시간이 많이 소요됨
  - 중간 요소의 추가 삭제는 재배열이 필요함

- LinkedList
  - 양방향 연결 리스트
  - 참조하려는 원소에 따라 순방향 또는 역방향으로 순회 가능
  - Head 에서부터 찾아야 하므로 시간복잡도는 O(n)
  - 중간 요소의 추가 삭제는 재배열이 필요 없음

## Map
- HashMap
  - key, value 쌍을 저장하는 자료구조
  - key 를 해싱하여 해시코드를 생성하고, 해시코드를 배열의 각 요소인 버킷의 인덱스로 사용
  - 해시코드가 충돌이 날 경우 LinkedList로 연결 -> hashcode()로 비교 후 동일하다면 equals()를 사용하여 비교. 따라서 hashcode()와 equals()를 함께 오버라이딩 해야함

- HashTable
  - HashMap 과 동일한 내부구조를 갖음
  - HashMap 과 달리 synchronized 된 메소드로 구성되어 있어 thread-safe

- ConcurrentHashMap
  - HashTable 의 성능을 개선한 버전
    - HashTable 과는 달리 put 메서드 자체에 synchronized 를 걸지 않고 해시 충돌 발생 처리 부분에만 synchronized 를 걸어 성능을 개선
  - Entry를 조작하는 경우에 bucket 별로 동기화를 진행
  - 동시성을 지원하여 thread-safe
  - Node 를 삽입하는 경우 lock 없이 CAS(Compare And Swap) 연산을 사용하여 원자성 보장

### ConcurrentMap 의 putVal()
1. key 의 해시코드를 구함
2. 해시코드를 이용하여 버킷의 인덱스를 구함 
3. 버킷이 비어있으면 node 를 담고 있는 volatile 변수를 통해 메인 메모리에 접근하여 CAS 연산을 통해 삽입
4. 버킷이 존재한다면 synchronized 블록을 통해 해당 버킷에 대한 동기화를 진행

- TreeMap
  - 이진 트리 구조
  - 레드 블랙 트리 높이 재정렬 logN 유지
  - key 기준 자동 정렬

- LinkedHashMap
  - 연결 리스트를 통해 삽입 순서를 보장

## 해시 충돌
- 해시 충돌: 서로 다른 키가 동일한 해시 코드를 가질 경우 발생
- 해결 방법
  - 개방 주소법: 빈 버킷을 찾음
    - 선형 조사: 다음 버킷을 찾음
    - 이차 조사: 제곱수를 더하여 다음 버킷을 찾음
    - 랜덤 조사: 임의의 위치로 이동
  - 체이닝: 같은 해시 값을 갖는 요소를 연결 리스트로 관리

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
  - 자바의 자료구조는 hashCode() 를 사용하여 객체를 저장하고 검색하기 때문 
  - equals() 를 재정의한다면 hashCode() 도 재정의해야함
- equals() 의 재정의 예시
```java
@Override
public boolean equals(Object o) {
    // 동일한 주소값인지 확인
    if (this == o) {
        return true;
    }
    // null 이거나 클래스가 다른지 확인
    if (o == null || getClass() != o.getClass()) {
        return false;
    }
    // 필드값이 같은지 확인
    Store store = (Store) o;
    return storeId == store.storeId && Objects.equals(name, store.name);
}
```
- hashCode() 의 재정의 예시
```java
@Override
public int hashCode() {
    return Objects.hash(storeId, name); // 동등성에 고려되는 필드들을 이용하여 해시코드 생성
}
```
