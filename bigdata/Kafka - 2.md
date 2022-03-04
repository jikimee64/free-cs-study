## Kafka 구조

### 인트로
- 카프카는 아래와 같은 개념이 있다.
  - 프로듀서
  - 브로커(클러스터)
  - 토픽
    - 파티션
    - offset
  - 컨슈머, 컨슈머 그룹
  - 레플리케이션 & ISR(In Sync Replica)
  - 주키퍼
- 차례대로 알아보자

### 몇가지 개념 간단히 알아보기
#### 토픽
![Untitled](https://media.vlpt.us/images/hyun6ik/post/73aaf558-bbcf-4b84-b3e7-2b9d78831238/image.png)
- 메시지의 종류를 구분하는 단위
- Topic_Payment, Topic_GPS 처럼 어떤 데이터를 담는지 네이밍 가능
- 한 개의 토픽은 한 개 이상의 파티션으로 구성
- 토픽은 논리적인 표현으로 시스템 상에 보이지 않는다

#### 파티션
![Untitled](https://user-images.githubusercontent.com/52563841/156781636-11e7685d-b7ca-41d4-ba80-f21233c100d4.png)
- 파티션은 메시지를 저장하는 물리적 파일
- 빨간 숫자는 파티션 내의 offset. 메시지가 저장되는 위치
- 컨슈머가 순차적으로 데이터를 가져감
- 중요한 점은 메시지 브로커와는 달리 데이터를 컨슈머가 가져가더라도 데이터가 즉시 삭제되지 않음
- 파티션은 오직 추가만 가능. 따라서 운영에 투입시 파티션을 늘리는것엔 신중해야 함
- 파티션의 데이터는 아래 기준에 맞춰 삭제된다
  - ```log.retention.ms``` : 최대 데이터 보존 시간
  - ```log.retention.btte``` : 최대 데이터 보존 크기(byte)

![Untitled](https://user-images.githubusercontent.com/52563841/156782551-6fc75757-8e51-4322-926b-143aba3d2af7.png)
- 데이터 전송시 키값을 지정하여 원하는 파티션에 전송할 수 있다.
  1) 키가 null일 경우 라운드 로빈으로 할당(카프카 2.4 이전)   
  2) 키가 있는경우 키의 해시값을 구하여 같은 파티션에 전송. 따라서 해당 메시지들의 순서 보장 가능
- 보통 Default 파티셔너를 사용한다
- Default 파티셔너는 Producer에서 보내는 메시지의 Key값을 Hash 알고리즘을 이용해 숫자로 만든 다음에
파티션의 개수를 가지고 나머지를 구한다. 그리고 나온 나머지 값에 해당하는 파티션으로 메시지를 보낸다
- 해당 동작의 전제 조건은 Key가 null이 아닐때이다.

※ 카프카 2.4 이후 이전 차이
![Untitled](https://media.vlpt.us/images/hyun6ik/post/d49b3995-7f8e-4bdf-b659-8205cfb11304/image.png)
- 2.4 이후에는 Sticky 정책으로 Batch가 닫힐 때 까지 파티션으로 메시지를 계속 보낸다

#### Segment
![Untitled](https://media.vlpt.us/images/hyun6ik/post/c58d4335-e03f-48fa-b24f-be5da2e15a11/image.png)
- 메시지(데이터)가 저장되는 실제 물리 File
- 데이터 삭제는 브로커만이 할 수 있고 파일단위로 이루어지는데 해당 단위를 로그 세그먼트라고 한다
- 세그먼트에는 다수의 데이터가 들어가 있어서 DB 처럼 선별해서 삭제할 수 없다
- Segment File은 분리시켜서 만든다. 하나의 시스템내에 10G, 100G 파일을 만들 수는 없기 때문이다.
  따라서 Segment File을 용량이나 시간에 따라 분리시켜서 만든다.


#### 컨슈머, 컨슈머 그룹 
![Untitled](https://user-images.githubusercontent.com/52563841/156784991-acba200e-5696-45f7-a34e-e7217289a91a.png)
- 각각의 컨슈머는 컨슈머그룹에 속함
- 컨슈머는 브로커와 연결할 때 어떤 그룹에 속한다고 지정
- 한개의 파티션은 한개의 컨슈머 그룹에 한 개의 컨슈머만 연결 가능, 서로 다른 그룹의 컨슈머는 공유 가능
- 위와 같이 제한을 둠으로서, 컨슈머 그룹 기준으로 파티션의 메시지 순서 보장 가능

#### 레플리케이션
![Untitled](https://user-images.githubusercontent.com/52563841/156807109-afe338de-e89a-4c79-b601-ba6fe851b0b0.png)
- 카프카 클러스터는 최소 3대 이상을 권장
- 한대의 브로커는 리더, 나머지 브로커들을 팔로워라 지칭
- 리더 브로커를 통해서만 프로듀서와 컨슈머가 메시지를 처리
- 리더 브로커에 토픽 생성시 복제수를 지정하여 리더와 팔로워 브로커의 파티션을 동기화 시킨다.
- 위 그림을 보면 모든 파티션들이 동기화된건 아니다 
- Partition들이 분산되는 방식은 Broker Cluster 내에서 최적화해서 최적의 곳에 Partition들을 위치 시킨다.
- 리더 브로커에 장애가 발생하는 다른 팔로워 브로커 중 하나가 리더가 된다.
- 위와 같은 리플리카를 사용하여 데이터 유실을 막고 서비스 지속성 유지 가능


### 프로듀서
- 프로듀서는 데이터를 카프카 서버(브로커)에 보내는 역할 == 브로커의 특정 topic에 publish한다.
- 프로듀서의 역할
  - Topic에 해당하는 메시지를 생성
  - 특정 Topic으로 데이터를 publish
  - 처리 실패, 재시도

#### 프로듀서의 기본 흐름
![Untitled](https://user-images.githubusercontent.com/52563841/156770848-69ce39c6-71fe-42b5-a063-32a7ea56d991.png)
- Serializer와 Partitioner를 거쳐 전달받은 메시지를 버퍼에 배치로 묶어서 저장
- Sender가 배치를 차례대로 가져와서 브로커에 전송

※ Serializer/Deserializer
![Untitled](https://media.vlpt.us/images/hyun6ik/post/e4df243c-ecc2-4db7-a487-e4070883f23f/image.png)
- 카프카는 데이터를 Byte Array로만 저장
- Producer에서 JSON, String, Avro 같은 형태를 Record 형태로 만들어서 Key와 Value 형태로 Send하면 그 내부에서 
  Producer안에 있는 Publish&Subscribe 라이브러리에서 Serializer(직렬화)가 이루어지게 되고 직렬화를 통해서 
  Byte Array로 변환되어 Kafka에게 전달된다.
- Consumer는 Kafka에서 Byte Array 데이터를 Deserializer(역직렬화)하여 원본 데이터로 활용한다.

#### Sender의 기본 동작
![Untitled](https://user-images.githubusercontent.com/52563841/156787493-2a8f99a2-8818-42e3-b7db-f3d9091ba636.png)
- `데이터를 버퍼에 보내는 과정`과 `Sender에서 배치를 전송` 작업은 별도의 스레드로 동작
- 따라서, 한곳의 실패가 발생해도 전파되지 않는다.
- Sender는 배치안의 메시지 개수(배치안에 메시지가 full 찼는지)에 상관을 하지 않고 배치를 전송하는 역할에 
충실

#### 처리량 관련 주요 속성
![Untitled](https://user-images.githubusercontent.com/52563841/156788153-647fbd2f-ca52-4b24-9dba-424c52ce1ef3.png)

※ 만약 서버에 전송하는 속도보다 데이터가 더 빠르게 쌓이면?
- 빠르게 쌓여서 buffer.memory 값을 초과하면 프로듀서는 max.block.ms 시간만큼 블로킹 한 후에 전송
- max.block.ms 시간을 초과하면 익셉션 발생
- buffer.memory 크기를 늘리던가 해서 해결

#### 전송결과
1. 실패에 대한 별도 처리 X
```
KafkaProducer<String, String> producer = new KafkaProducer<String, String>(configs);
ProducerRecord record = new ProducerRecord("click_log", "login");
producer.send(record);
``` 
- 실패에 대한 별도 처리가 필요없는 메시지 전송에 사용

2. 전송 결과확인 O, but 블로킹 방식
```
Future<RecordMetadata> f = producer.send(new ProducerRecord<>("topic","value"));
try{
  RecordMetadaa meta = f.get(); // 블로킹
}catch(ExecutionException ex){

}
```
- 블로킹 방식으로 인해 배치에 데이터를 한개씩 담음
- 처리량이 낮아도 되는 경우에만 사용

3. 확인 O, 논블로킹
```
producer.send(new ProducerRecord<>("simple", "value"),
  new Callback() {
  @Override
  public void onCompletion(RecordMetadata metadata, Exception ex){
  
  }
});
```
- 전송이 완료되면 onCompletion 메소드로 받음

#### Acknowlegment(acks)
- 메시지의 종류에 따라 ack 설정
- 실패 응답을 받으면 데이터 저장에 실패
1. acks = 0
  - 카프카 서버의 응답을 기다리지 않고 계속 전송하기만 함
  - 전송보장이 되지 않으므로 데이터 유실의 위험성 존재
  - 메시지의 손실을 감안하더라도 빠르게 메시지를 보내는게 중요할 경우 사용하면 적절
  - Message LOSS : 상, SPEED : 상
2. acks = 1
  - 파티션의 리더에 저장되면 응답 받음
  - 리더에 장애가 발생할 경우 메시지 유실 가능성 존재
    - 리더로 보낸 메시지가 팔로워에 복제되기 전에 장애가 발생하면 팔로워에 메시지가 저장되지 않았으므로 
    메시지 유실 가능성 존재 
    - 그러나 빈번하게 일어나지 않음
  - 가장 많이 사용하는 옵션
  - Message LOSS : 중, SPEED : 중
3. acks = all(또는 -1)
  - 모든 리플리카에 저장되면 응답 받음
  - 브로커의 min.insync.replicas 설정값에 따라 달라진다
  - 메시지의 손실률이 매우 중요한 경우에 적합한 옵션
  - Message LOSS : 하, SPEED : 하
※ min.insync.replicas(브로커 옵션)
  - 프로듀서 ack 옵션이 all일 때 저장에 성공했다고 응답할 수 있는 동기화된 리플리카 최소 개수
  - 예시1)
    - 브로커 총 개수 = 3, ack = all, min,insync.replicas = 2
    - 리더에 저장하고 팔로워 중 한 개에 저장 하면 성공 응답 발생
    - 가장 안정적인 방식
  - 예시2)
    - 브로커 총 개수 = 3, ack = all, min,insync.replicas = 1
    - 리더에 저장되면 성공 응답, 리더 장애시 메시지 유실 가능
- 예시3)
  - 브로커 총 개수 = 3, ack = all, min,insync.replicas = 3
  - 리더와 팔로워 2개에 저장되면 성공 응답
  - 팔로워 중 한개라도 장애가 나면 저장조건 부족으로 저장에 실패
  - 해당 경우가 조심해야 하는 경우. 따라서 브로커 총 개수랑 해당 설정값을 동일시 하면 안됨


#### 에러 유형
- 전송과정에서 실패
  - 전송 타임 아웃(일시적인 네트워크 오류 등)
  - 리더 다운에 의한 새 리더 선출 진행 중
  - 브로커 설정 메시지 크기 한도 초과
- 전송 전에 실패
  - 직렬화 실패, 프로듀서 자체 요청 크기 제한 초과
  - 프로듀서 버퍼가 차서 기다린 시간이 최대 대기 시간 초과

- 실패 대응1 : 재시도 
  - 재시도 가능한 에러는 재시도 처리
    - 예: 브로커 응답 타임 아웃, 일시적인 리더 없음 등
  - 재시도 위치
    - 프로듀서는 자체적으로 브로커 전송 과정에서 에러가 발생하면 재시도 가능한 에러에 대해 재전송 시도(retries 속성)
    ※ 일반적으로 retries 속성 보단 delivery.timeout.ms로 재발송 제어
    ※ delivery.timeout.ms 
      - send() 메서드를 호출하고 성공과 실패를 결정하는 상한시간
      - 브로커로부터 ack을 받기 위해 대기하는 시간이며 실패 시 재전송에 허용된 시간
    - send() 메서드에서 익셉션 발생시 익셉션 타입에 따라 send() 재호출
    - 콜백 메서드에서 익셉션 받으면 타입에 따라 send() 재호출
  - 가장 무난한 방법

- 실패 대응2 : 기록
  - 추후 처리 위해 기록
    - 별도 파일, DB 등을 이용해서 실패한 메시지 기록
    - 추후에 수동(또는 자동) 보정 작업 진행
  - 기록 위치
    - send() 메서드에서 익셉션 발생시
    - send() 메서드에 전달한 콜백에서 익셉션 받는 경우
    - send() 메서드가 리턴한 Future의 get() 메서드에서 익셉션 발생시

※ 재시도와 메시지 중복 전송 가능성
![Untitled](https://user-images.githubusercontent.com/52563841/156796026-6efe7e19-e3d6-4eae-865b-f14353057f6a.png)
- 브로커의 ack 응답이 타임아웃으로 실패처리되서 전송을 재시도할 경우 데이터 중복 발송 가능성이 있음
- 멱등성 producer는 동일한 데이터를 여러번 전송해도 단 한 번만 저장되는 것을 의미
- ```enable.idempotence``` 속성을 true(false가 기본값)로 주게되면 producer에서 데이터를 단 한번만 전송하는 exactly-once을 지원
  - 멱등성을 적용하기위해선 아래 설정도 추가로 필요
    - max.in.flight.requests.per.connection를 5보다 작거나 같은 값으로 설정
    - retries을 0보다 큰 값으로 설정
    - acks를 all로 설정
- 멱등성 Producer 원리
  - 데이터를 전달할 때 Producer PID(Producer unique ID)와 시퀀스 넘버를 함께 전달하여 중복 데이터 
  적재를 방지
  - 시퀀스 넘버는 데이터를 전달할 때마다 1씩 증가하여 보내짐(같은 데이터 일경우 시퀀스 넘버는 같음)
  - 브로커는 데이터를 전달받을 때 PID와 시퀀스 넘버를 이용해 같은 데이터인지를 확인하고, 같은 데이터인 경우
  데이터를 저장하지 않음
  - 멱등성 프로듀서는 동일한 세션에서만 정확히 한 번을 보장. 따라서 Producer가 재시작되는 경우 PID가 
  달라질 수 있음

※ 재시도와 순서
![Untitled](https://user-images.githubusercontent.com/52563841/156800819-ff736bc8-7b33-4dc5-a04a-444d606baded.png)
- ```max.in.flight.requests.per.connection```
  - 블로킹없이 한 커넥션에서 전송할 수 있는 최대 전송중인 요청 개수
  - 이 값이 1보다 크면 재시도 시점에 따라 메시지 순서가 바뀔 수 있음
    - 전송 순서가 중요하면 이 값을 1로 지정

#### Producer 코드
```
public class Producer {
    public static void main(String[] args) {
        Properties configs = new Properties();
        configs.put("bootstrap.servers", "localhost:9092,localhost:9093");
        configs.put("acks", "1");
        configs.put("buffer.memory", 33554432);
        configs.put("batch.size", "16384");
        configs.put("linger.ms", "0");
        configs.put("max.request.size", "1MB");
        configs.put("key.serializer", "org.apache.kafka.common.serialization.StringSerializer");
        configs.put("value.serializer", "org.apache.kafka.common.serialization.StringSerializer");

        KafkaProducer<String, String> producer = new KafkaProducer<String, String>(configs);
        ProducerRecord record = new ProducerRecord("click_log", "login");
        producer.send(record);
        producer.close();
    }
}
```

※ 키값과 파티션을 1:1매칭 시킨 상태서 파티션 하나를 더 추가한다면?
![Untitled](https://user-images.githubusercontent.com/52563841/156801953-2b2eb6e6-fc40-49b7-addd-9a833b4af8e8.png)
![Untitled](https://user-images.githubusercontent.com/52563841/156802023-ff491ead-7d68-4516-b5b7-d3ef64a73b1b.png)
- 키와 파티션의 매칭이 깨져서 키<->파티션의 일관성은 보장되지 않음
- 따라서 키를 사용할 경우 유의해서 파티션 개수를 생성해야되며, 사전에 확실히 정하는걸 추천
- 만약 해당 상황이 발생할 경우 커스텀 파티셔너를 활용

### 브로커 & 클러스터 & Broker Discovery

#### 브로커
![Untitled](https://media.vlpt.us/images/hyun6ik/post/fbaf7d79-df63-466a-b960-b09dfb54de24/image.png)
- Topic과 Partition에 대한 Read 및 Write를 관리하는 소프트웨어
- Kafka Server라고 부르기도 한다.
- Topic내의 Partition들을 분산, 유지 및 관리해준다.
- 각각의 Broker들은 ID로 식별된다.(단 ID는 숫자)
- Topic의 일부 Partition들을 포함한다.
  - Topic 데이터의 일부분(Partition)을 가지고 있을 뿐 데이터 전체를 가지고 있지는 않다.

#### 클러스터
여러개의 Broker들로 구성된다.
Client는 특정 Broker에 연결하면 전체 Cluster에 연결된다.
최소 3대 이상의 Broker를 하나의 Cluster로 구성해야 한다. (4대 이상을 권장한다.)

#### Broker Discovery
- Producer와 Consumer는 리더 브로커를 어떻게 알 수 있는 걸까?
![Untitled](https://media.vlpt.us/images/hyun6ik/post/4459f5a0-b7bc-4aa2-9a9d-87cc7f132b37/image.png)
- 모든 카프카 브로커는 bootstreap server라 한다
- Client((Producer, Consumer)가 특정 하나의 Broker에만 연결을 하면 자동으로 Broker가 Broker 전체 List를 전달해준다.
- Client는 이 정보를 통해서 내가 접속해야(Read/Write) 하는 Topic 그리고 그 Topic을 구성하는 Partition이 
  어디에 있는지 알게 되고 Client는 자동으로 자기가 필요한 Broker들로 연결된다.
- 하지만 특정 Broker 장애를 대비하여 전체 Broker List(IP, Port)를 파라미터로 입력하는 것을 권장한다.

### 주키퍼
- 브로커를 관리하며, 브로커의 리스트를 가지고 있다.
- 각 파티션에 대한 리더 브로커를 선정
- 새로운 토픽 생성이나 브로커 장애 발생 및 복구 등의 변경사항을 카프카에 알람을 보냄
- Kafka는 zookeeper 없이 작동하지 않는다(2022년에 Zookeeper를 제거한 정식 버전 출시 예정중)
- 과반수 투표방식으로 결정하기 때문에 홀수로 구성해야 하고, 과반수 이상 살아 있으면 정상으로 동작합니다.