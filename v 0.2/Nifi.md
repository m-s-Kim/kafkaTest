## Nifi


![image](https://github.com/user-attachments/assets/baa7cad6-2796-4873-b10e-1eae3daf7864)


 빅데이터와 관련된 주제로써 이번에는 NiFi를 소개하겠습니다. NiFi는 미국 국가안보국(NSA)에서 Apache에 기증한 dataflow 엔진입니다. 기본적으로 데이터를 Extract, Transformation, Load (ETL)할 수 있는 툴로써 UI를 통해 다양한 기능들을 통해 데이터들을 flow화 시킬 수 있습니다


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




참조 

https://github.com/apache/nifi

https://nifi.apache.org/components/

https://nifi.apache.org/docs/nifi-docs/html/overview.html#nifi-architecture

https://nifi.apache.org/nifi-docs/nifi-in-depth.html
