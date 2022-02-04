---
layout: posts
title:  "port게임 zelda picross 한글 버젼 적용"
---

### 미리 빌드된 자료 이용하여 기기에 설치하는 방법 (miyoo mini 한정)

- 아래 링크의 zeldapicross.zip 압축을 풀고 

[https://github.com/trngaje/miyoo-mini/releases/tag/port_zeldapicrossv0.6](https://github.com/trngaje/miyoo-mini/releases/tag/port_zeldapicrossv0.6)

- zeldapicross 폴더는 /App/ 폴더 밑에 복사한다.
- 필요한 library 가 있으면 /miyoo/lib/ 밑에 복사한다.

  (추후 필요한 파일은 정리해서 올릴 예정입니다.)
  
### 툴체인에서 빌드하는 방법

- SDL2.0 환경 (ogs,oga,rk2020)

    git clone -b sdl2_kor https://github.com/trngaje/zeldapicross_sdl2.git
    make -f Makefile.ogs

data 폴더랑 Zelda-Picross 을 같이 복사해 넣으면 됨

- SDL1.2 환경 (미유 미니)

    git update
    apt install libsdl-gfx1.2-dev:armhf libsdl1.2-dev:armhf libsdl-image1.2-dev:armhf libsdl-mixer1.2-dev:armhf
    	
(miyoo mini source 업데이트 후에 빌드 방법 추후 기술 예정)


### 조이스틱 키값 키보드 메세지로 매핑

미유 미니는 dingux 기반의 기기와 마찬가지로 모튼 버튼 값이 키보드 메세지로 출력되고 있고 homebrew porting 게임들도 모두 키보드 값으로 동작하고 있기 때문에
 소스를 변경하지 않는 이상 ogs,oga,rk2020 기기에서는 xboxdrv 로 키보드 값으로 변경해 주어야 합니다.

    sudo /home/odroid/util/xboxdrv/xboxdrv \
    --evdev /dev/input/event2 \
    --evdev-no-grab \
    --silent \
    --quiet \
    --detach-kernel-driver \
    --force-feedback \
    --deadzone-trigger 30% \
    --deadzone 30% \
    --mimic-xpad \
    --evdev-keymap BTN_EAST=a,BTN_SOUTH=b,BTN_NORTH=x,BTN_WEST=y,BTN_TL=lb,BTN_TR=rb,BTN_TL2=tl,BTN_TR2=tr,BTN_TRIGGER_HAPPY1=back,BTN_TRIGGER_HAPPY2=start \
    --evdev-absmap ABS_X=dpad_x,ABS_Y=dpad_y,ABS_RX=x2,ABS_RY=y2 \
    --evdev-absmap ABS_HAT0X=dpad_x,ABS_HAT0Y=dpad_y \
    --ui-buttonmap a=KEY_LEFTCTRL,b=KEY_LEFTALT,x=KEY_LEFTSHIFT,y=KEY_SEMICOLON,tl=KEY_F4,tr=KEY_DELETE,lb=KEY_S,rb=KEY_F \
    --ui-buttonmap start=KEY_ENTER,back+start=KEY_ESC \
    --ui-buttonmap du=KEY_UP,dd=KEY_DOWN,dl=KEY_LEFT,dr=KEY_RIGHT \
    &

    ./Zelda-Picross

    sudo killall xboxdrv
