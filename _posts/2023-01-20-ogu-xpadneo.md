---
layout: posts
title:  "ogu xpadneo 구동"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,xpadneo]
---

BT gamepad에서 진동 기능을 사용하려면 xpadneo를 설치해야 합니다.
- 기기 내에 linuxheader 를 설치 하고

      git clone --depth 1 -b odroidgoU-4.9.y https://github.com/tobetter/linux.git
      zcat /proc/config.gz > .config
      make oldconfig
      아래 옵션을 사용하려면 메모리 스왑먼저 해줍니다.
      make -j6
      sudo make headers_install INSTALL_HDR_PATH=/usr

      cd /lib/modules/$(uname -r)
      sudo ln -sf 위커널소스경로 source
      sudo ln -sf 위커널소스경로 build

위 명령어로는 header 파일만 설치가 되니 수정된 드라이버는 별도로 설치해 주어야 합니다.

- linux kernel 버젼이 낮아 xpadneo를 빌드할 수 있게 patch 적용해야 합니다.
xpadneo 를 우선 빌드 합니다.

      git clone https://github.com/atar-axis/xpadneo.git
      sudo ./install.sh


구동 시 주요 에러 로그 위치

    drivers/hid/hid-core.c: hid_err(parser->device, "unknown main item tag 0x%x\n", item->tag);

장치 추가
drivers/hid/hid-core.c
02e0 는 8Bitdo SN30 Pro 장치와 동일합니다.

