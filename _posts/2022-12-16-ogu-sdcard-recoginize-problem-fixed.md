---
layout: posts
title:  "ogu sdcard 인식문제 해결 방법"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,kernel,uboot,sdcard인식]
---

v1.1 (2022/09/26) ubuntu 20.04 기본 이미지에서는 reboot 시 sdcard 인식되지 않는 문제를 해결하기 위해서는 uboot와 kernel을 수정해야 합니다.

[https://forum.odroid.com/viewtopic.php?f=224&t=45790](https://forum.odroid.com/viewtopic.php?f=224&t=45790)

포럼 회신과는 달리 12/16일 현재 개선되지 않은 것 같음

### uboot 빌드


[https://wiki.odroid.com/odroid_go_ultra/software/building_u-boot](https://wiki.odroid.com/odroid_go_ultra/software/building_u-boot)


      wget https://releases.linaro.org/archive/13.11/components/toolchain/binaries/gcc-linaro-aarch64-none-elf-4.8-2013.11_linux.tar.xz
      wget https://releases.linaro.org/archive/14.04/components/toolchain/binaries/gcc-linaro-arm-none-eabi-4.8-2014.04_linux.tar.xz



      git clone https://github.com/hardkernel/u-boot.git -b odroidgoU-v2015.01


      sudp apt update
      sudo apt-get install lib32z1

      cd u-boot
      make odroidgou_defconfig
      make -j4


> uboot writing


      cd sd_fuse
      $ ./sd_fusing.sh <device/path/of/your/OGU>
      ./sd_fusing.sh /dev/sdb

      sudo fdisk -l
      Disk /dev/sdb: 116.59 GiB, 125174808576 bytes, 244482048 sectors
      Disk model: File-Stor Gadget
      Units: sectors of 1 * 512 = 512 bytes
      Sector size (logical/physical): 512 bytes / 512 bytes
      I/O size (minimum/optimal): 512 bytes / 512 bytes
      Disklabel type: dos
      Disk identifier: 0xab064922

      Device     Boot  Start       End   Sectors   Size Id Type
      /dev/sdb1        32768    262143    229376   112M  c W95 FAT32 (LBA)
      /dev/sdb2       262144 244482047 244219904 116.5G 83 Linux


> ODROIDBIOS.BIN 업데이트

      mkdir mount
      sudo mount /dev/sdb1 ./mount
      cp -v ./tools/odroid_resource/ODROIDBIOS.BIN ./mount/

> recovery 화면 버튼 숫자 업데이트

      cp -v ./tools/odroid_resource/res/* ./mount/res/

      sync
      sudo umount /dev/sdb1

      $ ./sd_fusing.sh /dev/sdb
      1725+1 레코드 들어옴
      1725+1 레코드 나감
      883568 bytes (884 kB, 863 KiB) copied, 0.169211 s, 5.2 MB/s
      eject: 꺼낼수 없습니다, 마지막 오류: 부적절한 인수
      Finished.


      uboot 버젼 확인을 위해서는
      grep -a -r -E -o "2015.01.{0,50}" /dev/mmcblk0 | grep -a "(" | cut -d' ' -f1


      2015.01-00028-g81707aac28


### kernel 빌드


      git pull
      $ git pull
      remote: Enumerating objects: 9, done.
      remote: Counting objects: 100% (9/9), done.
      remote: Compressing objects: 100% (1/1), done.
      remote: Total 5 (delta 4), reused 5 (delta 4), pack-reused 0
      Unpacking objects: 100% (5/5), 543 bytes | 108.00 KiB/s, done.
      From https://github.com/hardkernel/linux
         e9109510..06e88564  odroidgoU-4.9.y -> origin/odroidgoU-4.9.y
      Updating e9109510..06e88564
      Fast-forward
       drivers/mfd/rk808.c | 12 +++++++-----
       1 file changed, 7 insertions(+), 5 deletions(-)

      On branch odroidgoU-4.9.y
      Your branch is up to date with 'origin/odroidgoU-4.9.y'.

      Changes not staged for commit:
        (use "git add <file>..." to update what will be committed)
        (use "git restore <file>..." to discard changes in working directory)
      	modified:   arch/arm64/boot/dts/amlogic/Makefile
      	modified:   drivers/input/joystick/odroid-gou-joypad.c

      Untracked files:
        (use "git add <file>..." to include in what will be committed)
      	arch/arm64/boot/dts/amlogic/meson64_gou_joypad_trngaje.dtsi
      	arch/arm64/boot/dts/amlogic/meson64_gou_joypad_trngaje.dtsi.bak
      	arch/arm64/boot/dts/amlogic/meson64_odroidgou_trngaje.dts
      	backup_trngaje.sh
      	drivers/gator/generated_gator_src_md5.h
      	drivers/input/joystick/odroid-gou-joypad.c.bak
      	net/wireguard/crypto/zinc/chacha20/chacha20-arm64.S
      	net/wireguard/crypto/zinc/poly1305/poly1305-arm64.S


      Verify that sd card is recognized successfully when rebooted after uboot/kernel writing.
      Thank you to developers who have worked hard for weeks.







      $ mkdir -p ./mount
      $ sudo mount /dev/sdb1 ./mount
      $ sudo cp arch/arm64/boot/Image.gz arch/arm64/boot/dts/amlogic/meson64_odroid*.dtb ./mount && sync && sudo umount ./mount

      $ sudo mount /dev/sdb2 ./mount
      $ sudo make modules_install ARCH=arm64 INSTALL_MOD_PATH=./mount && sync && sudo umount ./mount


문제점
1. 업데이트 후 'sudo reboot' 을 하면 usb로 부팅이 된다. (12/20)
