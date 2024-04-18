### stream vs for

### 실습 내용
- for, 스트림, 병렬 스트림 실행 시간 비교 
- 5000만개의 배열과 리스트를 순회하는 로직 시간 비교
- 랜덤한 수로 이루어진 배열과 리스트의 요소들 중 3으로 나누어 떨어지는 것들의 합을 구하는 로직
- 테스트 환경 : MacBook Air 로컬 환경 (Apple M1 RAM 8GB, CPU 8 core)

### 실행 결과

|                       | 실행 시간 (ms) |
|-----------------------|------------|
| for int[]             | 71         |
| stream int[]          | 348        |
| parallel stream int[] | 62         |
| for List              | 164        |
| stream List           | 588        |
| parallel stream List  | 204        |

- for ~= parallel stream > stream
- Collection 은 Heap 영역을 참조하기 때문에 primitive 보다 느림

#### [실습 코드 저장소](https://github.com/pushedrumex-labs/java/blob/main/src/for_stream/ForStream.java)
#### [학습 정리](https://github.com/pushedrumex/TIL/blob/main/2024/03/06/stream.md)