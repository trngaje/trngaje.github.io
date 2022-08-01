---
layout: posts
title:  "miyoo-mini st 적용"
categories : [miyoomini]
tags : [miyoomini]
---

### 개요

st 는 sdl 기반의 simple terminal 입니다. 가상 키보드를 사용하여 터미널 입력을 할 수 있습니다.
retroarch rgui 영어 폰트와 갈무리 한글 폰트를 사용하여 수정 했습니다.


### 구현 된 기능

- miyoo mini 에서 화면 반전 없이 표시되도록 수정

- 키 값 miyoo mini 에 맞게 수정

- 영어/한글 폰트 적용


### 사용 방법

> 처음 실행하면 조작 방법 설명이 표시된다.

![](/images/2022-03-09/miyoo_mini_st_help.png)

> X 버튼을 누르면 가상키보드를 표시/혹은 감출 수 있다.

![](/images/2022-03-09/miyoo_mini_st_1.png)

![](/images/2022-03-09/miyoo_mini_st_2.png)

> Y 버튼을 누르면 가상키보드를 위 혹은 아래에 표시할 수 있다.

![](/images/2022-03-09/miyoo_mini_st_3.png)

> ls 커맨드로 현재 디렉토리 정보를 확인 할 수 있다.

![](/images/2022-03-09/miyoo_mini_st_4.png)

> 외부 프로그램 evtest를 실행하여 키보드 버튼 입력 값을 확인 할 수 있다.

![](/images/2022-03-09/miyoo_mini_st_5.png)


### 이미 빌드된 자료 다운로드

아래 경로의 st.zip 압축을 풀어 App 경로에 복사해서 사용합니다.

[https://github.com/trngaje/miyoo-mini/releases/tag/st_v.1.0](https://github.com/trngaje/miyoo-mini/releases/tag/st_v.1.0)


### 툴체인에서 빌드하는 방법

    git clone -b kor https://github.com/trngaje/st-sdl.git
    make


### 라이센스

MIT License



### 폰트 라이센스

영어: retroarch rgui 5x10 폰트

한글: 갈무리11x11 폰트

[https://github.com/quiple/galmuri](https://github.com/quiple/galmuri)
