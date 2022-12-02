---
layout: posts
title:  "ogu attractmode"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,attractmode]
---

기존 ogs용으로 libgo2 를 사용하여 작업되어 있는 것을 libgou로 변경합니다.

sfml 빌드

    sudo apt-get install cmake libx11-dev libx11-xcb-dev libflac-dev libogg-dev libvorbis-dev libopenal-dev libfreetype6-dev libxcb-randr0-dev libxcb-image0-dev libxcb-util0-dev libxcb-ewmh-dev libxcb-keysyms1-dev libxcb-icccm4-dev libudev-dev libavutil-dev libavcodec-dev libavformat-dev libavfilter-dev libswscale-dev libavresample-dev libfontconfig1-dev

    make build
    cd build
    cmake .. -DSFML_RPI=1 -DSFML_OS_LINUX=1 -DSFML_OPENGL_ES=1 -DEGL_INCLUDE_DIR=/usr/include  -DEGL_LIBRARY=/usr/local/lib/aarch64-linux-gnu/libEGL.so -DGLES_INCLUDE_DIR=/usr/include  -DGLES_LIBRARY=/usr/local/lib/aarch64-linux-gnu/libGLESv2.so

    cmake .. -DSFML_RPI=1 -DSFML_OS_LINUX=1 -DSFML_OPENGL_ES=1 -DEGL_INCLUDE_DIR=/usr/include -DEGL_LIBRARY=/usr/lib/aarch64-linux-gnu/libEGL.so -DGLES_INCLUDE_DIR=/usr/include -DGLES_LIBRARY=/usr/lib/aarch64-linux-gnu/libGLESv2.so


    make -j7
    sudo make install
    sudo ldconfig

    sudo cp -P ../sfml-pi/build/lib/libsfml* /usr/lib/aarch64-linux-gnu/


void gou_display_present(gou_display_t* display, gou_surface_t* surface,
            int srcX, int srcY, int srcWidth, int srcHeight, bool mirrorX, bool mirrorY,
            int dstX, int dstY, int dstWidth, int dstHeight);

attract 빌드

    sudo apt-get install libjpeg-dev
    make USE_GLES=1 -j4

빌드는 되지만 Failed to activate the window's context 에러 발생함.


시간 표시를 제대로 하려면 timezone 한국으로 설정함

    sudo timedatectl set-timezone Asia/Seoul

configure 메뉴 한글 표시를 하기 위해서는
attract 9p theme 사용시 폰트 위치

    ./usr/share/fonts/truetype/BinggraeMelona.ttf

이슈
1. fe_window.cpp : close() 호출시 abort 발생함
   sfml : Window.cpp : delete m_context; 시 abort 발생함
2. killall 에 의해 프로세스가 종료 되지 않고 kworker cpu 100% 가 나오고 있음
  killall -w 옵션으로 종료 때까지 대기 가능함
3. "sudo graphics 1" 를 graphic 화면 초기화 전에 호출하면 무한 대기하는 것으로 보임
4. ION_IOC_ALLOC failed. 가 자주 발생함

<s> 5. "run_program으로 bash *.sh 실행 시 종료되지 않음 src/fe_util.cpp:run_program" ==> exfat 으로 포맷되어 있는 sd card 롬 실행시 retroarch 에서 holding 되는 것으로 보임 </s>

exfat 으로 sdcard 포맷한 경우 아래 유틸리티가 설치되어 있어야 롬실행이 문제 없이 된다.
sudo apt install exfat-fuse exfat-utils
설치 후에는 재 부팅을 해준다.

    $ id odroid
    uid=1000(odroid) gid=1000(odroid) groups=1000(odroid),4(adm),27(sudo),29(audio),44(video),107(input),109(render)

    sudo mount -t exfat -o defaults,uid=1000,gid=1000,umask=000 /dev/mmcblk1p1 /mnt/sdcard

참고:

https://registry.khronos.org/EGL/sdk/docs/man/html/eglCreateWindowSurface.xhtml

 NativeWindowType
eglCreateWindowSurface


odroid@gou:~/Dockerfileogu/advmame/crashoverride$ grep -r 'wl_egl_window_create'
SDL-ge2d/src/video/wayland/SDL_waylandsym.h:SDL_WAYLAND_SYM(struct wl_egl_window *, wl_egl_window_create, (struct wl_surface *, int, int))
SDL-ge2d/src/video/wayland/SDL_waylandwindow.c:        data->egl_window = WAYLAND_wl_egl_window_create(data->surface, data->drawable_width, data->drawable_height);


m_surface = eglCheck(eglCreateWindowSurface(m_display, m_config, window, NULL));



src/SFML/Window/EglContext.cpp:    createSurface((EGLNativeWindowType)owner->getSystemHandle());

NativePixmapType (*egl_create_pixmap_ID_mapping)(mali_pixmap *);
