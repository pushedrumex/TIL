# GC 알고리즘

## Serial GC
```
java -XX:+UseSerialGC -jar Application.java
```
- Young 영역은 Mark Sweep 알고리즘으로 수행
- Old 영역은 Mark Sweep Compact 알고리즘으로 수행
    Compact: Heap 을 정리하기 위한 단계로, 유효한 객체들이 연속되게 쌓이도록 힙의 가장 앞 부분부터 채워서 빈공간을 확보
- 단일 스레드로 수행되기 때문에 CPU 코어가 1개인 환경에서 사용

## Parallel GC
```
java -XX:+UseParallelGC -jar Application.java
```
- java 7,8 의 기본 알고리즘
- 기본적인 처리 과정은 Serial GC 와 동일
- Young 영역은 멀티 스레드를 통한 병렬 처리

## Parallel Old GC
- Young 영역 뿐만 아니라 Old 영역도 멀티 스레드를 통한 병렬 처리
- Old 영역에서 Mark Summary Compact 알고리즘으로 수행
    Summary: Mark 를 수행한 영역에 대해서 별도로 유효 객체를 식별

## CMS(Concurrent Mark Sweep) GC
```
java -XX:+UseConcMarkSweepGC -jar Application.java
```
- java 9 에서 deprecated, java 14 에서 사용 중지
- Mark Sweep 알고리즘을 Concurrent 하게 수행
- CPU를 많이 필요로하며 Compaction 단계가 기본적으로 제공되지 않음
- 조각난 메모리가 많을 경우 Compaction 작업을 실행하면 다른 GC보다 stop the world 시간이 증가

## G1(Garbage First) GC
```
java -XX:+UseG1GC -jar Application.java
```
- java 7부터 지원, java 9 이후 기본 GC로 등극
- Region 이라는 개념을 도입하여 Heap 영역을 균등하게 여러 Region 으로 나눔
- garbage 가 많은(Garbage First) Region 에 대해서만 GC 를 수행하여 stop the world 시간을 줄임
- 각 Region 을 Eden, Survivor, Available/Unused, Humongous, Old 으로 동적으로 설정
  - Eden: 새로 생성된 객체가 할당되는 영역
  - Survivor: Eden 영역에서 살아남은 객체가 이동하는 영역
  - Available/Unused: 사용되지 않는 영역
  - Humongous: 크기의 50%를 초과하는 객체를 저장하는 영역
  - Old: 오래된 객체가 저장되는 영역
- Minor GC
  1. Young 영역의 Eden 영역이 가득 차면 GC 가 발생
  2. 이 때, garbage 가 많은 Region 에 대해 우선적으로 GC 를 수행
  3. 살아 남은 객체를 다른 지역으로 이동
  4. 이동되는 지역이 Available/Unused 영역이면 해당 영역은 이제 Survivor 영역
  5. Eden 영역은 Available/Unused 영역으로 변경
- Major GC
  시스템이 운영되다가 객체가 너무 많아 빠르게 sweep이 불가능할 때 Major GC(Full GC)가 수행
