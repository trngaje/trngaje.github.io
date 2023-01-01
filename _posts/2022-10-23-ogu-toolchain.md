---
layout: posts
title:  "ogu toolchain"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,toolchain]
---

pc 환경에서 ogu 소스를 빌드하기 위해서는 toolchain을 설치해야 합니다.
pc에서 빌드해야 하는 소스는 linux kernel과 uboot 이 있습니다.

https://wiki.odroid.com/odroid_go_ultra/software/building_kernel

https://wiki.odroid.com/odroid_go_ultra/software/building_u-boot

pc 환경이라고는 해도 cross compile 설치를 위해서 docker로 구성하는게 편합니다.


    $ git clone --depth 1 https://github.com/hardkernel/linux.git -b odroidgoU-4.9.y
    $ cd linux
    $ make odroidgou_defconfig
    $ make -j$(expr $(expr $(nproc) \* 6) \/ 5)


### mame build

    export ARCH=arm64
    export CROSS_COMPILE=aarch64-linux-gnu-
    export PATH=/opt/toolchains/gcc-linaro-7.4.1-2019.02-x86_64_aarch64-linux-gnu/bin/:$PATH

    cross compile 환경에서 aarch64 라이브러리 설치를 해야 함

    sudo apt-get install software-properties-common
    sudo dpkg --add-architecture aarch64
    sudo apt update
    sudo apt-get install libc6:aarch64
    sudo apt-get install crossbuild-essential-aarch64

    mame standalone 빌드 qt가 있어야 빌드가 가능함
    sudo apt-get install git build-essential python3 libsdl2-dev libsdl2-ttf-dev libfontconfig-dev libpulse-dev qtbase5-dev qtbase5-dev-tools qtchooser qt5-qmake

    standalone 빌드 레트로 아레나에 있는 것 동작확인됨
    나도 한번 빌드 해봅니다.
    git clone https://github.com/mamedev/mame.git
    make SUBTARGET=arcade -j2

    git clone https://github.com/libretro/mame.git
    make -f Makefile.libretro

    sudo apt-get install libsqlite3-dev

    make CROSS_BUILD=1 PLATFORM=arm64 TARGETOS=linux  NOASM=1 NOWERROR=1 ARCHOPTS_CXX=-stdlib=libc++ ARCHOPTS_OBJCXX=-stdlib=libc++ OVERRIDE_CC=aarch64-linux-gnu-gcc OVERRIDE_CXX=aarch64-linux-gnu-g++ ARCHOPTS=-Wl,-R,/opt/toolchains/gcc-linaro-7.4.1-2019.02-x86_64_aarch64-linux-gnu/lib LIBRETRO_CPU=arm64 -f Makefile.libretro -j4

아래와 같은 에러 발생함
aarch64-linux-gnu-g++: error: unrecognized command line option '-m64'

    mame/barcrest/mpu4vid.o ../../../../libretro/obj/libretro/src/mame/barcrest/mpu5.o ../../../../libretro/obj/libretro/src/mame/barcrest/mpu5sw.o
    make[2]: Leaving directory '/home/trngaje/export/mame/build/projects/retro/mame/gmake-linux'
    make[1]: *** [makefile:1348: linux] Error 2
    make[1]: Leaving directory '/home/trngaje/export/mame'
    make: *** [Makefile.libretro:251: build] Error 2
    trngaje@26fe4135239f:~/export/mame$

mame 를 pc cross compile 하기 위해서는

    sudo apt update
    sudo apt install python3
