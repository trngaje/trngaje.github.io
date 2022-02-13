---
layout: posts
title:  "miyoo-mini glutexto 한글 버젼 적용"
---

### 미리 빌드된 자료 이용하여 기기에 설치하는 방법
- 아래 링크의 glutexto.zip 압축을 풀고 

[https://github.com/trngaje/miyoo-mini/releases/tag/tool_glutexto_v1.0](https://github.com/trngaje/miyoo-mini/releases/tag/tool_glutexto_v1.0)
- glutexto 폴더는 /App/ 폴더 밑에 복사한다.
- glutexto 폴더 내 libSDL_net-1.2.so.0 및 libsparrow3d.so 을  /miyoo/lib/ 에 복사

### 화면 별 할당된 버튼 기능

- 대표화면

SELECT : 설정 메뉴

START : 주 메뉴

A : 가상키보드 활성화

X : 파일 불러오기

Y : 현재 파일 저장


- 가상키보드가 표시되어 있는 화면

A : 글자 입력

B : 백 스페이스

X : 공백

R1 : 줄바꿈

L1 : 탭

Y : 가상 키보드 종료


(화면 상에 표시되는 문구가 일부 잘못 표기되어 있을 수 있습니다.)

  
### 툴체인에서 빌드하는 방법
    git update
    apt install libsdl-net1.2-dev:armhf
    git clone -b sdl2_ogs https://github.com/trngaje/sparrow3d-sdl2-ogs.git
    git clone -b sdl2_ogs https://github.com/trngaje/glutexto-sdl2-ogs.git
    
    cd sparrow3d-sdl2-ogs
    make TARGET=miyoomini
    
    cd glutexto-sdl2-ogs
    make TARGET=miyoomini



{% if page.comments %}
  {% include disqus.html %}
{% endif %}
