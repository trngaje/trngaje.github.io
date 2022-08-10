---
layout: posts
title:  "3ds emus3ds(nes,md,pce) 한글 리스트 구현"
comments: true
categories : [3ds]
tags : [3ds]
---

### 개요

emus3ds 는 nes, md, pce 구동을 위한 stand-alone 에뮬레이터 입니다.
구동 성능은 만족스럽지만 파일리스트가 영어로만 표시되어 개선해 봅니다.

### 수정 내용

1. 한글리스트 표시

### toolchain 설치

emus3ds 를 빌드하기 위한 toolchain 을 설치한다.

> linux 환경에서 docker 설치

    git clone https://github.com/trngaje/3ds-dockers.git
    cd 3ds-dockers/3ds_r45
    ./build_container.sh
    ./run_container.sh

> windows 환경에서 msys2 와 toolchain을 설치한다.

1. msys2 최신 버젼을 설치한다.

msys2-x86_64-20220603.exe

2. c 드라이브에 devkitPro 폴더를 생성하고 devkitARM 을 설치한다.

devkitARM_r45-win32.exe

4. 환경변수를 설정한다.

       export DEVKITPRO="/c/devkitPro"   
       export DEVKITARM="$DEVKITPRO/devkitARM"
       export CTRULIB="$DEVKITPRO/libctru"

5. libctru를 설치한다.

 /c/devkitPro/libctru 폴더를 생성한다.

        tar -xvzf libctru-1.0.0.tar.gz
        cd libctru-1.0.0/libctru
        pacman -Sy
        pacman -S make
        pacman -S git
        make
        cp -rv include /c/devkitPro/libctru/
        cp -rv lib /c/devkitPro/libctru/

6. citro3d-1.0.0.tar.bz2

        tar -xvf citro3d-1.0.0.tar.bz2 -C /c/devkitPro/libctru/

### 빌드 방법

한글리스트 반영된 소스 위치는 추후 업데이트 예정입니다.

    git clone -b kor https://github.com/trngaje/emus3ds.git

> md 에뮬 빌드

    make -f picodrive-make

> pce 에뮬 빌드

    make -f temperpce-make

> nes 에뮬 빌드

참고) nes는 linux 환경에서는 windows.h 파일이 존재하지 않아 빌드되지 않습니다.

    make -f virtuanes-make


### 문제 해결

1. picodrive-make 시 아래 오류가 발생하면
https://github.com/bubble2k16/emus3ds/issues/8
/src/cores/picodrive/pico/carthw/svp/ 경로 내
`stub_arm.S`  을  `stub_arm.s` 로 변경한다.

2. virtuanes-make 을 빌드하기 위해서는
windows.h 가 필요하다. - windows 상 msys2 환경에서 빌드 가능함
