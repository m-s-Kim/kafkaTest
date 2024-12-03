# kafkaTest


![image](https://github.com/user-attachments/assets/d6e14dd4-1782-4f1b-86d6-102aeca95764)

파티션은 순서를 보장하는 로그 구조를 가지고 있다. 이 구조 덕분에 각 메시지는 파티션 내에서 고유한 offset 갑승ㄹ 가지게 되고 이 오프셋을 통해 메시지의 위치를 정확하게
식별 가능하다.
새메시지가 파티션에 추가될 때마다 그 멧지에는 마지막 메시지의 오프셋에 1을 더한 값에 할당된다.
ex) Message A 다음에 Message B를 저장하면 Mesage B는 offset 1 이 할당된다.

이와 같은 구조로 인해 메시지의 순서가 보되므로 컨슈머는 데이터를 정확한 순서대로 처리 할 수 있따.
파티션 복제시 
replication factor= 1


![image](https://github.com/user-attachments/assets/eac5db86-81ff-494f-88b1-c83e0157b4f4)

 메시지의 처리 순서는 토픽이 아닌 파티션별로 관리된다. 
 시스테 부하시 더 많은 파티션을 추가하여(스케일아웃) 처리능력을 확장할 수 있다.
 데이터 저장 단위 : 세그먼트
 세그먼트는 kafka 파티션 내의 데이터를 저장하는 기본 단위이다. 각 세그먼트 파일은 일정 크기에 도달하거나 특정 시간이 경과되면 새로운 세그먼트 파일로 전환 된다.
  



 출처 : https://medium.com/@greg.shiny82/apache-kafka-%EA%B0%84%EB%9E%B5%ED%95%98%EA%B2%8C-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0-343ad84a959b
