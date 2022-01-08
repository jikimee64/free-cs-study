# TCP ( Transmission Control Protocol)
- OSI 계층 모델에서 전송 계층(Transport Layer)에 해당합니다.
- 종단간 호스트강의 신뢰적인 연결지향 서비스를 제공합니다.

## TCP의 특징
- 신뢰성있는 데이터 전송
  - 패킷 손실, 중복, 순서바뀜 등이 없도록 보장합니다.
- 연결 지향적입니다.
  - 3-way handshaking 과정을 통해 연결을 설정하며, 4-way handshaking을 통해 연결을 해제합니다.
- 흐름제어 
  - 수신자 버퍼 오버플로우 방지
  - 송신(송신전송률) 및 수신(수신처리율) 속도를 일치시킵니다.
  - 대표적으로 Sliding Window를 사용해 수신자의 버퍼에 남은 공간만큼 데이터를 전송 받습니다.
- 혼잡제어 
  - 패킷 수가 과도하게 증가하는 현상 방지
- 전이중 전송방식/양방향성 (Full-Duplex)   
- 멀티캐스트가 불가능한 1:1 연결
  - 1:1 로 연결 통로가 설정된다.
  
## TCP 헤더 정보
![](https://user-images.githubusercontent.com/55661631/143238811-98e44dbe-54a1-4bb8-a0f8-5f19b4e147aa.png)

![](https://user-images.githubusercontent.com/55661631/143239026-95dcb79e-0e75-45ea-85f2-af8e58a427b7.png)

## TCP 제어비트 (Flag Bit) 정보
![](https://user-images.githubusercontent.com/55661631/143239148-48413217-312d-4a06-aae3-bd740c33fa9c.png)

## TCP의 연결 및 해제 과정
![](https://nesoy.github.io/assets/posts/20181010/2.png)

### TCP Connection (3-way handshake)
1. 먼저 open()을 실행한 클라이언트가 SYN을 보내고 SYN_SENT 상태로 대기한다.
2. 서버는 SYN_RCVD 상태로 바꾸고 SYN과 응답 ACK를 보낸다.
3. SYN과 응답 ACK을 받은 클라이언트는 ESTABLISHED 상태로 변경하고 서버에게 응답 ACK를 보낸다.
4. 응답 ACK를 받은 서버는 ESTABLISHED 상태로 변경한다.
### TCP Disconnection (4-way handshake)
1. 먼저 close()를 실행한 클라이언트가 FIN을 보내고 FIN_WAIT1 상태로 대기한다.
2. 서버는 CLOSE_WAIT으로 바꾸고 응답 ACK를 전달한다. 동시에 해당 포트에 연결되어 있는 어플리케이션에게 close()를 요청한다.
3. ACK를 받은 클라이언트는 상태를 FIN_WAIT2로 변경한다.
4. close() 요청을 받은 서버 어플리케이션은 종료 프로세스를 진행하고 FIN을 클라이언트에 보내 LAST_ACK 상태로 바꾼다.
5. FIN을 받은 클라이언트는 ACK를 서버에 다시 전송하고 TIME_WAIT으로 상태를 바꾼다. TIME_WAIT에서 일정 시간이 지나면 CLOSED된다. ACK를 받은 서버도 포트를 CLOSED로 닫는다.
> 참고 : 연결을 끊는 것은 서버인지 클라이언트 인지에 따라 해제하는 과정에서 상태가 뒤바뀔 수 있다.

---
# UDP ( User Datagram Protocol )
- 전송계층의 비연결 지향적 프로토콜
  - 연결 과정이 없기 때문에 TCP보다는 빠른 전송
  - 데이터 전달의 신뢰성은 TCP에 비해 낮다.
  - 데이터의 수신 순서를 알 수 없다.
  - 패킷이 유실될 수 있으며 이 때 유실된 데이터를 재전송하지 않습니다.
  
## UDP의 특징
- 비연결 지향
  - 순서화 되지 않은 Datagram 서비스
- 신뢰성이 없다.
  - 확인 응답이 없다.
  - 순서/흐름 제어가 없다.
- 멀티캐스팅 가능
- 헤더가 단순
- TCP 보단 전송속도가 빠르다.
- 데이터의 경계를 구분한다.

## UDP의 헤더 정보
![](https://user-images.githubusercontent.com/55661631/143240258-a5b26fae-784f-4ec0-9012-475554d1439d.png)

# 표로 비교하는 TCP vs UDP

||TCP|UDP|
|:---|:---|:---|
|**공통점**|포트 번호를 이용하여 주소를 지정|데이터 오류 검사를 위한 체크섬이 존재|
|연결방식|Connection-oriented protocol(연결지향형 프로토콜)	|Connection-less protocol(비 연결지향형 프로토콜)|
||Connection by byte stream(바이트 스트림을 통한 연결)|	Connection by message stream(메세지 스트림을 통한 연결)
||Congestion / Flow control(혼잡제어, 흐름제어)|	NO Congestion / Flow control(혼잡제어와 흐름제어 지원 X)
|전송순서/속도|Ordered, Lower speed(순서 보장, 상대적으로 느림)|	Not ordered, Higer speed(순서 보장되지 않음, 상대적으로 빠름)
|신뢰성|Reliable data transmission(신뢰성 있는 데이터 전송 - 안정적)|	Unreliable data transmission(데이터 전송 보장 X)
|패킷 교환 방식|TCP packet : Segment(세그먼트 TCP 패킷)|	UDP packet : Datagram(데이터그램 UDP 패킷)
|주로 쓰는 곳|HTTP, Email, File transfer에서 사용|	DNS, Broadcasting(도메인, 실시간 동영상 서비스에서 사용)
|통신 방식|1:1 전이중 통신|멀티캐스트 1:N, N:M가능||



# 참고
[TCP / UDP의 개념과 특징, 차이점](https://coding-factory.tistory.com/614)

[TCP](http://www.ktword.co.kr/test/view/view.php?m_temp1=347)

[UDP](http://www.ktword.co.kr/test/view/view.php?nav=2&no=323&sh=UDP)

[TCP & UDP](https://github.com/alstjgg/cs-study/blob/main/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC/TCP%20%26%20UDP.md)

[TCP 와 UDP 차이점](https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4)
