## MySQL 격리 수준

- 여러 트랜잭션이 동시에 처리될 때 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있게 허용할지 말지를 결정하는 것

### READ UNCOMMITTED

- 각 트랜잭션의 변경 내용이 COMMIT 이나 ROLLBACK 여부에 상관 없이 다른 트랜잭션 에서도 보임
- 더티리드: 어떤 트랜잭션에 처리한 작업이 커밋되지 않았는데 다른 트랜잭션에서 볼 수 있는 현상

### READ COMMITTED

- COMMIT 이 완료된 데이터만 다른 트랜잭션에서 조회 가능
- COMMIT 이 완료되지 않았을 경우 다른 트랜잭션에서는 언두 로그의 데이터 조회
- 더티리드가 발생하지 않음
- REPEATABLE READ(항상 같은 결과를 가져와야함) 불가능

### REPEATABLE READ

- InnoDB 스토리지 엔진의 디폴트 격리 수준
- 트랜잭션은 해당 트랜잭션 시작 전에 커밋한 내용만 조회 가능
    - 중간에 다른 트랜잭션이 커밋을 했다면 언두 영역 조회
- 언두 영역에 백업된 이전 데이터를 이용해 동일 트랜잭션 내에서는 동일한 결과를 보여줄 수 있도록 보장
- 팬텀리드: 다른 트랜잭션에 수행한 변경 작업에 의해 레코드가 보였다가 안보였다 하는 현상
- InnoDB 에서 REPEATABLE READ 수준에서는 갭락과 넥스트키락 덕분에 팬텀리드가 발생하지 않음
    - 언두 레코드에는 잠금을 걸 수 없어서 현재 레코드의 값을 가져옴
    - 하지만 예외적으로 non-locking 조회 후, locking 조회를 하는 경우 팬텀 리드 발생

### SERIALIZABLE

- non-locking 조회도 공유 잠금을 획득해야 가능
- 한 트랜잭션에서 읽고 쓰는 레코드를 다른 트랜잭션에서 절대 접근할 수 없음