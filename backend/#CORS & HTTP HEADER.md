CORS 의 동작과정과 preflight, response header 설정과의 관계
-cors, preflight 과정(http헤더와 cors설정과 함께)
https://www.popit.kr/cors-preflight-%EC%9D%B8%EC%A6%9D-%EC%B2%98%EB%A6%AC-%EA%B4%80%EB%A0%A8-%EC%82%BD%EC%A7%88/
공식문서: https://developer.mozilla.org/ko/docs/Web/HTTP/CORS 
참고 https://codediver.tistory.com/135
-application/x-www-form-urlencoded 형태는 일반요청이라 괜찮지만, json은 일반요청이 아니기에 preflight를 날림(왜? 이 과정은 어떻게?)

http mesaage의 content-type
cors 시 아래와 같은 일반적인 요청만 가능 
application/x-www-form-urlencoded => r.send(형식을 해당 양식으로 바꾸니 됌)
multipart/form-data
text/plain

일반 요청이 아닌경우 preflight를 보냄
Response to preflight request doesn't pass access control check: It does not have HTTP ok status 하지만 아래 헤더를 추가안해주면 허락이 안됨
아래 추가시 된다. json이니까
response.setHeader("Access-Control-Allow-Headers","Origin, X-Requested-With, Content-Type, Accept");
헤더 밸류 값들 공부하기

-@ResponseBody와 @ModelAttribute의 차이와 어떤 것을 받는가
-response header 설정 중 각각의 의미와 headers에 새롭게 추가한 것 때문에 왜 되는지 설명

cors
WebMvcConfigurer or @CrossOrign 어노테이션과 속성값들
https://dev-pengun.tistory.com/entry/Spring-Boot-CORS-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0
@Configure 어노테이션과 WebMvcConfigureer 알아보기 => 설정시 어떤 헤더가 붙는가, vary로 붙는 헤더 2개는 무슨 의미이고 없으면 안되나?
=>cors처리가 gin에서 되어있는게 아니라 그냥 헤더만 붙여준거 같은데 이게무슨의미인가?
cors 다 주석처리하고 header만 붙이고 실행들 해보자 어떻게 달라지나.. 실제 응답 헤더들도 gin과 다르다
현재 로컬에서 해서 그런가.. gin 웹서버에서 실행해보고 cors 설정해주자
=>gin 실행 후 웹서버64432도 동작하면 cors 에러 때문에 로그인 페이지로 계속 넘어가는 에러(상태코드 문제?)가 생기는 듯하다
allow origin을 다르게 설정 시 왜 뒤와 같은 현상이 생기는가?=>gin 실행 후 웹서버64432도 동작하면 cors 에러 때문에 로그인 페이지로 계속 넘어가는 에러(상태코드 문제?)가 생기는 듯하다

추가적) 파일형태는?
**xmlrequest와 ajax는?
