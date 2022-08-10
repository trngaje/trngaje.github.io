---
layout: posts
title:  "3ds retroarch 개발환경"
comments: true
categories : [3ds]
tags : [3ds]
---

### 개요

3ds 는 코어와 retroarch 가 합쳐져서 빌드가 되는 구조 입니다.
코어가 동적 라이브러리가(.so) 아닌 정적 라이브러리(.a) 형태로 컴파일 됩니다.
retroarch 빌드시에는 적어도 1개 이상의 코어가 포함되어 한다는 의미입니다.

### toolchain 설치

retroarch 를 빌드하기 위한 toolchain 을 설치한다.

    git clone https://github.com/trngaje/3ds-dockers.git
    cd 3ds-dockers/docker4retroarch
    ./build_container.sh
    ./run_container.sh

### 빌드 방법

> core 1개만 빌드하는 경우

우선 코어 하나를 빌드합니다. 예를들어 nes9x_2005 이면 전체 파일 명은 snes9x2005_libretro_ctr.a 이 됩니다.
이름을 libretro_ctr.a 로 바꾸고 빌드 합니다.

    make -f Makefile.ctr.salamander USE_CTRULIB_2=1
    make -f Makefile.ctr USE_CTRULIB_2=1


> 여러개의 core를 각각의 이름으로 빌드하는 경우

미리 빌드해 놓은 코어를 이름 변경 없이 retroarch 소스 내 /dist_scripts/ 에 복사합니다.
그 다음에 다음과 같이 빌드 합니다.

    export USE_CTRULIB_2=1
    ./dist_cores.sh ctr

> 코어 는 아래와 같이 빌드 합니다.

    make platform=ctr
