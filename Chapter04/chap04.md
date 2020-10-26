# Chapter 2. Application Layer
> - 어플리케이션 프로토콜의 개념적, 구현적 측면 알아보기
> - 많이 사용되는 어플리케이션 단계의 프로토콜을 조사함으로써 프로토콜 학습

## Outline
- Principles of Network Application
- Web and HTTP
- Electronic Mail
- DNS
- P2P Application
- Video Streaming and Content Distribution Networks
- Socket Programming with UDP and TCP

## 2.1 Principles of Network Application
> 네트워크 어플리케이션의 유형을 알아보자

### 2.1.1 Some Network Apps
```
예 : e-mail, web, text messaging, remote login, P2P file sharing, multi-user network games, ...
```

### 2.1.2 Creating a Network App
- 네트워크 어플리케이션은 각각 다른 end system에서 실행되며 네트워크 위에서 상호작용함
```
예 : 웹 서버 소프트웨어는 다른 end system의 브라우저 소프트웨어와 상호작용
```
- 네트워크 코어 디바이스는 유저 어플리케이션을 실행하지 않기 때문에 이를 위한 프로그램을 작성할 필요는 없음

### 2.1.3 Application Architectures
- Client-Server Architecture
- Peer-to-Peer (P2P)

### 2.1.4 Client-Server Architecture
- Server 
  - 항상 호스팅되고 있어야 함
  - 영구적인 IP 주소(바뀌지 않는 IP 주소)
- Client 
  - 서버와 통신하는 주체
  - 지속적으로 연결되어 있지 않고 간헐적인 연결
  - 동적인 IP 주소
  - 다른 클라이언트와 직접적으로 소통하지 않고 서버를 거쳐 소통

### 2.1.5 P2P Architecture
- 항상 서버를 거쳐 소통하지 않고 임의의 클라이언트 끼리 직접적으로 통신
- peer는 다른 peer들에게 서비스를 요청하고 요청을 보낸 peer에게 응답
- 자가 확장성(self scalability) : 새로운 peer들이 각각 새로운 서비스를 요청하고 제공함
- peer들 끼리는 간헐적으로 연결되고 IP 주소를 변경, 관리가 어려움

### 2.1.6 Processes Communicating
- 프로세스 : 호스트에서 동작하는 프로그램
- 같은 호스트의 다른 프로세스들은 운영체제로 부터 정의된 inter-process communication으로 소통함
- 다른 호스트의 프로세스들은 메세지를 교환하는 방식으로 소통함
```
- Client Process : 소통을 시작하는 프로세스
- Server Process : 연결을 기다리는 프로세스
```

### 2.1.7 Socket 
> 어플리케이션 레이어에서 실행되는 프로세스는 다른 프로세스와 소켓이라는 문으로 메세지를 주고 받게 됨 즉, 호스트의 Application Layer와 Transport Layer간의 Interface(API)

- 소켓은 프로세스가 네트워크를 통해서 메세지를 주고 받기 위해서 열어야 하는 문과 같은 것이다.
- 호스트에서 동작하는 프로세스가 다른 호스트의 프로세스와 메세지를 보내고 받는 과정은 이들의 소켓으로 주고 받음
- 전송 프로세스는 수신 프로세스의 소켓에 메세지를 반대편의 문(socket)에 전달하는 전송층의 하부구조에 의존함 
- 이러한 전송층의 하부구조(infra structure)는 운영체제에 의해 제어됨

### 2.1.8 Addressing processes
> 프로세스가 메세지를 주고 받을 때 고유한 host의 주소(식별자)를 참고하여 주고 받음
- 이렇게 메세지를 받기 위해서는 프로세스가 실행되는 호스트의 식별자를 반드시 가지고 있어야 함
- 호스트 디바이스는 유일한 32bit의 IP 주소를 가지고 있음
- 식별자에는 이러한 IP와 포트 넘버를 포함하고 있음
```
- HTTP Server Port Number : 80
- Mail Server Port Number : 25
- 예 : 128.119.245.12:80
```

### 2.1.9 App-layer Protocol Defines
> 어플리케이션 레이어에서 사용하는 프로토콜에 포함되는 내용을 알아보자
- 교환되는 메세지의 타입 (Types of Messages Exchanged)
- 메세지 타입의 문법 (Message Syntax)
- 메세지의 의미 (Message Semantics)
- 언제, 어떻게 메세지를 전송하고 응답하는지를 결정하는 규칙(Rules)

