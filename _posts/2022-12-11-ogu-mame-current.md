---
layout: posts
title:  "ogu mame current"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,mame,sdl2]
---

mame는 빌드시 memory를 많이 필요로 하기 때문에 빌드 중간에 멈추는 현상을 방지하려면 사전에 swap 메모리 확보가 필요합니다.


### standalone build

    git clone https://github.com/mamedev/mame.git
    make SUBTARGET=arcade


### libretro core build

    sudo apt-get install libsqlite3-dev
    git clone https://github.com/libretro/mame.git
    make -f Makefile.libretro


### 메모리 확보

    sudo fallocate -l 8G /swapfile
    sudo chmod 600 /swapfile
    sudo mkswap /swapfile
    sudo swapon /swapfile

    odroid@gou:/mnt/sdcard/roms/wonderswan$ free
                  total        used        free      shared  buff/cache   available
    Mem:        1908424      359160      960640        3188      588624     1522008
    Swap:       8388604           0     8388604


standalone build 12시간 소요됨, 아래와 같은 에러로 구동되지 않음

    odroid@gou:~/develop/mame$ ./mamearcade
    INFO: Created pixmap handle 1
    INFO: Created pixmap handle 2
    INFO: Created pixmap handle 3
    WARNING: lavapipe is not a conformant vulkan implementation, testing use only.
    Segmentation fault
    odroid@gou:~/develop/mame$




        sudo modprobe zram
        sudo zramctl --find --size 4096M
        sudo mkswap /dev/zram0
        sudo swapon /dev/zram0
        sudo fallocate -l 1G /swapfile
        sudo mkswap /swapfile
        sudo swapon /swapfile

    참고)



    sdcard에 8g swap 공간을 잡기 위해서는 아래 내용을 참고합니다.

    https://psychoria.tistory.com/717

        sudo free -m
        sudo swapon -s
        sudo dd if=/dev/zero of=/mnt/sdcard/swapfile bs=1M count=8192

    이게 안먹어서
    <s> sudo fallocate -l 8G /mnt/sdcard/swapfile </s>

        sudo chmod 600 /mnt/sdcard/swapfile
        sudo mkswap /mnt/sdcard/swapfile
        sudo swapon /mnt/sdcard/swapfile

        make -f Makefile.libretro
