---
layout: posts
title:  "rgnano ffmpeg build"
comments: true
categories : [rgnano]
tags : [rgnano]
---

ffmpeg, ffplay 를 구동하기 위한 빌드환경을 설명합니다.

## ffmpeg 빌드

> Anbernic

최근 소스 기준

    sudo apt-get update -qq && sudo apt-get -y install \
      git-core \
      meson \
      ninja-build \
      pkg-config \
      texinfo \
      yasm \
      libfreetype6-dev:armhf \
      libvorbis-dev:armhf \
      libmp3lame-dev:armhf \
      libx264-dev:armhf

      wget http://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2
      tar xjvf ffmpeg-snapshot.tar.bz2
      cd ffmpeg
      
    ./configure --enable-cross-compile \
     --enable-static \
     --enable-shared \
     --prefix=/usr \
     --enable-avfilter \
     --enable-logging \
     --enable-optimizations \
     --enable-avdevice \
     --enable-avcodec \
     --enable-avformat \
     --enable-network \
     --enable-swscale-alpha \
     --enable-dct \
     --enable-fft \
     --enable-mdct \
     --enable-rdft \
     --enable-runtime-cpudetect \
     --enable-hwaccels \
     --enable-swscale \
     --enable-indevs \
     --enable-alsa \
     --enable-outdevs \
     --enable-pthreads \
     --enable-zlib \
     --disable-bzlib \
     --enable-libvorbis \
     --enable-muxer=ogg \
     --enable-encoder=libvorbis \
     --enable-ffmpeg \
     --disable-openssl \
     --enable-libmp3lame \
     --disable-libmodplug \
     --disable-libspeex \
     --enable-libfreetype \
     --enable-armv6 \
     --enable-vfp \
     --enable-neon \
     --enable-pic \
     --cpu=cortex-a7 \
     --arch=arm \
     --target-os=linux \
     --pkg-config-flags="--static" \
     --host-cc='/usr/bin/arm-linux-gnueabihf-gcc' \
     --cross-prefix=arm-linux-gnueabihf- \
     --extra-libs=-latomic \
     --disable-version3 \
     --disable-extra-warnings \
     --disable-gray \
     --disable-small \
     --disable-crystalhd \
     --disable-dxva2 \
     --disable-hardcoded-tables \
     --disable-mipsdsp \
     --disable-mipsdspr2 \
     --disable-msa  \
     --disable-cuda \
     --disable-cuvid \
     --disable-nvenc \
     --disable-avisynth \
     --disable-frei0r \
     --disable-libopencore-amrnb \
     --disable-libopencore-amrwb \
     --disable-libdc1394 \
     --disable-libgsm \
     --disable-libilbc \
     --disable-libvo-amrwbenc \
     --disable-symver \
     --disable-doc \
     --disable-gpl \
     --disable-nonfree  \
     --disable-ffplay \
     --disable-libv4l2 \
     --disable-ffprobe \
     --disable-libxcb \
     --disable-postproc \
     --disable-libfdk-aac \
     --disable-libcdio \
     --disable-gnutls \
     --disable-libdrm \
     --disable-libopenh264 \
     --disable-vaapi \
     --disable-vdpau \
     --disable-mmal --disable-omx --disable-omx-rpi --disable-libopencv --disable-libopus --disable-libvpx --disable-libass --disable-libbluray --disable-libmfx --disable-librtmp --disable-libtheora  --disable-iconv  --disable-fontconfig --disable-libopenjpeg --disable-libx264 --disable-libx265 --disable-libdav1d --disable-x86asm --disable-mmx --disable-sse --disable-sse2 --disable-sse3 --disable-ssse3 --disable-sse4 --disable-sse42 --disable-avx --disable-avx2 \
     --disable-altivec --disable-stripping

    아래는 없는 커맨드라고 함
     --enable-libwavpack \
     --disable-avresample \

    ffmpeg 실행 됨, 변환된 mng,mp3 파일 advmenu에서 play됨 (8/12)
    아래 라이브러리 추가 필요함
    liblzma.so.5 : --disable-lzma
    libmp3lame.so.0


추가 라이브러리 경로

    export LD_LIBRARY_PATH=/mnt/bin

참고:

[https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu](https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu)
[https://github.com/andybalaam/retropie-recording/blob/master/build-retroarch-with-ffmpeg.sh](https://github.com/andybalaam/retropie-recording/blob/master/build-retroarch-with-ffmpeg.sh)
