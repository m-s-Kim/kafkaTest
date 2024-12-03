## Nifi


![image](https://github.com/user-attachments/assets/baa7cad6-2796-4873-b10e-1eae3daf7864)


Web Server
 NiFi의 UI를 제공하는 Web Server로써 API를 사용할 수 있습니다.

 

Flow Controller
 Processor들을 관리하는 주체로써 thread를 할당할 수 있고 스케쥴링할 수 있습니다.

 

Extension
 NiFi에서 제공하는 기본 Processor 외에 사용자가 직접 커스텀한 processor입니다.

 

FlowFile Repository
 FlowFile 상태와 속성 값들의 정보들을 담고있는 WAL(Write-Ahead-Log)를 저장하는 곳입니다.

 

Content Repository
 실제 데이터를 저장하는 곳으로써 데이터를 블럭화해서 저장할 수 있습니다.

 

Provenance Repository
 FlowFile를 처리했던 event 정보들을 보관하는 곳입니다. 아래와 같이 FlowFile이 각 Processor를 거쳤을 때 content 또는 attribute 값이 계속해서 변경이되는데  Provenance Repository를 통해서 해당 FlowFile의 모든 변화 과정을 볼 수 있습니다. 가장 유용하게 사용할 수 있는 것은 Processor의 전, 후 값들을 비교하며 볼 수 있다는 점입니다. 최종 FlowFile이 원하는 결과가 아닐 때, 어느 Processor에서 잘못되었는지를 확인할 수 있습니다.


 ![image](https://github.com/user-attachments/assets/cbfca5cd-0089-4fb5-b493-491369ee08df)
