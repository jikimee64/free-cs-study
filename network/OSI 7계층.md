
####  개인 블로그에 포스팅한 내용의 요약본입니다. 자세한 내용은 [OSI 7계층](https://dncjf64.tistory.com/379) 글에서 확인해주세요.

## OSI 7계층

### 개념

- OSI 7계층이란 국제표준화기구(ISO)에서 개발한 모델로, 컴퓨터 네트워크 프로토콜 디자인과 통신을 계층으로 나누어 설명한 것
- 네트워크에서 통신이 일어나는 과정을 7계층으로 나눈 모델

### 7계층으로 나눈 이유

```
PC방에서 오버워치를 하는데 연결이 끊겼다.
어디에 문제가 있는지 확인을 해보자.

모든 PC가 문제가 있다면 라우터의 문제 : 3계층 네트워크 계층
광랜을 제공하는 회사의 회선 문제 : 1계층 물리 계층
한 PC만 문제가 있고 오버워치 소프트웨어에 문제가 있다면 7계층 Application 계층
오버워치 소프트웨어에 문제가 없고, 스위치에 문제가 있다면 2계층 data-link 계층
해당하는 계층만 문제가 있다고 판단해 다른 계층에 있는 장비나 소프트웨어를 건들이지 않는것이다.
```

- 통신이 일어나는 과정을 단계적으로 파악하기 용이
- 각 계층은 독립적인 상하구조의 계층적 특성으로, 특정 계층에서 이상이 생겼을 때 문제가 있는 계층만 고쳐서 문제 해결 가능

## OSI 7계층과 TCP/IP 5계층

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FdmlenL%2FbtrpQXc2oJT%2FKnbEmKDy07wsgSAHlUyfrk%2Fimg.png)

- OSI 7계층
  - 데이터 표준을 정리
  - 가이드 역할로는 충실하지만, 실제 구현의 예가 거의 없는 관계로 신뢰도 하락

- TCP/IP 5계층
  - 해당 이론을 실제로 사용하는 오늘날의 인터넷 표준(인터넷 모델)
  - 미국방성의 연구에 의해 OSI 7계층보다 먼저 개발
  - OSI 7계층으로부터 진화된 계층이 아닌, 별도의 모델

  
## OSI 7계층

### 1계층 - 물리 계층(Physical Layer)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcqTsFK%2FbtrpwPN3W3Y%2FklIJzjEgtQejeMfk9wy1lK%2Fimg.png)

- 최소 두대의 컴퓨터가 통신하기 위해 0, 1의 디지털 신호를 아날로그 신호로 바꾸어 전선으로 흘려 보내고 
아날로그 신호를 0, 1의 디지털 신호로 해석하는 역할을 수행
- 단순히 데이터만 전달하기 때문에 오류 제어기능은 수행하지 않음
- 물리 계층의 미디어 타입은 유선으론 구리, 광섬유 / 무선으론 라디오파, 마이크로파, 적외선 등이 존재

- 대표적인 장비 
  - 통신 케이블
  - 리피터 : 리피터는 세기가 약해진 아날로그 신호를 받아서 다시 강화시켜주는 역할, 이제는 잘 사용하지 않음
  - 허브 : 컴퓨터를 여러대 연결할 수 있는 네트워크 장비, 여러 컴퓨터의 집합을 만들어서 서로 데이터를 공유
  - 랜카드 : 디지털 신호 <-> 아날로그 신호 전환
  

### 2계층 - 데이터링크 계층(DataLink Layer)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FUQjZU%2Fbtrput56uu8%2FMYcIkS2seIIyiz4kVaaoV0%2Fimg.png)

- 단위가 '프레임(Frame)'인 데이터를 생성, 목적지의 MAC 주소와, 출발지의 MAC 주소가 포함
- 프레임과 스위치(MAC 주소 테이블)를 이용하여 원하는 통신장비에만 데이터 전송 가능
- 또한, 스위치의 구조상 전송시 데이터 충돌을 방지(전이중 통신 방식)
- ARP 프로토콜을 사용하여 목적지 컴퓨터의 IP 주소를 기반으로 MAC 주소를 찾을 수 있음
- 같은 네트워크 대역대에 있는 여러 대의 통신 장비들이 스위치를 통해 데이터를 주고 받는 역할을 수행
- 물리 계층에서 발생할 수 있는 오류를 찾고 수정하는데 필요한 기능
  - 프레이밍: Physical Layer를 통해 받은 신호를 조합해 Frame 단위의 데이터 유닛으로 만들어 처리
  - 흐름제어: 데이터를 송수신 시, 너무 많거나 너무 적은 데이터를 송수신하지 않도록 흐름제어
  - 오류제어: 프레임 전송 시 발생한 오류를 복원하거나 재전송
  - 접근제어: 매체 상 통신 주체(장치)가 여러 개 존재할 때, 데이터 전송 여부 결정
  - 동기화: 프레임 구분자 (특별한 bit 패턴)
