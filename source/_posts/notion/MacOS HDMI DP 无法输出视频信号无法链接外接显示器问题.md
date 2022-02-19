---
categories:
  - Mac
created_time: February 19, 2022 9:30 AM
date: 2020/07/01
show_category: No
status: 待发布
updated_time: February 19, 2022 11:12 AM
title: MacOS HDMI DP 无法输出视频信号无法链接外接显示器问题
---


解决方式：

1. 用其他电脑查看是否是线和显示器本身有问题
2. 如果显示器和线本身没有问题，重启重新插线尝试
3. 「设置」-〉「显示器」 -〉 按住option键，点击查找显示器
4. PRAM/NVRAM 重置
    1. 关机
    2. 按住Option，Command，P，R开机，直到出现苹果logo开机
    3. 重新插线尝试
5. 查看电脑是否禁用过独立显卡
    1. 打开命令行，输入`pmset -g` 回车，`gpuswitch` 为0说明独显被禁用
    2. 打开自动切换显卡，命令行输入`sudo pmset -a GPUSwitch 2` 然后输入本机密码（密码不显示，输就行了），输完回车
    3. 再次重新插线尝试
6. 还不行就打官方客服电话吧