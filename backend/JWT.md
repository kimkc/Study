### JWT란
> JSON Web Token (JWT) is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. This information can be verified and trusted because it is digitally signed. JWTs can be signed using a secret (with the HMAC algorithm) or a public/private key pair using RSA or ECDSA.  
>> RFC(Request for comments): 인터넷(Internet)과 TCP/IP에 대한 제안, 접수된 일련의 조사결과, 측정, 아이디어, 기술, 관찰 뉴스, 표준 등의 종합체를 말한다. 인터넷국제표준화기구(IETF)는 일부 RFC를 인터넷 표준으로 받아들이기도 한다.

JSON 웹 토큰은 두 당사자간에 JSON object로 안전하게 정보를 제공하기 위해 간결하고 독립적인 방법으로 정의된 개방형 표준[RFC 7519](https://tools.ietf.org/html/rfc7519) 이다.   
정보는 **디지털 서명**이 되어 있으므로 **유효성을 검증**하고 신뢰할 수 있다.   
이때, HMAC알고리즘을 사용하는 단일 키와 공개/ 개인 키 쌍을 사용하는 RSA, ECDSA 알고리즘이 있다.

---

### JWT 사용 
> Authorization: This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token. Single Sign On is a feature that widely uses JWT nowadays, because of its small overhead and its ability to be easily used across different domains.

**권한**: JWT를 사용하여 유저가 로그인 후, JWT를 가지고 있다면 허용된 토큰과 함께 서비스, 경로, 자원에 접근할 수 있다. 싱글 사인온은 작은 오버헤드와 여러 도메인에서 쉽게 사용될 수 있기 때문에 오늘날 널리 사용되는 기능이다.

> Information Exchange: JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are. Additionally, as the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.

**정보 교환**: JWT는 두 당사자간 안전하게 정보를 교환하는데 좋은 방법이다. JWT에 담겨있는 **헤더(header)**, **정보(payload)**을 통해(키와 같이) **서명(signature)**되어 **콘텐츠가 변조되지 않았는지 확인**할 수도 있다.   

---

### JWT 구조
- **Header** : 알고리즘, 토큰 유형 정보를 담는다.   

- **Payload** : 클레임을 포함하는 페이로드이다. 클레임은 엔터티 및 추가 데이터에 대한 설명이다. 클레임에는 registered, public, private **3가지 종류의 클레임**이 있다. 자세한 설명은 참고문헌을 참고하길 바란다.
**서명된 토큰도 변조로부터 보호되지만 누구나 읽을 수 있다. 암호화 되지 않은 경우 jwt의 페이로드 또는 헤더 요소에 비밀 정보를 넣으면 안된다!**   
Header와 Payload는 **Base64Url로 인코딩**된다.   

- **Signature**: 인코딩된 header, 인코딩된 payload, key와 헤더에 지정된 알고리즘을 가져와 서명한다. HMAC SHA256 알고리즘을 사용한 경우, 아래와 같은 형태이다.   
> HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret(키))     

= header인코딩.payload인코딩.서명   
header와 payload는 디코딩하면 누구나 읽을 수 있다. header, payload를 key와 사용된 암호 알고리즘을 통해 해독하여 얻은 header와 payload와 기존 header인코딩.payload인코딩과 비교하여 **변조 여부 확인**을 한다. 또한 **같은 키**을 통해 해독하여 **유효성 검증**을 할 수 있다. 

출처: https://jwt.io/introduction/   
해당 사이트에서 디버거를 통해 인코딩된 값을 디코딩하여 디버깅을 할 수 있다.

---

### GO(gin)에서 만든 JWT를 JAVA(SPRING BOOT)에서 유효성 검사하기

클라이언트에서 로그인 시 gin서버에서 계정 확인 후 JWT 보내준다.
또한 여러 요청시 JWT 검증이 필요한 요청은 유효성 검증 후 응답을 해준다.


#### 이슈
원인
- JWT에 대한 낮은 이해도
- 예외 확인(디버깅) 미흡
- encode(byte, string)   
   
- JWT에 대한 구조, 동작 과정에 대한 이해 없이 GIN에 있는 코드(JWT-GO)를 SPRING BOOT(JJWT)에서 구현하였다. **코드 작성 전에 해당 라이브러리에 대해 공식문서, 구조, 동작 과정(코드), 사용 이유 등 기초적이고 구조적**으로 이해 하지 못해 방향을 잃고 더 많은 시간을 보냈다. JWT에 대해 알아가며 미리 분석했다면 일어나지 않을 시도(키의 위치, 키 타입, 인코딩, 암호화)를 하고 있음을 느꼈다.   
   
- **예외를 통해 어떠한 문제가 있는지 확인**할 수 있다. 하지만 예외 사항에 대한 이해가 부족하여 문제 파악을 제대로 하지 못했다. SignatureException 시 "jwt tempered"라는 출력을, ExpiredJwtException 시 "jwt expired" 출력을 하였다. 하지만 예외 종류에 대한 이해가 부족하여 jwt 뒤 출력문의 차이를 이해하려기 보다 "jwt" 만보고 추상적으로 문제가 있다는 생각만하여 해결을 위해 다른 시도를 하였다.
- ClaimJwtException: JWT 권한claim 검사가 실패했을 때
- ExpiredJwtException: 유효 기간이 지난 JWT를 수신한 경우
- MalformedJwtException: 구조적인 문제가 있는 JWT인 경우
- PrematureJwtException: 접근이 허용되기 전인 JWT가 수신된 경우
- SignatureException: 시그너처 연산이 실패하였거나, JWT의 시그너처 검증이 실패한 경우
- UnsupportedJwtException: 수신한 JWT의 형식이 애플리케이션에서 원하는 형식과 맞지 않는 경우
   
   
- expired 예외를 던져도 signature예외라 생각하였다. 키를 사용하는데 생성할 때와 검증 할 때, GIN에서는 BYTE[] 형태를 사용하였지만, String으로 해보고, java의 secretkey 객체, 여러 인코딩 방법으로 key값 인코딩 등 키 관련 시도만 하였다. 유효성 검사 후 토큰 만료 에러를 던져도 키 관련 시도만 하였다.
   
   
   
###### 결론   
위와 같은 과정들을 통해 JWT 공식 문서를 보고, JJWT, JWT-GO, JWT 디버깅 통해 토큰 생성, 유효성 검증 동작을 확인하며 JWT가 던지는 예외를 통해 문제 파악을 제대로 할 수 있었다. 이해를 바탕으로 SPRING BOOT에서 JWT 생성해보고 GIN과 같은 JWT 값을 가지도록 만들고, 이때, 사용된 키(타입)를 사용하니 제대로 유효성 검증이 되었다. 그 후 GIN에서 만든 만료되기 전 토큰 값을 가지고 한 번 더 SPRING BOOT에서 확인한 결과 문제가 해결되었다.   
결국, 키의 STRING -> BYTE 문제였는데, 초반에 제대로 검증 후 EXPIRED 예외를 보지 못하고 키에 대한 시도들만 하며 SIGNATURE 예외를 보며 방향 잃은게 제일 큰 문제였다.
