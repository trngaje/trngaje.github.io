---
layout: posts
title:  "ogu basiliskii II 설치"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,basiliskii]
---
machintosh emulator 인 basiliskii 를 ogu에 맞게 빌드 후 사용해 봅니다.

### 빌드 방법

    git clone --recursive https://github.com/cebix/macemu.git

    sudo apt update
    sudo apt install -y libsdl2-dev autoconf automake oss-compat

    cd BasiliskII/src/Unix

    ./autogen.sh --enable-sdl-video --enable-sdl-audio --without-x --without-gtk --with-sdl2 --disable-vosf --disable-jit-compiler --without-mon --without-esd

    make -j4

### 기본 설정

    nano  ~/.basilisk_ii_prefs

아래 내용 추가 기입 합니다. os가 설치되어 있는 hd1.img google 에서 받아서 실행합니다.

    rom    /home/trngaje/Quadra-650.ROM
    frameskip 0
    cpu 4
    model 14
    ramsize 67108864
    disk   /home/trngaje/hd1.img


![](/images/2023-02-25/1.hdd1.jpg)


### 실행

기본 실행

    BasiliskII

특정 디스크 이미지 실행

    BasiliskII --disk MS_Arcade.dsk


### empty hdd image 만드는 방법

    wget -O ~/mkmacdisk.sh https://raw.githubusercontent.com/djdarien/macpi/main/mkmacdisk.sh

아래와 같이 실행 후

    sh ./mkmacdisk.sh

설정 값을 아래와 같이 입력해 줍니다. MacHDD.dsk 파일이 500Mbytes 사이즈로 생성됩니다.

    MacHDD
    500

해당 스크립트는 pi 용으로 설정되어 있어서 odroid 에서 사용하려면 user (odroid) 로 소유자를 변경해주면 됩니다.

스크립트 내용은 아래와 같습니다.

    #!/bin/bash
    sudo apt-get update
    sudo apt-get install hfsutils -y

    echo -n "Enter the name of the disk: "
    read dskname

    echo -n "Enter the size of disk in MB (ie: 100): "
    read size

    sudo dd if=/dev/zero of=$dskname.dsk bs=1M count=$size
    sudo hformat -l $dskname $dskname.dsk

    sudo chown odroid:odroid $dskname.dsk

### 빈 hdd disk 에 os 설치

확인된 이미지는 아래와 같습니다.

    md5sum Quadra-650.ROM
    69489153dde910a69d5ae6de5dd65323  Quadra-650.ROM

     md5sum System753.iso
    e3644a5fb68fd9d7797eef950fd5cac6  System753.iso

    md5sum MacOS8_1.iso
    4a88bcda344356faac62af9749f4c61e  MacOS8_1.iso

1.먼저 System753 을 설치한다. (아래 model 14로 되어 있는 부분은 없는 설정 값이니 무시됩니다.)

    nano ~/.basilisk_ii_prefs


    rom    /home/odroid/Quadra-650.ROM
    frameskip 0
    cpu 4
    model 14
    ramsize 67108864
    disk    /home/odroid/System753.iso
    disk    /home/odroid/MacHDD.dsk

![](/images/2023-02-25/2.system7.5.3boot.jpg)
![](/images/2023-02-25/3.system7.5.3install.jpg)
![](/images/2023-02-25/4.select_hdd.jpg)

.basilisk_ii_prefs 상에 CD 이미지(disk    /home/odroid/System753.iso)는 제거하고 다시 부팅 합니다. 초기화가 완료되면 시스템을 종료합니다.

![](/images/2023-02-25/5.install.jpg)



2.CD 이미지를 OS8.1로 교체(disk    /home/odroid/MacOS8_1.iso)해서 부팅 한 후 OS8.1을 설치한다. (디스크 부팅 순서 중요!) 이 때 OS8.1 인스톨러를 실행하기 위해서 "modelid 14" 값이 설정 되어야 합니다.

    nano ~/.basilisk_ii_prefs

    rom    /home/odroid/Quadra-650.ROM
    frameskip 0
    cpu 4
    modelid 14
    ramsize 67108864
    disk    /home/odroid/MacHDD.dsk
    disk    /home/odroid/MacOS8_1.iso

![](/images/2023-02-25/6.os_8.1boot.jpg)
![](/images/2023-02-25/7.select_hdd.jpg)
![](/images/2023-02-25/8.install.jpg)
![](/images/2023-02-25/9.complete.jpg)
![](/images/2023-02-25/10.reboot.jpg)

### 화면 캡쳐 방법

alt  +shift + 3 을 눌러 캡쳐 할 수 있음.
확장 자 없이 파일이 생성되는데 아래와 같이 변환 가능합니다.

    convert 'Picture 1' games9.png



### xboxdrv 로 마우스 및 키보드 키 값 정의

    xboxdrv --config ~/basiliskii.xboxdrv &

basiliskii.xboxdrv 내용은 아래와 같습니다.

    # for basiliskii
    [xboxdrv]
    evdev = /dev/input/event3
    deadzone-trigger=15%
    deadzone=4000
    mimic-xpad=true
    dpad-as-button=true
    trigger-as-button=true
    mouse=true

    [evdev-keymap]
    BTN_SOUTH=b
    BTN_EAST=a
    BTN_NORTH=x
    BTN_WEST=y
    BTN_TL=lb
    BTN_TR=rb
    BTN_TL2=lt
    BTN_TR2=rt
    BTN_TRIGGER_HAPPY1=guide
    BTN_TRIGGER_HAPPY2=tl
    BTN_TRIGGER_HAPPY3=select
    BTN_TRIGGER_HAPPY4=start
    BTN_TRIGGER_HAPPY5=tr

    [evdev-absmap]
    ABS_X=x1
    ABS_Y=y1
    ABS_RX=x2
    ABS_RY=y2
    ABS_HAT0X=dpad_x
    ABS_HAT0Y=dpad_y

    [ui-buttonmap]
    start=BTN_LEFT
    tr=BTN_RIGHT
    dl=KEY_KP4@keyboard
    dr=KEY_KP6@keyboard
    du=KEY_KP8@keyboard
    dd=KEY_KP2@keyboard

    a=KEY_SPACE@keyboard
    b=KEY_TAB@keyboard
    X=KEY_P@keyboard
    rb=KEY_LEFTALT@keyboard+KEY_N@keyboard
    lb=KEY_LEFTALT@keyboard+KEY_P@keyboard

    [ui-axismap]
    x1=REL_X:2
    y1=REL_Y:-2
    x2=KEY_UP@keyboard:KEY_DOWN@keyboard
    y2=KEY_LEFT@keyboard:KEY_RIGHT@keyboard



### 구동 화면

![](/images/2023-02-25/11.game1.jpg)
![](/images/2023-02-25/12.game1.jpg)
![](/images/2023-02-25/13.game1.jpg)
![](/images/2023-02-25/14.pop.jpg)
![](/images/2023-02-25/15.game2.jpg)
![](/images/2023-02-25/16.game3.jpg)
![](/images/2023-02-25/17.game4.jpg)


### 참고 사이트

[https://github.com/macmade/Macintosh-ROMs/)](https://github.com/macmade/Macintosh-ROMs/)
[https://www.macintoshrepository.org/7038-all-macintosh-roms-68k-ppc-](https://www.macintoshrepository.org/7038-all-macintosh-roms-68k-ppc-)
