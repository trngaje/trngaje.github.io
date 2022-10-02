---
layout: posts
title:  "advmame msys2 에서 windows용 build"
comments: true
categories : [advmame]
tags : [advmame,emulator,advmenu,msys2]
---

advmame와 advmenu를 windows 환경에서 build 하기 위해서는 msys2 개발 환경을 설치 해야 합니다.

### MSYS2 설치

https://www.msys2.org/

toolchain 설치

a) for 32-bit:

    pacman -S mingw-w64-i686-gcc


b) for 64-bit:

    pacman -S mingw-w64-x86_64-gcc
    pacman -S mingw-w64-x86_64-toolchain

all 로 설치



NASM 0.98.33 (or newer)

    pacman -S nasm

SDL 2
64bit

    pacman -S mingw-w64-x86_64-SDL2

32bit

    pacman -S mingw-w64-i686-SDL2


SDL 1.2
64bit

    pacman -S mingw-w64-x86_64-SDL

32bit

    pacman -S mingw-w64-i686-SDL

FreeType 2.1.7 (or newer)
64bit

    pacman -S mingw-w64-x86_64-freetype

32bit

    pacman -S mingw-w64-i686-freetype

autogen.sh 을 위해서 사전에 아래 패키지를 설치할 것

    pacman -S msys/libtool
    pacman -S msys/autoconf
    pacman -S msys/automake-wrapper

    ./autogen.sh
    ./configure
    ./make -j8

### advmame와 advmenu 를 실행하기 위해서는 의존성이 있는 라이브러리(.dll)가 필요합니다.

C:\msys64\mingw64\bin 에 있는 *.dll 파일 중 필요한 것을 골라서 advmame.exe와 advmenu.exe 경로에 같은 경로에 두면 됩니다.

### 빌드 에러시 대처 방법

1.아래와 같은 에러 발생 시

./configure: line 8255: syntax error near unexpected token `SLANG,'
./configure: line 8255: `       PKG_CHECK_MODULES(SLANG, slang, ac_lib_slang=yes, ac_lib_slang=no)'

    pacman -S mingw-w64-x86_64-toolchain
    autoupdate
    ./autogen.sh
    ./configure

2.아래와 같은 에러 발생 시 GWLP_WNDPROC로 변경 필요합니다

advance/windows/mraw.c:372:38: error: 'GWL_WNDPROC' undeclared (first use in this function); did you mean 'GWLP_WNDPROC'?
  372 |  if (SetWindowLong(raw_state.window, GWL_WNDPROC, (LONG)raw_state.proc) != (LONG)mouseb_rawinput_proc) {
