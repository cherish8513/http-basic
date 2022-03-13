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
  
  
## HTTP(HyperText Transfer Protocol)
  
### HTTP 역사
- HTTP/0.9 1991년: GET 메서드만 지원, HTTP 헤더 X
- HTTP/1.0 1996년: 메서드, 헤더 추가
- HTTP/1.1 1997년: 현재 가장 많이 사용되고 있는 형태
- HTTP/2 2015년: 성능 개선
- HTTP/3 현재: TCP 대신 UDP 사용, 성능 개선

### HTTP 특징
- 클라이언트 서버 구조
  - Request Response 구조
  - 클라이언트는 서버에 요청을 하고 응답을 대기
  - 서버가 요청에 대한 결과를 만들어서 응답
- 무상태 프로토콜 지향(Stateless)
  - 서버가 클라이언트의 상태 보존X
  - 서버 확장성이 높음(Scale out)
  - 클라이언트가 추가 데이터 전송
  - 상태 유지를 완전히 대체하기 힘듬(ex 로그인)
- 비 연결성
  - 연결 유지 X
  - 일반적으로 초 단위의 빠른 속도로 응답
  - 동시에 처리하는 요청은 사용 수에 비해 매우 적음
  - 최소한의 자원 사용
  - TCP/IP 연결을 새로 맺어야함 -> 3 way handshake 시간 추가
  - HTML 요청시(js, css, 이미지 등) 수많은 자원이 함께 다운로드<br>
  -> HTML을 다 받을 때 까지 지속 연결(Persistent Connections)로 문제 해결(HTTP2, HTTP3(UDP라 연결 속도도 줄임)은 더욱 최적화)

### HTTP 메시지 구조
start-line <br>
\* (header-field CRLF) <br>
(CRLF) <br>
[ message-body ]
- start-line = request-line(요청) / status-line(응답)
- request-line = method SP(공백) request-target SP HTTP-version CRLF(엔터)
- 요청시 start-line에 HTTP 메서드, URL 리소스가 들어감
- 응답시 start-line에 HTTP 상태 코드가 들어감
- header 에는 메타 정보들이 들어감
  
### HTTP 메서드 속성
- 안전(Safe Methods): 리소스 변경이 없다(GET, HEAD)
- 멱등(Idempotent Methods): 몇 번을 호출하든 결과가 같다(GET, PUT(덮어씀), DELETE)
  -> 자동 복구 매커니즘의 근거로 사용
- 캐시 가능(Cacheable Methos): GET, HEAD

### HTTP 메서드 설계
- 문서
  - 단일 개념
- 컬렉션
  - 서버가 관리하는 리소스 디렉토리
  - 서버가 리소스의 URI를 생성하고 관리
- 스토어
  - 클라이언트가 관리하는 자원 저장소
  - 클라이언트가 리소스의 URI를 알고 관리
- 컨트롤러, 컨트롤 URI
  - 문서, 컬렉션, 스토어로 해결하기 힘든 추가 프로세스 실행
  - 동사를 직접 사용(ex: /members/{id}/delete)
  
### HTTP/1.1
- TCP를 주로 사용
 
