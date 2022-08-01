---
layout: posts
title:  "miyoo-mini gmu 한글 적용"
categories : [miyoomini]
tags : [miyoomini]
---

### 개요

gmu는 mp3 등의 파일을 플레이할 수 있는 프로그램 입니다. shauninman 이 작성한 custom sdl1.2 과는 다른 custom sdl을 사용합니다.
(shauniman 의 sdl1.2로는 mp3 list가 표시되지 않는 것을 확인합니다.)

### 구현 된 기능

- 한글 출력되도록 수정


### 사용 방법

> 기본 키.

Start 키를 눌러 표시 화면을 바꿉니다.

하단에 기능키 설명이 표시됩니다.

Menu 키를 누르면 Menu 키를 함께 눌렀을 때의 기능 설명이 표시됩니다.

Menu + Start 키를 같이 누르면 Gmu 가 종료됩니다.

![](/images/2022-05-01/gmu_filelist.png)

![](/images/2022-05-01/gmu_playlist.png)

![](/images/2022-05-01/gmu_trackinfo.png)



### 설정 값 변경

> 기본 폴더는 SD카드 내 Media 폴더를 처음 표시합니다. 바꾸고 싶으면 설정파일(gmu.miyoo.conf)에서 바꾸어 주세요.

    Gmu.DefaultFileBrowserPath=/mnt/SDCARD/Media

> 테마 파일은

themes/default-modern-large/

theme.conf 에 정의되어 있는 파일을 사용합니다.

리스트 표시를 위한 흰색, 파란색 폰트 파일(.png)이 존재하며 각각

    letters_large_white.png

    letters_large_blue.png

상단의 곡 정보는 대문자로만 구성되어 있는 폰트를 사용합니다.

    letters_lcd_2.png


### 이미 빌드된 자료 다운로드

아래 경로의 파일 압축을 풀어 각각의 경로에 복사해서 사용합니다.
(자료 중복되지 않고 아래 링크 임시로 사용합니다.)
[https://cafe.naver.com/moopung/92112](https://cafe.naver.com/moopung/92112)
> 1.제조사 이미지에서는..

아래 압축파일(gmu220501.zip)을 다운로드 받은 후 SD Card의 App 폴더에 Gmu 폴더를 복사합니다.


> 2.simplemenu 이미지에서는

위 압축파일을 다운로드 받은 후 SD Card의 App 폴더(없으면 만들어서) Gmu 폴더를 복사합니다.
(폴더위치는 원하는 위치로 변경하면 됩니다.)

gmu.fgl 과 /snap 폴더를 SD card의 .simplemenu/apps/ 에 복사합니다.

Gmu 폴더를 임의의 폴더에 복사하고 싶으면 gmu.fgl 에서 경로를 수정하면 됩니다.

    title=뮤직플레이어
    exec=/mnt/SDCARD/App/Gmu/gmu.sh


### 툴체인에서 빌드하는 방법

    git clone -b kor https://github.com/trngaje/gmu.git
    apt install libwavpack-dev:armhf
    ./build_miyoo.sh

빌드를 하면
frontends/sdl.so 와 gmu.bin 이 생성된다.
