# IPC
- Inter Process Communication
- 서로 다른 프로세스 간에 발생하는 통신
- 프로세스는 원래 독립적이지만 예외적으로 프로세스 끼리 통신이 필요한 경우가 있음

## 메시지 전달 방식 (Message Passing)
- 커널을 통해 메시지를 전달하는 방식으로 데이터를 주고 받음
- 커널이 관리하기 때문에 동기화 문제가 발생하지 않음
- 커널에 의존성을 갖게 되고 속도가 낮아짐
- 메시지 큐, 파이프, 소켓 등이 있음

## 공유 메모리 방식 (Shared Memory)
- 프로세스들이 주소 공간의 일부를 공유하는 방식
- 공유 메모리를 사용하는 시스템콜을 지원하여 서로 다른 프로세스들이 주소 공간의 일부를 공유할 수 있도록 함
- 여러 프로세스가 공유 메모리에 접근할 수 있음
- 동기화 문제가 발생할 수 있음