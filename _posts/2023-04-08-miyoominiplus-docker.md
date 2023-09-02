---
layout: posts
title:  "miyoo mini + docker"
comments: true
categories : [miyoominiplus]
tags : [miyoominiplus]
---

mm+ 용 에뮬레이터를 빌드하기 위해서는 아래 toolchain을 사용해야 합니다. build 확인된 commit 시점으로 이동합니다.

    git clone https://github.com/shauninman/union-miyoomini-toolchain.git

    git reset --hard c1d90d8accd168991692a2cfa15e8cea866bd41f
    sudo make shell
