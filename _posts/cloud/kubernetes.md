## 쿠버네티스?

[docker-swarm](https://docs.docker.com/swarm/overview/), [marathon](https://mesosphere.github.io/marathon/)과 같은 **container orchestration 툴**

#### **container orchestration 툴**!

###### orchestration... 지휘...

컨테이너를 지휘한다;

예로 1000개 노드에 50000 개의 컨테이너를 띄우는 것

* 여러 host를 묶어 클러스터를 구성

* container 를 적절한 위치에 배포

* container 가 죽으면 자동으로 복구

* 필요에 따라 container 를 매끄럽게 추가\(scaling\), 복제\(replication\), 업데이트\(rolling update\), 롤백\(rollback\)

### 아키텍처

master 에는 \(현재는 master 가 단일 노드이지만 추후 multi-node master 가 지원될 예정\)

작업을 위한 api server

state 를 관리하기 위한 분산

* scheduler
* controller manager  
  등이 있습니다.

* node \(= minion\) 에는

![](../assets/arc_official.png)

