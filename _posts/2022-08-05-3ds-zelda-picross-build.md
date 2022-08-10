---
layout: posts
title:  "3ds zelda picross 빌드"
comments: true
categories : [3ds]
tags : [3ds]
---

### 개요

zelda picross 는 홈브류 게임입니다.
3ds 용으로 빌드하기 위해서는 빌드 방법이 다소 복잡해서 미리 빌드된 라이브러리 포함해서 docker를 구성했습니다.


### 수정 내용

1. 한글 스크립트 표시
2. 기본 시간 x 3배로 설정함
3. 새로운 판 시작 후 A 버튼 입력 전까지 버튼 눌림 안되는 오류 임시 수정
4. -D_3DS 구문으로 기존 sdl2 및 미유미니 대상 소스에 병합함

### toolchain 설치

zelda picross 를 빌드하기 위해서는 아래 라이브러리가 설치 되어 있어야 합니다.
- devkitARM r46
- libctru v1.2.0
- citro3d v1.2.0
- zlib
- libjpeg-turbo
- libpng
- sf2dlib
- sfillib

zelda picross 를 빌드하기 위한 toolchain 을 설치한다.

    git clone https://github.com/trngaje/3ds-dockers.git
    cd 3ds-dockers/docker4zeldapicross
    ./build_container.sh
    ./run_container.sh

### 빌드 방법

    git clone -b sdl2_kor https://github.com/trngaje/zeldapicross_sdl2.git
    make -f Makefile.ctr cia

### 구현 화면
