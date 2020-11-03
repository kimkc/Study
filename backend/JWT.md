### JWT란
> JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.  
RFC(Request for comments): 인터넷(Internet)과 TCP/IP에 대한 제안, 접수된 일련의 조사결과, 측정, 아이디어, 기술, 관찰 뉴스, 표준 등의 종합체를 말한다. 인터넷국제표준화기구(IETF)는 일부 RFC를 인터넷 표준으로 받아들이기도 한다.

JSON 웹 토큰은 두 당사자간에 JSON object로 안전하게 정보를 제공하기 위해 간결하고 독립적인 방법으로 정의된 개방형 표준[RFC 7519](https://tools.ietf.org/html/rfc7519) 이다.   
정보는 **디지털 서명**이 되어 있으므로 **유효성을 검증**하고 신뢰할 수 있다.   
이때, HMAC알고리즘을 사용하는 단일 키와 공개/ 개인 키 쌍을 사용하는 RSA, ECDSA 알고리즘이 있다.


### JWT 사용 
> Authorization: This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token. Single Sign On is a feature that widely uses JWT nowadays, because of its small overhead and its ability to be easily used across different domains.

**권한**: JWT를 사용하여 유저가 로그인 후, JWT를 가지고 있다면 허용된 토큰과 함께 서비스, 경로, 자원에 접근할 수 있다. 싱글 사인온은 작은 오버헤드와 여러 도메인에서 쉽게 사용될 수 있기 때문에 오늘날 널리 사용되는 기능이다.

> Information Exchange: JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are. Additionally, as the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.

**정보 교환**: JWT는 두 당사자간 안전하게 정보를 교환하는데 좋은 방법이다. JWT에 담겨있는 헤더(header), 정보(payload)을 통해(키와 같이) 서명(signature)되어 콘텐츠가 변조되지 않았는지 확인할 수도 있다. 

### JWT 구조
- **Header** : 알고리즘, 토큰 유형 정보를 담는다.
- **Payload** : 클레임을 포함하는 페이로드이다. 클레임은 엔터티 및 추가 데이터에 대한 설명이다. 클레임에는

Header와 Payload는 **Base64Url로 인코딩**된다.
- **Signature**: header + payload + key를 통해 서명된다. 이는 유효성 검증 때, 같은 키(싱글 키, 공개/비공개 키)를 가지고 인코딩된 서명을 디코딩하여 유효성을 체크한다.   
xxxxx.yyyyy.zzzzz 최종 형태이다. 


출처: https://jwt.io/introduction/

### GO(gin)에서 만든 JWT를 JAVA(SPRING BOOT)에서 유효성 검사하기

#### 이슈
- encode(byte, string):
- 에러 확인(디버깅): 4가지 에러
