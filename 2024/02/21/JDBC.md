# JBDC

## 애플리케이션 서버와 DB
- 주로 TCP/IP 를 사용하여 커넥션을 연결
- 애플리케이션 서버는 DB 가 이해할 수 있는 SQL 문을 커넥션을 통해 DB 에 전달
- DB 는 전달된 SQL 을 수행하고 결과를 응답
- 데이터베이스는 여러 종류가 있고 각각 다른 방식으로 커넥션을 연결하고 DB 와 통신

## 데이터베이스의 커넥션 연결 및 통신 방식의 다양성이 갖는 문제점
- 데이터베이스를 변경할 경우 애플리케이션 서버의 데이터베이스 사용 코드도 변경해야함
- 개발자가 각각의 데이터베이스마다 커넥션 연결, SQL 전달, 결과를 응답 받는 방법을 새로 학습 해야함

## 해결 방법: JDBC 자바 표준 인터페이스
- Java Database Connectivity
- 자바에서 데이터베이스에 접속할 수 있도록 하는 자바 API
- Connection(연결), Statement(SQL 을 담은 내용), ResultSet(SQL 요청 응답) 인터페이스를 정의해서 제공
- 각 데이터베이스 벤더는 JDBC 인터페이스를 구현한 드라이버를 제공 ex) Oracle 드라이버, MySQL 드라이버 등
- 개발자는 표준 인터페이스만 사용해서 개발하면 됨

## SQL Mapper
- JdbcTemplate, MyBatis
- SQL 응답 결과를 객체로 편리하게 변환
- 개발자가 SQL 를 직접 작성

## ORM
- 하이버네이트, 이클립스
- JPA 는 자바 진영의 ORM 표준 인터페이스고 하이버네이트와 이클립스 링크가 이를 구현
- 객체를 관계현 데이터베이스 테이블과 매핑
- 개발자는 반복적인 SQL 을 작성하지 않아도 됨

```
SQL Mapper 나 ORM 도 내부적으로는 모두 JDBC 를 사용
```

## DriverManager
- 라이브러리에 등록된 DB 드라이버들을 관리하고 커넥션을 획득하는 기능을 제공
- 라이브러리에 등록된 드라이버 목록을 자동으로 인식