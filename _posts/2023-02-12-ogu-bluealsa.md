---
layout: posts
title:  "ogu bluealsa 설치"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,bluealsa]
---
bt a2dp sink 출력을 하기 위해서는 pulseaudio를 설치해야 합니다만, bt 오디오 출력이 없어도
일정 수준으로 cpu를 사용하고 있기 때문에 다른 대안 중에 하나가 bluealsa 입니다.

### pulse audio 제거

    sudo apt-get remove --purge pulseaudio-module-bluetooth
    sudo apt-get remove --purge pulseaudio

삭제 후 dolphin 사운드 정상 출력됨

### 패키지 설치

이 방법이 되면 간단합니다.

    sudo apt-get install bluez-alsa

### bluealsa build

패키지 설치가 되지 않는다면, 소스를 빌드 해야 합니다.

    sudo apt-get install libasound2-dev dh-autoreconf libortp-dev bluez bluetooth bluez-tools libbluetooth-dev libusb-dev libglib2.0-dev libudev-dev libical-dev libreadline-dev libsbc1 libsbc-dev

    sudo find / -name 'alsa-lib'
    /usr/lib/aarch64-linux-gnu/alsa-lib
    /usr/lib/arm-linux-gnueabihf/alsa-lib

    git clone https://github.com/arkq/bluez-alsa
    cd bluez-alsa
    mkdir build
    cd build
    ../configure --disable-hcitop --with-alsaplugindir=/usr/lib/aarch64-linux-gnu/alsa-lib
    make
    sudo make install

### 동작 확인

> wav 파일 출력

    aplay -D bluealsa:DEV=C1:12:31:00:07:5E,PROFILE=a2dp /mnt/sdcard/roms/ports/cdogs-sdl/sounds/key.wav

    amixer -D bluealsa
    Simple mixer control 'UGREEN HiTune X6 A2DP',0
      Capabilities: pvolume pswitch
      Playback channels: Front Left - Front Right
      Limits: Playback 0 - 127
      Mono:
      Front Left: Playback 127 [100%] [0.00dB] [on]
      Front Right: Playback 127 [100%] [0.00dB] [on]

> 볼륨 설정

    amixer -D bluealsa sset 'UGREEN HiTune X6 A2DP' 50%
    Simple mixer control 'UGREEN HiTune X6 A2DP',0
      Capabilities: pvolume pswitch
      Playback channels: Front Left - Front Right
      Limits: Playback 0 - 127
      Mono:
      Front Left: Playback 64 [50%] [-9.75dB] [on]
      Front Right: Playback 64 [50%] [-9.75dB] [on]

### sound default 출력 설정

> 모든 사운드를 bluetooth 로 하기 위해서는

    sudo nano ~/.asoundrc

    pcm.btspeaker {
     type plug
      slave {
        pcm {
          type bluealsa
          device C1:12:31:00:07:5E
          profile "a2dp"
        }
      }
      hint {
        show on
        description "Bluetooth Speaker"
      }
    }

    pcm.!default {
        type plug
        slave.pcm "btspeaker"
    }

> retroarch 사운드만  bluetooth 로 하기 위해서는

    sudo nano ~/.asoundrc

    pcm.btspeaker {
     type plug
      slave {
        pcm {
          type bluealsa
          device C1:12:31:00:07:5E
          profile "a2dp"
        }
      }
      hint {
        show on
        description "Bluetooth Speaker"
      }
    }

retroarch.cfg 에서는 아래와 같이 설정해 준다.

    audio_device = "bluealsa"
    audio_driver = "alsathread"


![](/images/2023-02-12/bluealsa.jpg)


설정 후 retroarch를 재 실행해야 적용되며, bt 연결이 끊어지더라도 speaker 출력으로 자동 전환되지는 않음

지울 때는 메뉴 상에서 start를 누르거나

    audio_device = ""



창고:

1. [https://forum.armbian.com/topic/6480-bluealsa-bluetooth-audio-using-alsa-not-pulseaudio/](https://forum.armbian.com/topic/6480-bluealsa-bluetooth-audio-using-alsa-not-pulseaudio/)
2. [https://www.sigmdel.ca/michel/ha/rpi/bluetooth_in_rpios_01_en.html](https://www.sigmdel.ca/michel/ha/rpi/bluetooth_in_rpios_01_en.html)
3. [https://introt.github.io/docs/raspberrypi/bluealsa.html](https://introt.github.io/docs/raspberrypi/bluealsa.html)
