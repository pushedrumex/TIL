# Stream
- java 8 에서 추가된 기능
- 컬렉션의 요소를 하나씩 참조하여 람다식으로 처리할 수 있도록 해주는 반복자
- 불필요한 for 문과 if 문 등의 코드를 줄여주고 가독성을 높여줌
- 원본 데이터를 변경하지 않음
- Stream 의 요소 처리 메서드는 함수형 인터페이스를 매개변수 타입으로 사용
- 내부 반복자를 사용하므로 병렬처리가 쉬움
  - 외부 반복자: 개발자가 직접 컬렉션의 요소를 반복
  - 내부 반복자: 컬렉션 내부에서 요소들을 반복시키고 개발자는 요소당 처리해야 할 코드만 제공
- 병렬(parallel) 처리
  - 한 가지 작업을 서브 작업으로 나누고, 서브 작업들을 분리된 스레드에서 병렬로 처리하는 것
  - 자바는 Fork Join 프레임워크를 사용하여 병렬 처리

## Stream 의 구조
```java
컬렉션.stream().중개연산().최종연산();
```
- 중개 연산: Stream 을 전달 받아 Stream 을 반환
  ex) filter(), map(), sorted()
- 최종 연산: Stream 을 전달 받아 다른 타입의 값을 반환
  ex) void forEach(), long count(), <R, A> R collect()

## Stream 생성

### 컬렉션 스트림 생성
```java
ArrayList<Integer> list = new ArrayList<>();
Stream<Integer> stream = list.stream();
stream.forEach(System.out::println);
```

### 배열 스트림 생성
```java
int[] arr = {1, 2, 3, 4, 5};
IntStream intStream = Arrays.stream(arr);
intStream.forEach(System.out::println);
```

### Stream.of() 메서드
```java
Stream<String> stringStream = Stream.of("a", "b", "c");
stringStream.forEach(System.out::println);
```

## 병렬 처리
멀티 코어 CPU 환경에서 하나의 작업을 분할하여 각각의 코어가 병렬 처리
- 병렬 스트림은 내부적으로 포크조인 프레임워크를 이용

### Fork Join Framework
1. 포크 단계
   - 하나의 작업을 여러 개의 작은 Task 로 나누는 것
   - Task 들을 멀티 코어에서 병렬로 처리
2. 조인 단계
   - Task 들의 결과를 합치는 단계
   - 모든 Task 가 완료될 때까지 대기
### ForkJoinPool
  - 각각의 코어에서 서브 요소를 처리하는 스레드 풀
  - Fork Join Framework 는 ForkJoinPool 을 이용하여 작업 스레드를 관리

### 병렬 스트림 생성
- parallelStream(): 컬렉션을 병렬 스트림으로 변환
- parallel(): 기존의 스트림을 병렬 스트림으로 변환

### 병렬 처리 성능
- 스트림 병렬 처리가 스트림 순차 처리보다 항상 빠른 것은 아님
- 병렬 처리는 스레드 풀 생성, 스레드 간의 작업 분할, 스레드 간의 작업 결과 합병 등의 오버헤드가 발생
- 요소의 수: 요소의 수가 적을 경우 순차 처리가 더 빠를 수 있음
- 요소의 처리 시간: 요소의 처리 시간이 짧을 경우 병렬 처리가 더 느릴 수 있음
- 코어의 수: 코어의 수가 적을 경우 병렬 처리가 더 느릴 수 있음
- 스트림 소스의 종류
  - ArrayList, 배열은 인덱스를 이용하여 요소를 처리하기 때문에 병렬 처리가 빠를 수 있음
  - LinkedList 는 랜덤 엑세스를 지원하지 않기 때문에 병렬 처리가 느릴 수 있음

## Stream vs for
- Stream 은 for 문보다 빠르지 않음 (병렬 처리 제외)
- Stream 을 사용할 경우 Stream 생성, 중개 연산, 최종 연산 등의 오버헤드가 발생
- Stream 은 Wrapper 클래스를 사용하기 때문에 기본 자료형을 사용하는 for 문보다 느릴 수 있음
- 자바 컴파일러가 for 문에 내부 최적화가 잘 되어 있음