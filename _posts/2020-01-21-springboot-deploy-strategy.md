---
layout: post
title:  "spring boot 개발환경과 서버마다 DB설정 다르게 적용하기."
date:   2020-01-21 09:06:00 +0900
categories: springboot intellij
---

## 1. application.yml 설정하기
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