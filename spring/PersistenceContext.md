# 영속성 컨텍스트
- 엔티티를 영구 저장하는 환경
- 영속성 컨텍스트에 속한 엔티티들은 JPA 가 관리
- 엔티티 매니저가 생성되면 영속성 컨텍스트와 1:1 로 매핑
- 엔티티 매니저를 통해 영속성 컨텍스트에 접근
- 엔티티 매니저는 자신의 영속성 컨텍스트에만 접근 가능

## 엔티티의 생명 주기
비영속(New) -> 영속(Managed) -> 준영속(Detached) or 삭제(Removed)
- 비영속: 영속성 컨텍스트와 전혀 관계가 없는 상태
```java
Member member = new Member();
```

- 영속: 영속성 컨텍스트에서 관리되는 객체
```java
Member member = new Member();
member.setId("pushedrumex");

entityManager.persist(member);
```

- 준영속: 영속성 컨텍스트에 저장되었다가 분리된 상태
```java
entityManager.detach(member);
```

- 삭제: 실제 DB 에서 삭제를 요청한 상태
```java
entityManager.remove(member);
```

## 영속성 엔티티 특징
1. 1차 캐시
```java
entityManager.persist(member); //  영속성 컨텍스트의 1차 캐시에 저장됨
entityManager.find(Member.class, member.getId()) // 1차 캐시에서 조회
```
- 만약, 1차 캐시에 없을 경우 db에서 직접 조회하지 않고 1차 캐시에 저장 후 반환
- PK 가 아닌 필드로 find 할 경우, db에서 직접 조회 후 1차 캐시에 저장
- 영속성 컨텍스트가 삭제되거나 clear 될 경우 1차 캐시도 초기화됨.
- 애플리케이션 전체에서 공유하는 것이 아님. (애플리케이션 전체에서 공유하는 것은 2차 캐시)

2. 동일성 보장
```java
Member member = new Member();
memberRepository.save(member);

member == memberRepository.findById(member.getId()).get(); // true 
```
- 영속성 컨텍스트는 1차 캐시에 있는 같은 인스턴스를 반환

3. 쓰기 지연
```java
entityManager.persist(member1);
entityManager.persist(member2);
```
- persist 를 할 때, insert 쿼리를 바로 보내지 않고 1차 캐시에 저장하고 쓰기 지연 SQL 저장소에 쌓아둠
- 트랜잭션이 커밋 되거나 강제로 flush 할 경우 쓰기 지연 SQL 저장소에 쌓여있던 쿼리들을 db에 전송

4. 변경 감지(Dirty Checking)
```java
Member member = new Member();
memberRepository.save(member);

member.setName("change name");

entityManager.flush();
```
- JPA 는 영속성 컨텍스트에 엔티티의 최초 상태인 스냅샷을 저장
- flush 가 호출될 때 현재의 엔티티와 스냅샷을 비교
- 변경 사항이 있을 경우 update 쿼리를 쓰기 지연 SQL 저장소에 추가
- flush 가 되면 쓰기 지연 SQL 저장소의 쿼리들을 db 로 전송

## flush 란?
- 영속성 컨텍스트의 변경 내용을 데이터베이스에 반영
- 트랜잭션이 커밋되기 전에 자동으로 호출
- 영속성 컨텍스트 flush 방법
  1. em.flush() 직접 호출
  2. 트랜잭션 커밋
  3. JPQL 쿼리 실행