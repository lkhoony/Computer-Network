# Chapter 3. Transport Layer
> - 전송 계층에서 제공하는 서비스 유형에 대한 이해
> - 전송 계층에서 사용되는 프로토콜에 대한 학습

## Outline
- Transprot Layer Services
- Multiplexing and Demultiplexing
- Connectionless Transport : UDP
- Principles of Reliable Data Transfer
- Connection-Oriented Transport : TCP
- Principles of Congestion Control
- TCP Congestion Control

## 3.1 Transport Layer Services
> 전송 계층에서 제공하는 서비스들에 대해 알아보기

### 3.1.1 Transport Services and Protocols

- 각각 다른 호스트들에서 실행되는 프로세스 앱들 사이의 논리적 연결을 제공함

- 각 종단 시스템에서 전송 계층 프로토콜이 실행됨

  - 송신 측 : 어플리케이션 계층에서 보내는 메세지를 세그먼트로 나누어 네트워크 계층으로 전송
  
  - 수신 측 : 세그먼트들을 조합해서 어플리케이션 계층으로 메세지를 전송
  
### 3.1.2 Transport vs. Network Layer

- Network Layer : 호스트들 간의 논리적 연결

- Transport Layer : 호스트에서 실행되는 프로세스간의 논리적 연결

### 3.1.3 Internet Transport Layer Protocols

- IP (Internet Protocol) : 네트워크 계층에서 사용되는 프로토콜

  - IP 서비스 모델은 호스트들 간에 논리적 통신을 제공하는 best-effort delivery service

  - 이것은 IP가 통신하는 호스트들 간에 세그먼트를 전달하기 위해 최대한 노력하지만, 어떤 보장도 하지 않는다는 것을 의미
  
  - IP는 세그먼트가 전달되는지, 순서대로 전달되는지 그리고 데이터의 무결성을 보장하지 않기 때문에 IP를 __비신뢰적인 서비스(unreliable service)__ 라고 부른다.

- UDP : 비신뢰적 순서가 없는 전송

  - 프로세스 대 프로세스 데이터 전달
  
  - 오류 검출
  
- TCP : 신뢰 가능한 순서가 있는 전송

  - UDP가 제공하는 서비스 + reliable data transfer
  
  - 혼잡 제어 (congestion control)
  
  - 연결 설정 (connection setup)
  
  - 흐름 제어 (flow control)
  
  
## 3.2 Multiplexing and Demultiplexing

> 전송 계층에서의 다중화(Multiplexing)과 역다중화(Demultiplexing) 과정 알아보기


### 3.2.1 Multiplexing/Demultiplexing

- 전송 측에서의 다중화 (Multiplexing at sender)

  - 네트워크 어플리케이션 프로세스는 어플리케이션 계층과 전송 계층 사이에서 소켓이라는 데이터 통로를 가지고 있음
  
  - 전송 계층은 실제로 데이터를 직접 프로세스로 전달하지 않고 중간 통로인 소켓을 사용하여 어플리케이션 계층과 네트워크 계층으로 전달하거나 받음
  
  - 전송 계층은 어플리케이션 계층으로부터 데이터를 받고 이를 세그먼트로 만들고 헤더정보를 넣음
  
  - 이 세그먼트들을 네트워크 계층으로 캡슐화 하여 전달하는데 이 과정을 __다중화(Multiplexing)__ 라고 함

- 수신 측에서의 역다중화(Demultiplexing)

  - 세그먼트의 데이터를 헤더 정보를 사용해서 올바른 소켓으로 전달하는 작업을 __역다중화(Demultiplexing)__ 라고 함
  
### 3.2.2 How Demultiplexing Works?

- 호스트는 IP Datagrams를 수신 받음

  - 각 데이터그램은 송신, 수신 IP를 포함하고 있음
  
  - 각 데이터그램은 하나의 전송 계층 세그먼트를 담고 있음
  
  - 각 세그먼트들은 송신, 수신 프로세스가 실행되는 port number를 가지고 있음
  
