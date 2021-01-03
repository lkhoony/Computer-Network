# Chapter 5. Network Layer : The Control Plane
> 네트워크 계층의 제어 영역의 서비스 유형을 이해

## Outline
- Introduction
- Routing Protocols
- Intra-AS Routing int the Internet : OSPF
- Routing Among the ISPs : BGP
- The SDN Control Plane
- ICMP : The Internet Control Message Protocol
- Network Management and SNMP

## 5.1. Introduction

### 5.1.1. Network Layer Functions

- Forwarding (Data Plane)

	- 라우터의 입력 버퍼로 들어온 패킷의 헤더에 있는 IP 주소와 포워딩 테이블의 IP 주소를 비교하여 적절한 포트번호를 가진 출력 버퍼로 패킷을 옮김

- Routing (Control Plane)

	- 출발지에서 목적지까지의 모든 경로를 결정

- Two Approaches to Structing Network Control Plane

	- Per-Router Control(Traditional) : 각각의 라우터에서 라우팅 알고리즘에 따라 forwarding table을 계산
	
	- Logically Centralized Control(Software Defined Networking) : 논리적으로 중앙 집중화된 제어 영역이 forwarding table을 계산
	
## 5.2. Routing Protocols

- 라우팅 프로토콜의 종류

	- 링크 상태 라우팅 프로토콜(Link State Louting Protocol)
	
	- 거리 벡터 라우팅 프로토콜(Distance Vector Louting Protocol)
	
### 5.2.1. A Link State Routing Algorithm

- 링크 상태 알고리즘은 다익스트라 알고리즘(Dijkstra 알고리즘)이라고도 함

