# Kafka

Kafka는 이벤트 스트리밍 플랫폼 입니다.
Pub-Sub 모델의 메시지 큐 형태로 동작하며 분산환경에 특화되어 있다.


## Kafka 도입 전 구조 

![image](https://github.com/user-attachments/assets/9ea4ea90-1443-47ab-a0e7-cfa1d357eca4)

Linked in 이 직면한 기존의 end-to-end 통신 방식의 문제점

시스템 복잡도(Complexity)의 증가
  1. 통합된 전송 영역이 없어 데이터 흐름을 파악하기 어렵고, 시스템 관리가 어려움
  2. 특정 부분에서 장애 발생 시 조치 시간 증가 (연결 되어있는 Application을 모두 확인해야 했기 때문)
  3. HW 교체 혹은 SW 업그레이드 시 관리 포인트가 늘어나고, 작업시간의 증가(연결되어 있는 Application에 Side Effect 가 없는지 확인 필요)

데이터 파이프라인 관리의 어려움
 1. 각 애플리케이션과 데이터 시스템 간 별도의 파이프라인이 존재하고, 파이프라인마다 데이터 포맷과 처리 방식이 다름
 2. 새로운 파이프라인 확장이 어려워지면서, 확장성 및 유연성이 떨어짐
 3. 데이터 불일치 가능성이 존재했기 때문에 신뢰도 감소

이러한 문제점을 직면한 Linked in 개발팀은 Kafka 개발팀을 구성하여 목표를 설정합니다.

## Kafka 도입 후 구조

![image](https://github.com/user-attachments/assets/484ee9e6-c4b6-497f-ac1b-72742dea4567)

Kafka 도입 후 개선점
1. 기존 end-to-end 통신 방식에서 벗어나 통합/중앙화된 전송 영역을 설계하자.
2. 메세지를 생성하는 Producer 와 Consumer 을 분리하자.
3. 대용량 메세지 처리와 더불어 빠른 처리량을 이루자.
4. 확장(Scale-out) 이 용이하도록 설계하자.

모든 시스템으로 데이터를 전송할 수 있고, 실시간 처리도 가능하며, 급속도로 성장하는 서비스를 위해 확장이 용이한 시스템을 구현 할수 있었습니다.




## Kafka 작동방식
카프카는 고성능 TCP 네트워크 프로토콜을 통해 통신하는 서버 와 클라이언트 로 구성된 분산 시스템입니다. 
온프레미스와 클라우드 환경의 베어 메탈 하드웨어, 가상 머신 및 컨테이너에 배포할 수 있습니다.

서버 : Kafka는 여러 데이터 센터 또는 클라우드 지역에 걸쳐 있는 하나 이상의 서버 클러스터로 실행됩니다. 이러한 서버 중 일부는 브로커라고 하는 스토리지 계층을 형성합니다. 다른 서버는 Kafka Connect를 실행하여 데이터를 이벤트 스트림으로 지속적으로 가져오고 내보내 Kafka를 관계형 데이터베이스 및 다른 Kafka 클러스터와 같은 기존 시스템과 통합합니다. 미션 크리티컬 사용 사례를 구현할 수 있도록 Kafka 클러스터는 확장성이 뛰어나고 내결함성이 있습니다. 서버 중 하나가 실패하면 다른 서버가 작업을 인계하여 데이터 손실 없이 지속적인 운영을 보장합니다.

클라이언트 : 네트워크 문제나 머신 장애가 발생하더라도 병렬로, 대규모로, 내결함성 방식으로 이벤트 스트림을 읽고, 쓰고, 처리하는 분산 애플리케이션과 마이크로서비스를 작성할 수 있습니다. Kafka에는 이러한 클라이언트가 일부 포함되어 있으며, Kafka 커뮤니티에서 제공하는 수십 개의 클라이언트 로 보강됩니다 . 클라이언트는 Java 및 Scala, 상위 수준 Kafka Streams 라이브러리, Go, Python, C/C++ 및 기타 여러 프로그래밍 언어와 REST API를 위해 제공됩니다.





## Kafka Architecture
![image](https://github.com/user-attachments/assets/d6e14dd4-1782-4f1b-86d6-102aeca95764)


Broker : 생산자와 소비자와의 중재자 역할을 하는 역할

Producer : 데이터를 Kafka 클러스터로 전송하는 역할을 하는 클라이언트 애플리케이션이다

Consumer : Kafka 클러스터에서 데이터를 읽는 역할을 하며 
컨슈머는 리더 파티션을 통해서만 데이터를 읽습니다. 팔로워 파티션은 복제용이며 고가용성을 위한것입니다.

Partition :

파티션은 순서를 보장하는 로그 구조를 가지고 있다. 이 구조 덕분에 각 메시지는 파티션 내에서 고유한 offset 값을 가지게 되고 이 오프셋을 통해 메시지의 위치를 정확하게
식별 가능하다.
새메시지가 파티션에 추가될 때마다 그 멧지에는 마지막 메시지의 오프셋에 1을 더한 값에 할당된다.
ex) Message A 다음에 Message B를 저장하면 Mesage B는 offset 1 이 할당된다.

이와 같은 구조로 인해 메시지의 순서가 보되므로 컨슈머는 데이터를 정확한 순서대로 처리 할 수 있다.


 ![image](https://github.com/user-attachments/assets/c4d1b99f-abb4-446a-be63-eda1de821512)
 
카프카는 리더가 장애가 발생하면 기존의 팔로워 중 하나가 리더가 될 수 있는 Failover 방식을 채용하고 있다.

출처 : https://ggop-n.tistory.com/91
 


![image](https://github.com/user-attachments/assets/eac5db86-81ff-494f-88b1-c83e0157b4f4)

 메시지의 처리 순서는 토픽이 아닌 파티션별로 관리된다. 
 시스테 부하시 더 많은 파티션을 추가하여(스케일아웃) 처리능력을 확장할 수 있다.
 데이터 저장 단위 : 세그먼트
 세그먼트는 kafka 파티션 내의 데이터를 저장하는 기본 단위이다. 각 세그먼트 파일은 일정 크기에 도달하거나 특정 시간이 경과되면 새로운 세그먼트 파일로 전환 된다.
  

 출처 : https://medium.com/@greg.shiny82/apache-kafka-%EA%B0%84%EB%9E%B5%ED%95%98%EA%B2%8C-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-343ad84a959b


#Flow

![image](https://github.com/user-attachments/assets/37036228-fd03-4e26-beb0-249f073acd9a)

buffer.memory : kafka producer가 메시지를 보내기 전 메모리에 보관하는 전체 메시지 용량을 의미합니다. (Default : 32MB)
max.request.size : 단일 요청으로 보낼 수 있는 최대 메시지의 크기를 의미합니다. (default : 1MB)
batch.size : 단일 배치 요청으로 단일 파티션에 요청을 보낼 수 있는 메시지들의 크기의 최대 합을 의미합니다.
linger.ms : producer가 batch.size에 도달하기까지 기다릴 수 있는 시간입니다.

request가 만들어지는 절차

1. message가 발행된다.

2. message들의 합이 batch.size가 될 떄 까지 기다린다.

3. linger.ms보다 먼저 batch.size를 넘어 설 경우, batch.size 크기로 batch를 묶는다.

4. linger.ms에 먼저 도달할 경우, 도달 전까지의 데이터만 batch 단위로 묶는다.

5. max.request.size보다 message 크기가 작은 경우만 전송이 완료된다.


![image](https://github.com/user-attachments/assets/c27d0daf-788d-41d1-a0d6-3bd7f59cc42d)

Fetcher
poll이 실행될 때, 적절한 크기의 record를 client에 return 하기 위해 Kafka Cluster로부터 record를 요청하고, client가 받기 직전에 메모리에 미리 저장하는 역할을 합니다.

Fetcher 성능에 영향을 주는 Configuration
fetch.min.byte ~ fetch.max.byte : broker에서 가져올 최소/최대 데이터 크기를 결정합니다. max.partition.fetch.bytes : 하나의 partition에서 가져올 데이터의 최대 크기를 결정합니다. fetch.max.wait.ms : fetch.min.byte에 도달하기까지 기다릴 수 있는 시간을 의미합니다. 이 시간이 지나도 min.byte에 도달하지 못하면 도달하지 못한 데이터 크기를 보냅니다.

coordinator
Kafka Cluster 내부의 coordinator(zookeeper, kraft)와 통신하여 어떻게 데이터를 consume할지, offset commit, consumer group, heartbeat 기능 등을 수행합니다.

poll configuration
데이터를 poll 할 시, 아래와 같은 설정값에 의해 동작이 달라질 수 있습니다. max.poll.records(default : 500)에 의해 fetcher로부터 가져 올 record 수가 정해진다. max.poll.interval.ms : poll 요청을 받은 메시지를 처리할 때 까지 최대 기다릴 수 있는 시간. 이 시간이 초과한다면, 해당 consumer는 rebalancing 될 때 consumer group에서 제외대상이 된다.

출처 : https://onepredict.github.io/kafka-message-flow-producer/

참조 : 

https://kafka.apache.org/20/documentation.html#introduction

https://medium.com/mo-zza/kafka-kraft-%EB%AA%A8%EB%93%9C-with-docker-%EB%8F%99%EB%AC%BC%EC%9B%90%EC%9D%84-%ED%83%88%EC%B6%9C%ED%95%9C-kafka-8b5e7c7632fa

https://oliveyoung.tech/2024-10-16/oliveyoung-scm-oms-kafka/

https://devocean.sk.com/blog/techBoardDetail.do?ID=165711&boardType=techBlog#none
