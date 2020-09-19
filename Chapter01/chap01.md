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
- 

