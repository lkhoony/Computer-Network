# Chapter 6. The Link Layer and LANs

## Outline
- Introduction, Service
- Error Detection, Correction
- Multiple Access Protocols
- LANs
- Link Virtualization
- Data Center Network
- A Day in the Life of a Web Request

## 6.1. Introduction, Service

### 6.1.1. Link Layer : Introduction

- 호스트와 라우터들 : 노드

- 인접 노드와 통신을 위해 연결된 경로인 통신 채널 : 링크

	- 유선 링크
	
	- 무선 링크
	
	- LANs
	
- Link Layer 계층은 프레임 단위로 데이터그램을 캡슐화 하여 통신

- Link Layer는 데이터그램을 링크를 통해 물리적으로 인접한 노드로 전송을 담당 

### 6.1.2. Link Layer : Context

- 데이터그램은 서로 다른 링크에서 서로 다른 프로토콜로 전송됨

	- 첫 번째 링크는 이더넷, 마지막 링크는 802.11일 수 있음
	
- 각각의 프로토콜은 다른 서비스를 제공함
	
	- 링크 상에서 신뢰적인 데이터 전달을 제공하거나 그렇지 않을 수 있음

### 6.1.3. Link Layer Services

- 프레임화, 링크 접속(Framing, Link Access)

	- 데이터그램을 헤더와 트레일러를 추가해서 데이터그램으로 캡슐화
	
	- 공유 매체인 경우 채널로 접속
	
	- 송신지와 수신지를 확인하기 위해서 MAC 주소를 사용
	
		- IP 주소와는 다른 주소
		
- 신뢰적 전달(Reliable Deliver between Adjacent nodes)

	- 비트 오류가 낮은 링크에서는 드물게 사용
	
	- 무선 링크와 같이 높은 비트 오류가 있을 때 사용
	
- 흐름 제어(Flow Control)

	- 인접한 노드 사이에서 송신과 수신의 속도를 맞추는 흐름 제어
	
- 오류 검출(Error Detection)

	- 에러는 신호의 감쇠 및 잡음의 원인이 된다.
	
	- 수신 노드는 오류를 검출하여 재전송 신호를 보내거나 프레임을 폐기
	
- 오류 정정(Error Correction)

	- 수신 노드는 오류름 확인하여 재전송 신호 없이 오류를 정정
	
- 반이중과 전이중(Half Duplex and Full Duplex)

	- 전이중 방식 (Full Duplex)
	
		- 하나의 전송 선로에서 데이터가 동시에 양쪽 방향으로 전송 가능
		
		- 양방향 방식
		
	- 반이중 방식 (Half Duplex)
	
		- 양방향으로 데이터가 전송될 수는 있으나 동시에 전송하는 것은 불가능
		
		- 단뱡향 방식

### 6.1.4. Where is the Link Layer Implemented?

- 각각의 모든 호스트에 구현되어있음

- 네트워크 인터페이스 카드(NIC, Network Interface Card)라고 불리는 어댑터 혹은 칩에 구현되어 있음

- 호스트 시스템의 버스에 연결 

- 하드웨어, 소프트웨어 그리고 펌웨어의 결합으로 구성됨

### 6.1.5. Adaptors Communicating

- 전송 측 

	- 데이터그램을 프레임으로 캡슐화
	
	- 에러 검출 비트, rdt, 흐름 제어를 추가함
	
- 수신 측

	- 에러와 rdt, 흐름 제어를 확인
	
	- 데이터그램을 추출하여 상위 계층으로 전달

## 6.2. Error Detection, Correction

### 6.2.1. Error Detection, Correction

