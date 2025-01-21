---
title: "STM32F03标准库cmake|demo"
date: 2025-01-21T7:00:45+08:00
draft: false
---

#### <div style="text-align: center;"> 使用强大的cmake、openocd开源编译器，不限平台。</div>

##### 一、安装必要工具 
1. MAC安装通过homebrew,linux等可以使用自带的包管理器。
```bash
#For mac,first install homebrew 
xcode-select --install 
git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install
/bin/bash brew-install/install.sh
rm -rf brew-install
```
```bash
#Then
brew install cmake make armmbed/formulae/arm-none-eabi-gcc openocd git 
```
##### 二、安装vscode
1. 从[Visual Studio Code官网](https://code.visualstudio.com/)下载并安装vscode
2. 安装必要的vscode插件：
    - C/C++
    - CMake
    - Cortex-Debug
##### 三、系统工程示意图
![系统工程示意图](https://pisces.now.cc/d/BQACAgUAAxkDAAOEZ49h08GanaUAAZKv9RO_g5z4K9lQAAJzFAACJwl4VGL7TMIYHgaGNgQ)
