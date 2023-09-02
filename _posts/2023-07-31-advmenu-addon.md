---
layout: posts
title:  "advmenu 추가 기능 설정 방법"
comments: true
categories : [advmenu]
tags : [advmenu]
---
advmenu 추가 기능 관련 내용을 정리합니다.
advmenu.rc 는 $HOME/.advance/advmenu.rc 경로에 위치합니다.

### runcommand

>기본 구조


    /runcommand/
      runcommand.sh
      cfg/
        snes.cfg
        gb.cfg
        ...


아래와 같이 mame2010 이라는 이름을 사용한다면 runcommand.sh 에 전달하는 이름도 mame2010로 전달해야 합니다.

    emulator "mame2010" generic "/mnt/runcommand/runcommand.sh" "mame2010 %p"
    emulator_roms "mame2010" "/mnt/mame2010"
    emulator_roms_filter "mame2010" "*.zip"
    emulator_altss "mame2010" "/mnt/mame2010/mng:/mnt/mame2010/snap"


runcommand 가 정의되어 있는 경로를 아래와 같이 지정합니다.

예)rgnano는

    ui_runcommand_cfg /mnt/runcommand


runcommand 메뉴를 바로 호출 하려면 키 값 등록을 아래와 같이 합니다.

    event_assign runcommand lshift


### 추가된 key event

    event_assign runcommand lshift
    event_assign cancel b


### 추가된 tile view

    mode tile_2x2

### 한글 메뉴명 지원

    misc_languagefile hangulmenu.lng

폰트는 한글을 지원하는 폰트로 교체해 주어야 한다.

    ui_bar_font "Galmuri11.ttf"
    ui_menu_font "Galmuri11.ttf"
    ui_text_font "Galmuri11.ttf"


### 상단 상태바 커스텀 사용

상단바를 ip address 와 배터리 값 표시 목적으로 사용할 수 있다.
한줄로 표시 가능한 스크립트(getip.sh) 나 command 모두 사용 가능합니다.
아래는 미유미니 플러스와 ogu에서 적용해본 예 입니다.

    #ui_ip /mnt/SDCARD/getip.sh
    #ui_ip ifconfig wlan0 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}' | tr -d '\n'
    #ui_battery cd /customer/app/ ; ./axp_test
    #ui_battery cd /customer/app/ ; ./axp_test |  awk 'match($0, /"battery"/){print substr($0, 12, 3)}' | sed 's/,//'

### BIOS 미표시 하도록 gropu 지정

    미지정 그룹 "<undefined>" 만 include 하면 미표시할 수 있음

    group_import catver "advmame" "advmame_bios.ini" "Category"
    advmame/group_include "<undefined>"

### build 방법

FunKey 용

    CFLAGS="-O3 -fomit-frame-pointer -fsigned-char -fno-stack-protector -Wall -Wno-sign-compare -Wno-unused" ./configure --host=arm-funkey-linux-musleabihf
