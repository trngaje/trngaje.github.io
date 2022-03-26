---
layout: posts
title:  "miyoo-mini simplemenu 적용"
---

### 개요

simple menu는 sdl1.2 기반의 frontend 입니다. 기본 구조는 gmenu 와 비슷하지만  theme 에 의해 화면 구성을 바꿀 수 있는 것이 특징입니다. 
shauninman 이 작성한 custom sdl1.2 기준으로 작업되었으며 생산자 이미지에서는 동작하지 않을 수 있습니다.

### 구현 된 기능

- miyoo mini 에서 동작하도록 수정
- 키 값 miyoo mini 에 맞게 수정
- utf8 글자 표시될 수 있도록 수정


### 사용 방법

> 게임 리스와 스냅이 한 화면에 표시된다.

![](/images/2022-03-26/simplemenu_emu_1.png)

![](/images/2022-03-26/simplemenu_emul_2.png)

> R 버튼을 누르면 전체 스냅화면으로 표시된다.

![](/images/2022-03-26/simplemenu_emul_2.png)

> select 버튼을 누르면 좌우 버튼 으로 에뮬레이터를 선택할 수 있다.

![](/images/2022-03-26/simplemenu_emul_select.png)

> start 버튼을 누르면 설정화면이 표시된다. 

![](/images/2022-03-26/simplemenu_setting.png)

> 키 설명은 도움말에서 볼 수 있다.

![](/images/2022-03-26/simplemenu_help.png)

> 추가 어플리케이션은 .fgl 스크립트 형태로 작성하면 리스트에 표시할 수 있다.

![](/images/2022-03-26/simplemenu_app.png)

> 추가 어플리케이션은 .opk 형태로 작성할 수 있다. (아직 실행은 되지 않음)

![](/images/2022-03-26/simplemenu_opk.png)

### 설정 값 변경
> .simplemenu/config.ini

media_folder 는 롬이내 앱이 저장된 위치를 기준으로 스냅(.png) 파일 저장 위치 지정한다.
디버깅 목저으로 로그를 저장하기 위해서는 logging_enabled 을 1로 설정한다.

    [GENERAL]
    media_folder = snap
    logging_enabled = 0

> .simplemenu/section_groups 내의 .ini 파일에서 각 에뮬레이터 분류하여 표시할 수 있다.

execs 에는 복수 개의 에뮬레이터를 지정할 수 있고, 리스트 화면에서 select 버튼으로 선택할 수 있다.

    [GAME BOY]
    execs = gambatte.sh,mga.sh
    romExts = .gb,.zip
    romDirs = /mnt/SDCARD/Roms/Game Boy (GB)/
    scaling = 1

> .simplemenu/apps 에는 .fgl 또는 .opk 파일이 존재하면 list 상에 표시됩니다.

.fgl 의 형식은 아래와 같습니다. snap은 exec에 지정되어 있는 이름.png 파일을 검색해서 표시합니다.

    title=심플 터미널
    exec=/mnt/SDCARD/.system/miyoomini/paks/Tools/st.pak/launch.sh

.opk 형식은 opk 파일명.png 파일을 검색해서 표시하는 것과 비교 됩니다.

### 이미 빌드된 자료 다운로드

아래 경로의 simplemenu220326.zip 압축을 풀어 각각의 경로에 복사해서 사용합니다.

[https://github.com/trngaje/miyoo-mini/releases/tag/simplemenu_220326](https://github.com/trngaje/miyoo-mini/releases/tag/simplemenu_220326)

(shauninman custom sdl1.2 alpha 이미지 기준으로 설명 합니다.)
아래 폴더를 USB의 최상위 경로에 복사합니다. (HOME 이 /mnt/SDCARD 위치로 설정된 경우)

    .simplemenu

아래 폴더를 .system/miyoomini/ 경로에 복사합니다.

    bin
    lib



### 툴체인에서 빌드하는 방법

libini, libopk 를 먼저 빌드한 후 아래 경로의 소스를 받아서 빌드합니다.
    
    git clone -b kor https://github.com/trngaje/simplemenu.git

    make PLATFORM=miyoomini

