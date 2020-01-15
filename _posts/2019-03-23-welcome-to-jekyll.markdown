---
layout: post
title:  "Cannot resolve table/Cannot resolve column 해결하기"
date:   2020-01-15 21:03:36 +0900
categories: IntelliJ
---
IntelliJ Community에서 ulitimate로 변경 후 JPA에 명명된 테이블과 컬럼을 아래의 이미지 같이 인식하지 못하는 문제가 있는 경우 설정 방법.
![테이블명과 컬럼을 인식 못하고 있다](../images/2020-01-15_001.png)

##1. Data Source 추가하기
우선 Data Source가 등록되어 있는지 확인하고, 등록된 Data Source가 없을 경우 추가를 해줘야 한다.
![Data Source 추가하기1](../images/2020-01-15_002.png)

![Data Source 추가하기2](../images/2020-01-15_003.png)

##2. Persistence에 Assign Data Source 설정하기
Persistence에서 오른쪽 마우스 버튼을 눌러 Assign Data Source를 클릭
![Assign Data Source 설정](../images/2020-01-15_004.png)

적당한 Data Source를 선택
![Data Source 선택](../images/2020-01-15_005.png)

