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

![](../../../assets/img/arc_official.png)



###  다양한 배포 방식



![쿠버네티스 배포 방식](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/workload.png)쿠버네티스 배포 방식



컨테이너와 관련된 많은 예제가 웹(프론트엔드+백엔드) 애플리케이션을 다루고 있지만, 실제 세상엔 더 다양한 형태의 애플리케이션이 있습니다. 쿠버네티스는 `Deployment`, `StatefulSets`, `DaemonSet`, `Job`, `CronJob`등 다양한 배포 방식을 지원합니다. Deployment는 새로운 버전의 애플리케이션을 다양한 전략으로 무중단 배포할 수 있습니다. StatefulSets은 실행 순서를 보장하고 호스트 이름과 볼륨을 일정하게 사용할 수 있어 순서나 데이터가 중요한 경우에 사용할 수 있습니다. 로그나 모니터링 등 모든 노드에 설치가 필요한 경우엔 DaemonSet을 이용하고 배치성 작업은 Job이나 CronJob을 이용하면 됩니다. ~~무슨 기능을 원하는지 몰라서 다 준비해놨어~~





### Ingress 설정



![Ingress](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/ingress.png)Ingress



다양한 웹 애플리케이션을 하나의 로드 밸런서로 서비스하기 위해 Ingress~~입장~~기능을 제공합니다. 웹 애플리케이션을 배포하는 과정을 보면 외부에서 직접 접근할 수 없도록 애플리케이션을 내부망에 설치하고 외부에서 접근이 가능한 `ALB`나 `Nginx`, `Apache`를 프록시 서버로 활용합니다. 프록시 서버는 도메인과 Path 조건에 따라 등록된 서버로 요청을 전달하는데 서버가 바뀌거나 IP가 변경되면 매번 설정을 수정해줘야 합니다. 쿠버네티스의 Ingress는 이를 자동화하면서 기존 프록시 서버에서 사용하는 설정을 거의 그대로 사용할 수 있습니다. 새로운 도메인을 추가하거나 업로드 용량을 제한하기 위해 일일이 프록시 서버에 접속하여 설정할 필요가 없습니다.

하나의 클러스터에 여러 개의 Ingress 설정을 할 수 있어 관리자 접속용 Ingress와 일반 접속용 Ingress를 따로 관리할 수 있습니다.





### 클라우드 지원



![Cloud](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/cloud-company.png)Cloud



쿠버네티스는 부하에 따라 자동으로 서버를 늘리는 기능AutoScaling이 있고 IP를 할당받아 로드밸런스LoadBalancer로 사용할 수 있습니다. 외부 스토리지를 컨테이너 내부 디렉토리에 마운트하여 사용하는 것도 일반적인데 이를 위해 클라우드 별로 적절한 API를 사용하는 모듈이 필요합니다. 쿠버네티스는 Cloud Controller를 이용하여 클라우드 연동을 손쉽게 확장할 수 있습니다. AWS, 구글 클라우드, 마이크로소프트 애저는 물론 수십 개의 클라우드 업체에서 모듈을 제공하여 관리자는 동일한 설정 파일을 서로 다른 클라우드에서 동일하게 사용할 수 있습니다.





### Namespace & Label



![Namespace & Label](https://subicura.com/assets/article_images/2019-05-19-kubernetes-basic-1/namespace-label.png)Namespace & Label



하나의 클러스터를 논리적으로 구분하여 사용할 수 있습니다. 하나의 클러스터에 다양한 프레임워크와 애플리케이션을 설치하기 때문에 기본(`system`, `default`)외에 여러 개의 네임스페이스를 사용하는 것이 일반적입니다. 더 세부적인 설정으로 라벨 기능을 적극적으로 사용하여 유연하면서 확장성 있게 리소스를 관리할 수 있습니다.

