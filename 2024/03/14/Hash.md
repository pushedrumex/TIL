# Hash
- 데이터를 저장할 위치(인덱스)를 간단한 연산(해시 함수)로 계산하여 저장하는 자료구조
- 키: 키값을 통해 해시값을 계산하고 해시값을 통해 데이터에 접근
- 해시 테이블: 해시값과 데이터를 저장하는 자료구조
- 버킷: 해시 테이블의 각 요소

## 해시 충돌
키를 통해 얻은 해시값이 다른 키의 해시값과 동일한 경우
- 체이닝: 버킷에 연결 리스트로 데이터들을 연결하는 방법
- 오픈 주소법: 다른 비어있는 버킷에 데이터를 삽입하는 방법
  - 선형 탐색: 다음 버킷에 데이터를 삽입
  - 이차 탐색: 제곱수(1, 4, 9, 16, ...)를 더한 버킷에 데이터를 삽입
  - 이중 해시: 해시 함수를 하나 더 사용