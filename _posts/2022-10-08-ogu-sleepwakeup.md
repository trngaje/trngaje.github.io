---
layout: posts
title:  "ogu 슬립기능 구현"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,sleep]
---

sleep/wakeup 이 정상 동작하기 위해서는

1.우선 power key가 shutdown 키로 동작하지 않도록 막아야 합니다. (아래와 같이 입력하면 파워키를 눌러도 반응이 없게됨)

sudo nano /etc/systemd/logind.conf
#HandlePowerKey=poweroff 을

    HandlePowerKey=ignore

2.oga_events 를 실행하기 위해서는 evdev 를 설치해야 함

    sudo apt-get install python3 python3-pip python3-dev
    pip3 install evdev

키값은 아래와 같음

    odroidgo_joypad
    up : 544
    down : 545
    left : 546
    right : 547
    b : 304
    a : 305
    x : 307
    y : 308
    sel (f1) : 704
    start (f6): 709
    f2 : 705
    f3 : 706
    f4 : 707
    f5 : 708
    l1 : 310
    r1 : 311
    l2 : 312
    r2 : 313

    gou-gpio-keys
    vol- : 114
    vol+: 115

3.suspend 동작하기 위해서는 권한 설정이 되어 있어야 함

    sudo nano /etc/polkit-1/localauthority/50-local.d/suspend.pkla

    [Allow all users to shutdown]
    Identity=unix-user:*
    Action=org.freedesktop.login1.suspend-multiple-sessions
    ResultInactive=no
    ResultActive=yes
    ResultAny=yes

주)sleep 은 되지만 wakeup 이 되지 않음, 배터리 탈거해야만 복구 됨
=> 방법이 나올 때까지 여기서 보류 합니다.
