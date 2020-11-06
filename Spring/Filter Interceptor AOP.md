## Filter, Interceptor, AOP 

스프링 부트로 프로젝트를 진행 중 HTTP header, Token 유효성 검사 등 controller마다 중복코드가 생겼다.   
주로 웹 개발 시, 인증 처리, XSS로부터 보안 처리, 로깅 처리, 페이지 인코딩 등이 공통되는 부분이다.      
코드가 중복되면 소스 여러 군데 사용되며 관리가 힘들어지고, 늘어나는 소스양은 프로젝트 단위를 키우고 서버에 대한 부하, 유지 보수에 어려움을 겪게 된다.   
이러한 **공통 부분에 중복을 제거하고 한 곳에서 관리**하는 방법을 고민하였다.   
그 결과, 위 3가지 방법이 나왔다.   
<br/>

스프링에서 Filter, Interceptor, AOP 는 모두 무슨 행동을 하기전에 먼저 실행하거나, 실행한 후에 추가적인 행동을 할 때 사용되는 기능들이다.   


#### 참고자료
- Filter, Interceptor, AOP 차이 및 개념
https://goddaehee.tistory.com/154   
   
- Filter, Interceptor, AOP 차이   
https://velog.io/@sa833591/Spring-Filter-Interceptor-AOP-%EC%B0%A8%EC%9D%B4-yvmv4k96   
   
- Filter, Interceptor, AOP 코드와 설명   
https://offbyone.tistory.com/32   
