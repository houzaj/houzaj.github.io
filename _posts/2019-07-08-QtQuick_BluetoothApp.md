---
layout: post
title: 'QtQuick | 蓝牙控制App'
date: 2019-07-08
author: HouZAJ
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190122%20OpenGLAniPop/20190122-01.png'
tags: Project
---

> QtQuick框架下制作的蓝牙控制App


### BB

<br>

### Introduction
本文主要介绍了蒟蒻使用QtQuick制作通过蓝牙连接蓝牙模块App中的核心步骤，包括了QtQuick Control的Material样式设置、使用Loader的页面切换、QtBluetooth的使用，定时器的使用等。  
<br>

### Hint
本节记录了一些在开发过程中遇到的坑点。
编译到Windows下运行：

- 使用MinGW_w64编译会出现无法使用蓝牙的问题，使用MSVC2017编译可以解决这个问题。  
- 使Qt自动配置MSVC2017编译器，可以安装VS 2017，没记错的话还要安装Windows SDK。  

编译到Android下运行：

- 貌似Android Target API要选最新的，不然会出现莫名其妙的编译错误。  
- 编译错误可能与Android NDK的版本有关。
- Android 6.0开始，权限在xml里记录是不够的，还需要动态申请，这部分貌似不能用QML实现，需要用C++实现。  
- 蓝牙连接不仅需要蓝牙权限，还需要位置权限。  
- QtBluetooth的DeviceDiscovery模式可以动态申请位置权限。  

与Qt自身有关的：

- QtQuick Control从2.0开始，把Style取消了。  

<br>

### Environment
- Windows 10 (1903, 18362.175)  
- Qt 5.12.3  
- Qt Creator 4.8.1  
- Android SDK (platform-tools: 29.0.1, platform: android-28, build-tools: 28.0.3)  
- Android NDK 19.2.5345600  
- MSVC 2017 (Microsoft Visual C++ Compiler 15.9.28307.718 x86_amd64)  
- Bluetooth module: HC-06  

<br>

### Steps

**一、Material Design样式**

在pro文件中加入

在main.cpp文件中加入