- 대표적인 장비
  - 스위치, 랜 카드

### 3계층 - 네트워크 계층(Network Layer)

![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbyqAGV%2FbtrpBDlQ1v5%2FSI6WYPAfUVWkEIPrA0mHYK%2Fimg.png)

- 다른 네트워크 간의 통신을 가능하게 해주는 역할을 수행
- 라우터에 등록된 라우팅 테이블을 기반으로해 해당 목적지까지 가는 최적의 경로를 안내하는 라우팅 기능 제공
- IP 패킷을 만듬, 패킷 안에는 출발지 IP 주소와 목적지 IP 주소가 존재  
- 목적지 IP 주소에 데이터를 넘겨주기 위해 경로 검색(라우팅)을 통해 찾은 다음 라우터에게 내 데이터를 넘겨주는(forwarding) 것을 반복하여 해당 네트워크에 방문하고, 목적지 IP에 내 데이터를 넘겨주는 역할을 수행
- 네트워크 계층은 운영체제의 커널에 소프트웨어적으로 구현
- 대표적인 장비
  - 라우터

#### 1 ~ 3계층 - 요약
![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FRlx5F%2FbtrputLOEq5%2FWtKKCRB8EHuKQwSTppSTfK%2Fimg.png)

### 4계층 - 전송 계층(Transport Layer)

![Untitled](https://media.vlpt.us/images/jakeseo_me/post/944d7f5f-9d2f-4e85-916e-0bd0b513c6ff/image.png)

- 패킷이 전송 과정에서 아무 문제 없이 제대로 수신지 컴퓨터에 도착할 수 있도록 패킷 전송을 제어하는 역할 수행
- TCP 헤더를 붙여 단위가 '세그먼트'인 데이터를 생성, 출발지 포트번호와 목적지 포트번호가 존재
- 포트번호는 동일한 통신장비 안에서 애플리케이션을 식별할 때 사용하는 애플리케이션의 주소
- TCP 프로토콜은 데이터 송수신 전에 3-way handshake를 통해 기종간의 연결이 잘 수립되었는지 먼저 확인, 연결을 끊을때는 4-way handshake
- UDP 헤더는 목적지 주소와 실 데이터만 존재
- 대표적인 프로토콜
  - ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fts5DW%2FbtrpKEdywyV%2F4t2HLi1MqTA8Iw5uuPIsFk%2Fimg.png)
  - TCP : 연결 수립을 위한 검증 절차를 거쳐야 하므로 TCP 구조가 복잡, 주로 신뢰성이 필요한 이메일이나 파일전송에 사용
  - UDP : 신뢰성을 중요시하지 않고 데이터를 효율적으로 빠르게 보내는 것이 목적

### 5계층 - 세션 계층(Transport Layer)
- 데이터가 서로 만나는 환경을 조성해주는 역할
- 앞선 1 ~ 4계층의 정상적인 동작으로 인한 네트워크 세션 생성, 생성된 세션을 통해 통신 장치들간의 상호작용을 운영체제가 확립, 유지, 중단 하는 작업을 수행
- 통신장치 간의 통신방식을 결정 가능, 종류로는 3가지, 즉 전이중 방식, 반이중 방식, 단방향 방식 등 존재
- ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcCrzq3%2FbtrpKk8Dvd6%2FhHwBKuet3CK3aYXmWZvtM1%2Fimg.png)
- 동기화 기능
  - 특정 지점에서 송수신 오류가 발생하면 이전에 동기점으로 설정한 통신이 완벽했던 지점부터 다시 재전송
- 대표적인 프로토콜
  - SSH, NetBIOS

### 6계층 - 표현 계층(Presentation Layer)
- 상위나 하위계층에서 사용하는 데이터 표현양식과 무관하게 사용할 수 있도록 해주는 환경 제공
- 파일의 확장자가 해당 계층에서 결정
- 데이터 압축(EBCDIC->ASCII), 암/복호화 작업 수행
- 대표적인 기술
  - SSL, JPEG, MPEG, ASCII, EBCDIC

### 7계층 - 응용 계층(Application Layer)
- 유저와 가장 가까운 계층으로서 응용 프로세스와 직접 관계하여 일반적인 응용 서비스를 수행
- 크롬 웹 브라우저, 메신저 등이 7계층에 해당하는 프로토콜을 보다 쉽게 사용하게 해주는 응용 프로그램
- 대표적인 프로토콜
  - HTTP, HTTPS, FTP, Telnet, DNS 등이 있습니다.