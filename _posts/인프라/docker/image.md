# 이미지?

#### 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 변하지 않음. 





# ![997FD205-6714-49EC-9F8B-559B76BE08B9](/Users/jongin/TIL/assets/997FD205-6714-49EC-9F8B-559B76BE08B9.png)



도커 이미지 덕분에 더 이상 의존성 파일을 컴파일하고 이것저것 설치할 필요가 없음.

새로운 서버가 추가되면 미리 만들어 놓은 이미지를 다운받고 컨테이너를 생성만 하면 됨. 수천대 서버도 쉽게 ..

도커 이미지는 [Docker hub](https://hub.docker.com/)에 등록하거나 [Docker Registry](https://docs.docker.com/registry/) 저장소를 직접 만들어 관리

#### Layer

도커 이미지는 레이어 방식으로 효율적인 용량 관리가 가능하다.

![](../assets/878FB5A3-FF72-40F7-AF9D-8DACDA36C793.png)

#### 경로

이미지는 url 방식으로 관리하며 태그를 붙일 수 있습니다.

ubuntu 14.04 이미지는 [docker.io/library/ubuntu:14.04](http://docker.io/library/ubuntu:14.04) 또는 [docker.io/library/ubuntu:trusty](http://docker.io/library/ubuntu:trusty) 이고 [docker.io/library](http://docker.io/library)는 생략가능하여 ubuntu:14.04로 사용할 수 있습니다.

이러한 방식은 이해하기 쉽고 편리하게 사용할 수 있으며 태그 기능을 잘 이용하면 테스트나 롤백도 쉽게 할 수 있습니다.

![](../assets/0604B069-9785-4A36-8090-8F0166FE0810.png)



