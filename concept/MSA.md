## MSA(Micro Service Architecture) 개념
![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FeP0niV%2FbtqT9Z7hT9F%2FP9vRWJNbcixltzbxLgWJck%2Fimg.png)
- MSA란 시스템을 여러개의 독립된 서비스로 나눠서 이 서비스를 조합함으로서 기능을 제공하는 아키텍처 패턴
- 애플리케이션 개발 초기에는 소프트웨어의 모든 구성요소가 한 프로젝트에 통합되어 있는 Monolithic Architecture 패턴을 사용했다.  
- 해당 패턴의 문제점은 다음과 같다.
    1. 부분 장애가 전체 서비스의 장애로 확대될 수 있다.
    2. Scahe-out이 어렵다.
    3. 서비스의 변경이 어렵고, 수정 시 장애의 영향도 파악이 힘들다.
    4. 배포 시간이 오래 걸린다.
    5. 하나의 Framework에 종속적이다.
- 이러한 문제점을 해결하고자 애플리케이션을 핵심 기능으로 세분화하는 MicroService(MS)라는 아키텍처 기반의 접근 방식이 탄생하게 되었다. 
- MSA 적용 사례
  - 넷플릭스 : 온프레이스 -> MSA 전환 후 18배의 비용 감소
  - 쿠팡 : 하루에 약 100회 이상의 배포를 이상없이 실행 
- 마이크로서비스의 속성과 관련하여 아직 산업적인 합의는 없으며 공식적인 정의도 없다.
- 애플리케이션의 복잡도에 따라 모놀로틱 아키텍처 스타일이 유리하다면 굳이 MSA로 바꿔서 성능 저하를 유발할 필요는 없다.


![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FQwu5X%2Fbtq5f4EdCyV%2FK1Qk4fOmS3budJs7RMBsv0%2Fimg.png)
- 아키텍처와 시스템 복자도에 관한 그래프
- 복잡도가 낮을때 MSA 아키텍처의 사용은 오히려 생산성을 떨어뜨린다.
- MSA 전환시 고려 사항
  - 기존 모놀리식 조직도였다면 MSA에 맞게 변경할 수 있는가?
  - 팀수준과 일정에 맞는 기술 스택 선정
  - MSA를 통해 달성하고자 하는 목표는 무엇인가?
  - MSA 사용전에 다른 대안을 고려했는가?
  - 시스템 복잡도가 MSA 방식으로 바꿀만큼 복잡한가?
  - 개발인력들이 데브옵스 환경에서 MSA 방식으로 적응할 수 있는 수준이 되는가?

시스템 규모의 경제가 되어야 MSA를 할 수 있다. 트래픽도 높아야 하고 사람도 많아야 한다.
단순하게 생각하면 기존에는 테이블 조인하나 하면 될걸, 데이터 싱크하고 맞추고 하면 비용이
10배정도 더 든다. 이런것들을 상쇄하고 남을만큼의 가치가 있는경우에 MSA로 넘어가는 것이 좋다.


## MSA 장점과 단점
### 장점
- 분산형 개발을 통해 효율적인 개발 가능
- 개별 서비스가 다른 서비스에 부정적인 영향을 주지 않으면서 작동 가능
- 언어 제약 없이 다른 서비스들과 유연하게 결합하며 향후 확장 밎 새로운 기능 통합 등에 대비할 수 있음
- 기존의 모놀로식에 비해 더욱 모듈화되었기 때문에 배포에 따른 우려 사항들이 적어짐
- 명확하게 식별된 경계로 개발자들이 각각의 서비스를 파악하고 개선하기에 용이해짐
- 개별 서비스의 변경 사항을 빠르게 적용 및 배포할 수 있다.
- 조직은 명확히 정의된 책임 영역을 담당하는 소규모 팀을 보유할 수 있다. 배달의 민족같은 경우 전산팀,주문팀 등으로 분배되어 있다.

단점
- 도메인에 대한 이해부족으로 서비스 경계를 잘못 설계하면 모놀리식 패턴보다 더 나빠진다.
- 서비스에 따른 독립된 DB를 사용하고 있어 트랜잭션을 유지하는것이 복잡
- 배포에 대한 자동화가 필수
- 서비스 별로 로그가 생성되기 때문에 로그 추적 및 모니터링이 어려움
- 통합 테스트가 어렵고 개발 환경과 실제 운영환경을 동일하게 가져가는 것이 어렵다
- 각 서비스는 API를 통해 통신하므로 네트워크 통신에 의한 오버헤드가 발생

