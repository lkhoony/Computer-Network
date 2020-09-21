# Introduction

## Overview
- what is the Internet?
- what is a protocol?
- network edge : hosts, access net, physical media
- network core : packet/circuit switching, Internet structure
- performance : loss, delay, throughput
- security
- protocol layers, service models
- history

## 1.1 what is the Internet?

### 1.1.1 what is the Internet : 인터넷 단위 구성 뷰
- Host(=End System) : 네트워크 프로그램이 실행되고 있는 수 많은 컴퓨팅 디바이스
```
ex) PC, Server, Wireless Laptop, SmartPhone
```

- Communication Links : host가 데이터를 전송하는 유, 무선 링크와 연결되어 데이터를 전송받음
```
bandwidth : 한 번에 얼마나 많은 양을 보내는지에 대한 척도
```        
- Routers and Switches : 수 많은 라우터와 스위치가 데이터(패킷)을 라우터, 스위치로 전송하여 host들이 데이터를 받을 수 있게 됨
```
ex) global ISP >> regional ISP >> home network louter >> 유,무선 연결로 데이터 전송
```

- Internet : "network of networks"

> Network of Networks : 네트워크들의 네트워크
```
- ISP(Internet Service Provider)를 포함한 네트워크들을 연결해주는 네트워크
- 프로토콜(Protocol)은 어떤 형태로 메세지를 보내고 받는지를 제어한다.
- RFC(Request for Comments) : 컴퓨터 네트워크 공학 등에서 인터넷 기술에 적용 가능한 새로운 연구, 혁신, 기법을 아우르는 메모
- IETF(Internet Engineering Task Force) : 국제 인터넷 표준화기구
```

### 1.1.2 A Service View
- 어플리케이션에 서비스를 제공하는 infrastructures
```
ex ) Web, Email, Games...
```
- 어플리케이션에 프로그래밍 인터페이스를 제공

### 1.1.3 what is the Protocol?
> 프로토콜은 네트워크 요소들을 포함하여 주고 받는 메세지의 형태와 순서를 정의한다.
- 네트워크에서 메세지를 주고 받을 때 정해진 규약
```
예 ) TCP : 연결요청 >> 승인 >> 데이터요청 >> 응답
```

# 1.2 Network Edge
> End Systems, Access Networks, Links

### 1.2.1 네트워크 상세 구조
- Network Edge : 네트워크 구조의 끝단으로 client와 server를 포함하는 hosts를 의미함
- Access Network, Physical Media : host와 wired 혹은 wireless되어 연결되는 부분
- Network Core : 라우터끼리 연결되어 여러개의 네트워크를 연결

### 1.2.2 End System의 네트워크 접근
- Edge Router와 End System는 어떻게 연결되나?
```
(1) 주거용(Residential Access Network) : Edge Router와 연결된 공유기 등과 연결
(2) 기관용(Institutional Access Network) : 기관의 Edge Router와 직접 연결
(3) 모바일용(Mobile Access Network) : Edge Router와 연결된 기지국과 연결
```

### 1.2.3 네트워크 접근 : Digital Subscriber Line(DSL)
- 각각의 사용자에게 할당된 Line으로 각각의 다른 주파수로 Central Office로 전송하여 라우터와 연결
```
ex ) voice, data를 사용자에게만 할당된 Line으로 DSLAM(DSL Access Multiplexer)에 전송 
      > voice는 telephone 네트워크로, data는 ISP(라우터)로
```

### 1.2.4 네트워크 접근 : Cable Network
- 공유된 케이블에 주파수를 구분하여 데이터를 전송하는 방식
```
DSL은 개인에게 할당된 라인만 사용하는 반면 공유된 케이블을 통해 전송하는 차이가 있음
```

### 1.2.5 이더넷(Ethernet)
- 기관(학교, 사무실 등)에서 ISP와 연결된 라우터(혹은 스위치)에 host들을 연결하는 네트워크 규격
```
현재 End System들은 이더넷 스위치와 연결되어 네트워크를 이룸
```

### 1.2.6 무선 네트워크 연결
- 공유된 무선 네트워크는 라우터와 host을 연결

### 1.2.7 호스트의 데이터 패킷 전송
- BandWidth(R) : 단위시간당 보낼 수 있는 데이터의 양
```
packet transmission delay = time needed to transmit L-bit packet into link 
= L (bits) / R(bits/sec)
```


# 1.3 Network Core
> Packet Switching, Circuit Switching, Network Structure

### 1.3.1 The Network Core
> 서로서로 연결되어 있는 라우터들의 집합체 (mesh of interconnected routers)
- Packet Switching : 호스트는 Application Layer의 메세지를 여러개의 패킷으로 나누어서 전송함
- 패킷들을 라우터에서 다음 라우터로 forwarding하는 방식으로 전송 호스트(Source)부터 도착지(Destination)까지의 경로로 전송
- 각 패킷들은 최대 전송률로 전송됨

