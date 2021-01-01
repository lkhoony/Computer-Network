# Chapter 4. Network Layer : The Data Plane
> 데이터 영역에 집중하여 네트워크 계층 서비스의 유형을 이해

> 인터넷의 구현 및 인스턴스 화

## Outline
- Overview of Network Layer
- Whats inside a Router
- IP : Internet Protocol
- Generalized Forward and SDN

## 4.1. Overview of Network Layer


### 4.1.1. Network Layer

- 송신 호스트에서는 전송 계층으로 부터 세그먼트를 받아 이를 데이터그램으로 캡슐화하여 전송

- 수신 호스트에서 세그먼트를 전송 계층으로 전송

- 모든 호스트와 라우터에 네트워크 계층 프로토콜이 적용됨

- 라우터는 해당 라우터를 통과하는 모든 IP 데이터그램에 있는 헤더 필드를 검사

### 4.1.2. Two Key Network Layer Functions
> 라우팅이 출발지에서 목적지까지 여행을 계획하는 과정이라면 포워딩은 여행 중간의 하나의 지점을 지나가는 과정

#### Forwarding

  - 라우터에는 입력 버퍼와 출력 버퍼가 있다.

  - 라우터의 입력 버퍼로 들어온 패킷의 헤더에 있는 IP 주소와 포워딩 테이블의 IP 주소를 비교하여 적절한 포트번호를 가진 출력 버퍼로 패킷을 옮김

#### Routing

  - 출발지에서 목적지까지의 모든 경로를 결정하는 것

  - 라우터에서 라우팅 알고리즘으로 포워딩 테이블을 만들고 포워딩 테이블에는 목적지 IP 주소와 해당 포트번호가 저장되어 있음

### 4.1.3. Network Layer : Data Plane, Control Plane

#### Data Plane

- 각 라우터의 포워딩 함수들을 동작
  
- 라우터의 인풋 포트로 도착한 데이터그램이 라우터의 어떤 아웃풋 포트로 포워딩 되는지를 결정
  
#### Control Plane

- 넓은 네트워크의 논리적인 부분을 담당하는 뇌의 역할을 한다.

- 데이터그램이 종단 사이에 라우터들에 의해 어떻게 라우팅되는지 결정

- 두 개의 control plane 접근법

  1. 전통적인 라우팅 알고리즘(traditional routing algorithm) : 하드웨어 측면에서 라우터들 내에 구현되어 있음
  
  2. SDN(Software Defined Networking) : 소프트웨어 측면에서 서버에 의해 구현

### 4.1.4. Per-Router Control Plane

- 각각의 라우터 별로 라우팅 알고리즘을 수행하여 라우터 별로 forwarding table을 유지

- 각 라우터마다 있는 라우팅 알고리즘이 Control plane의 상호작용을 한다.

