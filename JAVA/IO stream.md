## IO Stream 
- Stream은 연속적인 데이타 흐름을 나타냅니다. 
- 프로그램이 가지고 있는 또는 가지고 있지 않은 데이터를 외부(다른 프로그램)로 보내거나 가져오는 동작을 말합니다. 
- Program에서 처리된 데이터    -----출력 스트림------>  모니터, 디스크 장치,네트워크(Destination)     
- 키보드, 디스크 장치, 네트워크 ----입력 스트림------> Program(Destination)  
- 스트림은 단방향 구조입니다. 따라서 오로지 출발지와 목적지를 지정하면 한곳으로만 데이터가 전송됩니다. 
- 스트림은 전송하려는 데이터가 많을 경우 지연시간이 발생합니다. 


|바이트 단위 처리(한글 처리 불가능)|  |2바이트 문자단위(한글 처리 가능)|
|---|---|---|
|InputStream|기본 입력 스트림 클래스|Reader(InputStreamReader)|
|OutputStream|기본 출력 스트림 클래스|Writer(OutputStreamWriter)|
|FileInputStream|파일 입력 스트림 클래스|FileReader|
|FileOutputStream|파일 출력 스트림 클래스|FileWriter|
|BufferedInputStream|버퍼 입력 스트림 클래스|BufferedReader|
|BufferedOutputStream|버퍼 출력 스트림 클래스|BufferedWriter|
- DataInputStream             데이터 타입을 지정하여 입력할수 있는 클래스 
- DataOutputStream            데이터 타입을 지정해서 출력할수 있는 클래스 
- ObjectInputStream ois;      Class 객체를 읽어 올 수 있습니다.  
- ObjectOutputStream oos;     Class 객체를 출력 할 수 있습니다. 
> 입력이나 출력(byte(바이너리))의 흐름을 Stream에 담고, 문자 단위 처리를 위한 Reader, Writer 사용
> 
> ex) BufferedReader br = new BufferedReader(new InputStreamReader(new System.in));
> 
> Reader 클래스의 하위 클래스
> - InputStreamReader: 문자 스트림을 읽이 위한 클래스
> - BufferedReader: 다른 Reader들을 버퍼링 하기 위한 클래스 
> - FilterReader: 필터링된 스트림으로부터 읽기 위한 클래스
> - PipedReader: 파이프 Writer로부터 읽기 위한 클래스
> - CharArrayReader: 문자 배열로부터 읽기 위한 클래스
> - StringReader: 문자열로 부터 읽기 위한 클래스

## BufferedReader
EX) BufferedReader in = new BufferedReader(new InputStreamReader(socket.getInputStream()));
- **InputStream**은 **바이트(바이너리)의 흐름을 받아들이거나 내보내는 객체**.(문자열과 Byte[]는 다른 형식의 데이터)
- 바이트를 문자열로 변환시키려면 인코딩 필요, 일일이 끊어 문자로 변호나 저장 필요
### Reader 사용: 바이너리 스트림을 문자로 바꿔서 읽어주는 역할
Reader나 Writer 객체는 한 문자 단위로 입출력 => 10개의 문자로 이루어진 문자열을 입출력 하려면 write나 read를 10번 호출해야함
- **BufferedReader는 단독으로 바로 InputStream을 받아서 쓰지 않는다**. 다른 하위 객체(예: InputStreamReader)를 인수로 받아들여 
그 Reader를 Buffering할 수 있게 도와주는 역할
- 버퍼는 메모리를 뜻한다. **읽어 들여야할 대상과 써야할 대상의 속도차이를 메모리를 사용하면서 병목 현상을 줄일 수 있다**.
- **readLine() 메소드**, 한 줄 읽기 편함
- **반드시 flush()나 close() 사용해서 버퍼 종료 필요**

## Scanner vs BufferedReader
|Scanner|BufferedReader|
|---|---|
|java.util|java.io|
|버퍼사이즈 1024byte(1KB)|버퍼사이즈 8192byte(8KB) 버퍼사용 입출력 효율|
|데이터 파싱하여 원하는 type| 데이터 파싱 X, String|
|I/O Exception 던지지 X|I/O Exception 던짐|
|Thread unsafe|Thread safe|

## (Scanner, BufferedReader).split() vs StringTokenize
문자열을 잘라쓰는 것은 같지만, 
- **split**는 정규식을 기반으로 자르는 로직이므로 내부 복잡
- **StringTokenizer**의 경우 단순히 공백을 처리
정규식 처리가 필요한게 아닌 경우 StringTokenizer가 효율적


## 결론
- 코딩테스트나 프로그래밍에서 많은 양의 문자를 다룰 때는 BufferedReader 사용, 적은 양과 타입이 여러개라면 Scanner를 사용(thread-safe X)
- 따로 정규식을 사용하지 않고 문자열을 자르려면 split가 아닌, StringTokenizer 사용

## 참고자료
- 티맥스 클라우드 네이티브 어플리케이션 개발자 양성과정 자바 교육
- https://dodocap.tistory.com/entry/JAVA%EC%9D%98-%EC%9E%85%EC%B6%9C%EB%A0%A5-%EA%B8%B0%EB%B3%B8%EA%B0%9C%EB%85%90-Stream-ReaderWriter-Buffered
- https://joont92.github.io/java/Scanner-BufferedReader-StringTokenizer/
- https://shs2810.tistory.com/19
