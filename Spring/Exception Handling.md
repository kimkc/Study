# 예외 처리

### 들어가기 전
발생할 예외를 미리 조사하거나 만든 후, try, catch로 예외를 바로 처리하거나 throws, throw를 통해 예외를 처리할 줄 알아야한다.   
**try, catch**: 해당 함수 안에서 일어난 에러를 바로 잡아서 처리   
**throws**: 해당 함수를 호출한 함수에게 예외를 던짐 = 책임전가   
**throw**: 강제로 예외를 발생시키는 경우(프로그래머의 판단에 따른 처리)   

## @ControllerAdvice
전역 예외 처리  
어플리케이션의 예외처리를 맡음
@@ExceptionHandler와 같이 사용된다.
@ResponseStatus: http status를 리턴, ResponseEntity를 사용하지 않는 경우 상태코드를 보내기 위해서 사용?

``` java
@ControllerAdvice  
@RestController  
public class GlobalExceptionHandler {  
  
    @ResponseStatus(HttpStatus.BAD_REQUEST)  
    @ExceptionHandler(value = BaseException.class)  
    public String handleBaseException(BaseException e){  
        return e.getMessage();  
    }  
  
    @ExceptionHandler(value = Exception.class)  
    public String handleException(Exception e){return e.getMessage();}  
  
  
}  

출처: https://springboot.tistory.com/33 [스프링부트는 사랑입니다]
```

> 만일 BaseException 또는 그의 자식 예외가 발생하면 handleException() 메소드가 아닌 handleBaseException()이 이 예외를 잡을 것이다.   

## @ExceptionHandler 
Controller(해당 컨트롤러 클래스에서만) 예외 처리   
해당 컨트롤러에서 @ExceptionHandler를 붙인 메서드에서 해당 컨트롤러에서 발생하는 예외를 처리해준다.   
위와 같이 @ControllerAdvice를 사용하는 클래스가 아닌 Controller에서 @ExceptionHander, 예외(다중 사용 가능), 해당 메서드를 만들어 사용할 수 있다.    

## try-catch
메서드 단위: 지양(코드 길이 증가, 가독성 감소)   
직접 try-catch로 처리시 우선순위가 더 높다.   
**예외 처리에 대한 좋은 예 추가하기**

## redteam 예제(Spring boot)
공부 이유: Restful API의 smtp 인증 실패, 토큰 만료 등에 대해 예외가 발생하면 팀과 정해 놓은 http status를 응답해야함.
행동: 이에 Service, DAO 등에서 발생하는 예외를 Controller로 throws   
Controller에서 try,catch로 특정 예외를 catch하여 ResponseEntity<Object>(내용, HttpStatus.상태코드)를 응답해주었다.   
**try, catch로 가독성이 떨어지는 코드 @ExceptionHander나 @ControllerAdvice로 리팩터링해보기**
<br />

### 참고자료
- 본문 코드예시   
https://springboot.tistory.com/33

- 다른 @ControllerAdvice, @ExceptionHandler, ResponseEntity 예   
https://velog.io/@aidenshin/Spring-Boot-Exception-Controller

- try-catch 지양, Exception전략   
https://cheese10yun.github.io/spring-guide-exception/#error-response-json-1