![image](https://user-images.githubusercontent.com/66773320/103451479-cf1fa700-4d07-11eb-8a2b-29b8f4d5e331.png)

- 링크 상태 알고리즘 프로세스

	- 각 라우터는 자신과 직접 연결된 네트워크를 파악
	
	- 각 라우터는 자신과 직접 연결된 네트워크 상의 이웃 라우터들과 Hello 패킷을 교환
	
	- 각 라우터는 각각의 직접 연결된 링크의 상태를 담고 있는 LSP(Link State Packet)을 생성
	![image](https://user-images.githubusercontent.com/66773320/103451488-e65e9480-4d07-11eb-9e82-34e871680922.png)
	
	- 각 라우터는 모든 이웃에게 LSP를 플러딩(Flooding)
	![image](https://user-images.githubusercontent.com/66773320/103451492-fbd3be80-4d07-11eb-9f33-96a59eb28e62.png)
	
	- 수신된 모든 LSP를 데이터베이스에 저장하고 LSP를 받은 라우터는 또 다른 라우터에게 전송
	
	- 각 라우터는 데이터베이스를 이용해 완전한 토폴로지 맵을 구성하고 각 목적지 네트워크로의 최적 경로를 계산
	![image](https://user-images.githubusercontent.com/66773320/103451506-1d34aa80-4d08-11eb-9dee-e9cd9748287e.png)

- 플러딩(Flooding)이란?

	- 어떤 노드에서 온 하나의 패킷을 라우터에 접속되어 있는 다른 모든 노드로 전달하는 방식
	
	- 라우팅 정보를 모든 노드에게 빠르게 분배하는 수단이며 최단경로우선 프로토콜(OSPF)에서 사용

- 다익스트라 알고리즘 의사코드

	![image](https://user-images.githubusercontent.com/66773320/103453822-3564f380-4d21-11eb-9f7c-bc8ab480b5b5.png)
	
	- 시작 노드로 부터의 최소비용(최소거리)을 key로 하는 우선순위 큐
	
	- 모든 노드를 우선순위 큐에 삽입하면 최소비용에 따라 정렬이되어있고 최소비용의 key를 갖는 노드(출발점)를 제거하고 해당 노드는 확정(permanent)됨
	
	- 해당 노드에 연결되어 있는 간선과 반대의 노드에 대해 최소 비용을 계산하여 이를 key로하여 우선순위 큐를 재정렬(add)
	
	- 재정렬된 우선순위 큐에서 최소 비용을 갖는 노드를 다시 제거하여 확정(permanent)시키고 위의 과정을 큐가 빌 때 까지 반복
	
- 최단경로 계산 과정

	- 모든 노드를 우선순위 큐에 삽입 
	
	- A노드를 제거하고 간선과 연결되어 있는 노드들의 비용(거리)을 계산하여 재정렬(add)
	
	- 비용(거리)가 가장 작은 노드(Net:14)제거(permanent), 해당 노드와 연결된 간선의 반대 노드들의 최소비용(거리) 계산(add)
	
	![image](https://user-images.githubusercontent.com/66773320/103453548-99d28380-4d1e-11eb-9fd5-871a8f135fc7.png)
	
	![image](https://user-images.githubusercontent.com/66773320/103453552-a35beb80-4d1e-11eb-8f30-62ac2a713996.png)

	![image](https://user-images.githubusercontent.com/66773320/103453559-ace55380-4d1e-11eb-8a0d-275456590572.png)
	
	![image](https://user-images.githubusercontent.com/66773320/103453566-bc649c80-4d1e-11eb-8eb4-54efcec40b5f.png)
	
	![image](https://user-images.githubusercontent.com/66773320/103453569-c4bcd780-4d1e-11eb-91d1-106b1548a5fc.png)
	
	![image](https://user-images.githubusercontent.com/66773320/103453586-d9996b00-4d1e-11eb-881b-42b65bb8734a.png)

- 출발노드(A)에서의 라우팅 테이블
	
	![image](https://user-images.githubusercontent.com/66773320/103454219-26804000-4d25-11eb-87fc-252059ccfc31.png)
	
	- A로부터 Net:8로의 라우팅 시 비용은 4, 다음 라우터는 E

### 5.2.2. A Distance Vector Routing Algorithm

- 거리 벡터 라우팅 프로토콜은 벨만 포드 알고리즘을 이용해 라우팅 테이블을 자신과 직접적으로 연결된 다른 노드들에게 전송

![image](https://user-images.githubusercontent.com/66773320/103471597-84fcfb00-4dc5-11eb-904b-230d5973108c.png)

- 라우터는 목적지까지의 모든 경로를 자신의 라우팅 테이블 안에 저장하는 것이 아니라 목적지까지의 거리(홉 카운트 등)과 그 목적지를 가려면 바로 거쳐야 하는 인접 라우터를 저장

	- 링크 상태 라우팅에서는 모든 토폴로지 정보를 데이터베이스에 저장
	
- 새로운 정보를 받을 때마다 인접한 라우터에 그 정보를 알려주고 이것을 반복하여 최종적으로 모든 라우터가 네트워크 전체의 정보를 갖게 됨

![image](https://user-images.githubusercontent.com/66773320/103471671-5fbcbc80-4dc6-11eb-96ed-07f5b371cc50.png)

- 라우터 A에서의 라우팅 테이블 업데이트 과정

![image](https://user-images.githubusercontent.com/66773320/103471683-811da880-4dc6-11eb-861f-d767c2794e13.png)

- 인접 라우터인 B로 부터 해당 라우터의 라우팅 테이블에 대한 메세지를 받는다.

```
one hop을 더하는 이유

- 거리 벡터 라우팅에서는 인접한 노드로부터 정보를 받음, 따라서 인접한 노드와의 거리는 1

- B라우터는 A라우터의 인접한 라우터기 때문에 A->B의 거리는 1

- 따라서 A로부터 B라우터를 거쳐 B라우터의 라우팅 테이블에 해당하는 목적지로 가기 위해서는 1이 더해져야 한다.

- 예) A라우터에서 Net:14로 가기 위한 거리 1, B라우터에서 Net:14로 가기위한 거리 1
	
	- A라우터에서 B를 거쳐 Net:14로 가기 위한 경로 : A->Net:14->B->Net:14

	- 따라서 거리는 2가 되고 거쳐야 하는 다음 라우터는 B로 라우팅 테이블이 조정됨
```

- A라우터에서 이전의 라우팅 테이블과 Combined

- 같은 네트워크로의 경로가 존재하면 비용이 더 저렴한 경로를 선택

## 5.3. Intra-AS Routing in the Internet : OSPF

### 5.3.1. Making Routing Scalable

- 이전까지의 라우팅 기법은 모두 이상적인 환경을 가정

- 하지만 실제 현실에서는 다음과 같은 문제점이 발생

	- Scale(규모)
		
		- 현실에서는 수억개의 목적지(호스트)가 존재
		
		- 모든 목적지를 라우팅 테이블에 저장할 수 없음
		
		- 이러한 라우팅 테이블 정보 교환은 링크의 대역폭을 고갈시킴
		
	- Administrative Autonomy(관리 자치권)
	
		- 인터넷은 여러 네트워크의 네트워크로 정의됨
		
		- 각 네트워크 관리자들을 각각의 네트워크에서의 라우팅을 제어하길 원함

### 5.3.2. Internet Approach to Scalable Routing

- 라우터들을 Autonomous Systems(AS) 혹은 도메인으로 알려진 영역으로 그룹화 시키는 것으로 문제들을 해결할 수 있다.

	- Autonomous System(AS)
	
		- 네트워크 관리자에 의해 관리되는 라우터들의 집단을 하나로 생각하는 것
		
		- 예를들어 K사라는 ISP업체의 라우터들을 관리자에 의해 설정되었다면 그 라우터들은 하나의 AS가 됨
		
- intra-AS Routing

	- 같은 자치 시스템(AS) 내부에 호스트와 라우터들 간의 라우팅 방식
	
	- 동일한 AS 내부의 라우터들은 같은 인트라 도메인 프로토콜(intra-domain protocol)을 수행
	
	- 게이트웨이 라우터(Gateway Router) : 자신의 AS내에서 가장 가장자리의 라우터로 다른 AS와 연결한다.
	
- inter-AS Routing
	
	- 서로 다른 자치 시스템(AS)간의 라우팅 방식
	
	- 게이트웨이 라우터가 인트라 도메인 라우팅 뿐 아니라 인터 도메인 라우팅도 수행
	
### 5.3.3. Interconnected ASes

- 포워딩 테이블은 intra-AS와 inter-AS의 라우팅 알고리즘에 의해 설정됨

- intra-AS 라우팅은 해당 AS내에서 목적지들에 대한 테이블 엔트리를 결정한다.

- inter-AS와 intra-AS 모두 외부 목적지들에 대한 테이블 엔트리를 결정한다.

### 5.3.4. Inter-AS Tasks

![image](https://user-images.githubusercontent.com/66773320/103473978-3ca00600-4de2-11eb-947b-f24630c80f82.png)

- AS1이 AS1 외부로 목적지가 설정 된 데이터그램을 전송받았다고 가정하면 

	- 라우터는 패킷을 게이트웨이 라우터로 전달해야 하는데 여러 게이트웨이 라우터 중 하나를 선택하는 방식이 필요
	
- AS1은 AS2, AS3를 경유하여 도달할 수 있는 목적지들을 학습해야 함

- 이러한 정보를 AS1 내의 모든 라우터에 전파해야 함

### 5.3.5. Intra-AS Routing

- AS 내에서의 라우팅 프로토콜

- Interior Gateway Protocols(IGP)라고도 함

- 대표적인 intra-AS 라우팅 프로토콜의 종류

	- RIP : Routing Information Protocol
	
		- RIP는 내부 네트워크에서 주로 사용
		
		- hop count에 따라 최단 경로를 동적으로 결정하는 거리 벡터 알고리즘을 사용
		
	- OSPF : Open Shortest Path First
	
	- IGRP : Interior Gateway Routing Protocol

### 5.3.6. OSPF(Open Shortest Path First)

- 개방형으로 라우팅 프로토콜이 공용으로 이용할 수 있어야 함

- 링크 상태 알고리즘을 사용

	- 링크 상태 패킷을 전파
	
	- 각 노드에 전체 AS의 토폴로지를 저장
	
	- 최소비용을 다익스트라 알고리즘을 사용하여 계산
	
- 라우터는 OSPF 링크 상태 정보(라우팅 정보)를 AS내의 전체 라우터에게 브로드캐스트한다.
	
	- 링크상태 정보를 Flooding
	
	- OSPF메세지를 TCP, UDP가 아닌 IP 데이터그램의 데이터부분(payload)에 담아 전송
	
- 대규모 엔터 프라이즈 네트워크에서 널리 사용되는 내부 게이트웨이 프로토콜(IGP, Interior Gateway Protocol)

### 5.3.7. OSPF Advanced Features

- 보안(Security) : 모든 OSPF 메세지는 악의적인 접근을 방지하기 위해 인증되어야 함

- 동일 비용의 경로가 다수 존재할 때 여러 경로를 사용할 수 있음(RIP 프로토콜은 오직 하나의 경로)

- 유니캐스트와 멀티캐스트를 지원

- 넓은 도메인 영역에서는 계층적 OSPF(hierarchical OSPF)를 지원

### 5.3.8. Hierarchical OSPF

![image](https://user-images.githubusercontent.com/66773320/103477622-0d01f580-4e04-11eb-951a-3edb860d99d4.png)

- AS가 크면 Flooding시 오버헤드가 증가하여 이름 방지하기 위해 계층적으로 영역을 나눈다.

- 2계층 구조

	- Backbone 영역
	
	- Local Area 영역
	
- 한 영역(Area)내에서만 링크 상태를 광고한다.

- 각각의 노드는 구체적인 영역 토폴로지를 갖게 되고 다른 영역에 대해서는 최단 경로의 방향만 알고있다.

- 지역 경계 라우터(Area Border Router, ABR)

	- 트래픽 양을 줄이기 위해 축약 정보를 backbone 영역의 라우터들에게 전파
	
	- 요약된 정보를 다를 ABR에게도 광고
	
- 백본 라우터(Backbone Router)

	- AS 내의 여러 Area들을 모두 연결하는 백본 Area에서 특별한 역할을 하는 라우터
	
	- OSPF 도메인 내 모든 링크상태 정보를 갖게됨
	
	- 자신의 Area에 있는 라우팅 정보를 모아서 다른 Area에 전달 및 전파
	
- 경계 라우터(Boundary Router)

	- 다른 AS와 연결되는 라우터

## 5.4 Routing among the ISPs : BGP

- Border Gateway Protocol(BGP) : AS간의 inter-AS 프로토콜을 의미

### 5.4.1. Internet inter-AS routing : BGP

![image](https://user-images.githubusercontent.com/66773320/103479304-2610a380-4e10-11eb-8245-4a03ed9954b2.png)

- exterior BGP(eBGP)
	
	- 이웃 AS로부터 목적지 도달을 위해 필요한 정보(서브넷 도달성 정보)를 얻음
	
	- TCP 기반 통신(179번 포트)으로 신뢰성 있는 통신을 함
	
	- 유니캐스트 방식으로 전송
	
- interior BGP(iBGP)

	- 해당 AS내의 모든 라우터에게 도달성 정보를 전파
	
	- AS내부에서 라우팅 정보를 주고 받음
	
- 도달성 정보와 AS 정책을 기반으로 다른 네트워크로의 좋은 경로를 결정한다.

- 다른 네트워크로 해당 서브넷이 존재하다는 것을 광고하여 정보 전달을 가능하게 함

### 5.4.2. BGP Basics

- BGP Session

	- 두 BGP 라우터들을 반 영구적인 TCP 연결을 통해 BGP 메세지(라우팅 정보)를 주고 받음
	
	- 
### 참고

- https://m.blog.naver.com/c_18/10179757156

- https://seokr.tistory.com/136

- https://movefast.tistory.com/55?category=765942

- https://blog.naver.com/demonicws/40108441909