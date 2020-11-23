# TCP와 UDP의 차이

Transport Layor : End Point간 신뢰성 있는 데이터 전송을 담당하는 계층

![screenshot](https://github.com/Jinsab/Learning/blob/main/Image/Transport%20Layor.PNG "Transport Screenshot")

신뢰성: 데이터를 순차적이고, 안정적이게 전달   
전송: 포트번호에 해당하는 프로세스에 데이터를 전달

**전송 계층의 중요성    **
1. 데이터의 순차 전송이 원할하지 않는다.    
  송신자가 1, 2, 3의 순서로 데이터를 분할해서 보냈지만 수신자가 2, 3, 1의 순서로 데이터를 분할해서 받게 되면 통신자의 의도대로 진행되지 않게 된다.
2. Flow Problem (흐름 문제)   
  송수신자 간의 데이터 처리 속도 차이로 인한 흐름 문제가 발생할 수 있다.   
  송신자가 보낸 것 만큼 수신자가 다 받지 못해 처리할 수 있는 데이터 량(한계치)을 초과하게 되면 데이터가 누락이 된다.
3. Congestion (혼잡 문제)   
  네트워크의 데이터 처리 속도로 인해 추가적으로 전송된 패킷들은 수신자에게 도달하지 못해 Loss가 발생하게 된다.   

이에 대한 해결책 -> TCP

## TCP (Transmission Control Protocol)

**TCP(Transmission Control Protocol)**
- 신뢰성있는 데이터 통신을 가능하게 해주는 프로토콜
- 특징: Connection 연결 (3 way-handshake) - 양방향 통신
- 데이터의 순차 전송을 보장
- Flow Controll (흐름 제어)
- Congestion Control (혼잡 제어)
- Error Detection (오류 감지)

**세그먼트(Segment) - TCP 프로토콜의 PDU**
TCP에서 처리할 때 프로토콜의 패킷처럼 프로토콜 안에서 데이터가 처리되고 움직이고 한다. 그럴 때 그 데이터의 단위를 세그먼트로 한다. 
세그먼트의 경우에는 어플리케이션 단에서 TCP로 데이터를 전송하면 프로토콜 안에서 내부적으로 데이터를 자르고 TCP Header 추가

**TCP Header 플래그**    
A (Ack): 승인 비트, 물어본 것에 대한 응답을 해줄 때 사용
S (Syn): 동기화 비트, 상대방과 연결을 시작할 때 무조건 사용되는 플래그
F (Fin): 종료 비트

**3-way handshake (Connection 연결)**
1. SYN 비트를 1로 설정해 패킷 송신 (클라 -> 서버)
2. SYN, ACK 비트를 1로 설정해 패킷 송신 (서버 -> 클라)
3. ACK 비트를 1로 설정해 패킷 송신 (클라 -> 서버)

**데이터 전송 방식**
1. 클라이언트가 패킷 송신
2. 서버에서 ACK 송신
3. ACK를 수신하지 못하면 재전송

**4-way handshake (Connection close)**
1. 데이터를 전부 송신한 Client가 FIN 송신
2. Server가 ACK 송신
3. Server에서 남은 패킷 송신 (일정 시간 대기)
4. Server가 FIN 송신
5. Client가 ACK 송신

**TCP의 문제점**
 - 전송의 신뢰성은 보장하지만
 - 매번 Connection을 연결해서 시간 손실 발생 (3 way-handshake)
 - 패킷을 조금만 손실해도 재전송

----

## UDP (User Datagram Protocol)

**UDP(User Datagram Protocol)
- TCP보다 신뢰성이 떨어지지만 전송 속도가 일반적으로 빠른 프로토콜
- 순차 전송 X, 흐름 제어 X, 혼잡 제어 X
- Connectionless (3-way handshake X)
- Error Detection
- 비교적 데이터의 신뢰성이 중요하지 않을 때 사용 (ex. 영상 스트리밍)

**유저 데이터 그램(User Datagram) - UDP 프로토콜의 PDU**
데이터를 쪼개지 않음. -> 직접 어플리케이션 단에서 쪼개줘야 함

**데이터 전송 방식**
1. 클라이언트가 패킷 송신 (오류 검출 X, 데이터가 제대로 안올 수 있음)

----

## TCP VS UDP

**응답 시간이 중요한 데이터(서비스)에서는 TCP를 사용해선 안된다.**   
예시: 영상 스트리밍, 실시간 게임

**신뢰성이 중요한 서비스에서는 UDP를 사용해선 안된다.**    
예시: 웹 서핑, 이메일, 파일 전송

----

## 번외

**RUDP**
- 데이터의 변형이 없다.
- 데이터의 손실이 없다.
- 데이터가 순차적으로 도착한다.

[강의 영상](https://www.youtube.com/watch?v=ikDVGYp5dhg)
