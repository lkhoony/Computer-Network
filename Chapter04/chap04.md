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
