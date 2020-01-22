---
layout: post
title:  "spring boot를 이용한 web과 DB를 docker로 구성하기"
date:   2020-01-21 09:06:00 +0900
categories: springboot intellij
---

Spring boot로 개발한 서버단을 docker에 올리고 db도 docker로 따로 구성해 두개의 docker container가 연결된 구성을 만들어보자.

## 1. application-test.yml 설정하기
본인의 환경은 application.yml을 local과 test로 구성했고 local은 h2를 이용하고 test는 postgresql을 이용하고자 한다.

[-> spring boot 개발환경과 서버마다 DB설정 다르게 적용하기](https://geeshow.github.io/springboot/intellij/2020/01/21/springboot-deply-strategy.html)

application-test.yml에서 jdbc url 설정은 아래와 같다.
```yml
spring:
  jackson:
    deserialization.fail-on-unknown-properties: true
  datasource:
    url: jdbc:postgresql://mypostgres:5432/example
    username: postgres
    password: pass
    driver-class-name: org.postgresql.Driver
...
```
url로 설정된 `mypostgres`는 docker postgres 컨테이너의 이름이기 때문에 postgres 컨테이너를 생성할때 `mypostgres`로 생성해야 한다.

## 2. postgres 컨테이너 생성
docker 명령으로 postgres 이미지를 실행시켜준다.
```
docker run -d --name mypostgres postgres
```
`run`을 했을때 해당 이미지가 없으면 자동으로 다운받는다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-21_001.png)

## 3. ubuntu 컨테이너와 postgres 컨테이너 연결
ubuntu 컨테이너를 생성할때 link 옵션을 사용해 postgres과 연결시켜 준다.
```
docker run -i -t --name web -p 8080:8080 --link mypostgres:mypostgres ubuntu /bin/bash
```
`-i -t` 옵션과 `/bin/bash`로 컨테이너를 생성하면 ubuntu 콘솔에 접근하게 된다.

`exit` 명령을 이용하면 ubuntu를 빠져나오면서 컨테이너도 같이 종료된다.
그래서 `Ctrl+p, Ctrl+q`를 이용해 빠져나와야 한다.

## 4. ubuntu에 openjdk 설치하기
아래 명령을 이용해 설치해주다.
```
apt-get update (apt-get목록을 최신으로 갱신. 2분정도 소요)
apt-get install default-jdk (open jdk를 다운 받는다. 30분 정도 소요)
```
설치가 정상적으로 완료 됐는지 `java -version`으로 확인해본다.