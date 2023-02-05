---
layout: posts
title:  "ogu attractmode 내 runcommand 메뉴 표시"
comments: true
categories : [ogu,attract]
tags : [ogu,odroid go ultra,오드로이드,attractmode,runcommand]
---

text console 출력인 runcommand를 attractmode 다이얼로그로 표시해 봅니다.

#### 1.실행 다이얼로그 내 default 코어 표시

shell script (getdefault.sh)와 attract mode squirrel(layout.nut) 으로 구동이 가능합니다.

    getdefault.sh

    #!/usr/bin/bash

    emulator=$1
    cat /home/odroid/runcommand/cfg/$emulator.cfg | grep 'DEFAULT' | awk -F= '{print $2}'

    layout.nut

    fe.plugin_command( "bash", "/home/odroid/runcommand/getdefault.sh " + fe.game_info(Info.Emulator), "default_callback" );

아래와 같이 laout.nut 만으로 적용 가능합니다.

    fe.plugin_command( "/bin/sh", "-c \"cat /home/odroid/runcommand/cfg/"+fe.game_info(Info.Emulator)+".cfg | grep 'DEFAULT' | awk -F= '{print $2}'\"", "default_callback" );



![](/images/2023-02-05/gb_default.jpg)

#### 2.코어 리스트 표시

shell script (getselectlists.sh)와 attract mode squirrel(layout.nut) 으로 구동이 가능합니다.

    <etselectlists.sh> 내용

    #!/usr/bin/bash

    emulator=$1
    cat /home/odroid/runcommand/cfg/$emulator.cfg | grep 'CORES' | awk -F= '{print $2}'

    <laount.nut> 내용

    fe.plugin_command( "bash", "/home/odroid/runcommand/getselectlists.sh " + fe.game_info(Info.Emulator), "selects_callback" );

아래와 같이 laout.nut 만으로 적용 가능합니다.

    fe.plugin_command( "/bin/sh", "-c \"cat /home/odroid/runcommand/cfg/" + fe.game_info(Info.Emulator) + ".cfg | grep 'CORES' | awk -F= '{print $2}'\"", "selects_callback" );


![](/images/2023-02-05/gb_cores.jpg)

#### 3.코어리스트에서 선택한 것 core file에 업데이트

setdefault.sh

    #!/usr/bin/bash
    DEFAULT=$1
    CORE_CONF_FILE="/home/odroid/runcommand/cfg/"$2".cfg"

    sed -i '/DEFAULT=/c\DEFAULT=\"'"${DEFAULT}"'\"' ${CORE_CONF_FILE}
