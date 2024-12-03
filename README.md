# kafkaTest
다수의 Producer가 Consumer가 필요에 의해 개별적인 연결을 구조라서
하나의 시스템이 추가될 경우 기하급수적으로 복잡해짐
이를 해결하기 위해서 
어플리케이션이나 웹에서 취합한 데이터를 스트림파이프라인을 통해 실시간 관리하고 보내기위한
분산 스트리밍 플랫폼
데이터를 소비하는 어플리케이션과 데이터를 생성하는 어플리케이션간


![image](https://github.com/user-attachments/assets/d6e14dd4-1782-4f1b-86d6-102aeca95764)


Broker : 생산자와 소비자와의 중재자 역할을 하는 역할

Producer : 데이터를 Kafka 클러스터로 전송하는 역할을 하는 클라이언트 애플리케이션이다

Consumer : Kafka 클러스터에서 데이터를 읽는 역할을 하며 
컨슈머는 리더 파티션을 통해서만 데이터를 읽습니다. 팔로워 파티션은 복제용이며 고가용성을 위한것입니다.

Partition :

파티션은 순서를 보장하는 로그 구조를 가지고 있다. 이 구조 덕분에 각 메시지는 파티션 내에서 고유한 offset 갑승ㄹ 가지게 되고 이 오프셋을 통해 메시지의 위치를 정확하게
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

-하나의 request가 만들어지는 절차
message가 발행된다.
message들의 합이 batch.size가 될 떄 까지 기다린다.
linger.ms보다 먼저 batch.size를 넘어 설 경우, batch.size 크기로 batch를 묶는다.
linger.ms에 먼저 도달할 경우, 도달 전까지의 데이터만 batch 단위로 묶는다.
max.request.size보다 message 크기가 작은 경우만 전송이 완료된다.


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