- 일부 단점에 대한 해결 방안
![Untitled](https://user-images.githubusercontent.com/52563841/151476111-96dea921-4ede-413c-9cbf-9f667c5d5281.PNG)


## MSA 예시
![Untitled](https://user-images.githubusercontent.com/49560745/105624758-7a370280-5e67-11eb-8972-795d7e327a35.png)
- CAFE를 도메인으로 하는 애플리케이션을 MSA 방식으로 구현한 예시


## MSA 표준 아키텍처
- ![Untitled](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=http%3A%2F%2Fcfile26.uf.tistory.com%2Fimage%2F993A3E465C6FA45621B74B)
1. 모바일 혹은 PC를 통해 인터넷 접속
2. API GateWay를 통해 접근하는 모든 서비스들에게 Policy Management, Mediation, Aggregation, Consumer Identify Provider를 제공
   - 일반적으로 사용자가 접근하면 인증을 처리하는 역할 및 원하는 서비스로 바인딩 하는 역할을 수행
3. Service Mesh
   - MSA의 핵심, Service Discovery의 동적 서비스 확장, Service Router의 로드밸런싱, Configuration의 설정 통합관리 등은 API의 유입을 통제하는 중요한 기능
   - 비즈니스 로직이 수행되며 이는 Database or Message Broker 등의 Backing Service로 연결
4. 그밖에 실제 MSA가 적대되는 Container와 Monitoring / Logging / Tracing등의 모듈로 구성

## 배달의 민족 MSA 전환기
- 2015년
- ![Untitled](https://media.vlpt.us/images/syleemk/post/10889ca3-5077-485e-b3f8-593d710f6341/image.png)
- 루비 DB가 죽는 순간, 전체 서비스가 마비
- 리뷰에 관련한 DB 테이블이 문제가 생기면 다른 DB 테이블에도 영향이 가서 전체 서비스가 마비
- 리뷰에 문제가 있더라도 고객의 주문에는 영향이 없어야 함

- 2016년
- ![Untitled](https://media.vlpt.us/images/syleemk/post/9ab1da0e-6cbc-4013-9cb3-462609aaa7d3/image.png) 
- 결제를 MSA로 분리
- 주문중계 게이트웨이 서비스를 둠으로써 가령 고객들이 치킨을 주문했을때 사장님들이 이 주문을 App,Pc,단말기 등으로 받을 수 있도록 포워딩 해주는 역할을 수행
- IDC -> AWS 클라우드 인프라로 이전

- 2017년
- ![Untitled](https://media.vlpt.us/images/syleemk/post/35f748b5-4b42-4606-a7af-0d980b8ba084/image.png)
- 메뉴, 정산, 가게목록 시스템 분리
- 광고와 검색이 하나의 프로젝트로 되어있었던 것을 루비 DB에서 떼어내서 엘라스틱 서치로 이전, 루비 DB에 가는 부하 감소

- 2018년도 하반기
- ![Untitled](https://media.vlpt.us/images/syleemk/post/387fdc40-8e69-4cde-9cce-58e2db8e8f8d/image.png) 
- 제일 복잡한 주문을 분리, 주문은 모든 시스템과 엮여져 있고 돈까지 왔다갔다 한다.
- ![Untitled](https://media.vlpt.us/images/syleemk/post/37a69b86-6e95-4988-9a27-dc1783f20279/image.png)
- 주문이 생성 -> 접수 -> 배달 -> 취소 하는 동안 수많은 라이프 사이클 존재, 그동안 수많은 외부 API 호출
- 리뷰 시스템이 장애가 나면, 주문 시스템도 영향을 받음
- ![Untitled](https://media.vlpt.us/images/syleemk/post/00cab42f-9067-4069-82b7-f0d1b4244957/image.png)
- 주문은 명확한 이벤트 기반으로 주문의 라이프 사이클 정의
- 주문 시스템에선 이벤트만 발행하고, 다른 시스템들이 발행된 이벤트들을 가져가서(구독) 원하는 비즈니스 로직 작성
- 주문 시스템은 주문 이벤트만 발행하면 끝나서 리뷰시스템이 죽어도 주문 시스템에선 아무런 문제가 일어나지 않음
- 기존 API 방식에서는 리뷰시스템이 죽었다 살아났을 때 이벤트가 날라가 버리는데, 이벤트 기반 시스템에서는 리뷰 시스템이 살아나는 순간 AWS SQS에 쌓여있는 이벤트들을 다시 consume 할 수 있다.

- 2018년 12월 - 먼데이 시작 직전
- ![Untitled](https://media.vlpt.us/images/syleemk/post/1fb86dd0-9b93-41a0-8862-5c87a75cdb52/image.png)
- 다음 복잡한 가게/업주랑 광고 시스템을 MSA로 분리가 필요
- 기존엔 가게, 광고 데이터 사용하는 시스템은 루비안의 가게랑 광고 데이터들을 보고 있었다.
- ![Untitled](https://media.vlpt.us/images/syleemk/post/f78629b3-68ad-4b3f-994d-92318410f10a/image.png)
- 시스템을 분할하고 MSA로 분리
- ![Untitled](https://media.vlpt.us/images/syleemk/post/28424dad-bd64-4803-bcd6-81c4a43a2934/image.png)
- API로 개발하고, 단순히 API조회 하는 방식 서비스, 데이터 동기화도 필요없다
- 사장님들이 가게 업주 데이터들을 바꾸면 가게 업주 서비스 내부에 있는 데이터베이스의 데이터들만 바꾸고, 나머지 시스템들은 실시간으로 API를 조회
- ![Untitled](https://media.vlpt.us/images/syleemk/post/cef5bc1e-f490-41ee-a692-35dbad96def6/image.png)
- 그러나 광고시스템 장애 발생시 광고 시스템의 API를 호출하는 모든 시스템 연쇄적으로 장애 발생
- 높은 트래픽이 오면 트래픽이 모든곳으로 전파된다

해결방안 : CQRS
- 핵심 비즈니스 명령(Command) 시스템과
- 조회(Query) 중심의 "사용자" 서비스
- 둘을 철저하기 분리
- Command And Query Responsibility Segregation(CQRS)

- ![Untitled](https://media.vlpt.us/images/syleemk/post/f5b73248-da93-4205-9e20-1259a2d9a4a3/image.png)
- 광고랑 가게업주 같은 명령형 시스템, 즉 비즈니스 사이드의 시스템을 Command에 놓고, 조회형 시스템(고객의 트래픽이 앞에서 들어오는 것들)을 Query에 놓음
- 가령 사장님 사이트에서 사장님이 가게의 이름이나 데이터를 바꿈. 그러면 가게 업주에서(내부적으로 DB 있고) 변경됬다는 이벤트 (SNS, SQS 이벤트)를 쏜다.
이벤트를 쏘면 각 시스템들이 가게 업주의 데이터가 필요하면, 이벤트를 consume해서 조회나 검색에 있는 데이터들을 갱신한다.

### 트래픽 처리(성능)
- ![Untitled](https://media.vlpt.us/images/syleemk/post/b80d51bb-9127-4a7f-81a1-bcba2696209b/image.png)
- 트래픽이 들어와도 조회용 쿼리와 관련된 시스템들만 트래픽을 받고 뒷단을 호출하지 않음

### 장애 격리
- ![Untitled](https://media.vlpt.us/images/syleemk/post/ae5e807e-249f-4d8e-843b-d4d457c1658f/image.png)
- 광고나 가게업주 시스템이 죽어도, 장애가 격리되고, 고객은 주문까지 넘어갈 수 있따.
  - 각 시스템이 내부에 필요한 데이터를 ㅂ관
  - 내부 서비스(광고, 검색)의 모든 변경 내역이 이벤트로 전달
  - 장애시 데이터 싱크가 늦어져도 고객 서비스 가능

### 데이터 싱크 장애 대응
- 이벤트 재발행
  - 큰 문제가 없으면 그냥 이벤트 재발행 하면됨
  - 원천데이터를 가지고있는 팀에서 이벤트를 재발행해주면 된다.
- 큐 장애 발생시 (SQS, SNS 자체 장애일시, 가끔 있다.)
  - 전체 IMPORT API 제공
  - 부분 IMPORT API 제공
  - 최근 업데이트 데이터를 분 단위로 부분 제공


