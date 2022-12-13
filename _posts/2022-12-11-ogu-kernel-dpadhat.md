---
layout: posts
title:  "ogu kernel dpad hat 변경"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,kernel,dpad,hat]
---

evtest 를 실행해보면 joystick driver 명을 확인할 수 있습니다.

    /dev/input/event0:      gou-gpio-keys
    /dev/input/event1:      rk8xx_pwrkey
    /dev/input/event2:      AML-AUGESOUND Headphones
    /dev/input/event3:      odroidgo_joypad

그 이름으로 검색해보면 아래와 같은 소스에서 사용 됨을 확인합니다.
ogs 와 동일한 방법으로 변경합니다.

    drivers/input/joystick/odroid-gou-joypad.c:#define DRV_NAME "odroidgo_joypad"


    drivers/input/joystick/odroid-gou-joypad.c
    가 기존 ogs와 차이나는 점은 아날로그 스틱용 adc 포트가 4개라 mux 처리가 필요하지 않다는 점입니다.
    코드를 비교해 봐도 ogs에서 함수명만 변경되었고 mux 관련 코드만 삭제되었습니다.

    ogs
    /*
    	+--------------------------------+
    	| IIO Channel : ADC_IN1          |
    	+--------------+-----------------+-----------------+--------+---------+
    	| EN(GPIO3.B5) | SEL_A(GPIO3.B3) | SEL_B(GPIO3.B0) | SELECT |  EVENT  |
    	+--------------+-----------------+-----------------+--------+---------+
    	|      0       |        0        |         0       |  R-Y   | ABS_RY  |
    	+--------------+-----------------+-----------------+--------+---------+
    	|      0       |        0        |         1       |  R-X   | ABS_RX  |
    	+--------------+-----------------+-----------------+--------+---------+
    	|      0       |        1        |         0       |  L-Y   |  ABS_Y  |
    	+--------------+-----------------+-----------------+--------+---------+
    	|      0       |        1        |         1       |  L-X   |  ABS_X  |
    	+--------------+-----------------+-----------------+--------+---------+
    	|      1       |        X        |         X       |  XXXX  |
    	+--------------+-----------------+-----------------+--------+
    */
    ogu
    /*
    	+--------------+--------+---------+
    	|  iio_channel | SELECT |  EVENT  |
    	+--------------+--------+---------+
    	|    ADC_AIN0  |  R-Y   | ABS_RY  |
    	+--------------+--------+---------+
    	|    ADC_AIN1  |  R-X   | ABS_RX  |
    	+--------------+--------+---------+
    	|    ADC_AIN2  |  L-Y   |  ABS_Y  |
    	+--------------+--------+---------+
    	|    ADC_AIN3  |  L-X   |  ABS_X  |
    	+--------------+--------+---------+
    */

아래 파일에서 joypad관련 설정은 별도 파일에서 관리되고 있습니다.
    ./arch/arm64/boot/dts/amlogic/meson64_odroidgou.dts

    #include "mesong12b.dtsi"
    #include "meson64_gou_pmic.dtsi"
    #include "meson64_gou_panel.dtsi"
    #include "meson64_gou_joypad.dtsi"

아래와 같이 hat 값으로 변경 합니다.

                    sw1 {
                            gpios = <&gpio GPIOX_0 GPIO_ACTIVE_LOW>;
                            label = "GPIO DPAD-UP";
    						linux,code = <ABS_HAT0Y>;
    						linux,input-type = <EV_ABS>;
    						linux,input-value = <0xffffffff>; // -1  
                    };
                    sw2 {
                            gpios = <&gpio GPIOX_1 GPIO_ACTIVE_LOW>;
                            label = "GPIO DPAD-DOWN";
                            linux,code = <ABS_HAT0Y>;         
                            linux,input-type = <EV_ABS>;
                            linux,input-value = <1>;  
                    };
                    sw3 {
                            gpios = <&gpio GPIOX_2 GPIO_ACTIVE_LOW>;
                            label = "GPIO DPAD-LEFT";
                            linux,code = <ABS_HAT0X>;
                            linux,input-type = <EV_ABS>;
                            linux,input-value = <0xffffffff>; // -1
                    };
                    sw4 {
                            gpios = <&gpio GPIOX_3 GPIO_ACTIVE_LOW>;
                            label = "GPIO DPAD-RIGHT";
                            linux,code = <ABS_HAT0X>;
                            linux,input-type = <EV_ABS>;
                            linux,input-value = <1>;
                    };


변경된 dtb 파일을 빌드 하기 위해서는 Makefile 에서 추가해 줘야 합니다.

    arch/arm64/boot/dts/amlogic/Makefile:dtb-$(CONFIG_ARCH_MESON64_ODROIDN2) += meson64_odroidgou.dtb

최종 빌드 후 아래와 같은 파일이 추가로 생성됩니다.

    ./arch/arm64/boot/dts/amlogic/meson64_odroidgou_trngaje.dtb


### build 된 kernel 을 적용하려면

    sudo fdisk -l
    ogu 가 /dev/sdb로 인식되고 있기 때문에

    $ mkdir -p ./mount
    $ sudo mount /dev/sdb1 ./mount
    $ sudo cp arch/arm64/boot/Image.gz arch/arm64/boot/dts/amlogic/meson64_odroid*.dtb ./mount && sync && sudo umount ./mount

    $ sudo mount /dev/sdb2 ./mount
    $ sudo make modules_install ARCH=arm64 INSTALL_MOD_PATH=./mount && sync && sudo umount ./mount


evtest 결과

    Event: time 1670771528.364124, type 3 (EV_ABS), code 16 (ABS_HAT0X), value -1
    Event: time 1670771528.364124, -------------- SYN_REPORT ------------
    Event: time 1670771528.536117, type 3 (EV_ABS), code 16 (ABS_HAT0X), value 0
    Event: time 1670771528.536117, -------------- SYN_REPORT ------------
    Event: time 1670771529.916132, type 3 (EV_ABS), code 16 (ABS_HAT0X), value 1
    Event: time 1670771529.916132, -------------- SYN_REPORT ------------
    Event: time 1670771530.076151, type 3 (EV_ABS), code 16 (ABS_HAT0X), value 0
    Event: time 1670771530.076151, -------------- SYN_REPORT ------------
    Event: time 1670771530.796115, type 3 (EV_ABS), code 17 (ABS_HAT0Y), value -1
    Event: time 1670771530.796115, -------------- SYN_REPORT ------------
    Event: time 1670771530.928244, type 3 (EV_ABS), code 17 (ABS_HAT0Y), value 0
    Event: time 1670771530.928244, -------------- SYN_REPORT ------------
    Event: time 1670771531.616121, type 3 (EV_ABS), code 17 (ABS_HAT0Y), value 1
