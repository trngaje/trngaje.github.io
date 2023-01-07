---
layout: posts
title:  "ogu dosbox pure 로 windows95/98 설치"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,dosbox pure]
---
ogu 에서 windows 95/98 용 프로그램을 실행하기 위해서는 dosbox pure를 사용하여
windows 95/98을 설치해야 합니다. 설치하기 위한 hdd.img 를 만들고, window 95/98
cd를 설치한 후, 원하는 게임롬과 함께 windows 95/98 os 를 실행해주면 됩니다.

> .iso windows 98 cd로 부터 하드디스크 이미지 만들기

1.retroarch 로 dosbox_pure core를 실행 합니다.

        retroarch -L dosbox_pure_libretro.so win98se.iso

2.dosbox_pure에서 win98se.iso 설치합니다.

메뉴에서 [Boot and Install New Operating System ] 을 선택합니다.
![](/images/2023-01-08/win95_boot_and_install.jpg)


3.설치할 하드 크기를 선택합니다. 512MB 를 선택합니다. (원하는 용량으로 선택하면 됩니다.)

![](/images/2023-01-08/win95_hard_disk.jpg)


bios 위치에 파일이 생성됩니다. (설치 위치는 retroarch.cfg 에 정의되어 있는 경로에 생성됩니다.)

![](/images/2023-01-08/dosbox_bios_location.png)


4.CD로 부팅 선택합니다.

![](/images/2023-01-08/win95_boot_from_cd_4.jpg)

5.Startup Menu 에서 "~ from CD ROM".을 선택 합니다. 메뉴 나오자 마자 바로 선택하면 CD 가 인식이 안되는 것 같습니다.

![](/images/2023-01-08/win95_startup.jpg)

![](/images/2023-01-08/win98_cd_prepare_hang.jpg)

만약 위 화면에서 멈춰서 진행하지 않으면 retroarch 를 종료 하고 생성된 hdd.img 파일을 삭제하고 1번 부터 다시 진행 합니다.

사용자 설치로 필요한 프로그램만 골라서 설치합니다.

![](/images/2023-01-08/win95_install_option.jpg)

인스톨이 완료되면 아래와 같이 화면이 표시됩니다.

![](/images/2023-01-08/win98_boot_init.jpg)

windows98 에서는 cd 인스톨만 해도 장치가 모두 정상 설치 됩니다.

대신 window95 에서는 아래와 같이 특정 장치가 설치가 안됩니다.
![](/images/2023-01-08/win95_system_property.jpg)

> 게임롬 실행

게임롬을 실행 합니다.
dosbox pure 메뉴 화면에서 win98 os를 실행해 줍니다.

![](/images/2023-01-08/win98_run_install.jpg)

![](/images/2023-01-08/win98_select_os.jpg)

windows 98화면에서 원하는 파일을 실행해 줍니다.

![](/images/2023-01-08/win98_select_starcraft_drive.jpg)
![](/images/2023-01-08/win98_select_starcraft.jpg)

> 게임 실행 화면

![](/images/2023-01-08/freecell_1.jpg)

![](/images/2023-01-08/starcraft_1.jpg)

![](/images/2023-01-08/starcraft_2.jpg)

![](/images/2023-01-08/starcraft_3.jpg)
