+++
title = "How to Design a Keyboard"
date = "2022-01-05T21:43:38+08:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["DIY", ""]
description = "如何DIY一把键盘"
showFullContent = false
readingTime = false
+++

## 前言

> 有一些比较微妙的事，它看起来非常难，但你就是觉得自己似乎能行，便会产生强烈的欲望去实现它。

## 我想做一把什么样的键盘

1. 配列：3x6+6
2. 旋钮 OLED
3. 热插拔轴座
4. 轴灯+底灯
5. 亚克力堆叠的GasKet结构
6. QMK + VIA

## 基本流程

1. 创造配列
2. 根据配列生成矩阵
3. 绘制pcb原理图
4. 3D建模
5. 焊接装配
6. 编写和调试固件

## 资源

编辑配列：http://www.keyboard-layout-editor.com/
矩阵原理：https://kbfirmware.com/
开源固件：https://qmk.fm/
构建模型：http://builder.swillkb.com/

软件：Freecad，kicad

## QMK

```shell
sudo pacman --needed --noconfirm -Sy git python-pip libffi

## pip config set global.index-url https://mirrors.cloud.tencent.com/pypi/simple

python3 -m pip install --user qmk

## add 'export PATH="$HOME/.local/bin:$PATH"' to $HOME/.xinitrc

git clone git@github.com:philopence/qmk_firmware.git --depth=1
git submodule update --init --depth=1
git remote add upstream "https://github.com/qmk/qmk_firmware.git"
sudo cp /home/arch/qmk_firmware/util/udev/50-qmk.rules /etc/udev/rules.d/

qmk setup ## for checked
```

## Log

2020-01-05: 产生了做这件事的想法，并从触手可及的地方开始实现。简单学习了kicad和freecad的用法
2020-01-06: 受限于当前设备性能，优先完成软件部分，建模和设计放后面做。遵循始于所立原则；还是得从硬件部分着手，因为固件模板依赖于原理图，且没有硬件无法调试；键盘设计主题为JoStar-Pad，拥有旋钮，热插拔，RGB，OLED等全部功能的pad键盘，旨在练习与熟悉相关设计与编码，小尺寸节约成本。
2020-01-07: 完成了PCB原理图的绘制工作，考虑到成本和焊接问题，可能会将MCU换成开发板做成热插拔的形式，进一步修改原理图。接下来需要学会修改定位板和底板，做成三明治结构
2020-01-08: 从结果出发追根溯源或许会简单一些。
