쿠버네티스로 노드 애플리케이션을 간단히 돌려보는게 목표!

Minikube provides a simple way of running Kubernetes on your local machine for free

### Minikube 설치

```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64&&
\ chmod +x minikube&&
\ sudo mv minikube /usr/local/bin/
```

### xhyve driver 설치

```
brew install docker-machine-driver-xhyve
```

### 퍼미션 설정

```
sudo chown root:wheel $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
sudo chmod u+s $(brew --prefix)/opt/docker-machine-driver-xhyve/bin/docker-machine-driver-xhyve
```

### kubernetes clusters와 통신 위한 kubectl 설치

```
brew install kubectl
```

### minikube cluster 시작

```
minikube start --vm-driver=xhyve
```

### minikube 컨텍스트 셋팅

```
kubectl config use-context minikube
```

### kubectl로 통신 되는지 확인

```
kubectl cluster-info
```

### 노드 코딩

```
var http = require('http');

var handleRequest = function(request, response) {
  console.log('Received request for URL: ' + request.url);
  response.writeHead(200);
  response.end('Hello World!');
};
var www = http.createServer(handleRequest);
www.listen(8080);
```

### 도커 컨테이너 이미지 생성

#### Dockerfile 생성

```
FROM node:6.9.2
EXPOSE 8080
COPY server.js .
CMD node server.js
```

??

```
eval $(minikube docker-env)
```

#### 도커 이미지 빌드

```
docker build -t hello-node:v1 .
```

### pod run

```
kubectl run hello-node --image=hello-node:v1 --port=8080
```

### 배포 확인

```
kubectl get deployments
```

```
NAME         DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
hello-node   1         1         1            1           3m
```

### pod 확인

```
kubectl get pods
```

```
NAME                         READY     STATUS    RESTARTS   AGE
hello-node-714049816-ztzrb   1/1       Running   0          6m
```

### 이벤트 확인

```
kubectl get events
```

### 서비스 생성

```
kubectl expose deployment hello-node --type=LoadBalancer
minikube service hello-node
```

[https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube](https://kubernetes.io/docs/tutorials/stateless-application/hello-minikube)

