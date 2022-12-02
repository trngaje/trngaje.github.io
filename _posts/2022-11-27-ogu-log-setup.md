---
layout: posts
title:  "ogu log"
comments: true
categories : [ogu]
tags : [ogu,odroid go ultra,오드로이드,log]
---

불필요한 log 가 쌓이는 것을 막기 위해서는 사이즈 제한을 해야 합니다.

현재 log 저장 상태를 확인하기 위해서는

    sudo du -h /var/log


크기 제한은 아래와 같이 합니다.

    sudo nano /etc/systemd/journald.conf
    Storage=none
    SystemMaxUse=5M
    MaxFileSec=1day

    sudo nano /etc/logrotate.d$rsyslog

    /var/log/syslog
    {
            rotate 7
            size 100k
            daily
            missingok
            notifempty
            delaycompress
            compress
            postrotate
                    /usr/lib/rsyslog/rsyslog-rotate
            endscript
    }
