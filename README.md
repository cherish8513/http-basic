## 인터넷 네트워크
복잡한 인터넷 서버망을 거치기 위해서는 어떻게 해야할까?
### IP(Internet Protocol) 역할
- 지정한 IP 주소(IP Address)에 데이터 전달
- 출발지 IP, 목적지 IP 등의 정보로 패킷(Packet(package + bucket)) 구성

### IP(Internet Protocol)의 한계
- ### 비연결성
  - 패킷을 받을 대상이 없거나 서비스 불능 상태여도 패킷 전송
- ### 비신뢰성
  - 중간에 패킷이 사라지는 상황일 때 해결 X
  - 패킷이 순서대로 안 오는 상황일 때 해결 X
- ### 프로그램 구분
  - 같은 IP를 사용하는 서버에서 통신하는 애플리케이션이 둘 이상이면?

### TCP(Transmisson Controll Protocol)
- 출발지 PORT, 목적지 PORT, 전송 제어, 순서, 검증 정보 등이 포함되어있는 정보로 패킷 구성
- IP의 한계를 보안해준다. <BR>
  ->보내려는 정보를 IP 계층(Network Layer)에 보내기 전에 TCP/UDP 정보를 씌운다.
- ### TCP 특징 -> 신뢰할 수 있는 프로토콜(데이터 양도 많고 느림)
  - 연결 지향 - 3 way handshake (가상 연결으로 전용 랜선 연결 개념 X)
    1. 클라이언트에서 서버로 SYN(접속 요청) 전달
    2. 서버에서 클라이언트로 SYN + ACK(요청 수락) 전달
    3. 클라이언트에서 서버로 ACK(+DATA) 전달
  - 데이터 전달 보증
  - 순서 보장
    - 최적화에 따라 다르지만 잘못된 패킷 순서 이후를 버리고 그 패킷부터 다시 보내라고 요청 <br>
  
### UDP(User Datagram Protocol)
  - IP와 거의 유사하고 PORT + 체크섬 정도만 추가되어있다 -> 단순하고 빠르다.
  - 어플리케이션에서 추가 작업이 필요하다
  - TCP 최적화에 한계가 있어서 그런 경우에 UDP로 최적화 해 사용하는 방식

### PORT
  - 같은 IP 내에서 프로세스 구분
  - 0 ~ 65535가 할당 가능한데 0 ~ 1023은 잘 알려진 포트로 사용하지 않는 게 좋다.
  
### DNS(Domain Name System)
  - 존재 이유 -> IP는 기억하기 어렵고 변경될 수 있다.
  - DNS 서버에 도메인 명(ex google.com)을 등록할 수 있다. -> 통신시에 도메인 이름부터 찾고 IP를 찾는다.
  
<br>
  
## URI와 웹 브라우저 요청 흐름

### URI(Uniform Resource Identifier)
- URI는 Locator(URL), Name(URN) 또는 둘 다 추가로 분류 될 수 있다. <br>
-> URI가 가장 큰 개념이고 URL과 URN이 속해있다.
  * URL은 리소스의 위치를 뜻한다. (웹 브라우저)
  * URN은 리소스의 이름을 뜻한다. (거의 안 씀)
- Uniform : 리소스를 식별하는 통일된 방식, Resource : 자원, Identifier : 식별
- URI와 URL을 보통 같은 의미로 사용한다.
  
### URL 사용형태 
scheme://[userinfo@]host[:port][/path][?query][#fragment]
- userinfo : 거의 사용되지 않는다.
- port : 일반적으로 생략되며 http는 80 https는 443
- path : 계층적 구조의 리소스 경로
- query : key=value 형태, ?로시작 &로 추가 가능하며 query parameter, string으로 불림
- fragment : html 내부 북마크에 사용되며 잘 사용되지 않는다.
  
### 웹 브라우저의 요청 흐름
1. 웹 브라우저가 HTTP 메세지 생성
2. Socket 라이브러리를 통해 전달
  A. TCP/IP 연결(IP, PORT)
  B. 데이터 전달
3. TCP/IP 패킷 생성, HTTP 메세지 포함
 
