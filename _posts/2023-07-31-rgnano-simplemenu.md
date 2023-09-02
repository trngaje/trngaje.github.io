---
layout: posts
title:  "rgnano simplemenu 구동"
comments: true
categories : [rgnano]
tags : [rgnano]
---

rgnano는 240x240 해상도를 갖고 있다. 하지만 simplemenu는 320x240, 640x480 테마만 가지고 있다.
rgnano에서 simplemenu 프론트엔드를 적용하기 위해서 기존 테마를 가공한 필요가 있었다.

### 테마 변경

320x240 을 240x240으로 비율 유지 하지 않고 늘리거나

    convert background.png -crop 240x240+0+0 background240240.png

320x240 을 왼쪽 위 모서리에서 240x240 크기로 자르는 방법을 사용 했다.

    convert "$org" -resize 240x240\! "$org"

### 펑키 메뉴 적용

picoarch / gmenu2x funkey 버젼을 참고하여 구현 했습니다.
/mnt 경로 내에 리소스를 사용하고 있어 usb mount 기능이 동작하지 않음
frontend 간 전환 되도록 수정함

### 다국어 스크립트 구현

config.ini 내 "language_file" 옵션 추가함.
kor.png 파일을 읽어서 영어 대신 표시해준다.

    [GENERAL]
    language_file = kor

kor.lang 파일 형식은 아래와 같다. <영문>=<번역스크립트>

    Confirm=선택
    Back/Function key=취소/기능
    Mark favorite=즐겨찾기 표시

영어도 필요한 경우 파일(예,eng.lang) 파일을 만들어서 재 정의 할 수 있습니다.
설정 메뉴 상에 english 언어 설정을 표시하기 위해서는 내용 없는 english.lang 파일을 같이 포함해 주면 표시됨. 이름의 고정되어 있지 않기 때문에 원하는 이름으로 *.lang 파일을 구성하면 됩니다.

### 기본 설정

app 내 snapshot 이미지를 표시하려면
.simplemenu/apps 폴더 snap 표시 기준
.opk .fgl 모두 파일명과 동일한 이름의 .png가 snap 폴더에 존재하면 표시함

    retroarch.fgl
    title=레트로아크
    exec=/mnt/bin/retroarch
    snap/retroarch.png (ok)

    st_funkey-s.opk
    snap/st_funkey-s.png (ok)

    DinguxCommander.fgl
    title=커맨더
    exec=/mnt/Applications/commander-funkey-s.opk
    snap/commander-funkey-s.png (ok)

### 폴더 구조

    /usr/games/
    	simplemenu
      invoker
    	resources/
    		akashi.ttf

    /usr/local/sbin
      frontend

    $HOME/.simplemenu/
    	apps/
    	games/
    	rom_preferences/
    	section_groups/
    	themes/
    	tmp/
    	config.ini


### 소스 빌드

    libini -> libopk -> invoker -> simplemenu 순으로 빌드 한다. 아래 libini, libok 빌드는 pre_simpemenu.sh 에서 정의해 놓았습니다.

> ./pre_simplemenu.sh

    #!/bin/bash

    sudo apt update
    sudo apt install -y cmake

    git clone https://github.com/pcercuei/libini.git
    cd libini
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

    cd ..
    cd ..

    rm -rf libini

    git clone https://github.com/pcercuei/libopk.git
    cd libopk
    mkdir build
    cd build
    cmake ..
    make
    sudo make install

    cd ..
    cd ..

    rm -rf libopk

> build invoker

    git clone https://github.com/trngaje/invoker.git
    cd invoker/invoker

funkey 용으로 빌드 하기 위해서는

    make PLATFORM=funkey

anbernic 용으로 빌드 하기 위해서는

    make PLATFORM=rgnano

> build simplemenu

    git clone -b kor https://github.com/trngaje/simplemenu.git
    cd simplemenu/simplemenu

funkey 용으로 빌드 하기 위해서는

    make PLATFORM=funkey

anbernic 용으로 빌드 하기 위해서는

    make PLATFORM=rgnano  


### 참고

[https://trngaje.github.io/miyoomini/miyoomini-simplemenu/](https://trngaje.github.io/miyoomini/miyoomini-simplemenu/)
