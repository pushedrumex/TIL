# Lock 실습

### 실습 내용
- 공유 락과 배타 락 개념 확인
- 회원(member) 테이블을 생성하여 실습 진행

### 실습1. 공유 락과 배타 락

#### 여러 트랜잭션에서 공유락을 동시에 획득할 수 있다.
1. 1번 트랜잭션의 공유락 획득
```
select * from member where member_id=1 for share ;
```
2. 2번 트랜잭션에서 공유락 획득 성공
```
select * from member where member_id=1 for share ;
```

#### 여러 트랜잭션에서 배타락을 동시에 획득할 수 없다.
1. 1번 트랜잭션에서 배타락 획득
```
select * from member where member_id=1 for update ;
```
2. 2번 트랜잭션에서 배타락 획득 실패
```
select * from member where member_id=1 for update ;
```
#### 한 트랜잭션에서 공유락과 배타락을 동시에 획득할 수 있다.
1. 1번 트랜잭션에서 공유락 획득
```
select * from member where member_id=1 for share ;
```
2. 1번 트랜잭션에서 배타락 획득 성공
```
select * from member where member_id=1 for update ;
```
<details>
<summary>그럼 배타락이 먼저 걸린다면?</summary>
배타락이 걸린 상태에서 동일한 트랜잭션에서 select for share 를 한다면 공유락을 따로 획득하지 않지만 락 조회는 가능
</details>

#### 여러 트랜잭션에서 공유락과 배타락을 동시에 획득할 수 없다.
1. 1번 트랜잭션에서 공유락 획득
```
select * from member where member_id=1 for share ;
```
2. 2번 트랜잭션에서 배타락 획득 실패
```
select * from member where member_id=1 for update ;
```

#### 여러 트랜잭션에서 공유락을 획득한 상태에서 배타락 획득을 시도할 경우 데드락이 발생한다.
1. 1번 트랜잭션에서 공유락 획득
```
select * from member where member_id=1 for share ;
```
2. 2번 트랜잭션에서 공유락 획득
```
select * from member where member_id=1 for share ;
```
3. 1번 트랜잭션에서 배타락 획득 시도
```
select * from member where member_id=1 for update ;
```
4. 2번 트랜잭션에서 배타락 획득 시도
```
select * from member where member_id=1 for update ;
```
5. 데드락 발생

`
[40001][1213] Deadlock found when trying to get lock; try restarting transaction
`