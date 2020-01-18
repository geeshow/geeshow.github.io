---
layout: post
title:  "spring boot 프로젝트 생성하기 by IntelliJ"
date:   2020-01-18 11:50:00 +0900
categories: springboot intellij
---
github를 사용하기 위해 필요한 기본적인 설정 방법과 유용한 github desktop에 대하여 알아보자.

## 1. spring boot 프로젝트 파일 다운받기
Intellij community 버전에서는 [https://start.spring.io/](https://start.spring.io/)에서 프로젝트 파일을 생성할 수 있다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_001.png)

추가로 프로젝트에서 사용할 의존성을 추가해줘야 한다.
자주 사용되는 의존성을 추가해보겠다.
```
1. Spring Web
2. Spring Data JPA
3. Lombok
4. H2
5. Thymeleaf
```
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_002.png)

`Generate - Ctrl` 버튼을 클릭해서 다운받은 압축된 프로젝트 파일을 원하는 폴더에 풀어준다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_003.png)

## 2. IntelliJ에서 Project 열기
IntelliJ에서 해당 폴더를 Open한다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_004.png)

처음 프로젝트를 Open하면 의존성을 다운받기 위해 약간의 시간이 걸린다. 그리고 아래와 같은 에러 이벤트를 확인할 수 있다.
Lombok을 사용하기 위해서는 Lombok 플러그인 설치가 필요하다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_005.png)

## 3. Lombok 설치하기
Setting > Plugins에서 Lombok Plugin을 설치한다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_015.png)

Setting > Build, Execution... > Compiler > Annotation Processors에서 Enable annotation processing을 체크한다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_006.png)

## 4. 프로젝트 실행하기
프로젝트가 정상적으로 설치되었는지 확인해보자.
아래와 같이 프로젝트를 구동한다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_007.png)

웹프라우저로 `http://localhost:8080`로 Whitelabel Error Page가 확인되면 정상적으로 프로젝트가 설치된 것이다.
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-18_008.png)





