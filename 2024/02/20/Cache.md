# Local Cache vs Global Cache

## Local Cache
- 개별 서버에 존재
- 다른 서버의 캐시를 참조하기 어려움
- 서버 내에서 동작하기 때문에 빠른 속도
- 로컬 서버의 리소스(Memory, Disk) 사용
- 분산 서버 환경일 경우, 캐시에 있는 데이터가 변경될 때 해당 서버를 제외한 다른 서버에게 변경 사항 전달 필요

## Global Cache
- 여러 서버에서 분리된 캐시에 접근
- 서버 간 데이터 공유 가능
- 네트워크를 사용하기 때문에 Local Cache 보다 느릴 수 있음
- Replication, Sharding 등을 통한 캐시 서버 확장 가능
