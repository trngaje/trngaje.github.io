---
layout: posts
title:  "ogu 기본 라이브러리와 에뮬레이터"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드]
---

기본 라이브러리인 libgou 와 retroarch core를 간단하게 구동할 수 있는 retrorun ogu 버젼을 소개합니다.

### libgou build 방법

https://github.com/OtherCrashOverride/libgou.git

    sudo apt-get install premake4 libopenal-dev libevdev-dev libasound2-dev libpng-dev
    sudo apt-get install mesa-common-dev libegl1-mesa-dev libgles2-mesa-dev
    sudo apt-get install mesa-utils
    premake4 gmake
    make -j4


libgou.so 를 라이브러리 경로에 복사한다.

    sudo mkdir /usr/include/gou
    sudo cp src/*.h /usr/include/gou/

=> docker container image 에 기본 포함되도록 적용 완료

### retrorun build 방법

    git clone -b gou https://github.com/OtherCrashOverride/retrorun-go2.git
    premake4 gmake
    make -j4


### ge2d 관련 테스트 ###
    ge2d 관련 테스트 코드
    https://github.com/OtherCrashOverride/ge2d-test.git
    make

0도

    $ ./ge2dtest 0

![](/images/2022-10-10/ogu_ge2d_test.jpg)

90도

    $ ./ge2dtest 1

180도

    $ ./ge2dtest 2

270도 (정상적으로 출력됨)

    $ ./ge2dtest 3

![](/images/2022-10-10/ogu_ge2d_270degree.jpg)
