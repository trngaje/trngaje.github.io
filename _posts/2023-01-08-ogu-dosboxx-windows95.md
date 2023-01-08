---
layout: posts
title:  "ogu dosbox-x build 및 windows95/98 설치"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,dosbox-x]
---

stand alone인 dosbox-x를 빌드해 보고, windows95/98을 설치해 봅니다.

> dosbox-x source 빌드

    git clone https://github.com/joncampbell123/dosbox-x.git
    sudo apt update
    sudo apt install -y automake gcc g++ make libncurses-dev nasm libsdl-net1.2-dev libsdl2-net-dev libpcap-dev libslirp-dev fluidsynth libfluidsynth-dev libavdevice58 libavformat-dev libavcodec-dev libavcodec-extra libavcodec-extra58 libswscale-dev libfreetype-dev libxkbfile-dev libxrandr-dev

    설치 : sudo apt install libslirp-dev libxkbfile-dev

    ./autogen.sh
    ./configure  --enable-dynrec  --enable-unaligned_memory  --disable-sdl  --enable-sdl2  --enable-mt32 --disable-opengl --disable-x11
    make

> 실행 방법

    dosbox-x -conf win95.conf

> .conf 파일 혹은 command line 내 mount 명령어

    IMGMOUNT C hdd.img

    MOUNT D .

> windows 95 설치

아래 가이드에 따라 window 95를 설치해 봅니다.

[https://dosbox-x.com/wiki/Guide%3AInstalling-Windows-95](https://dosbox-x.com/wiki/Guide%3AInstalling-Windows-95)

드라이버/소프트웨어 설치

[http://dosbox95.darktraveler.com/guide%20part%204.html](http://dosbox95.darktraveler.com/guide%20part%204.html)

> windows 98 설치

[https://dosbox-x.com/wiki/Guide%3AInstalling-Windows-98](https://dosbox-x.com/wiki/Guide%3AInstalling-Windows-98)

> xobxdrv 를 사용하여 마우스 메세지 생성

    xboxdrv \
    --evdev /dev/input/event3 \
    --silent \
    --detach-kernel-driver \
    --force-feedback \
    --mimic-xpad \
    --trigger-as-button \
    --no-extra-devices \
    --deadzone-trigger 15% \
    --deadzone 4000 \
    --evdev-keymap BTN_SOUTH=b,BTN_EAST=a,BTN_NORTH=x,BTN_WEST=y,BTN_TL=lb,BTN_TR=rb,BTN_TL2=tl,BTN_TR2=tr,BTN_TRIGGER_HAPPY3=back,BTN_TRIGGER_HAPPY4=start \
    --evdev-absmap ABS_X=x1,ABS_Y=y1,ABS_RX=x2,ABS_RY=y2 \
    --evdev-absmap ABS_HAT0X=dpad_x,ABS_HAT0Y=dpad_y \
    --ui-axismap x1=REL_X@mouse:2,y1=REL_Y@mouse:-2 \
    --ui-buttonmap a=BTN_LEFT@mouse,b=BTN_RIGHT@mouse,start=KEY_F5@keyboard,back=KEY_ESC@keyboard \
    --ui-buttonmap du=KEY_UP@keyboard,dd=KEY_DOWN@keyboard,dl=KEY_LEFT@keyboard,dr=KEY_RIGHT@keyboard &
