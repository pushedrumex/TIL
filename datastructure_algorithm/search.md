## 선형 탐색
- 원하는 값을 갖는 요소를 만날 때까지 맨 앞부터 순서대로 요소를 탐색하는 방법
- O(n)

## 이진 탐색
- 배열 내부의 데이터가 정렬되어 있어야만 사용할 수 있는 알고리즘
- 탐색범위를 절반으로 좁혀가며 데이터를 탐색
- O(log n)
- 탐색 방법
    
  1. 시작점과 끝점을 통해 중간점 찾음
  2. 찾으려는 데이터보다 중간점의 데이터가 크다면 끝점을 중간점 - 1로 변경한 후 1을 실행
  3. 찾으려는 데이터보다 중간점의 데이터가 작다면 시작점을 중간점 + 1로 변경한 후 1을 실행