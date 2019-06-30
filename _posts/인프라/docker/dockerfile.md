```
# vertx/vertx3 debian version

FROM subicura/vertx3:3.3.1 
MAINTAINER chungsub.kim@purpleworks.co.kr 
ADD build/distributions/app-3.3.1.tar / 
ADD config.template.json /app-3.3.1/bin/config.json 
ADD docker/script/start.sh /usr/local/bin/ 
RUN ln -s /usr/local/bin/start.sh /start.sh 
EXPOSE 8080 
EXPOSE 7000 
CMD ["start.sh"]
```

#### 이미지를 만들기 위한 파일

자체 DSL \(Domain-specific language\)언어를 이용하여 이미지 생성 과정을 적어서 이미지 생성

