---
layout: posts
title:  "미유 미니 spec 및 개발을 위한 환경 설정"
categories : [miyoomini]
tags : [miyoomini]
---

### 제품 스펙

- Model: MIYOO mini.
- Screen size: 2.8-inch IPS screen 640*480 resolution.
- Operating system: Linux.
- Sensor: Vibration motor.
- CPU: ARM Cortex-A7 dual-core 1.2G.
- Color: Retro grey, white.
- Size: 93.5mmX65mmX18mm.
- Storage expansion: 32GB MicroSD (TF) Card Supports a maximum expansion of 128GB.

### 크로스 컴파일을 위한 docker 기반 툴체인 설치

[툴체인](https://github.com/shauninman/union-miyoomini-toolchain)
아래와 같이 도커 컨테이너를 생성하여 독립된 작업환경을 구성합니다.

    git clone https://github.com/shauninman/union-miyoomini-toolchain
    sudo make shell

### docker 기반에서 빌드시 주의사항

- 패키지 설치전에 한상 repository를 update 해준다.
- 반복해서 필요한 패키지는 Dockerfile 을 업데이트 해준다.
    아래와 같이 초기화 해준다.

        apt update   
        apt install git

- 빌드시 gcc 등의 경로를 arm compiler로 변경해 준다.
    Makefile 내용을 아래와 같이 변경해 준다.

        TOOLCHAIN =arm-linux-gnueabihf-
        CC          = $(TOOLCHAIN)gcc
        CCP         = $(TOOLCHAIN)g++
        LD          = $(TOOLCHAIN)gcc

configure 로 Makefile을 생성할 수 있는 경우에는 아래와 같이 설정 한다.

    ./configure --host=arm-linux-gnueabihf

- build 후 생성된 파일이 arm 용인지 확인 할 것 (개별 설정을 하지 않으면 x86 format으로 생성이 됩니다.)
    아래와 같이 한번더 확인해 준다.

        file <생성된 파일>
        readelf -d <생성된 파일>

### miyoo mini 관련 홈페이지

[미유미니관련홈페이지](https://github.com/TriForceX/MiyooCFW/wiki/Miyoo-Mini)
