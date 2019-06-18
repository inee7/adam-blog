---
layout: post
title: Restful 하게 get하고 싶어요
date: 2018-12-11
description: 설명
img: rest.png
tags: restful api
author: jongin

---

최근 당황스러웠던 api는 "post v1/getXXX"라는 api였다

post인데 get이라는 url.. 거기다가 카멜케이스..??

어디서부터 시작된거일까 궁금했다

알아보니 단지 대량의 id를 요청해서 그에 해당하는 데이타를 받아오는 api였다

그런데 get방식으로는 쿼리스트링으로 제한이 있어서 (브라우저 평균 2000자 정도 ) post 방식으로 body에다가 json을 넣고 싶었던 것이다.

2000자 이상 요청 정보를 받고 할 정도 api면 기획이 잘 못 됐거나 클라이언트에서 해결 할 수 있다고 생각했는데... 실무의 방향은 그당시 실무자의 판단이 있었기에 존중하려한다.. 자세히 알 방법도 없다.. 이미 떠난 실무자들 (이런 경우가 굉장히 많다. 그래서 보수하려하는자를 설득하지 못하면 레거시를 모셔야된다.. )

우선 아무리 구글을 두드려봐도 rest api에서 조회는 get이지 post가 아니다 라는 글밖에 없다. stackoverflow는 그런데 okky는 그냥 조회에 대량을 넣고자하는 사람밖에 없더라.. 결국 표준을 깨고 클라이언트 혹은 비지니스개발 속도를 위해 많은 선택을 하는것 같았다.

----


이것저것 구글링하다가 REST Api 설계 시 GET의 query string vs path parameter 라는 글을 봤는데

최근에 내가 고민했던 부분이다

#### QueryString

ex) 게시판 글 목록을 가져올 경우

​    www.naver.com/board?start=3&end=10 (좋은 예)

​    www.naver.com/board/3/10 (나쁜 예)

위 처럼 데이터를 특정 조건으로 필터링을 하거나 소팅하여 가져올 경우 QueryString을 사용하여 설계할 수 있다.

#### PathParameter

ex) 특정 게시글을 가져올 경우

​    www.naver.com/board/3 (좋은 예)

​    www.naver.com/board?num=3 (나쁜 예)

​    www.naver.com/user/virus (좋은 예)

​    www.naver.com?id=virus (나쁜 예)

위 처럼 특정 데이터를 선택하여 가져올 경우는 쿼리스트링보다 PathParameter를 사용하면 더욱 깔끔한 URI 설계를 할 수 있다.

출처:

http://virusworld.tistory.com/129
