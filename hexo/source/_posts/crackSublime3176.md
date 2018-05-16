---
title: 破解linux版sublime text 3176
date: 2018-05-14 11:28:55
tags: crack
---

- 我是基于apt安装的sublime text 3176，在命令行中输入下面的命令即可（原则上debian系的所有发行版均可，其他请自行测试）

***
### 操作过程

 1. 先在hosts中添加``0.0.0.0 license.sublimehq.com``, 这是防止证书被移走

 2. 然后新建脚本，保存为crack_sublime.sh

`` #!/bin/bash

SUBLIME_EXECUTABLE="$1"
CRACK="$2"

OFFSET_1=$(("$(binwalk -R '\x41\x56\x53\x48\x83\xEC\x70\x49\x89\xFE\x48\x8D\x7C\x24\x48' $SUBLIME_EXECUTABLE | tail -n2 | awk '{ print $2 }')" - 1))
OFFSET_2=$(("$(binwalk -R '\x48\x89\xFB\x80\x3B\x00\x74\x22\x48\x8D\x7B' $SUBLIME_EXECUTABLE | tail -n2 | awk '{ print $2 }')" - 1))
OFFSET_3=$(("$(binwalk -R '\x22\x00\x55\x41\x57\x41\x56\x41\x55\x41\x54' $SUBLIME_EXECUTABLE | tail -n2 | awk '{ print $2 }')" + 2))

printf "OFFSET_1=0x%04x\n" "$OFFSET_1"
printf "OFFSET_2=0x%04x\n" "$OFFSET_2"
printf "OFFSET_3=0x%04x\n" "$OFFSET_3"

if [[ $CRACK == 'true' ]]; then
    printf '\xC3' | dd seek=$(("$OFFSET_1")) conv=notrunc bs=1 of="$SUBLIME_EXECUTABLE"
    printf '\xC3' | dd seek=$(("$OFFSET_2")) conv=notrunc bs=1 of="$SUBLIME_EXECUTABLE"
    printf "\x48\xc7\xc0\x01\x00\x00\x00\xc3" | dd seek=$(("$OFFSET_3")) conv=notrunc bs=1 of="$SUBLIME_EXECUTABLE"
fi
`` 

 3.到脚本目录运行
`` sudo bash ./crack_sublime.sh /opt/sublime_text/sublime_text true ``

### 效果图

![效果图](http://ww1.sinaimg.cn/mw690/6ace449bgy1fraprms8yij208x054t8r.jpg)