static const struct hid_device_id hid_have_special_driver[] = {

...

    { HID_BLUETOOTH_DEVICE(USB_VENDOR_ID_MICROSOFT, 0x02FD) },
    { HID_BLUETOOTH_DEVICE(USB_VENDOR_ID_MICROSOFT, 0x02E0) },

문제점:
1.연결 후 일정 시간 지나면 화면이 꺼지는 증상 있음
2.첫 연결 시 진동이 계속 울림 disconnect 후 connect 하면 정상 동작함
3.연결 시 진동 기능 없이 연결되는 경우 있음 (코드 정리 후 이건 더 안 나오는 증상)
4.8Bito SN30 Pro 외에 의미 없는 event5,6이 같이 생성됨

    /dev/input/event4:      8Bitdo SN30 Pro
    /dev/input/event5:      8Bitdo SN30 Pro Consumer Control
    /dev/input/event6:      8Bitdo SN30 Pro Keyboard

진동 기능을 갖고 연결되었을 때 dmesg 로그는 아래와 같음

    [  250.566400] loaded hid-xpadneo v0.9-131-g0150346-dirty
    [  250.566474] xpadneo 0005:045E:02E0.0001: buggy firmware detected, please upgrade to the latest version
    [  250.566478] xpadneo 0005:045E:02E0.0001: pretending XB1S Windows wireless mode (changed PID from 0x02E0 to 0x028E)
    [  250.566482] xpadneo 0005:045E:02E0.0001: working around wrong SDL2 mappings (changed version from 0x00000903 to 0x00001130)
    [  250.566486] xpadneo 0005:045E:02E0.0001: report descriptor size: 307 bytes
    [  250.566488] xpadneo 0005:045E:02E0.0001: fixing up report descriptor size
    [  250.566948] xpadneo 0005:045E:02E0.0001: battery detected
    [  250.566953] xpadneo 0005:045E:02E0.0001: gamepad detected
    [  250.566956] xpadneo 0005:045E:02E0.0001: enabling compliance with Linux Gamepad Specification
    [  250.567093] input: 8Bitdo SN30 Pro as /devices/virtual/misc/uhid/0005:045E:02E0.0001/input/input6
    [  250.568368] xpadneo 0005:045E:02E0.0001: input,hidraw0: BLUETOOTH HID v11.30 Gamepad [8Bitdo SN30 Pro] on 00:e0:4c:23:99:87
    [  250.568496] input: 8Bitdo SN30 Pro Consumer Control as /devices/virtual/misc/uhid/0005:045E:02E0.0001/input/input7
    [  250.568595] xpadneo 0005:045E:02E0.0001: consumer control added
    [  250.568682] input: 8Bitdo SN30 Pro Keyboard as /devices/virtual/misc/uhid/0005:045E:02E0.0001/input/input8
    [  250.568779] xpadneo 0005:045E:02E0.0001: keyboard added
    [  250.568791] xpadneo 0005:045E:02E0.0001: controller quirks: 0x00000007
    [  250.568795] xpadneo xpadneo_welcome_rumble start
    [  251.235675] xpadneo xpadneo_welcome_rumble took 668ms
    [  251.235694] xpadneo 0005:045E:02E0.0001: 8Bitdo SN30 Pro [e4:17:d8:2d:ec:66] connected
    [  419.810492] rk818-bat: coffset calib again 0.., max_cnt=1
    [  423.842374] rk818-bat: coffset calib again 1.., max_cnt=1
    [  427.878845] rk818-bat: new offset:c=0x802, i=0x841, p=0xffffffc1

이건 또 다른 dmesg 로그

    [  400.407443] hid_xpadneo: loading out-of-tree module taints kernel.
    [  400.408416] loaded hid-xpadneo v0.9-131-g0150346-dirty
    [  400.408541] xpadneo 0005:045E:02E0.0001: buggy firmware detected, please upgrade to the latest version
    [  400.408547] xpadneo 0005:045E:02E0.0001: pretending XB1S Windows wireless mode (changed PID from 0x02E0 to 0x028E)
    [  400.408551] xpadneo 0005:045E:02E0.0001: working around wrong SDL2 mappings (changed version from 0x00000903 to 0x00001130)
    [  400.408556] xpadneo 0005:045E:02E0.0001: report descriptor size: 307 bytes
    [  400.408558] xpadneo 0005:045E:02E0.0001: fixing up report descriptor size
    [  400.409125] xpadneo 0005:045E:02E0.0001: battery detected
    [  400.409129] xpadneo 0005:045E:02E0.0001: gamepad detected
    [  400.409133] xpadneo 0005:045E:02E0.0001: enabling compliance with Linux Gamepad Specification
    [  400.409317] input: 8Bitdo SN30 Pro as /devices/virtual/misc/uhid/0005:045E:02E0.0001/input/input4
    [  400.409663] xpadneo 0005:045E:02E0.0001: input,hidraw0: BLUETOOTH HID v11.30 Gamepad [8Bitdo SN30 Pro] on 00:e0:4c:23:99:87
    [  400.409812] input: 8Bitdo SN30 Pro Consumer Control as /devices/virtual/misc/uhid/0005:045E:02E0.0001/input/input5
    [  400.414462] xpadneo 0005:045E:02E0.0001: consumer control added
    [  400.414574] input: 8Bitdo SN30 Pro Keyboard as /devices/virtual/misc/uhid/0005:045E:02E0.0001/input/input6
    [  400.414658] xpadneo 0005:045E:02E0.0001: keyboard added
    [  400.414665] xpadneo 0005:045E:02E0.0001: controller quirks: 0x00000007
    [  400.414667] xpadneo xpadneo_welcome_rumble start
    [  401.080944] xpadneo xpadneo_welcome_rumble took 664ms
    [  401.080955] xpadneo 0005:045E:02E0.0001: 8Bitdo SN30 Pro [e4:17:d8:2d:ec:66] connected
    [  440.762887] xpadneo 0005:045E:02E0.0001: battery registered
    [  546.841686] xpadneo 0005:045E:02E0.0001: throttling rumble reprogramming

관련 없는 설정 값

    /etc/modprobe.d/xpadneo.conf
    alias hid:b0005g*v0000045Ep000002E0 hid_xpadneo
    alias hid:b0005g*v0000045Ep000002FD hid_xpadneo
    alias hid:b0005g*v0000045Ep00000B05 hid_xpadneo
    alias hid:b0005g*v0000045Ep00000B13 hid_xpadneo
    alias hid:b0005g*v0000045Ep00000B20 hid_xpadneo
    alias hid:b0005g*v0000045Ep00000B22 hid_xpadneo
﻿

참고:


    drivers/hid/hid-ids.h:#define USB_VENDOR_ID_MICROSOFT		0x045e
    #define USB_DEVICE_ID_8BITDO_SN30_PRO_PLUS      0x02e0

    Bus 001 Device 005: ID 045e:028e Microsoft Corp. Xbox360 Controller
