---
layout: posts
title:  "ogu openbor"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,openbor]
---

    sudo apt install libvpx-dev libvorbisidec-dev
    git clone https://github.com/DCurrent/openbor

아래는 빌드에러가 있으니 적용하지 말 것

    patch -p1 < 001openbor-Fix-build-on-modern-systems.patch  

아래만 적용해 본다.

    patch -p1 < openbor-02-addplatform.patch
    ./version.sh
    make BUILD_LINUX_aarch64=1


OpenBOR 파일만 복사하면 됩니다.
실행은 아래와 같이 실행한다. 사운드가 출력되지 않음 (exfat에 롬파일을 넣었더니 문제가 발생하는 듯/emmc에서는 문제 없음)

    $ ./OpenBOR /mnt/sdcard/roms/openbor/KOF\ BeatEmUP_Plus.pak

dependency 참고:

toolchain SDL2 libogg libvorbisidec libvpx libpng
