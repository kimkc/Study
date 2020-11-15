# SMTP

## SMTP
SMTP는 Simple mail Transfer protocol의 약자이다.   
이메일 전송에 직접적으로 쓰이는 응용 계층의 프로토콜이다.   
전송계층 프로토콜로는 TCP를 통해 전달합니다.
<br/><br/>

#### 예시 - 편지와 우체국(우체통)
편지를 보낼 때(SMTP) 우체국이 관리하는 우편통이나 우체국에 직접가서 쓴다.   
편지를 받을 때는 집 앞 우편함에 도착해있는 편지를 확인하면 된다.(POP, IMAP, HTTP)      
<br/><br/>

#### 동작 과정
<p align="center"><img src="./img/mailProcess.png" height="400px" width="400px" /></p>

> 내 메일 : abc@naver.com   
상대방 메일 : def@daum.net<br/><br/>
내가 상대방에게 메일을 보낸다고 가정해봅시다.   
naver 메일 서버와 daum 메일 서버가 각각 존재합니다.   
저는 naver 의 메일서버에 보낼데이터를 전송합니다. (SMTP)<br/><br/>
naver 메일 서버는 daum 메일 서버에게 메일을 보냅니다. (SMTP)   
그러면 그 메일 정보는 daum 메일 서버 보관함에 저장되고,   
상대방은 daum 메일 서버 보관함에서 메일을 가져오게 됩니다. (POP, IMAP, HTTP)<br/><br/>
>> **왜 메일을 가져올 때는 SMTP를 이용하지 않을까?**   
SMTP는 심플하기 때문에  Mail Box (Po box) 까지만 갈 수 있습니다.   
수신자는 태블릿, 컴퓨터, 휴대폰 등등에서 모두 메일을 확인할 수 있기 때문에,     
SMTP는  메일 서버 보관함까지만 작용하고, 수신자는 위 3가지 옵션을 통해 직접적으로 메일을 가져오게 됩니다.

출처: https://programming119.tistory.com/152
<br/><br/>

#### SMTP Format
<p align="center"> <img src="./img/SMTPFormat.png" height="400px" width="500px" /> </p>

메일 구조가 복잡해지면서, 아래의 문제를 해결 발생   
**1. 각 데이터들을 분리할 수 있어야 한다.**     
=> 각 데이터 사이를 boundary 문자열을 이용하여 분리   
**2. 각 데이터의 타입을 알아야 한다.**    
=> 데이터가 plain/text인지 binary 데이터인지 알아야한다. 그래서 smtp 각 데이터의 앞부분에 부가정보(애플리케이션 타입 정보 포함)를 담은 header를 두어 해결.    
**3. Base64 encode: SMTP는 printable한 텍스트 데이터를 처리하는 것을 목적으로 만들었다.**    
=> 바이너리 데이터를 텍스트로 변환할 필요가 생길 때, Base64( = 알파벳 대소문자 52 + 숫자10 + "+"기호 + "/"기호) encoding 기법을 사용한다. 데이터를 받으면 Base64 decode 과정을 통해 원래 데이터로 복원한다.   
    
<br/>

#### 위와 같은 문제 해결을 통한 구조( 헤더 1개 + 각 헤더, 바디 같이 3개)
<p align="center"> <img src="./img/SMTPFormatExample.png" height="400px" width="300px" /></p>

출처: https://www.joinc.co.kr/w/Site/Network_Programing/Documents/SMTP
---
<br/><br/>

### 참고 자료
- 메일 전송의 원리(POP3, IMAP 설명 포함)   
https://unabated.tistory.com/m/entry/mail-%EC%A0%84%EC%86%A1%EC%9D%98-%EC%9B%90%EB%A6%AC

- smtp 중점의 메일 전송 과정   
https://programming119.tistory.com/152
   
- **구체적인 smtp 설명(통신 예제 포함)**   
https://www.joinc.co.kr/w/Site/Network_Programing/Documents/SMTP   
   
- 텔넷을 통한 smtp 연결테스트   
https://hope.pe.kr/50   

