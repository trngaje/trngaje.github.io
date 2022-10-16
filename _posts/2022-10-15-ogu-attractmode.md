---
layout: posts
title:  "ogu attractmode"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,attractmode]
---

기존 ogs용으로 libgo2 를 사용하여 작업되어 있는 것을 libgou로 변경합니다.

sfml 빌드

    sudo apt-get install cmake libx11-dev libx11-xcb-dev libflac-dev libogg-dev libvorbis-dev libopenal-dev libfreetype6-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util0-dev libxcb-ewmh-dev libxcb-keysyms1-dev libxcb-icccm4-dev libudev-dev libavutil-dev libavcodec-dev libavformat-dev libavfilter-dev libswscale-dev libavresample-dev libfontconfig1-dev

    make build
    cd build
    cmake .. -DSFML_RPI=1 -DSFML_OS_LINUX=1 -DSFML_OPENGL_ES=1 -DEGL_INCLUDE_DIR=/usr/include  -DEGL_LIBRARY=/usr/local/lib/aarch64-linux-gnu/libEGL.so -DGLES_INCLUDE_DIR=/usr/include  -DGLES_LIBRARY=/usr/local/lib/aarch64-linux-gnu/libGLESv2.so

    make -j7
    sudo make install
    sudo ldconfig

    sudo cp -P ../sfml-pi/build/lib/libsfml* /usr/lib/aarch64-linux-gnu/


void gou_display_present(gou_display_t* display, gou_surface_t* surface,
            int srcX, int srcY, int srcWidth, int srcHeight, bool mirrorX, bool mirrorY,
            int dstX, int dstY, int dstWidth, int dstHeight);

attract 빌드

    sudo apt-get install libjpeg-dev
    make USE_GLES=1 -j4

빌드는 되지만 Failed to activate the window's context 에러 발생함.
