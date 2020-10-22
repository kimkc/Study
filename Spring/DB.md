## JDBC 개요

## JDBC(Java Database Connectivity)의 정의
- 자바를 이용한 데이터베이스 접속과 SQL 문장의 실행, 그리고 실행 결과로 얻어진 데이터의 핸들링을 제공하는 방법과 절차에 관한 규약
- 자바 프로그램 내에서 SQL문을 실행하기 위한 자바 API
- SQL과 프로그래밍 언어의 통합 접근 중 한 형태
- JAVA는 표준 인터페이스인 JDBC API를 제공
- 데이터베이스 벤더, 또는 기타 써드파티에서는 JDBC 인터페이스를 구현한 드라이버(driver)를 제공한다.

Q) java.sql패키지를 보면 대부분이 interface로 되어 있는 것을 알 수 있습니다.
이를 실제로 구현하는 것은 DBMS를 만든 회사입니다.
java.sql외에 JAVA가 인터페이스만 대부분 제공하는 패키지는 또 어떤 것이 있을까요?

## JDBC 클래스의 생성 단계
*해야할 일* DataSource와 각 jdbc 클래스와 관계
https://www.edwith.org/boostcourse-web-be/lecture/58939/#

## 커넥션풀과 DataSource
*해야할 일* Hicari CP 목적, 동작과정, 이와 같은 기술들,


## MyBatis vs JPA
*해야할 일* +JDBC와 차이, jdbc클래스와 연관시켜 설명하고 차이, 표형태로 요약


### 참고자료
- [[부스트코스] 웹 백엔드](https://www.edwith.org/boostcourse-web-be/lecture/58939/)
- 서적 코드로 배우는 스프링 웹 프로젝트 개정판