![image](https://user-images.githubusercontent.com/66773320/103647386-92b9a880-4f9e-11eb-8b8c-82555da21807.png)

- EDC(Error Detection and Correction bits) : 에러 검출 및 정정 비트

- D(Data protected by error checking, may include header fields) : 에러 검출로 부터 보호되는 데이터, 헤더 필드를 포함할 수 있음

- 에러 검출은 100% 검출은 아님
	
	- 에러를 가끔 놓칠 수 있지만 신뢰할 만 할 수준임
	
	- EDC 필드가 커야 에러 검출과 정정에 효과적임
	
### 6.2.2. Parity Checking

![image](https://user-images.githubusercontent.com/66773320/103647926-67838900-4f9f-11eb-946d-115b341309fe.png)

- 단일 비트 패리티

	- 하나의 비트에 대한 오류만 검출 가능
	
- 2차원 비트 패리티
	
	- 단일 비트 오류를 검출하고 정정 

### 6.2.3. Internet Checksum

- 오직 전송 계층에서 세그먼트의 에러를 검출하는데에 사용

- 송신 측

	- 세그먼트의 값들을 16비트의 정수의 열로 간주
	
	- 모든 값들을 더한 후 1의 보수를 적용
	
	- 이를 checksum 필드에 입력
	
- 수신 측

	- 수신 한 세그먼트로 부터 체크섬을 계산
	
	- 계산 된 체크섬 값과 수신 된 세그먼트의 체크섬 필드값과 비교
	
		- 값이 다르면 오류 검출, 같으면 오류 없음
		
## 6.3. Mutiple Access Protocols

### 6.3.1. Multiple Access Links, Protocols
> 다중 접속 링크와 프로토콜

- 링크를 공유하는 경우 원활한 통신을 위해 전송 규약이 필요

- 허브를 통해 구성된 LAN의 경우나 무선통신의 경우 매체를 공유하는 경우에 해당됨

- 링크에는 두가지 타입이 존재

	- Point-to-Point
	
		- 다이얼업 접속의 PPP
		
		- 이더넷 스위치와 호스트 간의 point-to-point 연결
		
	- Broadcast(shared wire or medium)
		
		- 이전의 이더넷
		
		- upstram HFC
		
		- 802.11 wireless LAN
		
### 6.3.2. Multiple Access Protocols

- 단일 공유 브로드캐스트 채널을 이용하여 두 개 이상의 노드가 동시에 데이터를 전송함

	- 동시에 두 개 이상의 노드가 신호를 보내면 충돌이 발생하게 됨
	
- 다중 접속 프로토콜

	- 노드들이 언제 전송을 해야하는지를 결정하는것과 같이 채널을 어떻게 공유할지에 대한 알고리즘
	
	- 채널 공유를 위한 통신에서도 채널을 사용하며 별도의 조정을 위한 채널은 존재하지 않음
	
### 6.3.3. An Ideal Multipole Access Protocol

- R bps의 브로드캐스트 채널이 주어졌다고 할 때 이상적인 프로토콜

	- 하나의 노드가 채널로 전송하고자 할 때 R bps의 처리율로 전송
	
	- M개의 노드가 채널로 전송하고자 할 때 각각의 노드들은 평균 R/M bps로 전송
	
	- 완전히 분산되어있는 프로토콜(decentralized)
	
		- 전송에 대한 조정을 담당하는 특별한 노드가 없음
		
		- 클럭과 슬롯의 동기화가 없음
	
	- 단순한 프로토콜
	
### 6.3.4. MAC Protocols : Taxonomy

- 세 가지 주요 MAC Protocols

	- 채널 분할(Channel Partitioning)
	
		- 채널들을 작은 단위로 나누어 각각을 사용자에게 할당
	
	- 임의 접근(Random Access)
	
		- 채널을 나누지 않고 충돌을 허용
		
		- 충돌으로 부터 회복하는 방법에 대해 기술
		
	- 순번(Taking Turns)
	
		- 노드들은 자신의 전송 순번에 전송하지만 많은 사용자가 전송할 경우 시간이 오래 걸릴 수 있음

### 6.3.5. Channel Partitioning MAC Protocols : TDMA

> TDMA : Time Division Multiple Access

- 노드들은 채널에 순환식으로 접근

- 각 노드들은 고정 크기(패킷 전송 시간)의 slot을 각 순환마다 할당 받음

- 사용되지 않는 slot은 idle 상태로 유지

![image](https://user-images.githubusercontent.com/66773320/103662746-a8859880-4fb3-11eb-979d-8d5e4fea33e3.png)   		

### 6.3.6. Channel Partitioning MAC Protocols : FDMA

> FDMA : Frequency Division Multiple Access

- 채널을 주파수 밴드로 분할

- 각 노드는 고정된 주파수 대역을 할당 받음

- 각 주파수 대역이 사용되지 않는 시간에는 idle 상태를 유지

![image](https://user-images.githubusercontent.com/66773320/103663279-46796300-4fb4-11eb-8dfa-1eb02944001d.png)

### 6.3.7. Random Access Protocols

> 임의 접근 프로토콜

- 노드가 패킷을 전송하고자 할 때 R bps로 채널의 모든 대역을 사용하고 우선순위를 조정하는 노드가 없음

- 두개 이상의 노드가 전송하고자 할 때 충돌이 발생

- 임의 접근 프로토콜은 충돌을 감지하고 어떻게 회복 하는지를 구체화 한다.

	- 예 : 재전송을 연기함으로써 충돌을 방지
	
- 임의 접근 MAC 프로토콜의 예시

	- slotted ALOHA
	
	- ALOHA
	
	- CSMA, CSMA/CD, CSMA/CA
	
### 6.3.8. Slotted ALOHA

- 전제조건

	- 모든 프레임이 같은 크기를 가지고 있음(L bits)
	
	- 하나의 프레임을 전송하는 시간인 slot으로 시간을 같은 크기로 분할(L/R sec)
	
	- 각 노드는 slot이 시작될 때만 프레임을 전송할 수 있음
	
	- 각 노드는 슬롯의 시작을 확인할 수 있도록 동기화
	
	- 만약 두 개 이상의 노드가 같은 슬롯에 전송을 시도하면 모든 노드가 충돌이 발생했음을 인지
	
- 동작

	- 각 노드는 전송할 프레임이 생기면 다음 slot에 전송하게 됨
	
		- 충돌이 발생하지 않으면 다음 slot에 프레임을 전송
		
		- 충돌이 발생했다하면 p의 확률로 다음 slot에서 성공할 때 까지 재전송
		
![image](https://user-images.githubusercontent.com/66773320/103732525-e376e300-502a-11eb-9183-3b296e0bf71f.png)

- 장점

	- 하나의 활성 노드가 최대 전송률인 R bits로 전송이 계속 가능하다.
	
	- 충돌 감지 및 재전송을 노드 각자가 결정하고 노드는 슬롯의 시간을 동기화만 필요
	
- 단점

	- 충돌이 발생하고 slot 시간을 낭비할 수 있음
	
	- idle한 slot이 발생할 수 있음
	
	- 노드들은 패킷을 전송하는 시간 보다 충동를 감지하는 시간이 더 오래걸릴 가능성이 있다.
	
	- clock 동기화가 필요하다.
	
- 전송효율

	- 각각의 slot에서 프레임을 전송 할 확률을 p라고 설정
	
	- N개의 노드가 있을 경우 각 슬롯에서 한 노드가 전송에 성공할 확률
	
		- p * (1-p) ^ (N-1)
		
		- 하나의 노드만 전송, 다른 N-1개의 노드는 전송하지 않음
		
	- 임의의 한 노드가 전송에 성공할 확률
	
		- N * p * (1-p) ^ (N-1)
		
	- 최대 효율은 N * p * (1-p) ^ (N-1)를 최대화 시키는 P*를 구하는 것
	
	- N에 극한값을 취하게 되면 최대 효율은 1/e = 37%로 구해짐
	
	- 많은 노드가 전송할 때 37% 슬롯만이 실제 전송에 사용됨
	
### 6.3.9. Pure(unslotted) ALOHA

- 슬롯과 동기화가 없고 완전히 노드로 분산된 프로토콜

- 프레임이 도착하는 즉시 전송을 수행함

- 따라서 충돌이 발생할 확률이 높아짐

	- t0에 전송된 프레임은 [t0-1,t0+1] 시간대에 전송된 다른 프레임과 충돌이 발생함
	
- 주어진 노드의 전송 성공 확률 = p(노드 전송) * p([t0-1,t0]에 다른 노드들이 전송하지 않음) * p([t0,t0+1]에 다른 노드들이 전송하지 않음)

- 위의 확률은 p*(1-p) ^ (2*(N-1))

- N에 극한값을 취하면 1/(2e) = 18%로 낭비가 slotted ALOHA보다 많아짐

### 6.3.10. CSMA(Carrier Sense Multiple Access)

> 각 노드들이 프레임을 전송하려고 공유 매체(반송파)에 접근하기 전에 먼저 매체가 사용중인지 확인(Carrier Sensing)하여 다중 접속하는 방식

- 반송파 감지 다중 접속 및 충돌 탐지

- 전송 전에 채널을 사용하고 있는지를 감지

- 채널이 idle 상태일 경우 프레임을 전송하고 사용 중이라면 전송을 연기

- Listen Before Talk(LBT) 형식

	- 채널이 비어있다면 자신의 패킷을 송출 전송
	
	- 사용중이면 채널이 빌 때 까지 대기
	
### 6.3.11. CSMA Collisions

- CSMA는 충돌의 가능성을 줄일 수 있으나 완전히 없앨 수는 없다.

	- 지국이 매체를 감지하여 매체가 idle한 것을 감지해도 지국이 전송한 프레임의 첫 번째 비트가 아직 도달중일 수 있음
	
	- 프레임이 전송(전파)되는 시간차 때문에 idle한 것을 감지하고 나서 프레임을 전송하는 경우 다른 곳에서 보낸 프레임이 도착하여 두 프레임이 충돌, 손상될 수 있음
	
		- 전파지연(propagation delay)의 영향
		
		- CSMA의 한계
		
	
### 6.3.12. CSMA/CD (Collision Detection)

- CSMA 방식에 충돌을 처리하는 절차를 더하는 것

- 유선링크의 경우 충돌을 확인할 수 있기 때문에 사용가능한 방식

- 프레임을 전송함과 동시에 두개의 다른 포트를 이용하여 충돌이 발생하는지 감시

- 프레임이 목적지에 도달할 시간 이전에 다른 프레임의 비트가 발견되면 충돌이 일어난 것으로 판단
 
- 빠른 시간안에 충돌을 검출하고 충돌 시 전송을 중단하여 채널 낭비를 줄임

- 유선 랜에서는 신호의 세기를 측정하고 전송된 신호와 수신된 신호의 세기를 비교하여 검출이 용이

![image](https://user-images.githubusercontent.com/66773320/103745473-e382dd00-5042-11eb-83b8-7f6bc04fa299.png)

### 6.3.13. Ethernet CSMA/CD Algorithm

- NIC(Network Interface Card)가 상위 계층인 네트워크 계층으로 부터 데이터그램을 받아 프레임을 생성

- NIC은 채널이 idle 상태라면 전송을 하고 그렇지 않다면 idle 상태가 될 때 까지 대기한 후 전송

- 프레임을 전송하는 도중 다른 노드의 전송이 감지되지 않았다면 프레임 전송을 완료함

- 만약 다른 노드의 전송이 감지되었다면 프레임 전송을 중단하고 잼 신호(충돌 발생 신호)를 전송

- 전송 중단 후 NIC는 이진 지수적 백오프(Binary Exponential Backoff) 단계 실행

	- M번의 충돌 이후에 NIC는 {0,1,2,...2^(M-1)} 중 K를 선택하여 K*512 bit 이후 Step2를 실행함
	
	- back off 인터벌이 길수록 충돌이 증가

### 6.3.14. CSMA/CD Efficiency

- Tprop : LAN안의 두 노드 간 최대 전파 지연 시간

	- 전파지연 : 물리적인 통신선로를 통해 전기적인 신호가 송신자로부터 수신자까지 도달하는 시간
	
- Ttrans : 최대 크기의 프레임을 전송하는데 걸리는 전송 시간
	
	- 전송지연 : 모든 비트들을 링크로 밀어내는 시간
	
![image](https://user-images.githubusercontent.com/66773320/103750000-90f8ef00-5049-11eb-8d48-9020ca20d27a.png)

- Tprop이 0으로 수렴, Ttrans가 무한대로 발산할 경우 효율성은 1로 수렴

### 6.3.15. Taking Turns MAC Protocols

- 풀링(Polling)

	- 마스터(master) 노드는 순서대로 슬레이브(slave)노드들에게 데이터를 전송하도록 풀링
	
	- 단점
		
		- 풀링 오버헤드가 발생하고 master 노드에서 문제가 발생할 시 채널 전체가 동작하지 않음
	
- 토근 전달(token passing)

	- 토큰이 순서대로 노드들에게 전달되도록 제어
	
	- 전송할 데이터가 있을 경우만 토큰을 잡고 그렇지 않으면 다음 노드로 전달
	
	- 단점
	
		- 토큰을 잡은 노드에서 문제가 발생하면 채널 전체가 동작하지 않음
		
## 6.4. LANs
> LAN : Local Area Network

### 6.4.1. MAC Addresses and ARP

- 32 bit IP 주소

	- 네트워크 계층에서 인터페이스에 사용되는 주소 형식
	
- MAC 주소(LAN 주소, 물리 주소, 이더넷 주소)

	- 프레임을 한 인터페이스에서 물리적으로 연결된 다른 인터페이스로 전송하는데에 사용되는 주소
	
	- 48 bit의 MAC 주소로써 NIC 혹은 소프트웨어의 세팅으로 정의됨

### 6.4.2. LAN Addresses and ARP

- LAN 내에서 각각의 어답터들은 고유의 LAN 주소를 가지고 있음

![image](https://user-images.githubusercontent.com/66773320/103755571-627f1200-5051-11eb-90be-7e74db382881.png)

- MAC 주소 할당은 IEEE에서 관리

- MAC 주소는 어댑터의 위치에 관계없이 고정되어 있는 평면 주소 구조(flat address)

	- IP주소는 노드가 부착된 IP 서브넷에 의존되어 이동할 수 없음
	
### 6.4.3. ARP : Address Resolution Protocol

- 네트워크 계층의 IP 주소를 링크 계층의 MAC주소로 변환하는 과정이 필요

- LAN 상의 IP 주소를 갖는 노드는 ARP 테이블을 가짐

	- LAN 안의 노드에서 IP주소와 MAC 주소를 매핑하는 테이블
	
	- TTL(Time to Live) : 테이블 내에서 각 매핑 엔트리가 삭제되는 시간
	
		- 보통 20분의 시간동안 엔트리를 유지
		
### 6.4.4. ARP Protocol : Same LAN

- 같은 LAN에서 A노드가 B로 데이터그램을 전송하고자 할 때

	- B의 MAC 주소가 A의 ARP 테이블에 없을 경우
	
- A는 B의 IP주소가 포함된 LAN 내의 모든 노드들에게 ARP 요청문을 브로드캐스트

	- 수신지의 MAC 주소를 FF-FF-FF-FF-FF-FF로 전송(모든 노드로 브로드 캐스트)
	
- B는 ARP 요청문을 수신하여 A에게 MAC 주소를 응답

	- 프레임은 A의 MAC 주소로 유니캐스트
	
- A는 IP-MAC 매핑 정보를 time out이 될 때 까지 ARP 테이블에 저장

- ARP는 plug and play 방식으로 작동

	- 각 노드들은 네트워크 관리자의 개입 없이 ARP 테이블을 갱신
	
### 6.4.5. Addressing : Routing to Another LAN

![image](https://user-images.githubusercontent.com/66773320/103771506-3a9ca800-506b-11eb-935b-48f9fe70eacf.png)

- 데이터그램을 A로 부터 R을 거쳐 B로 전송하는 과정

- A는 B의 IP 주소를 알고 있다고 가정

- A는 첫번째 홉 라우터인 R의 IP 주소를 알고 있다고 가정

- A는 R의 MAC 주소를 알고 있다고 가정

![image](https://user-images.githubusercontent.com/66773320/103772318-aaf7f900-506c-11eb-812c-fbdf41a866cc.png)

- A 노드는 송신지를 A, 수신지를 B로 하는 IP 데이터그램을 생성

- 링크 계층에서는 해당 IP 데이터그램을 포함하고 도착지의 MAC 주소를 R의 주소로 하는 프레임을 생성하여 전송

![image](https://user-images.githubusercontent.com/66773320/103772756-5b65fd00-506d-11eb-890a-13b30808a9ad.png)

- R은 프레임을 수신하여 데이터그램을 네트워크 계층으로 전달

- R은 A의 IP주소를 송신지, B의 IP주소를 수신지로 하는 데이터그램을 포워딩

- R의 링크 계층에서는 해당 데이터그램을 포함하고 도착지의 MAC 주소를 B의 주소로 하는 프레임을 생성하여 전송

### 6.4.6. Ethernet

- 오늘날 가장 많이 사용되는 LAN 기술

### 6.4.7. Ethernet : Physical Topology

![image](https://user-images.githubusercontent.com/66773320/103775530-85212300-5071-11eb-84d3-cf09458c9da8.png)

- bus(버스)

	- 90년대에 많이 사용된 방식
	
	- 채널을 공유하기 때문에 노드 간 충돌이 발생
	
- star(스타 네트워크)

	- 가장 널리 사용되는 물리적 토폴로지
	
	- 중앙의 연결지점에 허브, 스위치, 라우터 같은 장비를 배치
	
	- 각각의 노드는 분리된 이더넷 프로토콜로 작동하여 충돌이 발생하지 않음
	
### 6.4.8. Ethernet Frame Structure

- 전송 측 어댑터는 IP 데이터그램을 이더넷 프레임으로 캡슐화

![image](https://user-images.githubusercontent.com/66773320/103775702-cc0f1880-5071-11eb-940e-dd5455dd9cf3.png)

- 프리앰블 (preamble)

	- 이더넷 MAC 프레임의 첫 번째 필드로써 0과 1을 반복하는 7바이트를 포함
	
	- 수신 측에 프레임이 도착하는 것을 알려주며 송신 측과 수신 측의 클록을 동기화하기위해 사용
	
- 주소 (address)

	- 송신지와 수신지의 MAC 주소(6바이트)
	
	- 어댑터의 주소가 목적지 주소와 같으면 이를 네트워크 계층으로 전달하고 그렇지 않으면 프레임을 버림
	
- 타입 (type)

	- 상위 계층 프로토콜을 표시
	
- CRC (cyclic redundancy check)

	- 수신 측에서 프레임 오류 검출
	
	- 오류 발생 시 프레임을 드랍함
	
### 6.4.9. Ethernet : Unreliable, Connectionless

- 비연결성 (Connectionless)

	- 송신 측과 수신 측의 NIC간의 핸드셰이킹 과정이 없음
	
- 비신뢰성 (Unreliable)

	- NIC는 ack 혹은 nack 메세지를 주고받지 않음
	
	- 상위 계층에서 TCP 프로토콜을 사용하면 손실된 데이터는 복구할 수 있지만 그렇지 않으면 손실이 발생
	
- 다중 이더넷 접속 프로토콜 

	- 이진 백오프를 갖는 unslotted CSMA/CD

### 6.4.10. 802.3 Ethernet Standards : Link & Physical Layer

- 여러 상이한 이더넷 표준이 존재

![image](https://user-images.githubusercontent.com/66773320/103777917-ed253880-5074-11eb-9c2e-17d23be73295.png)

### 6.4.11. Ethernet Switch

- 링크 계층 장치로 하드웨어 적으로 발달

	- 이더넷 프레임을 포워딩 및 저장
	
	- 입력되는 프레임의 MAC 주소를 판단하여 하나 혹은 그 이상의 링크로 포워딩
	
		- CSMA/CD 프로토콜 사용
		
- 호스트는 스위치의 존재에 대해 알지 못함(투명성, transparent)

- 스위치는 설정이 필요 없이 사용 가능(plug and play, self-learning)

### 6.4.12. Switch : Multiple Simultaneous Transmissions

![image](https://user-images.githubusercontent.com/66773320/103794671-5a8e9480-5088-11eb-8ce0-5693bc8daa72.png)

- 호스트는 스위치와 직접적으로 연결 됨

- 각 입력 링크에서 이더넷 프로토콜이 사용되지만 충돌은 없음

	- 전이중(full duplex)방식
	
	- 각각의 링크가 자신의 충돌 도메인이 됨
	
- 스위칭(switching)

	- A에서 A', B에서 B'로 충돌없이 동시에 전송이 가능하다.

### 6.4.13. Switch Forwarding Table

- 스위치가 interface 4를 통해서 A', interface 5를 통해서 B'로 도달한다는 것을 어떻게 아는가?

	- 각각의 스위치는 (MAC 주소, interface, time stamp)의 엔트리로 이루어진 테이블을 가지고 있음

### 6.4.14. Swtich : Self-Learning

- 어떻게 엔트리를 테이블에 추가하고 유지하는가?

	- 스위치는 어떤 인터페이스를 통해 어떤 호스트로 도달할 수 있는지를 스스로 학습한다.
	
		- 스위치로 프레임이 도착했을 때 LAN 세그먼트가 들어오는 송신지의 위치를 학습
		
		- 스위치 테이블에 송신지/위치 쌍을 기록

### 6.4.15. Switch : Frame Filtering/Forwarding

- 프레임이 스위치에 도착하게 되면

	- 송신 호스트의 MAC 주소와 입력 링크를 기록
	
	- 프레임의 목적지 MAC 주소를 스위치 테이블에서 검색

	- 목적지의 MAC 주소가 엔트리에 있다면
	
		- 세그먼트의 목적지가 도착한 프레임의 목적지와 같다면 프레임을 drop
		
		- 그렇지 않다면 엔트리에 등록된 interface로 포워딩

	- 그렇지 않다면 도착한 입력 interface 이외의 모든 interface로 flood
	
### 6.4.16. Self-Learning, Forwarding : Example

- 프레임의 도착지가 A'일 때 테이블에 등록되어있지 않을 경우

	- 모든 목적지로 브로드캐스트하여 A'에 해당하는 링크를 학습
	
### 6.4.17. Interconnecting Switches

![image](https://user-images.githubusercontent.com/66773320/103852035-6d3bb480-50ee-11eb-9df9-5a155c52116e.png)

- 자가 학습 스위치들은 상호 연결이 가능함

	- 단일 스위치 시스템과 동일하게 스위치 간의 자가 학습 가능
	
### 6.4.18. Institutional Network

![image](https://user-images.githubusercontent.com/66773320/103854892-d292a400-50f4-11eb-87fc-36def996f115.png)

### 6.4.19. Switches vs. Routers

- 두 장치 모두 패킷을 저장 및 포워딩 하는 장치

	- 라우터 
	
		- 네트워크 계층의 장비
		
		- 네트워크 계층의 해더를 결정
		
	- 스위치
	
		- 링크 계층의 장비
		
		- 링크 계층의 해더를 결정
		
- 두 장비 모두 포워딩 테이블을 가지고 있음

	- 라우터 
	
		- 라우팅 알고리즘과 IP 주소를 활용하여 테이블을 계산
		
	- 스위치
	
		- MAC주소를 사용하여 flooding으로 직접 학습하여 포워딩 테이블 계산 
		
### 6.4.20. VLANs : Motivation
> Virtual LAN : 논리적으로 분할된 스위치

- VLAN이 나눠져있지 않으면 브로드케스트 도메인이 커지므로 불필요한 브로드케스트 프레임 및 받지 말아야 할 프레임 증가

	- 보안 이슈가 생김
	
### 6.4.21. VLANs

- 스위치를 하나의 물리적 LAN 구조에서 여러개의 논리적 LAN으로 분할하여 설정하는 방법

![image](https://user-images.githubusercontent.com/66773320/103856033-4f268200-50f7-11eb-8384-29252ac22aab.png)

- Port-based VLAN

	- traffic isolation
	
		- 논리적으로 나누어진 스위치 포트 내에서만 프레임을 주고 받을 수 있음
		
		- 스위치 포트 뿐 아니라 MAC 주소를 기준으로 VLAN을 나눌 수 있음
		
		- 다른 VLAN과 통신할 경우 라우터를 거쳐야 함
	
	- dynamic membership
	
		- VLAN 내에서 포트는 동적으로 할당됨
		
	- forwarding between VLANS
	
		- 논리적으로 나누어진 스위치간 통신 시 라우팅으로 포워딩 됨
		
### 6.4.22. VLANS Spanning Multiple Switches

![image](https://user-images.githubusercontent.com/66773320/103857218-66666f00-50f9-11eb-8f5a-5a98741a5e29.png)

- 하나의 포트에 다수의 VLAN 트래픽이 통과할 수 있는 방식

- 일반적으로 서로 다른 스위치에 동일 VLAN이 존재하는 경우 사용

- trunk 포트를 사용하는 경우에도 스위치끼리 통신하기 위해서는 동일 VLAN 사이에서만 가능

- 서로 다른 VLAN 사이에서 통신을 하기 위해서는 라우팅 과정 필요

- IEEE 802.1Q VLAN frame format을 주로 사용

![image](https://user-images.githubusercontent.com/66773320/103857450-df65c680-50f9-11eb-810f-a3ed279cc1c6.png)

## 6.5. Link Virtualization MPLS

### 6.5.1. Multiprotocol Label Swtiching

- 고정된 길이의 라벨을 사용하여 IP 데이터그램 포워딩을 빠르게 하기 위하여 개발

- 기존 네트워크 환경에서 라우터는 네트워크 계층의 헤더만 보고 각 패킷의 포워딩을 실행함

- 이러한 작업은 모든 홉(hop)에서 실행되기 때문에 기존의 방식에서는 전송 속도가 느려짐

- MPLS은 미리 결정된 고효율 경로를 설정하여 패킷이 처음 네트워크로 진입할 때 특정 FEC(Forwarding Equivalance Class)에 할당됨

	- 이는 패킷에 label로 표시

![image](https://user-images.githubusercontent.com/66773320/103859668-ca8b3200-50fd-11eb-967a-2bb81d202b7d.png)

## 6.6. A Day in the Life of a Web Request

![image](https://user-images.githubusercontent.com/66773320/103861089-2ce53200-5100-11eb-88c4-b69b2148aa6d.png)

- 연결된 호스트(labtop)는 DHCP를 사용하여 호스트의 고유의 IP, first-hop 라우터, DNS 서버의 주소를 받아와야 함

	- DHCP 요청은 UDP로 캡슐화되고 이는 IP, 802.3 Ethernet으로 순차적으로 캡슐화
	
	- 이더넷 프레임은 LAN 내에서 실행중인 DHCP 서버가 있는지 브로드캐스트 메세지를 보냄
	
	- DHCP 서버가 실행되는 라우터는 해당 메세지를 받아 IP, UDP, DHCP 역다중화를 순차적으로 수행
	
	- DHCP 서버는 호스트(labtop)의 IP주소, first-hop 라우터의 IP 주소, DNS의 이름 및 IP주소를 캡슐화하여 LAN을 통해 전송
	
		- 스위치는 스스로 학습하여 테이블에 엔터티를 등록
		
	- 호스트는 아래의 정보가 포함된 DHCP ACK 메세지를 수신
	
		- 호스트(labtop)의 IP 주소
		
		- DNS의 이름 및 IP 주소
		
		- first-hop 라우터의 IP 주소

![image](https://user-images.githubusercontent.com/66773320/103862470-85b5ca00-5102-11eb-8b29-f887cae00d65.png)

- HTTP 요청을 보내기 전에 DNS에 www.google.com에 해당하는 IP 주소가 필요

	- DNS 쿼리를 생성하여 UDP, IP, Ethernet 순서로 순차적으로 캡슐화
	
	- 프레임을 라우터로 전송하기 위해서 라우터의 MAC 주소를 질의해야함
	
	- ARP 질의가 브로드캐스트 되고 라우터는 이를 받아서 MAC 주소를 응답
	
	- 호스트(labtop)은 first-hop 라우터의 MAC주소에 대한 정보를 얻었기 때문에 DNS 요청을 보낼 수 있게 됨
	
![image](https://user-images.githubusercontent.com/66773320/103863050-7b480000-5103-11eb-8f99-48e9f0770562.png)

- IP 데이터그램이 포함된 DNS 요청이 LAN 스위치에 의해서 호스트(labtop)의 first-hop 라우터로 전송

- campus network로 부터 comcast network로 IP 데이터그램을 포워딩

- RIP, OSPF, IS-IS, BGP 등의 라우팅 프로토콜을 사용하여 DNS 서버로 라우팅

- DNS 서버에서는 DNS 요청 메세지를 역다중화

- 호스트(labtop)에게 IP 주소를 응답

![image](https://user-images.githubusercontent.com/66773320/103863308-f6a9b180-5103-11eb-93ac-8152997552e0.png)

- 호스트(labtop)는 HTTP 요청을 전송하기 위해 웹 서버와의 TCP 소켓을 실행

- 3-way-handshake

	- 호스트(labtop)는 TCP SYN 세그먼트를 전송
	
	- 웹 서버는 SYNACK 메세지를 전송
	
- TCP 연결 수립

![image](https://user-images.githubusercontent.com/66773320/103863674-9e26e400-5104-11eb-890c-82351c737005.png)

- HTTP 요청이 TCP 소켓을 통하여 전송됨

- HTTP 요청을 포함하는 IP 데이터그램이 www.google.com으로 라우팅

- 웹 서버는 해당 HTTP 요청에 대한 웹 페이지를 포함하여 응답

### 참고

- https://m.blog.naver.com/three_letter/220557368529

- https://itstory.tk/entry/CSMA-CSMACD-CSMACA-%EB%9E%80