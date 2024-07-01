# HTTPS
HTTP에서 보안이 강화된 프로토콜로, 누구나 볼 수 있었던 메시지를 통신 당사자들 끼리만 볼 수 있도록 암호화

## 암호화 기법

### 대칭키 기법
- 하나의 키로 암호화와 복호화를 둘 다 할 수 있는 기법
- 서버와 클라이언트가 같은 대칭키를 갖고, 데이터를 보낼 때 대칭키를 사용하여 함호화하여 전송 -> 데이터가 왔을 때 대칭키로 복호화
- 단점 : 키를 안전하게 서로 교환하기 어려움, 탈취의 위험이 존재

### 공개키 기법
- 서로 다른 두 개의 키(공개키, 개인키)로 암호화/복호화
- 공개키 : 누구나 가질 수 있는 키 / 개인키 : 소유자 한 명만 가질 수 있는 키
- 공개키로 암호화한 데이터는 개인키로만 복호화, 개인키로 암호화한 데이터는 공개키로만 복호화
- 공개키로 암호화한 데이터를 보내면 개인키로만 복호화할 수 있어 대칭키의 단점을 해결
- 단점 : 대칭키 기법에 비해 암호화 과정이 복잡하여 속도가 느림

# SSL(Secure Socket Layer)
클라이언트와 서버가 서로 데이터를 암호화해서 통신할 수 있도록 돕는 보안 계층
> 대칭키 기법과 공개키 기법을 함께 사용하여 서로의 단점을 보완

## 동작 과정
1. 클라이언트가 지원 가능한 암호화 방식 목록을 서버에게 전달
2. 서버는 암호화 방식 목록 중 선택한 방식과 ssl 인증서를 클라이언트에게 전달
   - ssl 인증서 : 공식적으로 인증된 기관인 CA에서 발급한 문서로, 서버가 신뢰할 수 있는 지 보장하는 역할
     - CA의 개인키로 암호화 되어있음
     - 서버의 공개키가 들어있음
     - 서버가 CA로부터 미리 발급 받아놓은 상태
3. 클라이언트는 CA에서 공유하는 공개키로 ssl 인증서를 복호화
4. 클라이언트는 대칭키를 만들어 ssl 인증서에 들어있는 서버의 공개키로 암호화하여 서버에게 전달
5. 서버는 자신의 개인키로 복호화하여 대칭키 획득
6. 클라이언트와 서버는 이 대칭키로 데이터를 암호화/복호화하며 통신