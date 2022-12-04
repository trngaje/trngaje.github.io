---
layout: posts
title:  "ogu exfat/ext4"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,exfat]
---

### exfat 포맷

    Device         Boot Start       End   Sectors  Size Id Type
    /dev/mmcblk1p1      32768 124735487 124702720 59.5G  7 HPFS/NTFS/exFAT

fstab 에 자동으로 마운트하도록 설정하기 위해서는

    sudo nano /etc/fstab

    변경전

    LABEL=BOOT /boot/oem vfat umask=0077 0 1
    UUID=e139ce78-9841-40fe-8823-96a304a09859 / ext4 errors=remount-ro 0 1


    mmcblk1
    │
    └─mmcblk1p1
         exfat  root  49DC-23D5                              37.9G    36% /mnt/sdcar

    UUID=9E67-34FE /mnt/ssd exfat defaults 0 0

또는 아래방법도 동일함

    /dev/mmcblk1p1 /mnt/sdcard exfat defaults,uid=1000,gid=1000 0 0


    sudo mkfs.exfat -n root /dev/mmcblk1p1
    sudo fsck.exfat /dev/mmcblk1p1

수동으로 마운트 하는 방법

    sudo mount -t exfat -o defaults,uid=1000,gid=1000,umask=000 /dev/sda1 /mnt/usb

### ext4 포맷

이미 exfat으로 포맷되어 있는 sdcard는 fdisk로 기존에 생성되어 있는 파티션을 지우고 포맷해야 합니다.

    sudo mkfs.ext4 /dev/mmcblk1p1
    sudo fsck.ext4 /dev/mmcblk1p1

ext4 포맷 방법 참고:
https://blog.naver.com/PostView.nhn?blogId=005334337&logNo=221111189224&parentCategoryNo=&categoryNo=78&viewDate=&isShowPopularPosts=true&from=search
