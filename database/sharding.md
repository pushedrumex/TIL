# Sharding
대규모 데이터베이스를 샤드라는 작은 단위로 분할하는 기술. 모든 샤드는 같은 스키마를 쓰지만 샤드에 보관하는 데이터 사이에는 중복이 없음

## 샤딩 키
데이터를 어떻게 분산할 지 정하는 하나 이상의 컬럼

## 샤딩 사용 시 고려해야할 사항
1. 데이터의 재샤딩
    데이터가 많아져서 하나의 샤드로 감당할 수 없게 되거나 데이터 분포가 균등하지 않다면 샤드 키를 계산하는 함수를 변경하고 데이터를 재배치 해야함. 안정해시를 통해 해당 문제를 최소화할 수 있음.
2. 유명 인사 문제
   특정 샤드에 쿼리가 집중되면 과부하가 걸릴 수 있음
3. 조인
    데이터가 분포되어있어 데이터를 여러 샤드에 조인하기 힘들어진다. 이 경우 데이터베이스를 비정규화하여 하나의 테이블에서 쿼리가 수행되도록 해야함

## 안정해시와 가상 노드
- 안정해시: 해시 테이블 크기가 조정될 때 재배치되는 데이터를 최소화하는 해시 기술
- 가상 노드: 실제 노드를 가리키는 가상 노드