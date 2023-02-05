---
layout: posts
title:  "ogu attractmode"
comments: true
categories : [ogu,attract,sfml]
tags : [ogu,odroid go ultra,오드로이드,attractmode]
---

ogu 장치에 맞게 attracmode 구동을 위한 sfml 라이브러리를 변경합니다.

### sfml 빌드

    sudo apt-get install cmake libx11-dev libx11-xcb-dev libflac-dev libogg-dev libvorbis-dev libopenal-dev libfreetype6-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util0-dev libxcb-ewmh-dev libxcb-keysyms1-dev libxcb-icccm4-dev libudev-dev libavutil-dev libavcodec-dev libavformat-dev libavfilter-dev libswscale-dev libavresample-dev libfontconfig1-dev


    git clone -b ogu https://github.com/trngaje/sfml-pi.git
    cd sfml-pi
    mkdir build
    cd build

    cmake .. -DSFML_OGU=1
    make -j4
    sudo make install
    sudo ldconfig

    sudo cp -P ../sfml-pi/build/lib/libsfml* /usr/lib/aarch64-linux-gnu/


### attract 빌드

    sudo apt-get install libjpeg-dev
    git clone -b ogu https://github.com/trngaje/attract.git
    cd attract
    make USE_GLES=1 -j4

### 9p theme 설치

    git clone -b ogu https://github.com/trngaje/attract_9pconfig.git
    cd attract_9pconfig
    ./install.sh

![](/images/2023-02-05/am_9p.jpg)

### 부가 설정

시간 표시를 제대로 하려면 timezone 한국으로 설정함

    sudo timedatectl set-timezone Asia/Seoul

configure 메뉴 한글 표시를 하기 위해서는
attract 9p theme 사용시 폰트 위치

    ./usr/share/fonts/truetype/BinggraeMelona.ttf

![](/images/2023-02-05/am_config.jpg)
