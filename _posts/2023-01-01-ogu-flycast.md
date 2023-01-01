---
layout: posts
title:  "ogu flycast"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,flycast]
---

libretro core github에 있는 flycast 동작이 이상합니다.
개선을 위해 보다 최신 github 소스를 사용해 봅니다


    https://github.com/flyinghead/flycast.git
    git submodule init
    git submodule update

    mkdir build
    cd build

> libretro core 빌드는..

    cmake .. -DCMAKE_BUILD_TYPE=Release -DLIBRETRO=ON -DUSE_OPENMP=OFF -DUSE_GLES2=ON
    make

> standalone 빌드는..

    sudo apt install -y cmake libsdl2-dev curl libcurl4-openssl-dev
    cmake .. -DCMAKE_BUILD_TYPE=Release -DUSE_OPENMP=OFF  -DUSE_GLES=ON
    make


-DUSE_GLES2=ON 옵션이 있으면 (해결됨)

    00:00:744 rend/gles/gles.cpp:550 N[RENDERER]: OpenGL ES version 3.2
    00:00:758 hw/mem/_vmem.cpp:494 N[VMEM]: Info: nvmem is enabled, with addr space of size 4GB
    00:00:772 hw/mem/_vmem.cpp:589 N[VMEM]: BASE 0x7ea8fe0000 RAM(16 MB) 0x7f34fe0000 VRAM64(8 MB) 0x7f2cfe0000 ARAM(2 MB) 0x7f297e0000
    00:00:783 hw/mem/_vmem.cpp:494 N[VMEM]: Info: nvmem is enabled, with addr space of size 4GB
    00:00:792 hw/mem/_vmem.cpp:589 N[VMEM]: BASE 0x7ea8fe0000 RAM(16 MB) 0x7f34fe0000 VRAM64(8 MB) 0x7f2cfe0000 ARAM(2 MB) 0x7f297e0000


bios는 아래 위치에 복사해 넣어야 함

    mv ~/.config/reicast/dc_boot.bin ~/.local/share/reicast/


CMakefilelist.txt  내용은 아래와 같습니다.

    option(ENABLE_CTEST "Enables unit tests" OFF)
    option(ENABLE_OPROFILE "Enable OProfile" OFF)
    option(TEST_AUTOMATION "Enable test automation" OFF)
    option(ENABLE_LOG "Enable full logging" OFF)
    option(ASAN "Enable address sanitizer" OFF)
    option(USE_GLES "Use GLES[3] API" OFF)
    option(USE_GLES2 "Use GLES2 API" OFF)
    option(USE_HOST_LIBZIP "Use host libzip" ON)
    option(USE_OPENMP "Use OpenMP if available" ON)
    option(USE_VULKAN "Build with Vulkan support" ON)
    option(LIBRETRO "Build libretro core" OFF)
    option(USE_OPENGL "Use OpenGL API" ON)
    option(USE_VIDEOCORE "RPI: use the legacy Broadcom GLES libraries" OFF)
    option(APPLE_BREAKPAD "macOS: Build breakpad client and dump symbols" OFF)


이슈

1. 바로 실행한 경우 flycast core가 실행되지 않음
- console 에서 실행한 경우에만 flycast core 가 실행됨
2. flycast 종료 키 입력 시 종료되지 않음 (1회)

빌드에러 (해결됨)

    home/odroid/export/flycast/core/hw/pvr/Renderer_if.cpp: In function ‘void rend_create_renderer()’:
    /home/odroid/export/flycast/core/hw/pvr/Renderer_if.cpp:161:2: error: expected primary-expression before ‘}’ token
      161 |  }
          |  ^

; 하나 추가해서 빌드


코어 종료시 아래와 같은 에러 문구 출력됨

    [INFO] [Core]: Saved core options file to "/home/odroid/.config/retroarch/retroarch-core-options.cfg".
    [INFO] [Video]: Does not have enough samples for monitor refresh rate estimation. Requires to run for at least 4096 frames.
    sh: 0: getcwd() failed: No such file or directory
