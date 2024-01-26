---
layout: posts
title:  "rgb30 빌드 환경"
comments: true
categories : [rgb30]
tags : [rgb30,jelos,arkos]
---

rgb30 기기를 위한 이미지는 크게 jelos와 arkos가 존재합니다.
각각 다른 빌드 환경에서 에뮬레이터/프론트엔드/코어를 빌드 합니다.

> jelos

각 플랫폼당 200gb 의 여유 공간이 있어야 빌드가 가능합니다.
사전에 docker가 설치되어 있어야 합니다. (아래 사이트 참고)
커널을 포함한 전체 빌드를 하여 이미지를 pc에서 생성합니다.

참고 사이트

[https://jelos.org/contribute/build/](https://jelos.org/contribute/build/)

### custom jelos 전체 빌드 방법

RK3566 base인 RGB30은 아래와 같이 빌드 합니다.
RK3566은 multi device를 부팅할 수 있도록 부트로더에서 이미 정의되어 있습니다.

    make docker-shell
    make RK3566

### 모듈 추가 및 개별 빌드 방법

    PROJECT=Rockchip DEVICE=RK3566 ARCH=aarch64 scripts/build advancemame-sa
    PROJECT=Rockchip DEVICE=RK3566 ARCH=aarch64 scripts/build gmu_kr
    PROJECT=Rockchip DEVICE=RK3566 ARCH=aarch64 scripts/build mame2003-plus-kaze-lr
    PROJECT=Rockchip DEVICE=RK3566 ARCH=aarch64 scripts/build openmsx-sa

### diff 에 의한 package 수정 방법

patch 생성

emulationstation

    ~/develop/emulationstation$ mkdir -p ../distribution/packages/ui/emulationstation/patches
    ~/develop/emulationstation$ git diff > ../distribution/packages/ui/emulationstation/patches/001-mds_extension_for_saturn.patch


fileman

    trngaje@ubuntu:~/develop/fileman$ git diff > ../../mygit/distribution/packages/apps/fileman/patches/001-detect-screen.patch
    trngaje@ubuntu:~/develop/fileman$

    PROJECT=Rockchip DEVICE=RK3566 ARCH=aarch64 scripts/build fileman

### update 경로 custom 하기

### theme 추가 하기

### build 오류 수정 하기

### custom 배포 에뮬 / 코어 정보

[https://github.com/trngaje/distribution/blob/dev/documentation/PER_DEVICE_DOCUMENTATION/RK3566/SUPPORTED_EMULATORS_AND_CORES.md](https://github.com/trngaje/distribution/blob/dev/documentation/PER_DEVICE_DOCUMENTATION/RK3566/SUPPORTED_EMULATORS_AND_CORES.md)

> arkos

chroot 에서 빌드 한 바이너리를 arkos 기기에 복사하여 구동할 수 있습니다.


### kernel build 방법

      git clone -b rk2023 https://github.com/trngaje/RG353VKernel.git

      git submodule init
      git submodule update

      make ARCH=arm64 rk3566_optimized_with_wifi_linux_defconfig && make ARCH=arm64 KERNEL_DTS=rk3566 KERNEL_CONFIG=rk3566_optimized_with_wifi_linux_defconfig

      mkdir -p mount
      sudo mount /dev/sdb3 ./mount
      sudo cp arch/arm64/boot/Image arch/arm64/boot/dts/rockchip/rk3566-rgb30_trngaje*.dtb ./mount && sync && sudo umount ./mount


      sudo mount /dev/sdb4 ./mount
      sudo make modules_install ARCH=arm64 INSTALL_MOD_PATH=./mount && sync && sudo umount ./mount




### chroot 방법

chroot 환경 설치

    sudo apt update
    sudo apt install -y build-essential debootstrap binfmt-support qemu-user-static

debian buster (10)-ubuntu18.04 bionic이후, bullseye(11)-ubuntu20.04 focal 이후

    sudo qemu-debootstrap --arch armhf buster /mnt/data/armhf http://deb.debian.org/debian/

    sudo qemu-debootstrap --arch arm64 buster /mnt/data/arm64 http://deb.debian.org/debian/

    sudo qemu-debootstrap --arch armhf bullseye /mnt/data/armhf http://deb.debian.org/debian/

    sudo qemu-debootstrap --arch arm64 bullseye /mnt/data/arm64 http://deb.debian.org/debian/

32bit 환경은

    sudo chroot /mnt/data/armhf/

64bit 환경은

    sudo chroot /mnt/data/arm64/

arkos 에서 사용하는 코어와 에뮬레이터는 아래와 같이 빌드 할 수 있습니다.

git clone https://github.com/christianhaitian/rk3566_core_builds
