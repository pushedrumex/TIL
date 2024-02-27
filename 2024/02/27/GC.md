# GC
- Garbage Collection
- JVM 의 Heap 영역에서 동적으로 할당했던 메모리 중 사용하지 않는 객체를 모아 주기적으로 제거하는 프로세스
- 메모리 누수를 방지하고 프로그램의 성능을 향상시키는데 도움을 줌
- GC 가 동작하는 동안에는 모든 애플리케이션 스레드가 중지되기 때문에 GC 가 자주 발생하면 성능에 영향을 줄 수 있음(STW: Stop-The-World)

# Heap 영역
- new 키워드로 생성된 객체, 배열 등이 저장되는 공간으로 GC 에 대상이 되는 공간
- Heap 영역은 크게 Young Generation, Old Generation 으로 나뉨

## Young Generation
- 짧게 살아남는 메모리들이 존재하는 공간
- 모든 객체는 처음에는 Young Generation 에 생성
- Old Generation 에 비해 공간이 적어 메모리 상의 객체를 찾아 빠르게 제거
- Minor GC: Young Generation 에서 발생하는 GC
- Young Generation 은 또다시 Eden, Survivor0, Survivor1 로 나뉨
- 모든 객체는 시작 점에서 Eden 영역에서 생성되며 점차 Survivor0, Survivor1, Old Generation 으로 이동
  1. Eden 영역이 가득 차면 Minor GC 가 발생
  2. 살아남은 객체들는 Survivor0 영역으로 이동
  3. Eden 영역이 가득 차면 Minor GC 가 발생
  4. 살아남은 객체들은 Survivor1 영역으로 이동
  5. 1~4 과정을 반복하다가 계속 살아남은 객체들은 Old Generation 으로 이동

## Old Generation
- 길게 살아남는 메모리들이 존재하는 공간
- Young Generation 에서 제거되지 않은 객체들이 Old Generation 으로 이동
- Young Generation 에 비해 공간이 커서 메모리 상의 객체 제거가 느림
- Major GC: Old Generation 에서 발생하는 GC
- Old Generation 이 가득 차면 Major GC 가 발생
- Old Generation 에서 GC 는 시간이 오래 걸리기 때문에 성능에 영향을 줄 수 있음