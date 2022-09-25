---
layout: posts
title:  "advmame 연사 기능 추가"
comments: true
categories : [advmame]
tags : [advmame,emulator,autofire]
---

advmame 에 연사 기능 추가하기 위해 advmame 메뉴 구조를 파악해 봅니다.

### 메뉴 추가

메뉴 표시를 위한 스크립트를 정의 하고 추가해 줍니다.


ui_text.h 에 script index 추가

    UI_autofire,
    UI_autofiredelay,

ui_text.c 에 script 문구를 추가해 줍니다.

    "Set Autofire",			
    "Autofire delay",

usrintrf.c 에 메뉴 선택히 호출되는 함수`menu_set_autofire`를 등록해 준다.

    ADD_MENU(UI_autofire, menu_set_autofire, 0);

![](/images/2022-09-25/advmame_autofire1.png)


호출 되는 함수는 아래와 같은 구조를 갖음

    static UINT32 menu_set_autofire(UINT32 state)

선택된 메뉴의 index는 아래와 같이 변환해서 사용합니다.

    int selected = state & 0x3fff;

메뉴를 표시하기 위한 구조체를 초기화 하고

    ui_menu_item item_list[10];
    memset(item_list, 0, sizeof(item_list));

메뉴의 대표 항목(.text) 과 설정 값 항목(.subtext) 에 문자열을 대입하며 메뉴를 구성할 수 있습니다. 아래의 total 값은 전체 메뉴 개수에 해당합니다.

    item_list[total].text = input_port_name(in);

    if (in->autofire)
      item_list[total].subtext = ui_getstring(UI_on);
    else
      item_list[total].subtext = ui_getstring(UI_off);

아래 함수를 호출하면 초기화된 메뉴 정보를 바탕으로 표시합니다.

    ui_draw_menu(item_list, total, sel);

![](/images/2022-09-25/advmame_autofire2.png)

메뉴의 마지막 항목 선택 시
그리고 메뉴 빠져나가기 키 `ESC` 입력시 처리는 아래와 같이 합니다.
return ui_menu_stack_pop(); 를 해야 이전 메뉴 화면이 표출이 됩니다.

    if (input_ui_pressed(IPT_UI_SELECT))
    {
      if (sel == total - 1) return ui_menu_stack_pop();
    }

    if (input_ui_pressed(IPT_UI_CANCEL))
      return ui_menu_stack_pop();

### 연사 동작 구현

inptport.c 에서

     static UINT8  autopressed[MAX_BITS_PER_PORT];

     int pressed = seq_pressed(input_port_seq(port, SEQ_TYPE_STANDARD));

     if (pressed) {
       if (port->autofire) {
         extern UINT32		autofiredelay;
         if (autopressed[bitnum] >= autofiredelay)
         {
           pressed = 0;
           autopressed[bitnum] = 0;
         }
         else
           autopressed[bitnum]++;							
       }
     }

### 게임별 딜레이 값 조절

Autofire delay 값은 기본 10 으로 하고
올림픽 게임인 trackfld 은 5 정도 너무 숫자가 낮으면 각도나
허들 점프시 타이밍 잡기 어려우니 적당한 수준으로 맞춘다.

### 한글 문구 표시

hangul.lng 파일내 문구 추가

    Set Autofire
    자동 연사설정			
    Autofire delay
    딜레이

### advmame.rc 와 연동

추후 업데이트 예정입니다. 
