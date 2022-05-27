---
layout: posts
title:  "miyoo-mini hocoslamfy 적용"
---

### 개요

hocoslamfy 는 Flappy Bird와 유사한 게임이며 dingux 로 소스가 오픈되어 있는 홈브류게임입니다.

미유미니에 맞게 작업했습니다.

### 구현 된 기능

- 키값 미유 미니에 맞게 변경
- libshake 함수 대신 간단한 모터 동작으로 진동 구현


### 사용 방법

> 기본 키.

L : 진동 켜기 끄기

R : 점수 표시 위치 변경

Start : 게임시작/일시 정지

Function : 게임 종료

A,B,X,Y : 벌을 위로 날개짓

![](/images/2022-05-28/hocoslamfy_start.png)
![](/images/2022-05-28/hocoslamfy_ingame.png)


> 하이 스코어

대나무에 부딫히거나 추락하면 현재 점수와 최고 점수가 같이 표시됩니다.
![](/images/2022-05-28/hocoslamfy_highscore.png)


### 이미 빌드된 자료 다운로드

아래 경로의 파일 압축을 풀어 각각의 경로에 복사해서 사용합니다.

[https://github.com/trngaje/miyoo-mini/releases/tag/hocoslamfy_220527](https://github.com/trngaje/miyoo-mini/releases/tag/hocoslamfy_220527)

> 1.제조사 이미지에서는..

위 경로에서 압축파일(hocoslamfy_220527_1.zip)을 다운로드 받은 후 SD Card의 App 폴더에 hocoslamfy 폴더를 복사합니다. 


> 2.simplemenu 이미지에서는

위 압축파일을 다운로드 받은 후 SD Card의 App 폴더(없으면 만들어서) hocoslamfy 폴더를 복사합니다.

hocoslamfy.fgl과 /snap  폴더를 SD card의 .simplemenu/games/ 에 복사합니다.


### 미유미니 툴체인에서 빌드하는 방법
    
    git clone -b miyoomini https://github.com/trngaje/hocoslamfy.git
    make platform=miyoomini
