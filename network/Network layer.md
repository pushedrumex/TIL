# Network Layer

## OSI 7계층
7. 응용 계층
- 어플리케이션 끼리의 데이터 송수신을 담당
- 데이터 단위 : 데이터 / 메시지
- HTTP, DNS

6. 표현 계층
- 데이터의 형식을 정의
- 데이터의 압축과 변환
- JPEG, MPEG

5. 세션 계층
- 데이터를 통신하기 위한 논리적인 연결을 담당
- socket

4. 전송 계층
- 데이터 전송과 흐름의 신뢰성을 보장하는 계층
- 데이터 단위 : 세그먼트
- 헤더 : 송수신 측의 포트 번호, 시퀀스 번호, ACK 번호, 컨트롤 비트
  - 송신측에서는 ACK번호, 수신측에서는 시퀀스 번호를 통해 데이터 누락을 체크
    - 시퀀스 번호 : 현재 송신되는 데이터 조각이 몇 번째 바이트에 해당하는 지
    - ACK 번호 : 수신측에서 몇 번째 바이트까지 수신했는 지
- TCP, UDP

3. 네트워크 계층
- 네트워크 상에서 데이터를 전송하는 계층
- 데이터 단위 : 패킷
- 헤더 : 송수신 측의 IP 주소
- IP, ARP(브로드캐스트를 통해 IP 주소에 해당하는 MAC 주소를 얻는 방법)

2. 데이터 링크 계층
- 데이터 단위 : 프레임
- 헤더 : MAC 주소
- MAC, LAN

1. 물리 계층
- 데이터를 전기적 신호인 0과 1로 변환하여 전달
- 케이블, 리피터, 허브

## TCP/IP
OSI 7계층을 단순화한 실용적인 모델

4. 응용 계층 (응용 계층 + 표현 계층 + 세션 계층)
3. 전송 계층 (전송 계층)
2. 인터넷 계층 (네트워크 계층)
1. 네트워크 접근 계층 (데이터 링크 계층 + 물리 계층)