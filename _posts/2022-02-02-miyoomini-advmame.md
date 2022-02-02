---
layout: posts
title:  "miyoo-mini advmame 한글 버젼 적용"
---

### 미리 빌드된 자료 이용하여 기기에 설치하는 방법
- 아래 링크의 advmame.zip 압축을 풀고 
[advmame.zip 파일 링크](https://github.com/trngaje/miyoo-mini/releases/tag/emul_advmame_v1)
- .advance 폴더는 sd 카드의 최상위 경로에 복사한다.
- advmame 폴더는 /Emu/ 폴더 밑에 복사한다.
- advmenu 폴더는 /App/ 폴더 밑에 복사한다.
- 필요한 library 가 있으면 /miyoo/lib/ 밑에 복사한다.
  (추후 필요한 파일은 정리해서 올릴 예정입니다.)
  
### 툴체인에서 빌드하는 방법
    git update
    apt install  libasound2-dev:armhf libfreetype6-dev:armhf zlib1g-dev:armhf libexpat1-dev:armhf libslang2-dev:armhf libncurses5-dev:armhf
    apt install automake
    	
    git clone https://github.com/trngaje/advancemame.git    
    ./configure --host=arm-linux-gnueabihf 
	  make -j4
    
    
