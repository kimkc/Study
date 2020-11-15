# 자바(spring boot) smtp 설정
후에 실제 코드와 합쳐진다면 spring 폴더로 옮기기

**1. JavaMail 라이브러리**

https://ktko.tistory.com/m/entry/JAVA-SMTP%EC%99%80-Mail-%EB%B0%9C%EC%86%A1%ED%95%98%EA%B8%B0Google-Naver

- JavaMail 라이브러리 설정 및 import
- 구글 또는 네이버 SMTP 환경 설정
- 개발   
    1-1. 발신자 메일 계정과 비밀번호 설정    
    1-2. **Property**에 **SMTP 서버 정보** 설정(호스트, 포트, TLS 등)   
    1-3. SMTP 서버 정보와 사용자 정보를 기반으로 **Session 클래스**의 인스턴스 생성   
    1-4. **Message 클래스**의 객체를 사용하여 **수신자**와 내용, 제목의 **메시지**를 작성   
    1-5. **Transport 클래스**를 사용하여 작성한 메시지 **전달**   
 
InternetAddress\[\] 배열을 message.addRecipients(메시지, 수신자)를 통해 **다중 메일 전송 가능**

**사용하는 메일의 환경설정에서 smtp허용하거나 smtp 관련 결정이 정해진 경우는 어떻게?**   
<br/><br/>

**2. JavaMailSender**   
**MailSender(찾아보기)** 인터페이스 상속 받은 JavaMailSender   
1. spring-boot-starter-mail 의존성을 추가   
2. SMTP를 통해 메일을 보내기 위해 사용하는 SMTP Sever(GOOGLE, NAVER 등)의 요구사항에 맞는 설정
3. application.property에서 사용자 내용 설정함(우리는 DB에서 불러와 설정)
4. Message 객체를 통해 수신자, 제목, 메세지 설정 후 전송

MimeMessageHelper를 통해 이미지, 첨부파일 등을 포함한 메일 발송 가능   
HTML element가 많다면 MailTemplateServiec 사용하면 편리하다.

#### 예상 구현
1. 1번을 사용   
2. postgreSQL의 SMTP Table에서 가져와서 사용자 계정과 비밀번호, SMTP 서버 정보 설정
3. 연결 테스트는 Session.getInstance(정보, 사용자) 세션을 얻고,   
    session.getTransport("smtp")로 Transport 객체를 얻어 tr.connect()로 연결 테스트   
    **예외 확인 후 예외 핸들링**   


나중에 프로젝트에서 smtp 설정 후 메세지 보낼 시   
DB의 Template Table의 내용을 가져와   
Message클래스의 MimeMessage를 통해 첨부파일이나   
MailTemplateService를 통해 HTML   
메세지 설정 후 전송   
=> front 코드 확인 후 수정
<br/><br/>

### 이슈
javaMailSender를 생성 안하고, property 설정 후 session 생성 후 transport객체를 이용하여 연결   
**이슈**: 연결이 어떨 땐 되고, 연결 후 아이디, 비밀번호를 바꾸고, host를 변경해도 연결이 됨.
   연결이 안되면 연결 될 때와 같은 설정을 해도 연결이 되지 않음.
   
``` java
        String host = smtp_info.getSmtpHost(); 
        String user = smtp_info.getSmtpId()+"@naver.com";
        String password = smtp_info.getSmtpPw();
        System.out.println(host+"  "+user+"   "+ password+"  "+smtp_info.getSmtpPort()+"  "+smtp_info.getSmtpTls()+"  "+smtp_info.getSmtpPort());
        
        // SMTP 서버 정보를 설정
        Properties props = new Properties();
        props.put("mail.smtp.user", user);
        props.put("mail.smtp.host", host);
        props.put("mail.smtp.password", password);
        props.put("mail.smtp.port", smtp_info.getSmtpPort());
        props.put("mail.smtp.startttls.enable", smtp_info.getSmtpTls().equals("1")?"true":"false");//smtp_info.getSmtpTls().equals("1")?"true":"false"
        props.put("mail.smtp.auth", "true");
        props.put("mail.smtp.socketFactory.port",smtp_info.getSmtpPort());
        props.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
        props.put("mail.smtp.socketFactory.fallback", "false");

        Session session = Session.getDefaultInstance(props, new javax.mail.Authenticator() {
            protected PasswordAuthentication getPasswordAuthentication() {
                return new PasswordAuthentication(user, password);
            }
        });

        Transport tr = session.getTransport("smtp");
        tr.connect();
        tr.close();
```

javaMailSender를 사용하여 해당 객체에 property를 설정하니 호스트, 아이디, 비밀번호 등을 제대로 하지 않으면 정확히 연결이 되고 안되고가 예외와 같이 던져짐   
**가정**: javaMailSender와 관련된 javaMailSenderImpl, MessageType?, Service, Session, Transport 등 javaMailSender가 다른 **객체, 인터페이스들과 안에서 어떻게 동작하는지 살펴보기**
   
   
**진행중 코드 (제대로 되는 보인다. 왜그런지 작성하기)**

``` java
JavaMailSenderImpl mailSender = new JavaMailSenderImpl();
        mailSender.setHost(smtp_info.getSmtpHost());
        mailSender.setPort(Integer.parseInt(smtp_info.getSmtpPort()));

        mailSender.setUsername(smtp_info.getSmtpId());
        mailSender.setPassword(smtp_info.getSmtpPw());

        Properties props = mailSender.getJavaMailProperties();
        props.put("mail.smtp.startttls.enable", smtp_info.getSmtpTls().equals("1")?"true":"false");//smtp_info.getSmtpTls().equals("1")?"true":"false"
        props.put("mail.smtp.auth", "true");
        props.put("mail.transport.protocol", "smtp");
        props.put("mail.smtp.socketFactory.port",smtp_info.getSmtpPort());
        props.put("mail.smtp.socketFactory.class", "javax.net.ssl.SSLSocketFactory");
        props.put("mail.smtp.socketFactory.fallback", "false");

        mailSender.testConnection();
```

---
<br/><br/>
### 참고 자료
- **java smtp properties**   
https://javaee.github.io/javamail/docs/api/com/sun/mail/smtp/package-summary.html   
   
- **java, spring boot javaMailSender**      
https://www.baeldung.com/spring-email   
   
- 자바 smtp와  메일전송   
https://ktko.tistory.com/m/entry/JAVA-SMTP%EC%99%80-Mail-%EB%B0%9C%EC%86%A1%ED%95%98%EA%B8%B0Google-Naver   
   
- 스프링부트메일전송 sendermail   
https://victorydntmd.tistory.com/m/342   
   
- Java transport를  통한  연결확인   
https://stackoverflow.com/questions/3060837/validate-smtp-server-credentials-using-java-without-actually-sending-mail   
다른 테스트, 예외 확인하고 처리하기   
https://github.com/bbottema/simple-java-mail/issues/116   
