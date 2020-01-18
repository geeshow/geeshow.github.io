---
layout: post
title:  "spring boot에서 h2 database 설정하기"
date:   2020-01-18 11:50:00 +0900
categories: springboot intellij
---

##1. application.yml 설정하기
resources 폴더에 application.properties가 있다. 개인적으로 properties보다 yml 파일을 선호하기 때문에 application.perperties 파일을 application.yml로 변경한다.
설정은 다음과 같이 한다.
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

##2. 샘플 Entity 만들기
project main 폴더에 domain 폴더를 만들고, Board.java파일을 생성한다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_016.png)

```java
@Entity
public class Board {
    @Id
    @GeneratedValue
    @Column(name="board_id")
    private Long id;
    private String subject;
    @Lob
    private String content;
    private String author;
    private String password;

    private LocalDateTime createdTime;
    private LocalDateTime adjustedTime;
}
```

##3. h2database에 자동 테이블 생성 확인
Entity 클래스를 이용해 자동으로 생성된 Table을 확인할 수 있다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_017.png)




