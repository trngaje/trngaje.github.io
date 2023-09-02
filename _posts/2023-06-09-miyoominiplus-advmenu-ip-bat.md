---
layout: posts
title:  "miyoo mini + advmenu ip 및 배터리 값 표시"
comments: true
categories : [miyoominiplus]
tags : [miyoominiplus]
---

휴대용 기기를 위한 ip 주소 값과 배터리 상태 값을 표시한다.
advmenu.rc 상에서 값 읽는 스크립트를 자유롭게 바꿀 수 있게 설계 한다.
메뉴 일부분을 한글로 구현 한다.
core와 에뮬레이터를 advmenu 메뉴를 통해 변경할 수 있다.

### 추가된 advmenu.rc

    ui_ip none
    ui_battery none
    event_assign runcommand lshift
    event_assign cancel none

> miyoo mini plus 에서는

    ui_ip ifconfig wlan0 | grep 'inet addr' | cut -d: -f2 | awk '{print $1}' | tr -d '\n'
    ui_battery cd /customer/app/ ; ./axp_test



>runcommand 키 할당을 위해서는

    event_assign runcommand lshift


### 구동 화면

ip address 및 배터리 % 표시

메뉴 한글화 작업

runcommand 연동

mmp에서는
사용을 위해서는 미리 정의해 놓은 /runcommand 폴더 및 파일이 필요하다.

### 참고 페이지

[https://www.advancemame.it/doc-advmenu](https://www.advancemame.it/doc-advmenu)