## 2.2. Web and HTTP

### 2.2.1. Http Overview
> HTTP (HyperText Transfer Protocol)

- 웹의 application 계층의 프로토콜

- Server-Client 모델로 작동
  - Client : HTTP 프로토콜을 사용하여 웹 객체들을 요청하여 화면에 보여주는 브라우저
  - Server : 요청에 대한 웹 객체들을 전송하는 웹 서버
  
- TCP를 이용하는 프로토콜
  - client는 80번(HTTP에 지정된 port번호) port로 server에게 연결을 요청
  - server는 client의 요청을 수락
  - browser(http client)와 web server(HTTP server) 사이에 HTTP message를 교환
  - 연결을 종료한다.

- HTTP에서는 상태 유지를 관리하는 것은 복잡하기 때문에 과거 요청에 대한 정보를 유지하지 않음

### 2.2.2. HTTP Connection

- Non-Persistent HTTP
  - 각각의 Object마다 하나의 TCP Connection을 사용
  - 여러 Object를 다운로드 하려면 여려 연결이 필요하여 Memory의 사용률을 높이고 Resource를 많이 소비
  
- Persistent HTTP
  - 클라이언트, 서버 간 단일 TCP 연결을 통해 여러 Object를 전송할 수 있음
  
### 2.2.3. Non-Persistent Http 

- Client는 Server에 TCP 연결을 시도
  - 3-Way-Handshaking 발생 ( SYN(Client) - ACK+SYN(Server) - ACK(Client) )
  
- Client는 Socket을 통해서 Request Message를 보냄

- Server는 Client에게 요청받은 Message에 해당하는 Object가 포함된 Reponse Message 전송

- Cliet는 Response Message를 처리하여 Web Browser에 출력

- Client와 Server의 TCP연결 종료

- Object 수 만큼 반복

### 2.2.4. Non-Persistent HTTP : Response Time

- RTT(Round Trip Time) : Client에서 송신된 작은 패킷이 Server까지 간 후 다시 Client로 되돌아 오는데 걸린 시간

- HTTP Response Time (HTML 파일 요청 응답시간)
  - TCP 연결을 초기화 하기 위한 1 RTT
  - HTTP요청을 하고 HTTP 응답을 받기 위해 필요한 1 RTT
  - 파일 전송시간
  
- 즉 Response Time = 2RTT + File Transmit Time

### 2.2.5. Persistent HTTP

- 한 번의 TCP 연결을 통해 여러 Obejct를 주고 받음

- 응답을 한 이후에도 연결상태 유지하여 이후의 HTTP 요청, 응답도 동일 연결에서 통신

- 객체를 참조하자 마자 요청을 송신하여 모든 참조 객체들에 대해 1 RTT만 필요

### 2.2.6. HTTP Request Message
- HTTP Request Header
```
GET /index.html HTTP/1.1\r\n
Host: www-net.cs.umass.edu\r\n
User-Agent: Firefox/3.6.10\r\n
Accept: text/html,application/xhtml+xml\r\n
Accept-Language: en-us,en;q=0.5\r\n
Accept-Encoding: gzip,deflate\r\n
Accept-Charset: ISO-8859-1,utf-8;q=0.7\r\n
Keep-Alive: 115\r\n
Connection: keep-alive\r\n
\r\n
```
- POST Method 
  - 사용자의 입력 값을 요청의 body에 담아 서버에 전송하는 방식
  
- GET Method
  - 사용자의 입력 값을 URL에 포함시켜 서버에 전송하는 방식
  
### 2.2.7. HTTP Response Message
- HTTP Response Header
```
HTTP/1.1 200 OK\r\n
Date: Sun, 26 Sep 2010 20:09:20 GMT\r\n
Server: Apache/2.0.52 (CentOS)\r\n
Last-Modified: Tue, 30 Oct 2007 17:00:02
GMT\r\n
ETag: "17dc6-a5c-bf716880"\r\n
Accept-Ranges: bytes\r\n
Content-Length: 2652\r\n
Keep-Alive: timeout=10, max=100\r\n
Connection: Keep-Alive\r\n
Content-Type: text/html; charset=ISO-8859-
1\r\n
\r\n
data data data data data ... 
```

- HTTP Response Status Codes

  - 200 OK : request succeeded, requested object later in this msg
  
  - 301 Moved Permanently : requested object moved, new location specified later in this msg
  
  - 400 Bad Request : request msg not understood by server
  
  - 404 Not Found : requested document not found on this server
  
  - 505 HTTP Version Not Supported