![image](https://user-images.githubusercontent.com/66773320/97148754-007f7200-17af-11eb-86fc-f2b1574fa247.png)

### 4.1.5. Logically Centralized Control plane

- 별도로 중앙에 배치된 컨트롤러가 local control agents(CAS)로 각 라우터와 상호작용한다. 중앙에서 control plane이 이루어짐.

![image](https://user-images.githubusercontent.com/66773320/97148783-0bd29d80-17af-11eb-8482-99565de44044.png)

### 4.1.6. Network Service Model

- 송신자에서 수신자에게 데이터그램들을 전송하는 채널에 대한 서비스 모델은 두 가지가 있다.

- 각각의 데이터그램에 대한 서비스 

  - 지연 시간이 40ms 보다 적은 상태로 전달된다.

- 일련의 데이터그램들에 대한 서비스 

  - 순서가 중요한 데이터그램을 위한 서비스 
  
  - 전송을 위해 최소한의 대역폭을 보장
  
  - 각 패킷간의 간격에서 변화(ex : Jitter)가 제한적이다.

## 4.2. What's Inside a Router
> 라우터 내에서 어떤 과정으로 포워딩 및 라우팅을 하는지 살펴보기

### 4.2.1. Router Architecture Overview

- 일반적인 라우터 구조의 뷰

![image](https://user-images.githubusercontent.com/66773320/97151051-54d82100-17b2-11eb-9a15-72cb08946080.png)

### 4.2.2. Input Port Functions

![image](https://user-images.githubusercontent.com/66773320/97151233-8bae3700-17b2-11eb-8c62-c21e77b90221.png)

- 패킷의 헤더 필드 값(목적지 IP주소)을 보고 포워딩 테이블에 있는 출력 포트 번호를 찾음

- 포워딩 테이블에 있는 IP주소와 패킷의 목적지 IP주소를 비교하는 데 시간이 오래 걸림

- 큐잉(Queuing) : 데이터그램이 포워딩하는 속도보다 더 빨리 입력 포트에 도착하면 큐(입력버퍼)에 저장

- 목적지 기반의 포워딩 : 목적지 IP 주소만을 이용하여 포워딩 (전통적인 방식)

- 일반화된 포워딩 : 헤더 필드 값의 전체를 이용하여 포워딩

### 4.2.3. Destination Based Forwarding

> 목적지 기반의 포워딩

![image](https://user-images.githubusercontent.com/66773320/97151553-feb7ad80-17b2-11eb-9c00-b10b40137922.png)

- IP 주소를 범위로 정해 포트 번호를 구분하는데 IP 주소 32비트를 모두 사용하면 테이블이 커짐

- 따라서 앞부분의 정해진 비트까지만 보여주고 범위(range)를 정하여 포트번호를 구분

### 4.2.4. Longest Prefix Matching

- 패킷의 목적지 IP주소가 포워딩 테이블에서 둘 이상의 IP주소 범위에 포함될 경우, 그 중 비트가 가장 많이 공개된(가장 긴 prefix) IP 주소 범위의 포트번호로 포워딩

### 4.2.5. Switching Fabrics

- 스위치 내부에서 입력 버퍼에 있는 패킷을 적절한 출력 버퍼로 전송하는 과정 필요

- 이를 가상의 회선으로 그물망 혹은 직렬 연결하여 구성되는 모양을 섬유(fabric)으로 비유한 것

- Switching rate : 입력 포트에서 출력 포트로 패킷을 전송할 수 있는 속도로, 종종 다수의 입출력 라인 속도에서 측정되어진다. n개의 입력이 있을 때, 스위칭 속도는 입력 라인 속도의 N배가 요구됨

![image](https://user-images.githubusercontent.com/66773320/97152633-8651ec00-17b4-11eb-8a81-9c9d3fb18273.png)

### 4.2.6. Switching via Memory

- CPU의 직접적인 제어 아래에 스위칭하는 전통적인 컴퓨팅 방식으로, 첫 세대 라우터들의 스위칭 구조이다.

- 패킷이 시스템 메모리로 복사된다.

- 속도가 메모리 대역폭에 의해 제한된다.

- 메모리에 접근하는 시간도 많이 걸린다.

### 4.2.7. Switching via a Bus

- 버스 구조는 2 이상의 신호선들을 모아놓은 것으로, 디지털 시스템에서 디지털 요소들을 상호 연결하는데 필요한 데이터 신호 통로들의 구조화된 그룹이다.

- 입력 포트 메모리로부터 출력 포트 메모리까지 하나의 공유된 버스를 통해 데이터그램을 스위칭한다.

- Bus contention : 스위칭 하는 속도는 버스의 대역폭에 의해 제한된다.

ex)　Cisco 5600 제품이 32Gbps의 속도를 가진 버스 구조로 접근성에 있어서 충분한 속도를 가진다.

### 4.2.8. Switching via Interconnection Network

- 제한적인 버스 대역폭을 해결한다.

- 다수의 프로세서들을 연결하기위해 개발된 interconnection nets, banyan networks, crossbar 망이다.

- 진보된 구성으로는, 데이터그램을 고정된 길이의 셀들로 분할하여 단편화한다.

ex) Cisco 12000 제품이 interconnection network를 통해 60Gbps의 속도로 스위칭한다.

### 4.2.9. Input Port Queuing

- 큐잉은 입력 큐(버퍼)에서 패킷이 저장되는 것을 의미

- 입력 큐(버퍼)가 꽉 차면 오버플로우(Overflow)가 발생했다고 함

- 큐잉 지연(Queuing delay) : 큐에서 패킷들이 대기하는 시간

- 오버플로우가 발생하면 큐잉 지연(Queuing delay)이 일어나고 전송 속도가 저하된다. 또한 큐에 들어오지 못한 패킷들은 손실된다.

- HOL blocking(Head-of-the-Line blocking)

  - 큐에서 저장된 패킷들 중 앞에 있는 패킷이 속도가 느려 뒤에 있는 패킷이 못가는 상황
  
  - 즉, 둘 이상의 다른 입력 버퍼에서 같은 출력 버퍼로 이동하는 패킷들이 있을 때, 하나가 이동하면 다른 입력 버퍼의 맨 앞의 패킷은 이동할 수 없다. 그러면 대기하는 패킷의 뒤에 있는 패킷들도 덩달아 대기해야하는 상황을 말한다.
  
![image](https://user-images.githubusercontent.com/66773320/97154830-c23a8080-17b7-11eb-9698-478feb05a3ec.png)

### 4.2.10. Output Port Queueing

- 스위칭 구조가 패킷의 전송속도(출력링크 속도)보다 더 빨리 출력 포트에 도착할 때 큐잉(버퍼 관리)가 필요

- 버퍼에 저장된 데이터그램들을 선택하여 전송(출력)하는 스케쥴링 규칙(scheduling discipline) 필요

- 스위치 구조를 경유한 패킷 도착 속도가 출력 라인 속도보다 클 경우 버퍼링

- 큐잉(지연)과 출력 버퍼 오버플로우로 인한 패킷 손실 발생

![image](https://user-images.githubusercontent.com/66773320/97154845-c8c8f800-17b7-11eb-8368-0a29afd24ec1.png)

### 4.2.11. Scheculing Mechanisms
> 링크로 보낼 다음 패킷을 결정하는 과정 및 방법

- 스케쥴링 : 링크로 보낼 다음 패킷을 결정하는 것

  - 오버플로우 발생 시 패킷을 버리는 방법

    - tail drop : 꼬리 자르듯이 도착하는 패킷들을 버린다.

    - priority : 우선순위를 기반으로 낮은 순위의 패킷들을 버린다.

    - random : 무작위로 패킷을 뽑아 버린다.
    
### 4.2.12. Scheduling Policies

#### Priority

- 가장 높은 우선순위를 가진 큐에 담긴 패킷들을 먼저 링크로 전송 즉, 순위가 낮은 큐는 순위가 높은 큐가 비어 있어야 패킷들을 전송할 수 있음

![image](https://user-images.githubusercontent.com/66773320/97154873-d41c2380-17b7-11eb-95f7-f05c831896cb.png)

#### Round Robin

- 다수의 큐들이 돌아가면서(Cyclically) 각각의 패킷들을 전송함

![image](https://user-images.githubusercontent.com/66773320/97154894-de3e2200-17b7-11eb-977b-5e7eeedd3603.png)

#### WFQ(Weighted Fair Queuing)

- 라운드 로빈(RR) 스케줄링과 유사하나, 각 큐마다 대기 시간이 있다는 점에서 차이가 있 즉, 각 큐는 돌아가면서 패킷을 전송하지만 전송되는 시간간격이 정해져 있다.

![image](https://user-images.githubusercontent.com/66773320/97154913-e4cc9980-17b7-11eb-9bad-9f160fe3684f.png)

## 4.3. IP : Internet Protocol

### 4.3.1. The Internet Network Layer

![image](https://user-images.githubusercontent.com/66773320/103330316-46550100-4aa4-11eb-93c2-2cbeb6d56e3e.png)

### 4.3.2. IP Datagram Format

![image](https://user-images.githubusercontent.com/66773320/103330341-6a184700-4aa4-11eb-869a-9e88c937eb28.png)

- IP 헤더는 최소 20Bytes의 크기를 갖는다.

	- 32bits는 4 * 8bits(1Byte) = 4Bytes며 최소 5줄이 있기 때문에 20Bytes 크기의 헤더 
	
	- data부분에 헤더를 포함한 TCP 혹은 UDP 세그먼트가 포함
	
	- 따라서 TCP 헤더는 20Bytes기 때문에 IP 데이터그램의 헤더는 최소 40Bytes
	
- IPv4 Datagram의 필드

	- ver : IP Protocol 버전 번호
	
	- head.len : 세그먼트의 헤더는 제외한 Header의 길이
	
	- type of service : IP 패킷 헤더 내 서비스 유형 및 혼잡 알림을 나타내는 8비트 필드
	
	- length : IP 데이터그램의 전체 길이(크기)
	
	- Identification : 패킷들로 분할하는 단편화와 재조합을 위한 16비트의 식별자, 각 조각이 동일한 데이터그램에 속하면 같은 일련번호를 공유함
	
	- flgs : 분열의 특성을 나타내는 플래그
	
	- fragment offset : 단편화, 재조합에 필요한 조각나기 전 원래의 데이터그램의 8바이트 단위의 위치
	
	- time to live(TTL) : 패킷의 수명, 최대 초기화 값은 255며 하나의 지점을 지나갈 때마다 1씩 감소하며 최종 목적지에 도착할 때 TTL 값은 0이 되어야 함
	
	- upper layer : 상위 계층의 프로토콜(TCP or UDP)
	
	- header checksum : 헤더에 대한 오류검사를 위한 필드, checksum은 전체 데이터그램을 합한 값의 1의 보수값을 저장
	
	- 32 bit source IP address : 출발지 IP주소(32bits)가 저장됨
	
	- 32 bit destination IP address : 목적지 IP주소(32bits)가 저장됨
	
	- data : TCP 혹은 UDP 세그먼트가 저장되어 있음
	
### 4.3.3. IP Fragmentation, Reassembly

- 네트워크 링크는 최대 전송 크기(Max Transfer Size, MTU)를 가지고 있음

- 크기가 큰 IP 하나의 데이터그램을 여러 데이터그램으로 단편화(Fragmentation)

- 최종 목적지에서 분할되었던 데이터그램의 IP해더를 참고하여 하나의 데이터그램으로 재조합(Reassembly)

- IP 헤더의 Identification 비트를 확인하여 재조합

![image](https://user-images.githubusercontent.com/66773320/103333341-02b4c400-4ab1-11eb-96d6-eb4e878c3bee.png)

```
- Datagram 크기가 4000byte고 MTU가 1500bytes일 때

- 하나의 데이터그램은 1500bytes보다 작은 여러개의 데이터그램으로 단편화

- origin data에서 header를 제외한 크기는 3980bytes

- 3980bytes 크기의 데이터를 크기가 MTU(1500bytes)인 데이터그램으로 단편화 하면 각각의 데이터그램에도 20bytes의 헤더가 필요

- 따라서 20bytes를 제외한 1480bytes크기의 데이터가 각각의 데이터그램에 단편화 됨

- 두 번째 데이터그램의 offset 필드에는 1480이 입력되며 데이터의 크기가 크기 때문에 8배 작은 값(185)가 저장됨
```

### 4.3.4. IP Addressing : Introduction

> host와 router간의 인터페이스

- IP 주소 : 32비트의 호스트와 라우터 인터페이스에 대한 식별자

- 인터페이스 : 호스트/라우터와 물리 링크 사이의 연결

	- 라우터는 일반적으로 여러개의 인터페이스를 갖는다.
	
	- 호스트는 일반적으로 하나 혹은 두개의 인터페이스를 갖게 됨
	
- IP 주소는 이러한 인터페이스와 연관됨

![image](https://user-images.githubusercontent.com/66773320/103339996-4960e900-4ac6-11eb-9a70-4daa360d5d75.png)

### 4.3.5. Subnets

- 하나의 네트워크가 분할되어 나누어진 작은 네트워크

- 네트워크를 분할 하는것을 서브네팅(subnettin)이라 하며 같은 서브넷 내에서는 라우터의 개입 없이 서로 물리적으로 접근 가능

- 서브넷을 결정하기 위해 호스트 혹은 라우터로 부터 각각의 인터페이스를 분리하고 분리된 네트워크 섬을 생성함

- 각각의 분할된 네트워크를 서브넷이라고 함

![image](https://user-images.githubusercontent.com/66773320/103340647-f0925000-4ac7-11eb-8aef-edc16891c7f4.png)

### 4.3.6. IP Addressing : CIDR

> CIDR : Classless InterDomain Routing

- 클래스 없는 도메인간 라우팅 기법

- Class는 CIDR가 나오기 전에 사용했던 네트워크 구분 체계로 클래스가 없다는 것은 네트워크 구분을 Class로 하지 않는다는 것

![image](https://user-images.githubusercontent.com/66773320/103341240-b4f88580-4ac9-11eb-8f4e-2a5652f33ca7.png)

- CIDR는 IP주소를 Subnet part와 host part로 구분

- subnet part로 각 네트워크 대역을 구분하고 host part로 각 서브넷 내에서 통신

- 구분된 네트워크 간 통신을 위한 주소체계

- x는 서브넷 부분을 뜻하는 비트의 갯수를 나타냄

![image](https://user-images.githubusercontent.com/66773320/103341597-b37b8d00-4aca-11eb-9846-5f37a9228eea.png)

### 4.3.7. IP Addresses : How to Get One?

> host가 IP 주소를 얻는 방법

- IP 주소를 할당하는 방식에는 두 가지 방법이 존재

	- 직접 system admin file에 입력하여 할당하는 방법
	
	- DHCP를 통해 동적으로 할당하는 방법
	
### 4.3.8. DHCP : Dynamic Host Configuration Protocol

> 호스트의 IP주소와 각종 TCP/IP 프로토콜의 기본 설정을 클라이언트에게 자동적으로 제공해주는 프로토콜

- 네트워크 서버로부터 네트워크에 접속할 때 동적으로 IP를 할당함

- DHCP 서버가 IP 주소를 영구적으로 호스트에 할당하는 것이 아니라 임대기간(Lease Time)동안 사용하도록 함

- DHCP를 사용하는 목적 

	- 네트워크에 연결되어 있을 때만 주소가 할당되기 때문에 다른 호스트가 재사용할 수 있다.
	
	- 짧은 시간 동안에 네트워크에 접속하고자 하는 모바일 사용자들을 지원함

- DHCP는 단지 서브넷의 IP주소 뿐 아니라 더 많은 것을 리턴함

	- 외부와 통신하기 위한 첫번째 홉 라우터 주소(default gateway IP)
	
	- DNS 서버 이름, IP 주소
	
	- 서브넷 마스크
	
### 4.3.9. DHCP Client-Server Scenario

![image](https://user-images.githubusercontent.com/66773320/103352926-df0e6f80-4aea-11eb-945d-c91e4d8f4458.png)

- DHCP Discover

	- Client의 DHCP 서버를 찾기 위한 시도 
	
	- 메세지 방향 : Client -> DHCP Server

	- 서버에 대한 정보가 없기 때문에 source IP를 0.0.0.0, Dest IP를 모든 목적지를 의미하는 255.255.255.255로 설정 (Broadcast 메세지)
	
- DHCP Offer

	- DHCP 서버의 Discover에 대한 응답
	
	- 메세지 방향 : DHCP Server -> Client
	
	- DHCP 서버의 존재와 단말에 할당할 IP 주소, DNS 서버 이름, 첫번째 홉 라우터 주소(default gateway), 서브넷 마스크를 담아서 전송

- DHCP Request

	- Client의 IP 요청
	
	- 메세지 방향 : Client -> DHCP Server
	
	- DHCP Offer에서 받은 IP 주소에 대한 임대 요청
	
- DHCP ACK 

	- DHCP Client의 IP 요청에 대한 응답
	
	- 메세지 방향 : DHCP Server -> Client
	
	- DHCP ACK 메세지를 받으면 할당 된 IP를 사용할 수 있게 됨
	
- Default Gateway란?

	- 동일 랜에 위치하지 않은 단말과 통신을 하기 위해 통과하는 첫번째 라우터
	
	- defalut gateway주소는 자신과 동일한 랜에 위치한 주소여야 함
	
	```
	IP 주소 : 1.1.1.10, default gateway : 1.1.1.1일 경우 동일한 LAN에 위치
	```

### 4.3.10. Hierarchical Addressing : Route Aggregation

![image](https://user-images.githubusercontent.com/66773320/103394869-651fca00-4b6e-11eb-8db5-585677225638.png)

- 서브넷이 IP주소를 받기 위해서 ISP의 주소 블록을 가져오게 됨

- 위에서는 ISP(Internet Service Provider)는 200.23.16.0/20으로 들어온 요청을 여러개의 기관들을 구분하기 위해 3개의 비트를 사용해서 총 8개의 기관식별(Subnets)에 사용

- ISP에 의해 계층적으로 IP주소가 관리되어 라우팅 정보를 효율적으로 알림

- ISP 내의 포워딩 테이블의 Entry 수는 서브넷을 대표하는 라우터의 개수

- ISP가 호스트 개개인의 IP를 관리하게 되면 포워딩 테이블에서 탐색하는 시간이 길어지기 때문에 대표 라우터의 IP주소로 테이블을 구성하여 탐색 시간을 줄임

![image](https://user-images.githubusercontent.com/66773320/103395167-1a9f4d00-4b70-11eb-8b59-dc38d14aa919.png)


### 4.3.11. NAT : Network Address Translation
> 네트워크 주소 변환

- IP 패킷의 TCP/UDP 포트 숫자와 소스 및 목적지의 IP 주소 등을 재기록하면서 라우터를 통해 네트워크 트래픽을 주고 받는 기술

- Subnet 내부에서 통신 할 때는 라우터를 거치지 않고 자신의 IP주소를 사용하여 통신한다.

- Subnet 외부와 통신 할 경우에는 다른 Subnet과 통신을 하게 되는데 다른 Subnet 내에서도 같은 IP주소가 존재할 수 있다.

- 따라서 서브넷 내에서 NAT 라우터를 거쳐갈 때 단 하나의 NAT IP주소로 변환되어 통신하게 됨

![image](https://user-images.githubusercontent.com/66773320/103398029-2a259280-4b7e-11eb-907b-a8f5be859a3f.png)

- NAT Router의 구현 과정

	- 외부로 나가는 데이터그램 : 호스트의 Source IP/Port는 NAT IP/Port로 매핑되고 데이터그램을 받은 client 혹은 server는 destination으로 해당 NAT IP/Port를 사용
	
	- NAT Router는 변환 테이블이 저장되어 있어 서브넷 내의 IP/Port를 NAT IP/Port로 매핑
	
	- 내부로 들어오는 데이터그램 : NAT Router에 저장되어 있는 NAT Table을 참고하여 NAT IP/Port를 호스트의 Source IP/Port로 매핑
	
- NAT IP 주소를 변환하는 과정에서 IP 패킷의 TCP/UDP 포트 숫자 및 IP 주소가 변경되는데 패킷에 변화가 생기면 체크섬도 다시 계산하는 과정이 필요하다.

- 이러한 네트워크 성능에 영향을 주는 요소들이 존재함에도 불구하고 하나의 공인 IP를 사용함으로써 라우터의 탐색 시간을 줄여 빠르게 통신한다.

- NAT Table은 서브넷 내에서 외부로 나갈 때 매핑 즉, 외부로 나가는 과정이 없으면 매핑이 되지 않는다.

	- 서브넷 내에서 실행되는 서버에 클라이언트가 연결을 요청하게 되면 내부 사설 IP는 서브넷 내에서만 유효하기 때문에 접근 불가능
	
	- 메세지를 받은 수신 측에서 응답 메세지를 보낼 때 송신 측에서 매핑된 NAT IP/Port를 destination으로 보내게 되는데 서브넷 내에서 실행되는 서버는 NAT 매핑 과정이 없이 사설 IP/Port를 사용 중이기 때문에 외부에서 접근 불가능

### 4.3.12. IPv6 : Motivation

- 32bits IP 표현방식(IPv4)이 곧 모두 할당 될 수 있기 때문에 새로운 IP 주소 체계가 필요

- 헤더의 형식이 프로세싱과 포워딩을 빠르게 하며 서비스 품질의 향상을 위한 헤더 형식을 변경함

- 헤더의 크기가 40bytes로 증가하였고 단편화를 허용하지 않는다.

### 4.3.13. IPv6 Datagram Format

> 128bit의 IP 주소체계

![image](https://user-images.githubusercontent.com/66773320/103399921-9a381680-4b86-11eb-9618-1ac49dcb45c9.png)

- IPv6 Datagram 필드

	- ver : IP버전을 저장하는 필드
	
	- priority : 혼잡되는 트래픽에 대해 페킷의 우선순위를 나타냄
	
	- payload len : data의 길이를 저장
	
	- next hdr(header) : 상위 계층의 프로토콜(IPv4의 upper layer와 유사)
	
	- hop limit : TTL(Time To Live)를 의미하며 최대 값은 255
	
	- source address : IPv6주소체계에서 128bit 송신 주소
	
	- destination address : IPv6주소체계에서 128bit 수신 주소
	
	- data : TCP 혹은 UDP 세그먼트가 저장되어 있음
	
- checksum 필드를 삭제하여 매번 라우터마다 체크섬 계산을 하는 비효율을 제거함

### 4.3.14. Transition from IPv4 to IPv6

- 모든 라우터가 동시에 업그레이드 되긴 어렵긴 때문에 실제로 보편화에 시간이 걸림

- 따라서 IPv4 데이터그램의 payload로 IPv6 데이터그램을 실어 IPv4 라우터를 이용하여 전송

![image](https://user-images.githubusercontent.com/66773320/103409319-9cf83300-4ba9-11eb-91e3-6ab31c91b027.png)

## 4.4. Generalized Forward and SDN

- 목적지 기반 포워딩

	- 목적지 IP만을 가지고 포워딩하는 방식
	
	- 제어 영역에 의해 생성된 포워딩 테이블(Forwarding Table)을 참고하여 패킷을 전달
	
- 일반화된 포워딩(Generalized Forward)

	- 각각의 라우터는 제어 영역에 의해 생성되는 플로우 테이블(Flow Table)을 참고하여 패킷을 전달
	
- Software Defined Network(SDN)

	- 소프트웨어로 네트워크 경로설정 및 제어, 운용 관리를 처리할 수 있음
	
	- 하나의 물리 네트워크 환경에서 다수의 가상 네트워크 환경 구축 가능
	
	- 네트워크 장비가 단순히 패킷만 전달하고 소프트웨어 제어기를 프로그래밍 하여 데이터의 흐름을 제어
	

### 4.4.1. OpenFlow Data Plane Abstraction

- OpenFlow : SDN을 구현하기 위해 처음으로 제정된 표준 인터페이스

- 패킷 제어 기능(control plane)과 전달 기능(data plane)을 분리하여 프로그래밍 기반 네트워크 제어

- 제어 및 데이터 평면을 __범용서버__에 설치하여 소프트웨어로 구현

- controller가 스위치에 명령, 스위치는 명령에 따른 패킷 전송, 수정, 폐기 처리

- L2 스위치에 OpenFlow 프로토콜 펌웨어를 추가하고 컨트롤러는 소프트웨어로 구현된다.

- OpenFlow 스위치는 다수의 플로우 테이블 및 다수의 플로우 엔트리로 구성

- 기본 동작

	- 패킷 발생 시 플로우 테이블에 해당 패킷의 정보가 있는지 확인
	
	- 플로우 테이블에 존재하면 바로 처리
	
	- 존재하지 않으면 controller에게 해당 패킷에 대한 정보 요청
	
	- 스위치로부터 제어정보를 요청받은 컨트롤러는 내부에 존재하는 패킷 제어정보 확인, 결과 전달
	
	- 받은 데이터를 플로우 테이블에 저장하여 동일 패킷 발생 시 이를 활용하여 처리
	
### 참고

- https://movefast.tistory.com/52?category=765942

- https://kim-dragon.tistory.com/9

- https://www.netmanias.com/ko/post/blog/5403/ip-ip-routing-network-protocol/subnet-mask-and-default-gateway

- http://itwiki.kr/w/%EC%98%A4%ED%94%88%ED%94%8C%EB%A1%9C%EC%9A%B0