### 1.3.2 Packet Switching : Stored-and-Forward
> Stored and Forward : 다음 링크로 전송되기 전에 전체의 패킷이 라우터에 도착해야됨
- Source로부터 Destination까지 경로에 하나의 라우터를 거치고 전파지연이 없다고 가정하면 end-end delay는 2L/R
```
end-end delay = Source에서 Router로 forward하는 시간(L/R) + Router에서 Destination으로 forward하는 시간(L/R)
```

### 1.3.3 Packet Switching : Queueing Delay, Loss
- Packet Switching은 공용선을 이용하여 메세지를 전송
- Queueing Delay : 만약 라우터에서의 패킷의 도착비율이 전송률을 초과하게 되면 큐에서 전송될 때 까지 메모리 버퍼에서 대기
- Loss : 메모리 버퍼가 꽉 차있다면 패킷을 잃을 수도 있음

### 1.3.4 Two Key Network-Core Functions
- Routing : 패킷 해더에 저장되어있는 value를 참고하여 routing algorithm에 따라 source에서 부터 destination의 경로를 결정하는 것
- Forwarding : 패킷을 다음 라우터로 보내는 것 

### 1.3.5 Alternative Core : Circuit Switching
- Packet Switching이 공용선을 이용한다면 Circuit Switching은 하나의 회선을 할당받음
- 소스로 부터 목적지까지 전체 회선을 독점
- 속도와 성능은 일정하지만 많은 회선을 만들어야하는 단점
```
FDM : 주파수를 나누어 해당 주파수 영역대의 회선만 사용하는 방법
TDM : 시간을 나누어 해당 시간에만 회선을 사용하는 것
```

### 1.3.7 Internet Structure : Network of Networks
- access network가 서로 연결되어 있다면 O(N^2)의 연결이 필요할 것이고 이것은 비현실적임
- access network는 여러개의 라우터의 집합들인 ISP(Internet Service Provider)과 연결되어 서로 메세지를 주고 받음
- 또한 ISP끼리 Peering Link 혹은 Internet Exchange Point로 연결되어 있음
- access network끼리 regional network를 형성하고 이것이 다시 ISP

# 1.4 Delay, Loss, Throughput in Networks

### 1.4.1 How do loss and delay occur?
- packet은 라우터에서 stored and forward 방식으로 보내지기 때문에 라우터의 큐에서 대기하게 되어 지연(delay)발생
- 하지만 라우터의 버퍼에 남은 공간이 없으면 loss 발생

### 1.4.2 Four Sources of Packet Delay
- 전송지연(Transmission Delay) : 네트워크 장비의 전송속도로 인해 발생하는 지연 ( L : packet length / R: link bandwith )
- 전파지연(Propagation Delay) : 전송거리와 전송 매개체로 인해 발생하는 지연 ( d : length of physical ling / s : propagagion speed )
- 노드처리지연(Node Processing Delay) : 비트 오류 검출 등의 작업으로 소요되는 시간 
- 큐지연(Queue Delay) : 모든 데이터가 도착할 때 까지 라우터의 큐에서 대기하는 시간 

### 1.4.3 Packet Loss
- 큐에는 유한한 용량의 패킷이 저장되기 때문에 꽉 차게되면 패킷의 유실이 발생함
- 프로토콜에 따라서 유실된 패킷을 재전송 하거나 하지 않음

### 1.4.4 Throughput 
- 어떤 노드나 터미널로부터 또 다른 터미널로 전달되는 단위시간 당 데이터의 양
- Rs bits/s로 전송된 데이터들을 라우터를 거쳐 Rc bits/s로 end point에서 데이터를 받을 경우의 Throughput
```
- Rs < Rc : Rs
- Rs > Rc : Rc
> 즉 Throughput은 min(Rc,Rs)를 따르게 됨
```

# 1.5 Protocol Layers, Service Models
> 네트워크들은 여러 레이어들로 구성되어있음

### 1.5.1 Why Layering?
- 새로운 프로그램을 실행해도 layed된 방법으로 데이터를 전송하면 됨
- layer를 모듈화 시킴으로써 네트워크 시스템의 유지보수와 업데이트를 용이하게 함
- 특정한 곳에 이상이 생기면 다른 단계의 시스템을 건들지 않고도 이상이 생긴 단계만 보수

### 1.5.2 Internet Protocol Stack
- Application : 네트워크 어플리케이션들을 지원(FTP, SMTP, HTTP)
- Transport : 호스트의 프로세스들을 연결 (TCP, UDP)
- Network : 데이터를 목적지로 라우팅 (IP, Routing Protocols)
- Link : 네트워크 요소들에 데이터를 전송
- Physical : 실제 물리적 와이어에 비트를 전송

# 1.6 Networks Under Attack : Security
> Network Security는 처음에는 고려되어 설계되지 않았지만 현재는 모든 레이어에서 고려되어야 함

### 1.6.1 Denial of Service(DoS)
- 호스트를 좀비PC로 만들어 Server에 과부하를 일으켜 실제로 해야될 일을 못하게 만드는 공격기법

### 1.6.2 Sniffing
- 네트워크 상의 데이터를 도청하는 행위

### 1.6.3 IP Spooping
- IP를 도용하여 실제로는 해당 IP의 명의로 스팸메일을 발송하거나 악의적인 데이터를 보내는  
