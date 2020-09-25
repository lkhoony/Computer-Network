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
> 어플리케이션 레이어에서 실행되는 프로세스는 다른 프로세스와 소켓이라는 문으로 메세지를 주고 받게 됨
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
- 
