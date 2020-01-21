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
개발환경에서는 기본적으로 local환경을 적용해야 하기 때문에 application.yml 파일에 local을 기본값으로 설정 해 둔다.

```yaml
spring:
  profiles:
    active: local
```

## 2. application-local.yml 만들기
project main 폴더에 domain 폴더를 만들고, Board.java파일을 생성한다.

```yaml
spring:
  jackson:
    deserialization.fail-on-unknown-properties: true
  datasource:
    url: jdbc:h2:tcp://localhost/~/firstexample
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        #      show_sql: true
        format_sql: true

logging:
  level:
    org:
      hibernate.SQL: debug
      hibernate.type: trace
      springframework.security: debug

    springframework:
      data:
        repository: DEBUG
```

## 3. application-test.yml 만들기
Entity 클래스를 이용해 자동으로 생성된 Table을 확인할 수 있다.

```yaml
spring:
  jackson:
    deserialization.fail-on-unknown-properties: true
  datasource:
    url: jdbc:postgresql://localhost:5432/example
    username: postgres
    password: pass
    driver-class-name: org.postgresql.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        #      show_sql: true
        format_sql: true

logging:
  level:
    org:
      hibernate.SQL: debug
      hibernate.type: trace
      springframework.security: debug

    springframework:
      data:
        repository: DEBUG
```

## 4. 애플리케이션 구동 시 yml 적용하기

```
java -Dspring.profiles.active=test -jar jar파일명
```