---
layout: posts
title:  "rgnano build sm64"
comments: true
categories : [rgnano]
tags : [rgnano]
---

빌드를 하기 위해서는 docker 환경 내에서 작업 합니다.

[https://trngaje.github.io/rgnano/rgnano-build/](https://trngaje.github.io/rgnano/rgnano-build/)


    git clone https://github.com/DrUm78/sm64-funkey
    cd sm64-funkey

아래 롬을 구해서 sm64-funkey 폴더에 복사해 넣는다

    baserom.us.z64 sha1: 9bef1128717f958171a4afac3ed78ee2bb4e86ce

채크섬 값은 아래와 같이 확인이 가능합니다.

    sha1sum baserom.us.z64

> anbernic 용으로 빌드하기 위해서는

    git apply --whitespace=fix -p1 ../sm64_rgnano.patch
    make -j4
    ./build_opk.sh rgnano

> funkey 용으로 빌드하기 위해서는

    git apply --whitespace=fix -p1 ../sm64_funkey.patch

    make -j4
    ./build_opk.sh funkey-s

### 구동 화면

![](/images/2023-09-03/sm64_us.png)

### 구동 영상
