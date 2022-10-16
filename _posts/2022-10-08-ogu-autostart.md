---
layout: posts
title:  "ogu 자동실행 구문 수정"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드]
---

ubuntu 에서 부팅시 자동으로 emulationstation을 실행해 주고 있으나, killall 로 죽여도 살아나기 때문에 개발에는 적합하지 않아 수정해 줍니다.

아래 시점에서 emulationstation 바로 실행하게 되어 있음

    cd /etc/systemd/system
    emulationstation.service

autologin 설정

    $ sudo systemctl enable getty@tty1.service
    Created symlink /etc/systemd/system/getty.target.wants/getty@tty1.service → /lib/systemd/system/getty@.service.


    $ systemctl status getty@tty1.service
    ● getty@tty1.service - Getty on tty1
         Loaded: loaded (/lib/systemd/system/getty@.service; enabled; vendor preset>
        Drop-In: /etc/systemd/system/getty@tty1.service.d
                 └─autologin.conf
         Active: inactive (dead)
           Docs: man:agetty(8)
                 man:systemd-getty-generator(8)
                 http://0pointer.de/blog/projects/serial-console.html


autologin.service는 아래와 같이 작성한다. agetty 커맨드 작성시 아래와 같이 내용 기입해야 두번 실행되지 않는다.



sudo nano /etc/profile.d/10-odroid.sh

아래와 같이 기입

    # launch our autostart apps (if we are on the correct tty)
    if [ "`tty`" = "/dev/tty1" ] && [ "$USER" = "odroid" ]; then
        bash "/home/odroid/autostart.sh"
    fi

emulationstation 바로 launch 하는 것 제거

    sudo systemctl disable emulationstation.service

    $ sudo mv /etc/systemd/systememulationstation.service ~/

sudo 입력 시 암호 입력하지 않도록 수정

    sudo echo "odroid ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers


화면 깜빡임이
sudo graphics 1 수행 후에는 발생하지 않음 (단 미리 설정하거나 그래픽 표출되기 전에 하면 효과가 없음)
After=go4.service 추가

### runcommand 설치

https://github.com/trngaje/attract_9pconfig.git
install.sh 내에
python -> python3로 변경할 것
sudo python3 setup.py install


runcommand 내 runcommand_setting.sh 동작 확인 (dpad 와 a 버튼으로 메뉴 선택이 가능한지 확인 완료)
sudo apt install curl
curl https://bootstrap.pypa.io/pip/2.7/get-pip.py -o get-pip.py
python get-pip.py

/home/odroid/.local/bin/pip install pyudev