### 2.2.8. User-Server State : Cookies

- 대부분의 웹 사이트들은 쿠키를 이용하여 사용자의 state를 추적하고 유지

- 4가지 구성요소
  - HTTP response message의 cookie header-line
  - HTTP request message의 cookie header-line
  - user's host에 저장되어 관리되는 cookie file
  - web-site의 back-end database 

- 어떻게 state를 유지하는지?
  - protocol endpoints: 여러 트랜잭션에서 송/수신자 상태를 유지
  - cookies: http 메시지는 state를 나타낸다(carry)
  
### 2.2.9. Web Caches (Proxy Server)
> orgin server 없이 클라이언트의 요청을 충족

- 클라이언트가 자신을 통해서 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해주는 서버를 Proxy Server라고 함

- 서버와 클라이언트 사이에서 중계기로써 통신을 수행하는 기능을 가르켜 proxy라고 함

- 브라우저는 모든 HTTP 요청을 캐시로 보냄
  - 캐시는 요청한 객체를 반환
  - 그렇지 않으면 origin 서버에 객체를 요청한 다음 객체를 클라이언트에게 반환
  - 이후 같은 요청이 들어오면 orgin 서버에 요청할 필요 없이 proxy 서버에서 바로 응답
  
### 2.2.10. More about Web Caching

- 캐시는 클라이언트와 서버로 작동

- 일반적으로 캐시는 ISP(대학, 회사, 가정용 ISP)에 의해 설치됨

- 캐시를 사용함으로써 클라이언트 요청에 대한 응답 시간을 줄이고 트래픽을 줄임


## 2.3. Electronic Mail
- Three Major Component
  - User Agent
  - Mail Server
  - Simple Mail Transfer Protocol : SMTP

### 2.3.1. User Agent
- Mail Reader라고 불리기도 함
- 메일 작성, 전송, 확인의 과정을 수행
- 전송되고 받는 메일들을 서버에 저장

### 2.3.2. Electronic Mail : Mail Server
- 수신한 메일을 담는 메일 박스(Mail Box)
- 전송하는 메일을 메시지 큐(message queue)에 저장
- Client와 Server는 SMTP 프로토콜로 통신
  - Client : 메일을 발신하는 서버
  - Server : 메일을 수신하는 서버 

### 2.3.3. Electronic Mail : SMTP
- 발신서버에서 수신서버의 25 port로 TCP 프로토콜을 사용하여 메세지를 주고 받음
- 전송 3단계
  - handshaking
  - transfer of message
  - closure
- Command와 Response의 상호작용으로 메세지를 주고 받음
  - Command : ASCII Text
  - Response : Status Code and Phrase
  
### 2.3.4. Scenario : Alice Sends Message to Bob
    (1) Alice는 User Agent를 사용하여 메일을 발신
    (2) Alice의 User Agent는 메일서버로 메세지를 보내 message queue에 저장
    (3) Alice의 메일 서버(Client) 는 SMTP를 오픈하고 Bob의 메일 서버(Server)와 TCP 연결 요청
    (4) Alice의 메일 서버는 TCP연결을 이용하여 message 전송
    (5) Bob의 메일 서버의 mail box에 저장
    (6) Bob은 User Agent를 이용해 메일을 읽음
    
- Sample SMTP Interaction

```
S: 220 hamburger.edu
C: HELO crepes.fr
S: 250 Hello crepes.fr, pleased to meet you
C: MAIL FROM: <alice@crepes.fr>
S: 250 alice@crepes.fr... Sender ok
C: RCPT TO: <bob@hamburger.edu>
S: 250 bob@hamburger.edu ... Recipient ok
C: DATA
S: 354 Enter mail, end with "." on a line by itself
C: Do you like ketchup?
C: How about pickles?
C: .
S: 250 Message accepted for delivery
C: QUIT
S: 221 hamburger.edu closing connection
```
  
### 2.3.5. SMTP Mail Message Format
- SMTP 메세지는 RFC822 규정을 따름
- Header
  - To
  - From
  - Subject
- Body
  - ASCII로 작성된 본문 
  
### 2.3.6. Mail Access Protocols 
- SMTP
  - Client UA(User Agent)에서 발신 메일 서버로 전송할 경우 사용
  - 발신 메일 서버 측에서 수신 메일 서버로 메세지를 전송할 때 사용
  - 메일을 보낼 때 사용(Push)
