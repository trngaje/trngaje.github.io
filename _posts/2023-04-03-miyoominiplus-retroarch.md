---
layout: posts
title:  "miyoo mini + retroarch 빌드"
comments: true
categories : [miyoominiplus]
tags : [miyoominiplus]
---

docker 환경에서 빌드 합니다.


## build 방법

    git clone -b rgui_kor_bitmap_font_update https://github.com/trngaje/RetroArch.git

    apt update
    apt-get install libsdl-gfx1.2-dev:armhf libsdl1.2-dev:armhf libsdl-image1.2-dev:armhf libsdl-mixer1.2-dev:armhf

    CFLAGS='-marm -mtune=cortex-a7 -march=armv7ve+simd -mfpu=neon-vfpv4 -mfloat-abi=hard -ffast-math -fomit-frame-pointer' ./configure --host=arm-linux-gnueabihf \
     --disable-x11 --disable-caca --disable-opengl --disable-alsa  --disable-pulse --enable-zlib --disable-xmb --disable-ozone --disable-tinyalsa --enable-neon --enable-floathard \
     --disable-nearest_resampler --disable-stb_font --disable-materialui --enable-overlay
    make -f Makefile.miyoomini

## overlay 표시 방법

원하는 overlay 파일(.cfg)을 입력한다.
core 별로 지정할 수 있다.

GBA

    input_overlay = "/mnt/SDCARD/.config/retroarch/overlay/GBA/GBA+3x+G1.cfg"

오버레이 화면

GBC

    input_overlay = "/mnt/SDCARD/.config/retroarch/overlay/GB-GBC/GBCL1.cfg"

오버레이 화면