- 각 세그먼트는 세그먼트가 전달될 적절한 소켓을 가리키는 특별한 필드를 가짐

  - 출발지(source) 포트 번호
  
  - 목적지(destination) 포트 번호
  
- 호스트는 __IP 주소, Port 넘버__ 를 사용하여 적절한 소켓으로 데이터를 전달함

- 즉, __네트워크 계층에서 IP datagrams를 사용하여 해당 IP로 송신, 수신 이후 전송 계층으로 세그먼트를 전달한다. 전달된 세그먼트에는 송신, 수신 프로세스가 실행되는 Port(Source Port, Destination Port)정보가 헤더에 포함되어 있고 이를 확인하여 적절한 소켓으로 전달한다.__

### 3.2.3 Connectionless Demultiplexing (비연결지향 역다중화)

- Datagrams를 생성하여 UDP 소켓으로 데이터를 전달하기 위해서는 도착지 IP와 도착지의 Port Number가 필요하다.

- UDP 연결의 IP 데이터그램에서 같은 수신지의 Port Number는 송신지의 정보가 다른 IP와 Port Number라 할지라도 같은 수신지의 소켓으로 전송됨

### 3.2.4 Connection Oriented Demultiplexing

- TCP 소켓은 송신지 IP와 Port Number, 수신지 IP와 Port Number로 정의가 됨

- 송신지는 네 개의 값들을 모두 사용하여 세그먼트가 전송되어야 하는 소켓에 전송

- 서버 호스트는 동시에 실행되는 TCP 소켓들을 지원해야 함

- 웹 서버는 클라이언트 각각의 연결에 대해 다른 소켓을 갖고 있다.

## 3.3. Connectionless Transport : UDP
> UDP : User Datagram Protocol

- 비연결 지향 : handshaking 과정이 불필요

- UDP 세그먼트는 다른 세그먼트들에 독립적으로 처리됨

### 3.3.1 UDP : Segment Header