- HTTP
  - 수신 메일 서버에서 수신 UA(User Agent)로 메세지를 전송할 때 사용
  - 메일을 읽을 때 사용(Pull)
  
## 2.4. DNS

### 2.4.1. DNS : Domain Name System
- 호스트 네임을 IP 주소로 변환하는 디렉터리 서비스
- 여러 Name Server의 계층구조로 구현 된 분산 데이터베이스
- 호스트와 Name Server간 주소와 도메인을 매핑(Resolve)을 위한 통신을 하는 어플리케이션 계층 프로토콜

### 2.4.2. DNS : Services, Structure
- DNS 서비스
  - 호스트 엘리어싱(Host Aliasing)
    - 간단한 별칭 호스트 네임을 복잡한 정식 호스트 네임으로 변환
  - 메일 서버 엘리어싱(Mail Server Aliasing)
  - 부하 분산(Load Distribution)
    - 여러 IP 주소를 하나의 도메인 이름으로 일치시키는 중복 웹 서버(Replicated Web Server)
    
- DNS를 중앙 집중화 하지 않는 이유
  - 서버 고장 시 전체가 작동하지 않음
  - 많은 트래픽양이 부하될 수 있음
  - 먼 거리의 중앙 집중 데이터베이스
  - 유지관리
  
### 2.4.3. DNS : A Distributed, Hierachical Database
- 클라이언트가 www.amazon.com의 IP주소를 얻기 위해서는
    - com의 DNS 서버를 찾기 위해 루트(root)서버에 질의
    - amazon.com의 DNS 서버를 찾기 위해 com의 DNS서버 (TLD 서버, Top Level Domain Server)에 질의
    - www.amazon.com의 IP 주소를 얻기 위해 amazon.com의 DNS 서버(책임 서버, Authoritative Server)에 질의

### 2.4.4. DNS : Root Name Servers
- local name server에서 도메인 이름이 매핑되지 않은 경우에 의해 연결됨
- 도메인 매핑이 안되었을 경우 권한 있는 네임 서버(Authoritative Name Server)와 연결됨
- 매핑을 얻어 로컬 네임 서버(Local Name Server)로 반환

### 2.4.5. TLD, Authoritative Servers
- Top Level Domain (TLD) Servers
  - com, org, net, edu 같은 상위 레벨 도메인과 kr, uk, fr, jp 등과 같은 국가의 상위 레벨 도메인에 대해 책임. 기관(회사, 대학 등)의 서버(웹 서버, 메일 서버 등)의 호스트 네임의 IP 주소 매핑
  
- Authoritative DNS Servers
  - 조직들의 고유한 DNS 서버로, 조직에 명명된 호스트들을 위해 권한있는 호스트 이름을 IP 주소로 매핑하는 과정을 제공함
  - 조직과 서비스 공급자에 의해 유지될 수 있음
  
### 2.4.6. Local DNS Name Server
- 로컬 네임 서버는 DNS 계층에 속하지 않음
- 각 ISP(가정 ISP, 회사, 대학)는 로컬 DNS 서버를 가짐
  - 디폴트 네임 서버(default name server)라고도 함
- 호스트가 DNS 질의를 하면 로컬 DNS 서버로 질의가 전송
  - 프록시와 같은 역할을 하며 질의를 계층으로 전달 
  
### 2.4.7. DNS Name Resolution Example
- 반복적 질의(iterative query) 
  - 접촉된 서버가 연결할 서버의 주소로 응답
  - Local DNS Server가 Root DNS Server, TLD DNS Server, Authoritative DNS Server에 반복적으로 질의하는 형식
  
- 재귀적 질의(recursive query)
  - 이름에 대한 해결(name resolution)이 서버에 대한 부담. 상위 계층 서버에 많은 부담.
  - Local DNS Server에서 Root DNS Server, Root DNS Server에서 TLD DNS Server, TLD DNS Server에서 Authoritative DNS Server로 순차적으로 질의하고 응답받는 형식
  
### 2.4.8. DNS: Caching, Updateing Records
- DNS 캐싱(DNS Caching) : DNS 서버가 어떤 이름에 대한 받은 응답 정보를 저장(caching)
- 캐싱된 정보는 일정 시간이 지나면 소멸
- 일반적으로 로컬 DNS 서버에 TLD 서버들이 캐싱. 따라서 루트 DNS 서버는 자주 방문하지 않음 