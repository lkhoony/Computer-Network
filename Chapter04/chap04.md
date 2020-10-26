# Chapter 4. Network Layer
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

- 네트워크 계층이 모든 호스트와 라우터에서의 프로토콜을 지정

- 라우터는 해당 라우터를 통과하는 모든 IP 데이터그램에 있는 헤더 필드를 검사

### 4.1.2. Two Key Network Layer Functions
> 라우팅이 출발지에서 목적지까지 여행을 계획하는 과정이라면 포워딩은 여행 준간의 하나의 지점을 지나가는 과정

#### Forwarding

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


