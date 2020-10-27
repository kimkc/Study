인증(Authentication):   
인가(Authorization):   

## 인증
#### Stateful Server
- 쿠키(cookie)를 이용한 세션기반의 인증 = 세션상태유지(stateful)
- Scale-out에 좋지 않다.
- 도메인이 다른 서버에서 요청시 세션 쿠키가 공유되지 않음

**구성도 그리기**

#### Stateless Server
- Scale-Out 가능(Stateless)
- http header에 인증 토큰을 같이 보낸다.
- JWT

**구성도 그리기**

#### JWT

[인증](https://www.youtube.com/watch?v=y0xMXlOAfss)

[OAuth](https://www.youtube.com/watch?v=JZgD8aPkHSc)


## 인가
