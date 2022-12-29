---
layout: posts
title:  "ogu devilutionX 구동"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,devilutionX,diablo]
---

diablo 이미지를 구동하기 위한 엔진입니다.
아래 방법으로 빌드 할 수 있습니다.
docker 환경에서 빌드 가능합니다.

      https://github.com/diasurgical/devilutionX

      sudo apt install -y cmake libsdl2-dev libsdl2-mixer-dev libsdl2-ttf-dev libbz2-dev
      cmake .. -DCMAKE_BUILD_TYPE="Release" -DDEVILUTIONX_STATIC_CXX_STDLIB=OFF \
      -DDISABLE_ZERO_TIER=ON -DBUILD_TESTING=OFF -DBUILD_ASSETS_MPQ=OFF -DDEBUG=OFF -DPREFILL_PLAYER_NAME=ON -DDEVILUTIONX_SYSTEM_LIBSODIUM=OFF
      make -j4

diablo 이미지 없이 실행하면 아래와 같이 에러 메세지가 발생됩니다. diabdat.mpq 혹은 spawn.mpq 파일이 존재해야 합니다.

      ERROR: Couldn't open /home/odroid/docker/aarch64/devilutionX/build/assets/ui_art/title.pcx
      ERROR: Data File Error
      Unable to open main data archive (diabdat.mpq or spawn.mpq).

      Make sure that it is in the game folder.
      ERROR: No message system available

diabdat.mpq 은 별도로 구해서 devilutionx 와 같은 경로에 복사 필요합니다.
설정 값 위치는 아래와 같지만 메뉴상에서 한글로 선택하면 됩니다.

      /home/odroid/.local/share/diasurgical/devilution/diablo.ini

      [Language]
      Code=ko

한글 폰트는 버젼에 맞는 파일을 여기서 다시 받아야 합니다.

      https://github.com/diasurgical/devilutionx-assets/releases/


실행 전에 조이스틱 설정을 해 줍니다.

      export SDL_GAMECONTROLLERCONFIG="190000004b4800000010000000010000,odroidgo_joypad,platform:Linux,a:b0,b:b1,x:b3,y:b2,back:b10,guide:b8,start:b11,leftstick:b9,rightstick:b12,leftshoulder:b4,rightshoulder:b5,dpup:h0.1,dpdown:h0.4,dpleft:h0.8,dpright:h0.2,misc1:b13,leftx:a0,lefty:a1,rightx:a2,righty:a3,lefttrigger:b6,righttrigger:b7,"

연결된 마우스로도 동작을 하며 조이스틱 버튼으로 이미 정해져 있는 기능으로 동작합니다.

      키는 select :
      start : 핫키
      right thumb : 마우스 버튼
      right stick : 마우스 이동
      B 선택(기술), A 취소(대화)
      left thumb : 맵
      L : 물약 사용
      LT : 캐릭터 상태
      RT : 소지품


구동화면은 아래와 같습니다.

![](/images/2022-12-29/diablo_kor_1.jpg)
![](/images/2022-12-29/diablo_kor_2.jpg)
