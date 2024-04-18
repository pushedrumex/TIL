# InnoDB Lock 실습

### 실습 내용
- InnoDB의 레코드 락, 갭락 실습
- 회원(member) 테이블을 생성하여 실습 진행
- 회원 테이블 컬럼
- 트랜잭션 격리 수준: `REPEATABLE_READ`

| 컬럼명       | Index          |
|-----------|----------------|
| member_id | PK, 클러스터링 인덱스  |
| nickname  | UNIQUE, 보조 인덱스 |
| age       | 보조 인덱스         |
| name      |                |

- 회원 데이터

| member_id | name | nickname  | age |
|-----------|------|-----------|-----|
| 1         | a    | nickname1 | 20  |
| 2         | a    | nickname2 | 23  |
| 3         | b    | nickname3 | 27  |
| 4         | b    | nickname4 | 20  |
| 5         | a    | nickname5 | 20  |
| 6         | b    | nickname6 | 30  |
| 7         | b    | nickname7 | 28  |
| 8         | b    | nickname8 | 28  |
| 9         | b    | nickname9 | 28  |
| 10        | b    | nickname10| 30  |

### 실습1. 레코드 락

#### PK 조건은 클러스터링 인덱스의 레코드를 잠근다.
- 쿼리
```
select * from member where member_id=1 for update ;
```
- lock

| ENGINE      | OBJECT_NAME | INDEX_NAME | LOCK_TYPE | LOCK_MODE | LOCK_DATA |
|-------------|--------|--------|--------|--------|--------|
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 1 |

#### Unique key 조건은 클러스터링 인덱스와 보조 인덱스의 레코드를 잠근다.
- 쿼리
```
select * from member where nickname='nickname1' for update ;
```
- lock

| ENGINE      | OBJECT_NAME | INDEX_NAME | LOCK_TYPE | LOCK_MODE | LOCK_DATA |
|-------------|--------|--------|--------|--------|--------|
| INNODB | member | nickname | RECORD | "X,REC_NOT_GAP" | "'nickname1', 1" |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 1 |

#### PK 와 Unique 가 아닌 인덱스 조건일 경우, 인덱스와 일치하는 레코드를 모두 잠근다.
- 쿼리
```
select * from member where age=28 for update ;
```
- lock

| ENGINE      | OBJECT_NAME | INDEX_NAME | LOCK_TYPE | LOCK_MODE | LOCK_DATA |
|-------------|--------|--------|--------|--------|--------|
| INNODB | member | idx_age | RECORD | X | "28, 7" |
| INNODB | member | idx_age | RECORD | X | "28, 8" |
| INNODB | member | idx_age | RECORD | X | "28, 9" |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 7 |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 8 |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 9 |
| INNODB | member | idx_age | RECORD | "X,GAP" | "30, 6" |

#### 인덱스 조건이 아닐 경우, 클러스터링 인덱스의 모든 레코드를 잠근다.
- 쿼리
```
select * from member where name='a' for update ;
```
- lock

| ENGINE      | OBJECT_NAME | INDEX_NAME | LOCK_TYPE | LOCK_MODE | LOCK_DATA |
|-------------|--------|--------|--------|--------|--------|
| INNODB | member | PRIMARY | RECORD | X | 1 |
| INNODB | member | PRIMARY | RECORD | X | 2 |
| INNODB | member | PRIMARY | RECORD | X | 3 |
| INNODB | member | PRIMARY | RECORD | X | 4 |
| INNODB | member | PRIMARY | RECORD | X | 5 |
| INNODB | member | PRIMARY | RECORD | X | 6 |
| INNODB | member | PRIMARY | RECORD | X | 7 |
| INNODB | member | PRIMARY | RECORD | X | 8 |
| INNODB | member | PRIMARY | RECORD | X | 9 |
| INNODB | member | PRIMARY | RECORD | X | 10 |

### 실습2. 갭 락, 넥스트 키 락

```
LockMode
X: 넥스트 키 락(레코드 락 + 갭 락)
X, REC_NOT_GAP: 레코드 락
X, GAP: 갭 락
```

#### 인접한 레코드 사이의 간격을 잠근다.

- 쿼리
```
select * from member where age between 20 and 25 for update ;
```

- lock

| ENGINE      | OBJECT_NAME | INDEX_NAME | LOCK_TYPE | LOCK_MODE | LOCK_DATA |
|-------------|--------|--------|--------|--------|--------|
| INNODB | member | idx_age | RECORD | X | "20, 1" |
| INNODB | member | idx_age | RECORD | X | "20, 4" |
| INNODB | member | idx_age | RECORD | X | "20, 5" |
| INNODB | member | idx_age | RECORD | X | "27, 3" |
| INNODB | member | idx_age | RECORD | X | "23, 2" |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 4 |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 5 |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 1 |
| INNODB | member | PRIMARY | RECORD | "X,REC_NOT_GAP" | 2 |

#### 결론
- innodb 엔진은 테이블의 레코드가 아닌 인덱스의 레코드를 잠근다.
- 인덱스 조건이 아닌 락의 경우 클러스터링 인덱스의 **모든** 레코드를 잠근다.
- innodb 는 갭 락으로 인해 `REPEATABLE_READ` 격리 수준에서도 `Phantom read` 가 발생하지 않는다.

#### [학습 정리](https://github.com/pushedrumex/TIL/blob/main/2024/02/14/Lock.md)