![image](https://user-images.githubusercontent.com/66773320/96838052-4c19df00-1482-11eb-961c-54cebfedc72d.png)

- 왜 UDP를 사용하는가?

  - 지연을 유발하는 연결 수립 과정이 없음
  
  - 송신단과 수신단 간의 연결 상태 관리를 하지 않아 간단함
  
  - 작은 헤더 사이즈와 혼잡 제어를 하지 않기 때문에 원하는 속도 만큼 전송할 수 있음
  
### 3.3.2. UDP Checksum

- 전송 된 세그먼트가 변형이 되지는 않았는지 확인하기 위해 세그먼트의 헤더에 체크섬 필드를 추가함

- 체크섬을 계산하는 방법

  - UDP 세그먼트의 도착 IP주소, 송신 포트번호, 입력 포트번호, 데이터 길이, 그리고 UDP 데이터들을 16비트단위로 쪼개서 전부 더함
  
  - 그리고 오버플로되서 캐리된 값들을 다시 더하고 1의 보수를 취하여 이렇게해서 나온 값을 체크섬 값으로 사용함
  
  ![image](https://user-images.githubusercontent.com/66773320/96839217-d44cb400-1483-11eb-8d9e-c09513f2c813.png)
  
## 3.4. Principles of Reliable Data Transfer

- 어플리케이션 계층에서 __신뢰 가능한 채널(reliable channel)인 전송 계층으로 데이터를 세그먼트화 하여 전달__

- 전송 계층에서는 이를 패킷화 하여 __비신뢰적 채널(unreliable channel)인 네트워크 계층__ 으로 전달

### 3.4.1 Reliable Data Transfer : Getting Started

![image](https://user-images.githubusercontent.com/66773320/96840558-93ee3580-1485-11eb-8dda-ff1ad29dd91b.png)

- rdt_send() : 어플리케이션 계층에서 전송 계층(reliable channel)으로 데이터를 세그먼트화 하여 전달하는 과정

- udt_send() : 전송 계층에서 네트워크 계층(unreliable channel)으로 데이터를 패킷화 하여 전송하는 과정

- rdt_rcv() : 패킷이 수신측에 도착하면 네트워크 계층에서 전송 계층으로 전달하는 과정

- deliver_data() : 전송 계층에서 어플리케이션 계층으로 세그먼트를 다시 조립하여 전달하는 과정

### 3.4.2. rdt1.0 : Reliable Transfer Over a Reliable Channel
> 완벽하게 신뢰적인 채널 상에서의 신뢰적인 데이터 전송


![image](https://user-images.githubusercontent.com/66773320/96841577-eaa83f00-1486-11eb-9ac7-56ae37e3b693.png)

- 비트 오류와 패킷 손실이 없는 채널 상에서의 데이터 전송

- 비트 오류와 패킷 손실이 없기 때문에 어떠한 피드백도 제공할 필요가 없음

- 송신 측
  
  - rdt_send(data) : 이벤트에 의해 상위 계층으로부터 데이터를 받음

  - packet = make_pkt(data) : 이벤트에 의해 데이터를 포함하는 패킷을 생성
  
  - udt_send(packet) : 이벤트에 의해 패킷을 네트워크 계층으로 내려보내며 송신
  
- 수신 측

  - rdt_rcv(packet) : 이벤트에 의해 하위 채널로부터 패킷을 수신
  
  - extract(packet,data) : 이벤트에 의해 패킷으로부터 데이터를 추출
  
  - deliver_data(data) : 이벤트에 의해 데이터를 전송 계층으로 전달
  
### 3.4.3. rdt2.0 : Channel with Bit Errors
> 패킷 손실은 없고 패킷 안의 비트들이 하위 채널에서 손상되는 모델

![image](https://user-images.githubusercontent.com/66773320/96841677-0b709480-1487-11eb-82d4-82fb3385dfb8.png)

- ARQ(Automatic Repeat Request) 프로토콜 : 재전송을 기반으로 하는 신뢰적인 프로토콜

  - Acknowledgement(ACKs) : 수신지가 패킷을 잘 받았고 에러가 없다는 메세지로 송신지에 피드백
  
  - Negative Acknowledgements(NAKs) : 수신자가 패킷을 받았지만 패킷에 에러가 있다고 송신지로 피드백

  - 송신지가 NAK 메세지를 받으면 패킷을 재전송
  
- 2.0 모델에서는 송신지가 수신지에서 일어나는 일을 전혀 모름

  - 수신지에서 ACK 메세지를 보냈는데 이것이 손실되면 송신지는 해당 패킷을 다시 한번 보냄
  
  - 수신지는 해당 패킷에 대한 __ACK를 보냈지만 동일한 패킷을 받는 경우가 발생하게 됨__
  
  - 중복 패킷에 대한 관리가 필요함
  
### 3.4.4. rdt2.1 : Sender, Handels Garbled ACK/NAKs
> rdt2.0에서 패킷에 대한 순서번호(Sequence Number)를 추가한 모델

#### rdt 2.1 Sender

![image](https://user-images.githubusercontent.com/66773320/96841742-1e836480-1487-11eb-8390-eebc12fa84b5.png)

#### rdt 2.1 Receiver

![image](https://user-images.githubusercontent.com/66773320/96841883-4d99d600-1487-11eb-97de-e0bce3ea0ca8.png)

- 송신 측은 0과 1을 Sequence Number로 사용하고 수신 측에서는 0과 1로 중복 패킷인지 여부를 결정

- 송신 측이 Sequence Number 0의 패킷을 보내면 0번에 대한 ACK/NAK을 기다림

- 수신 측은 패킷을 잘 수신한 경우 ACK 메세지를 보내며 Sequence Number 1의 패킷이 수신되기를 기다림

- 송신 측은 ACK를 받으면 Sequence Number 1의 패킷을 보내고 NAK를 받거나 피드백 패킷에 오류가 있을 경우 Sequence Number 0의 패킷을 재전송

- 수신 측이 Sequence Number 1의 패킷을 기다리고 있는데 Sequence Number 0의 패킷이 도착한다면 __해당 패킷이 중복 도착했으므로 중간에 문제가 생겼음을 알고 Sequence Number 0에 대한 ACK를 전송__

### 3.4.5. rdt 2.2 : A NAK-Free Protocol
> rdt2.1과 동일하지만 NAK를 사용하지 않고 ACK만 사용하는 모델

![image](https://user-images.githubusercontent.com/66773320/96841948-61453c80-1487-11eb-97e2-5290660823a7.png)

### 3.4.6. rdt 3.0 : Channels With Errors and Loss
> 비트 오류와 패킷 손실이 모두 존재하는 모델

- 패킷 손실의 검출을 Timer로 고려함

  - ACK 메세지가 Timer에 의해 __설정된 시간안에 수신되지 않는다면 패킷 손실이 일어났을 가능성이 있기 때문에 패킷을 재전송__

![image](https://user-images.githubusercontent.com/66773320/96846554-0878a280-148d-11eb-9f8f-f8c310fa809b.png)

![image](https://user-images.githubusercontent.com/66773320/96846653-28a86180-148d-11eb-89e6-3b7e49bcb6e7.png)

### 3.4.7. Performance of rdt3.0

- rdt3.0은 정확한 프로토콜이지만 오늘날의 고속 네트워크에서 만족스러운 성능을 기대하기 힘듦

![image](https://user-images.githubusercontent.com/66773320/96847123-c56aff00-148d-11eb-8619-786a8e464bb7.png)

- __RTT (Round Trip Time)__ : 패킷이 송신지에서 수식지로 전송되고 ACK가 수신지에서 송신지로 돌아오는데 걸리는 시간

- 8000bit의 패킷을 1 Gbps link로 보내는 시간 
```
D = L/R = 8000bits/10^9bits/sec = 8usec
```

- 이용률(Utilization) : 전체 전송 시간동안 실제로 데이터를 전송한 시간
```
U = (L/R) / (RTT + L/R) = 0.008 / 30.003 = 0.00027
```

- 이용률을 따져보면 30.008msec동안 송신 측은 0.008msec동안만 실제로 데이터를 전송한 것이고 __이는 아주 형편없는 이용률__


### 3.4.8. Pipelined Protocols
> ACK를 기다리지 않고 여러 패킷을 전송하도록 허용하는 프로토콜

![image](https://user-images.githubusercontent.com/66773320/96848890-ecc2cb80-148f-11eb-9858-75afac3b84d1.png)

- 패킷을 파이프라인에 채워넣음으로써 이용률을 높이게 됨
```
U = 3 * (L/R) / (RTT + L/R) = 0.008 / 30.003 = 0.00081 (송신자의 이용률이 3배 증가)
```

- pipelined protocol을 사용하려면 __sequence number의 범위가 증가__ 하고 여러 패킷 이상을 담을 수 있는 __버퍼가 필요__ 해졌으며 파이프라인에서의 __패킷 손실과 지연 패킷에 대한 처리방식이 필요해짐__

### 3.4.9. Pipelined Protocols : Overview

__Go-back-N__

  - 전송측은 N개의 ACK 메세지를 받지 않은 패킷을 가지고 있을 수 있음
  
  - 수신측은 오직 누적 된 ACK 메세지만을 보냄
  
    - 패킷들 사이에 차이가 있으면 안됨 즉, 연속적인 Sequence Number의 패킷만 받음
  
  - 전송측은 제일 오래된 ACK 메세지를 받지 못한 패킷에 대한 타이머를 갖고 있음
    
    - 타이머가 만료되면 이 패킷으로 부터 N개의 ACK를 받지 못한 패킷을 
  
__Selective Repeat__ 

  - 전송측은 N개의 ACK 메세지를 받지 않은 패킷을 가지고 있을 수 있음
  
  - 수신측은 각각의 패킷들에 대한 ACK 메세지를 독립적으로 보내게 됨
  
  - 전송측은 ACK메세지를 받지 못한 패킷들에 대한 타이머를 각각 유지함
  
### 3.4.10. GBN in Action

![image](https://user-images.githubusercontent.com/66773320/96850775-2a285880-1492-11eb-85e9-6875df04f81d.png)

- GBN 작동 순서

  1. 설정된 전송 측의 window size(N=4)만큼 순서대로 패킷을 전송

  2. sequence number가 0,1인 패킷은 수신측이 제대로 받았다는 ACK메세지를 각각 전송

  3. sequence number가 2인 패킷을 전송측은 전송 하였지만 손실이 발생

  4. 이후 수신 측에서는 sequence number가 3인 패킷을 받았지만 sequence number가 2인 패킷을 아직 받지 못하였기 때문에 제일 마지막으로 받은 패킷에 대한 ACK메세지(ACK1)을 전송

  5. 전송 측은 0,1 패킷에 대한 ACK메세지를 받은 후 sliding window를 두 칸 이동하여 4,5번 패킷을 전송 (window size를 4로 유지)

  6. 수신 측은 2번째 패킷을 아직 못받았기 때문에 3,4,5번째 패킷을 폐기하고 ACK1을 다시 보냄

  7. 송신 측의 2번째 패킷에 대한 timeout이 발생하면 가장 오래된 ACK를 받지 못한 패킷부터 window size 만큼의 패킷을 다시 전송

- Sequence Number를 나타내는 bit가 M bits일 경우 2^M-1개의 아웃스탠딩 프레임이 가능, 즉 window 크기는 2^M-1을 넘으면 안됨

  1. 위의 경우에서는 Sequence Number를 나타내는 bit(M)이 2인 경우
  
  2. b에서는 window 크기가 4(2^2)인 경우며 window에 포함되는 0,1,2,3패킷이 수신 측으로 전송됨
  
  3. 수신 측은 패킷을 하나씩 받으면서 그 다음 패킷으로 sliding됨 (window 크기가 1)
  
  4. 수신 측은 0,1,2,3패킷을 받아서 프레임을 한칸 씩 이동하여 새로운 0번째 패킷을 받기를 기다림
  
  5. 수신 측에서 전송한 ACK0,1,2,3 메세지가 손실이 되었다면 전송 측에서 0번째 패킷에 대한 타이머가 작동하고 타임아웃 되면 이전의 0번째 패킷을 다시 전송
  
  6. 수신 측에서는 이 패킷을 새로운 0번째의 패킷으로 인식하게 되어 문제가 발생함
  
  7. 이것을 방지하기 위해 전송 측의 window 크기는 최대 3(2^2-1)이 되야 함
  
![image](https://user-images.githubusercontent.com/66773320/96853822-c738c080-1495-11eb-8f78-59b558408555.png)
  
### 3.4.11. Selective Repeat

![image](https://user-images.githubusercontent.com/66773320/96858505-4c72a400-149b-11eb-93b7-37ef3cfe4375.png)

- Selective Repeat 작동 순서

  1. 설정된 전송 측의 window size(N=4)만큼 순서대로(0,1,2,3) 패킷을 전송
  
  2. 수신 측의 window size도 송신 측의 window size(4)와 동일하게 설정됨
  
  3. sequence number가 0,1인 패킷은 수신측이 제대로 받았다는 ACK메세지를 각각 전송

  4. sequence number가 2인 패킷을 전송측에서 전송할 때 손실이 발생(수신 측에 도착하지 않음)
  
  5. 전송 측은 0,1 패킷에 대한 ACK메세지를 받은 후 sliding window를 두 칸 이동하여 4,5번 패킷을 전송 (window size를 4로 유지)
  
  6. 2번 패킷이 도착하기 전에 3,4,5 패킷이 도착했기 때문에 즉, 순서대로 도착하지 않았기 때문에 수신측은 버퍼에 3,4,5패킷을 저장
  
  7. 전송 측에서 2번 패킷 타이머가 타임아웃이 발생하고 2번 패킷을 재전송
  
  8. 전송 측에 4,5 패킷에 대한 ACK메세지(ACK4, ACK5)가 도착하고 이를 기록
  
  9. 수신 측에 2번 패킷이 도착하면 버퍼에 있던 3,4,5 패킷과 함께 전송하고 ACK2 메세지를 전송 측에 전송

### 3.4.12. Selective Repeat Dilemma

- Sequence Number를 나타내는 bit가 M bits일 경우 2^(M-1)개의 아웃스탠딩 프레임이 가능, 즉 window 크기는 2^(M-1)을 넘으면 안됨

![image](https://user-images.githubusercontent.com/66773320/96966777-6a96dd80-1549-11eb-83cf-142973ff5b70.png)

  1. sequence number가 0,1,2,3이고 이를 나타내는 bit 수는 2개(M=2)
  
  2. b의 경우 window 크기를 2^(2-1) = 2 보다 크게(3)으로 설정 됨
  
  3. 전송 측에서 0,1,2 패킷을 전송하여 수신 측에 도착하면 이를 받았다는 ACK0,1,2 메세지를 보냄
  
  4. 수신 측의 window는 순서대로 패킷을 받아 한칸 씩 밀려나서 (3,0,1)을 가리킴
  
  5. 그러나 송신 측에는 ACK0,1,2메세지가 도착하지 않아 0번 패킷의 타임아웃이 발생하여 0번 패킷을 재전송
  
  6. 하지만 수신 측에서는 다음 window의 0번 패킷을 가리키고 있어 이전의 0번 패킷을 다음 0번 패킷으로 인식하여 문제가 발생함
  
### 3.5. Connection Oriented Transport : TCP

#### 3.5.1. TCP : Overview

- Point to Point

  - 하나의 전송 측은 하나의 수신 측과 연결됨

- Reliable, in order byte stream

- Pipelined

  - Window 크기를 설정함으로써 TCP 혼잡과 흐름제어가 가능

- Full Duplex Data

  - 같은 연결에서 양방향 통신이 가능함

- Connection Oriented

  - handshaking 과정으로 데이터를 교환하기 전에 송신, 수신 측의 연결상태를 수립 

- Flow Controlled
  
#### 3.5.2. TCP Segment Structure

![image](https://user-images.githubusercontent.com/66773320/96971468-16dbc280-1550-11eb-9e8b-e4cd5af24ae1.png)

#### 3.5.3. TCP Seq. Numbers, ACKs

- Sequence Number

  - Sequence number의 관점은 전송된 byte의 stream으로 봄

  - 예를 들어서 MSS가 1000byte, data stream이 500000byte 일 때 첫 번째 data stream의 byte는 0이고 TCP는 500개의 segment를 만들 수 있음

  - 첫 번째 segment는 sequence number 0, 두 번째 segment는 sequence number 1000을 받을 것임
  
- Acknowledgement Number

  - TCP가 full duplex라는 점에서 전송 측에서 데이터를 보내는 와중에 데이터를 받을 수 있음
  
  - 수신 측으로부터 도착한 segment에는 수신 측에서 전송 측으로 날라온 데이터에 대한 sequence number가 포함됨

  - 전송 측이 segment에 넣은 acknowledgement number는 전송 측이 수신 측으로부터 기대하는 다음 byte의 sequence number가 됨
  

