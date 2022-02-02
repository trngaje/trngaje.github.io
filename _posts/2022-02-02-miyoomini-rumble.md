---
layout: posts
title:  "miyoo-mini rumble motor 제어 방법"
---

### 기술 관련 전문 (motor 테스트 앱에 포함되어 있는 pdf 에 적혀 있는 내용을 내마음 대로 번역해 보았습니다.)

진동 모터 개선을 위한 설명

사실 Stu 는 나쁜 모터의 성능 문제를 개선하기 원했지만 진동 모터를 재설계한 후에 진동하지 않는 문제가 사라졌다.
그래서 Stu 는 진동 모터를 부스트하는 방법을 공유해야만 했다.

Describe the motor modification
In fact, Stu originally wanted to fix the problem of poor motor performance, but after remodeling the vibration motor, the problem of non-vibration disappeared.
So Stu had to share how to boost the vibration motor.

![](/images/2022-02-02/miyoo_mini_rumble_1.PNG)

Stu가 처음 meter 위에서 모터를 진동시키기 위해 Darlington 회로를 사용해서 그 회로에는 익숙했다.

PNP 트랜지스터가 없었고 내 미유미니 장치에서 이 회로를 보게될 줄 몰랐다.

The circuit was so familiar that Stu first used the Darlington circuit to vibrate the motor on the meter because he had no hands.
There was no PNP transistor on it. I didn't expect to see this circuit on my Miyu Mini handheld.

![](/images/2022-02-02/miyoo_mini_rumble_2.PNG)

Stu 는 저항 값을 35R 에서 1R로 변경했다. (더 작은 저항 값은 더 큰 진동을 갖게 한다.)
Stu changed 35R to 1R (the smaller the resistance, the stronger the vibration).

![](/images/2022-02-02/miyoo_mini_rumble_3.PNG)

Stu 는 얼마간 미유 미니 시스템을 살펴보았지만 진동 모터를 제어할 방법을 찾을 수 없었다.

나는 PS simulator 가 진동을 지원한다는 것을 알아냈고 역방향으로 해보니 Miyu Mini 가 GPIO 48 진동 모터 제어 신호를 사용하고 있음을 알아냈다. 


Stu spent some time looking at the Miyo Mini system, but couldn't find a way to control the vibration motor.
I found out that the PS simulator has vibration support, so I turned it upside down and found out that Miyu Mini was using GPIO.-48 Control.
vibration control motor

![](/images/2022-02-02/miyoo_mini_rumble_4.PNG)

제어하는 방법은 아래와 같다.
Control methods are as follows:

     # echo 48 > /sys/class/gpio/export
     # echo out > /sys/class/gpio/gpio48/direction
     # echo 0 > /sys/class/gpio/gpio48/value

     # echo 1 > /sys/class/gpio/gpio48/value
     # echo 48 > /sys/class/gpio/unexport


### mgba core 에서 진동 기능 적용
아래 소스 내에
src/platform/libretro/libretro.c:
진동 모터를 돌릴 때

    system("echo 48 > /sys/class/gpio/export");	
    system("echo out > /sys/class/gpio/gpio48/direction");	
    system("echo 0 > /sys/class/gpio/gpio48/value");	

진동 멈추를 멈출 때

    system("echo 1 > /sys/class/gpio/gpio48/value");	
    system("echo 48 > /sys/class/gpio/unexport");	

### 향후 개선 필요한 부분
- mgba 코어에서 on / off 명령어가 반복적으로 나가지 않도록 수정 필요함
- 미유 미니에서 mgba core 구동 성능이 떨어지기 때문에 gpsp 에서 rumble 기능 구현 필요하지만, 현재 적용된 소스를 구할 수 없음
- 진동 대비 진동모터에서 들리는 소음이 더 크기 때문에 게임 플레이 하기에는 적당하지 않음. 물리적인 모터를 좀 조용한 것으로 교체 필요함
 
