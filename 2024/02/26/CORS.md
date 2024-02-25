# CORS
`Cross-Origin Resource Sharing`, 요청 브라우저에서 다른 출처를 갖는 서버로 요청이 갈 때 `브라우저`에서 발생하는 보안 정책

> 출처 비교는 서버가 아닌 브라우저에 구현된 스펙임!

### 다른 출처란?
- 프로토콜 (http, https)
- 도메인 (www.aaa.com, www.api.aaa.com, www.bbb.com)
    - 서브도메인도 다른 도메인으로 간주
- 포트번호 (www.aaa.com:3000, www.aaa.com:8080)

# SOP
`Same-Origin Policy`, 출처가 동일한 프로토콜, 포트번호, 도메인에서만 자원을 사용 가능 하도록 하는 보안 정책

### SOP 정책이 없다면
https://aaa.com 이라는 사이트가 있고 이 사이트의 api 주소를 https://aaa.com/api 라고 가정하자. 사용자가 로그인을 한 후 인증 토큰을 받은 상태에서 http://bbb.com 이라는 사이트에 접속하면, 이 사이트에서 사용자의 인증 토큰을 가지고 https://aaa.com/api 를 요청할 수 있게 된다. 이렇게 되면 사용자의 정보가 노출될 수 있다.

## CORS 요청의 종류

### Simple Request
- GET, POST, HEAD 메소드 중 하나를 사용하는 요청
- Accept, Accept-Language, Content-Language, Content-Type, DPR, Downlink, Save-Data, Viewport-Width, Width 헤더일 경우
- Content-Type 헤더가 `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain` 중 하나일 경우

1. 클라이언트가 다른 출처로 요청을 보낼 때 브라우저가 요청 메시지에 Origin 헤더 (현재 사이트의 프로토콜 + 호스트 + 포트)를 추가
2. 서버는 `Access-Control-Allow-Origin` 헤더에 허용하는 Origin 을 명시 (전부 허용하는 경우 *)
3-1. 응답을 받은 브라우저는, `Access-Control-Allow-Origin` 값이 요청을 보낸 Origin 과 다르면 응답을 사용하지 않고 버리고 CORS 에러 발생
3-2. 응답을 받은 브라우저는, `Access-Control-Allow-Origin` 값이 요청을 보낸 Origin 과 같으면 응답 데이터를 클라이언트로 전달

### Preflight Request
- Simple Request 조건을 만족하지 않는 요청

1. 브라우저는 내부적으로 진짜 요청을 보내기 전에 `OPTIONS` 라는 메소드로 preflight 요청을 보내서 CORS 와 관련하여 서버에게 해당 요청을 보내도 되는지 물어봄
```
OPTIONS /resource
Access-Control-Request-Method: 요청에서 사용될 메서드
Origin: 요청을 보낸 출처
```

2. 서버는 어떤 Origin, Method 들이 허용되는지 응답
```
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: 허용되는 출처
Access-Control-Allow-Methods: 허용되는 메서드
Access-Control-Max-Age: prfilgt 요청을 캐싱할 시간(초)
```

3. 응답을 받은 브라우저는 응답 헤더 값이 요청을 보낸 Origin 과 다르면 진짜 요청을 보내지 않고 CORS 오류 발생

> CORS 해결책은 서버의 응답 헤더인 `Access-Control-Allow-Origin` 에 달렸다! 서버에서 클라이언트의 요청을 허용할지 말지 결정하는 것이다.

### Credential Request
클라이언트에서 서버에게 자격 인증 정보를 실어 요청할 경우
- 자격 인증 정뵈 쿠키, Authorization 헤더 등
- 클라이언트에서 `withCredentials` 옵션을 true 로 설정해야 인증 관련 헤더를 요청에 실어 보낼 수 있음
- 서버에서 `Access-Control-Allow-Credentials` 헤더를 true 로 설정해야 응답 데이터를 클라이언트로 전달할 수 있음

## CORS 해결책
1. 프록시 사용: 클라이언트에서 서버로 요청을 보내는 대신, 클라이언트에서 프록시 서버로 요청을 보내고 프록시 서버에서 서버로 요청을 보내는 방법
  서버 to 서버는 CORS 정책이 적용되지 않음

2. 서버에서 CORS 관련 응답 헤더를 설정

```java
@Configuration
public class WebConfig implements WebMvcConfigurer {
  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry.addMapping("/**")
            .allowedOrigins("http://aaa.com")
            .allowedMethods("GET", "POST")
            .allowCredentials(true)
            .maxAge(3000);
  }
}
```
