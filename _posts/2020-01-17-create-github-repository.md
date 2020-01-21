---
layout: post
title:  "github repository 사용하기"
date:   2020-01-17 22:00:00 +0900
categories: git github
---
github를 사용하기 위해 필요한 기본적인 설정 방법과 유용한 github desktop에 대하여 알아보자.

## 1. git 설치하기
github는 단순히 원격 저장소이므로 github를 사용하기 위해서는 git이 설치되어 있어야 한다.

[https://www.git-scm.com/](https://www.git-scm.com/)에서 다운받을 수 있다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_004.png)


## 2. github에 repository 추가하기
github에서 회원가입을 완료한다. 그리고 메인 페이지에서 add버튼을 눌러 repository를 추가 페이지로 이동.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_001.png)

repository 명과 설명을 작성한다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_002.png)

## 3. Repository Clone(내려받기)
생성된 Repository로 들어가서 `Clone or download` 버튼을 클릭한다. 해당 Repository의 url을 복사한다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_005.png)

## 4-1. git 명령을 이용한 내려받기
Git용 유틸리티 없이 git 명령을 이용하여 clone하는 방법을 먼저 알아보자.
repository용 디렉토리를 생성 후 해당 위치에서 clone을 한다.
```
mkdir temp_repo
cd temp_repo
git clone https://github.com/geeshow/openBank.git ./
```
![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_009.png)

## 4-2. Github Desktop 이용하기
Git 명령을 GUI로 제공하는 다양한 프로그램 중 Github Desktop은 단순한 인터페이스와 github에서 제공하는 프로그램이다.

[https://desktop.github.com/](https://desktop.github.com/)에서 다운받을 수 있다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_003.png)

github 계정으로 로그인하면 자신의 repository 목록을 확인할 수 있으며, Clone도 가능하다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_007.png)

다른 사람의 Repository를 Clone하기 위해서는 URL을 직접 입력해야 한다.

![](https://raw.githubusercontent.com/geeshow/geeshow.github.io/master/images/2020-01-17_006.png)




