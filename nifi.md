## Nifi


![image](https://github.com/user-attachments/assets/baa7cad6-2796-4873-b10e-1eae3daf7864)


 빅데이터와 관련된 주제로써 이번에는 NiFi를 소개하겠습니다. NiFi는 미국 국가안보국(NSA)에서 Apache에 기증한 dataflow 엔진입니다. 기본적으로 데이터를 Extract, Transformation, Load (ETL)할 수 있는 툴로써 UI를 통해 다양한 기능들을 통해 데이터들을 flow화 시킬 수 있습니다

JDK 21 이상 지원 

Web Server : 
 NiFi의 UI를 제공하는 Web Server로써 API를 사용할 수 있습니다.

Flow Controller : 
 NiFi가 사용하는 스케줄러이다
 Processor들을 관리하는 주체로써 thread를 할당할 수 있고 스케쥴링할 수 있습니다.
 특정 간격 또는 Cron 표현식( 으로 스케줄링을 할 수 있으며, 클러스터 환경에서 동시에 실행되는 것을 막기 위해 Primary Node(Primary Node는 여러 노드에서 Processor가 실행되지 않고, 특정 단일 노드에서만 실행하고자 할때 사용하는 대표 노드)에서만 실행할 수도 있다.
 Controller Service를 이용해 Processor간 자원을 공유할 수 있다.
 
Extension :
 NiFi에서 제공하는 기본 Processor 외에 사용자가 직접 커스텀한 processor입니다.

FlowFile Repository : 
 FlowFile 상태와 속성 값들의 정보들을 담고있는 WAL(Write-Ahead-Log)를 저장하는 곳입니다.

Content Repository : 
 실제 데이터를 저장하는 곳으로써 데이터를 블럭화해서 저장할 수 있습니다.

FlowFile : 
 FlowFile은 시스템을 통해 이동하는 각 객체를 나타내며, 각 객체에 대해 NiFi는 키/값 쌍 속성 문자열과 0바이트 이상의 관련 콘텐츠 맵을 추적합니다.
 일반적인 데이터를 인식하는 데이터 단위
 Processor와 Processor를 이동할 때 마다 복사본이 만들어져서 추적이 가능 (내용 복사는 아니고 포인트 정보만 복사)

Provenance Repository : 
 FlowFile를 처리했던 event 정보들을 보관하는 곳입니다. 아래와 같이 FlowFile이 각 Processor를 거쳤을 때 content 또는 attribute 값이 계속해서 변경이되는데  Provenance Repository를 통해서 해당 FlowFile의 모든 변화 과정을 볼 수 있습니다. 가장 유용하게 사용할 수 있는 것은 Processor의 전, 후 값들을 비교하며 볼 수 있다는 점입니다. 최종 FlowFile이 원하는 결과가 아닐 때, 어느 Processor에서 잘못되었는지를 확인할 수 있습니다.
 
Connection : 
 Processor과 Processor을 연결해 FlowFile을 전달


 ![image](https://github.com/user-attachments/assets/cbfca5cd-0089-4fb5-b493-491369ee08df)


 출처 : https://taaewoo.tistory.com/40#Cluster%--Coordinator
 


보장된 배달
NiFi의 핵심 철학은 매우 큰 규모에서도 보장된 전달이 필수라는 것입니다. 이는 목적에 맞게 구축된 지속적인 미리 쓰기 로그와 콘텐츠 저장소를 효과적으로 사용하여 달성됩니다. 이들은 함께 매우 높은 트랜잭션 속도, 효과적인 로드 스프레딩, 쓰기 시 복사를 허용하고 기존 디스크 읽기/쓰기의 강점을 활용하도록 설계되었습니다.

Back Pressure 및 압력 해제를 통한 데이터 버퍼링
NiFi는 대기 중인 모든 데이터의 버퍼링을 지원하고, 대기 중인 데이터가 지정된 한도에 도달하면 Back Pressure를 제공하거나 지정된 기간(값이 소멸됨)에 도달하면 데이터를 만료시키는 기능을 제공합니다.

우선 순위 대기
NiFi는 큐에서 데이터를 검색하는 방법에 대한 하나 이상의 우선 순위 체계를 설정할 수 있습니다. 기본값은 가장 오래된 것이 먼저이지만, 데이터를 가장 최신 것이 먼저, 가장 큰 것이 먼저 또는 다른 사용자 지정 체계로 가져와야 할 때가 있습니다.

흐름별 QoS(지연 시간 대 처리량, 손실 허용 범위 등)
데이터가 절대적으로 중요하고 손실이 허용되지 않는 데이터 흐름 지점이 있습니다. 또한 가치가 있으려면 몇 초 이내에 처리되고 전달되어야 하는 경우도 있습니다. NiFi는 이러한 우려 사항에 대한 세분화된 흐름 특정 구성을 가능하게 합니다.



참조 

https://github.com/apache/nifi

https://nifi.apache.org/components/

https://nifi.apache.org/docs/nifi-docs/html/overview.html#nifi-architecture



