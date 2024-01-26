---
layout: posts
title:  "8bitdo m30 for linux"
comments: true
categories : [pad,jelos]
tags : [m30,8bitdo,jelos]
---

Press and hold for 5 sec. If LED blinks, the mode has changed.
There are three modes that can be set, and they are as follows.

combo key | name in evtest
------ | --------
home + down (default) |  /dev/input/event5:	6B controller
home + left | /dev/input/event5:	Nintendo Co., Ltd. Pro Controller
home + up |  /dev/input/event5:	Microsoft X-Box 360 pad

in X-Box 360 dpad

key | event
------ | --------
X | BTN_NORTH
Y | BTN_WEST
A | BTN_SOUTH
B | BTN_EAST
C | ABS_RZ
Z | BTN_TR
START | BTN_START
SELECT | BTN_SELECT
L | BTN_TL
R | ABS_Z


In xbox360 mode, you can use dpad for your purpose.
For compatibility, you can match it with left analog stick.
Depending on the game, you may need it in hat mode, so please use it according to the game.
There are three modes that can be set as follows. (Press and hold for 5 sec.)

combo key | name in evtest
------ | --------
mode + up |  hat
mode + left | left analog stick
mode + right | right analog stick
