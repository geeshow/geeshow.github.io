---
layout: post
title:  "Docker 명령문 정리"
date:   2020-01-13 21:03:36 +0900
categories: Docker
---
Docker에서 자주 사용되는 명령문

## Docker image 명령
- 이미지 검색
`docker search ubuntu`

- 이미지 다운로드
`docker pull ubuntu:latest`

- 저장된 이미지 목록 확인
`docker images`

- 이미지 삭제
docker rmi ubuntu:latest

- 이미지로부터 컨테이너 생성
```
docker run -i -t --name 컨테이너명 ubuntu /bin/bash
docker run --name 컨테이너명 -d -p 80:80 -v hostdata:/data hello:0.1
```
-d 백그라운드 실행
-p 80:80 호스트와 컨테이너의 포트를 연결하고 외부에 노출
-v 호스트와 컨테이너 공유 폴더

- history명령 
`docker history hello:0.1`
이미지 이름을 사용


## Docker container 명령
- 등록된 컨테이너 확인
`docker ps -a`

- start/restart/attach/stop/remove containers
`docker start 컨테이너명`
`docker restart 컨테이너명`
`docker attach 컨테이너명`
`docker stop 컨테이너명`
`docker rm 컨테이너명`

- 컨테이너에 명령 실행
`docker exec 컨테이너명 echo "Hello World"`

- cp 컨테이너에서 파일 꺼내기
`docker cp 컨테이너명:대상파일위치 저장위치`
`docker cp hello-nginx:/etc/nginx/nginx.conf ./`


## Image 생성
- build로 이미지 생성하기
`docker build --tag hello:0.1 .`
현재 디렉토리에 Dockerfile이 존재 해야함

- commit 명령으로 이미지 생성
docker commit -a "drken <ken2484@gmail.com>" -m "add message.txt" hello-nginx hello:0.2

- diff 변경파일 확인 (이미지 -> 컨테이너)
A:추가된 파일
C:변경 파일
D:삭제 파일

## Dockerfile 샘플
```
FROM ubuntu:14.04
MAINTAINER geeshow <geeshow@이메일.com>

RUN apt-get update
RUN apt-get install -y nginx
RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf
RUN chown -R www-data:www-data /var/lib/nginx

VOLUME ["C:\projects\docker\data", "/etc/nginx/site-enabled", "/var/log/nginx"]

WORKDIR /etc/nginx

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
```