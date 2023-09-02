---
layout: posts
title:  "rgnano build 환경"
comments: true
categories : [rgnano]
tags : [rgnano]
---

rgnano 용 이미지는 2종 (anbernic 생산 버젼, funkey DrUm78 custom 버젼) 존재합니다.
빌드 환경이 서로 다르기 때문에 각각 빌드된 프로그램은 실행되지 않음

1.생산 버젼(anbernic)

anbernic 생산 버젼에서는 miyoomini 와 동일한 빌드 환경으로 구동이 가능합니다.

기존에 알려져 있는 miyoo sdk 로는 musl 로 shared 속성이 없기 때문에 libretro core (.so)가 빌드 되지 않습니다.

[https://github.com/MiyooCFW/toolchain/releases/download/v2.0.0/miyoo-toolchain-v2.0.0-arm-buildroot-linux-musleabi_sdk-buildroot.tar.gz ](https://github.com/MiyooCFW/toolchain/releases/download/v2.0.0/miyoo-toolchain-v2.0.0-arm-buildroot-linux-musleabi_sdk-buildroot.tar.gz )

아래와 같이 환경 설정 해서 사용하면 된다고 나와있다

    CROSS_ROOT=/opt/miyoo
    CROSS_TRIPLE=arm-buildroot-linux-musleabi
    SYSROOT="${CROSS_ROOT}/${CROSS_TRIPLE}/sysroot"
    PATH="${CROSS_ROOT}/bin:${SYSROOT}/usr/bin:${PATH}"
    ARCH=arm
    CROSS_COMPILE="${CROSS_TRIPLE}-"

물론 advmame/advmenu/st 와 같은 어플리케이션은 빌드 후 기기에서 정상 구동 확인했습니다.

기본 미유미니의 툴체인은 아래와 같습니다.

[https://github.com/shauninman/union-miyoomini-toolchain](https://github.com/shauninman/union-miyoomini-toolchain)

아래와 같이 바로 docker 환경에서 빌드가 가능합니다.

    sudo apt install make
    sudo make shell

위 환경은 miyoomini용 라이브러리가 포함되어 있기 때문에 싹 지우고 새로 환경을 만들었습니다.

필요한 패키지는 sudo apt install 커맨드로 추가 하면 됩니다.

아래는 retroarch 빌드 방법 입니다.

    sudo apt-get install libsdl-gfx1.2-dev:armhf libsdl1.2-dev:armhf libsdl-image1.2-dev:armhf libsdl-mixer1.2-dev:armhf

    CFLAGS='-marm -mtune=cortex-a7 -march=armv7ve+simd -mfpu=neon-vfpv4 -mfloat-abi=hard -ffast-math -fomit-frame-pointer' ./configure --host=arm-linux-gnueabihf \
     --disable-x11 --disable-caca --disable-opengl --disable-alsa  --disable-pulse --enable-zlib --disable-xmb --disable-ozone --disable-tinyalsa --enable-neon --enable-floathard \
     --disable-nearest_resampler --disable-stb_font --disable-materialui --enable-overlay


2.funkey custom 버젼

ubuntu20.04 에서 아래 것 사용하면 됩니다.

[https://github.com/DrUm78/FunKey-OS/releases/download/FunKey-OS-DrUm78/FunKey-sdk-2.3.0.tar.gz](https://github.com/DrUm78/FunKey-OS/releases/download/FunKey-OS-DrUm78/FunKey-sdk-2.3.0.tar.gz)

custom sdk라 apt 로 추가 패키지 설치가 안됩니다.

3.빌드 방법

구글상에서 배포되어 있는 docker 환경이 외국 기준이라, build 시 한국 시간과 맞지 않은 부분과 sudo 계정을 user 계정으로 수정해서 구성했습니다.

> anbernic 용 빌드 환경 구성

    wget https://github.com/trngaje/rgnano_binary/raw/master/docker/anbernic/Dockerfile
    wget https://github.com/trngaje/rgnano_binary/raw/master/docker/anbernic/build_container.sh
    wget https://github.com/trngaje/rgnano_binary/raw/master/docker/anbernic/run_container.sh

    ./build_container.sh
    ./run_container.sh

> funkey 용 빌드 환경 구성

    wget https://github.com/trngaje/rgnano_binary/raw/master/docker/funkey/Dockerfile
    wget https://github.com/trngaje/rgnano_binary/raw/master/docker/funkey/build_container.sh
    wget https://github.com/trngaje/rgnano_binary/raw/master/docker/funkey/run_container.sh

    ./build_container.sh
    ./run_container.sh

제가 배포하는 소스의 build 옵션은 혼동을 피하기 위해서 아래와 같이 통일 예정

    rgnano anbernic : rgnano
    rgnano funkey : funkey
