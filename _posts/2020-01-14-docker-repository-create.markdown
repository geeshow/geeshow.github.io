---
layout: post
title:  "Docker desktop 개인 저장소 구축"
date:   2020-01-14 19:23:36 +0900
categories: Docker
---
## 1.Daemon 설정
Insecure registries에 localhost:5000 등록
![데몬설정](../images/2020-01-13_001.png)

## 2. 레지스트리 서버 받기
`docker pull registry:latest`
![레지스트리 서버 받기](../images/2020-01-13_002.png)

## 3. 컨테이너 실행 하기
`docker run -d -p 5000:5000 —name hello-registry –v C:\projects\docker\registry:/tmp/registry registry`

## 4. 개인 저장소에 이미지 올리기
`docker tag hello:0.1 localhost:5000/hello:0.1`
`docker push localhost:5000/hello:0.1`
![개인 저장소에 이미지 올리기](../images/2020-01-13_003.png)

## 5. 개인 저장소의 이미지를 원격에서 내려받기
`docker pull 저장소_HOST_IP:5000/hello:0.1`