---
layout: post
title: '计 · QtQuick - 蓝牙控制App'
date: 2019-07-16
author: HouZAJ
cover: 'https://houzajblog-1252277898.cos.ap-chengdu.myqcloud.com/20190716%20QtQuick_BlueTooth/main-01.png'
tags: Project
---

> QtQuick框架下制作的蓝牙控制App，包括了QtQuick Control的Material样式设置、使用Loader的页面切换、QtBluetooth的使用，定时器的使用  


### BB
这次嵌入式，因为看错题，以为App需要自己做，然后又从来没有开发过Android App，又不熟悉Java，学习+开发的时间最多又只有一天，咋办呢？最后蒟蒻选择了QtQuick做成了跨平台App，从学习到开发用了不到一天的时间，说明入门学习成本不高，实现还ok。  
本文不会给出整个项目代码，只记录一些特定功能的实现。  
<br>

### Introduction
本文主要介绍了蒟蒻使用QtQuick制作通过蓝牙连接蓝牙模块App中的核心步骤，包括了QtQuick Control的Material样式设置、使用Loader的页面切换、QtBluetooth的使用，定时器的使用。  
蒟蒻写的应该有许多bug，建议各位dalao还是要看看Qt官方文档 orz  
另外写完本文会有一段时间不会更新的，要综合训练了暂时不啃新点  
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

### Functions
**Material Design样式**  
Material Design样式QtQuick Control的版本至少为2  
在pro文件中加入  
```js
QT += quickcontrols2
```
在main.cpp文件中加入
```cpp
QQuickStyle::setStyle("Material");
```
在ApplicationWindow中加入，就可以指定主题了  
```js
Material.theme: Material.Light
Material.accent: Material.Blue
```
<br>

**Loader页面切换**  
如果在桌面端开发，程序小的话页面切换其实不是刚需，但是因为这次要照顾Android，所以需要进行页面切换。  
实现页面切换只需要改变`Loader.source`即可。  
```js
Loader {
    id: ld
    anchors.fill: parent
    source: "startPage.qml"
}
```
因为Loader切换时，位于source的qml中的组件会全部GG，所以程序小的话可以开一个全局的qml，然后在里面放Loader和各种全局组件。项目大的话应该有更好的方法，时间紧迫没研究过。  
<br>

**QtBluetooth**  
要连接蓝牙设备需要用到`BluetoothDiscoveryModel`用于搜索蓝牙设备，`BluetoothSocket`用于蓝牙设备间的连接、数据传输等。  
项目中`BluetoothDiscoveryModel`的代码如下。其中`discoveryMode`为`BluetoothDiscoveryModel.DeviceDiscovery`是搜索设备，为`BluetoothDiscoveryModel.FullServiceDiscovery`或`BluetoothDiscoveryModel.MinimalServiceDiscovery`为搜素蓝牙服务。  
由于`BluetoothDiscoveryModel.DeviceDiscovery`可以申请位置权限，实现蓝牙搜索，而蒟蒻又不会手写申请权限，所以蒟蒻的策略是先使用`BluetoothDiscoveryModel.DeviceDiscovery`，待到按钮触发时才改为`BluetoothDiscoveryModel.MinimalServiceDiscovery`。  
搜索到新设备时，会触发`onServiceDiscovered`，这时蒟蒻的策略是匹配MAC地址，匹配ok就传递给socket。  
```js
BluetoothDiscoveryModel {
    id: btModel
    running: true
    property bool isFound: false
    discoveryMode: BluetoothDiscoveryModel.DeviceDiscovery

    onErrorChanged: {
        if (error != BluetoothDiscoveryModel.NoError) {
            console.log(error)
        }
    }

    onServiceDiscovered: {
        if(!isFound && service.deviceAddress == string_macAddress) {
            console.log("Found new service " + service.deviceAddress + " " + service.deviceName + " " + service.serviceName);
            socket.setService(service)
            isFound = true
        }
    }
}
```
项目中`BluetoothSocket`的代码如下。当连接时，会触发`BluetoothSocket.Connected`，此时再把`BluetoothDiscoveryModel`关了，不传递时就关闭是因为还未连接，关闭会有bug出现！  
当断开时，会触发`BluetoothSocket.Unconnected`。  
目标设备发送内容，会触发`onStringDataChanged`；要向目标设备发送内容，只需要更改`socket.stringData`的内容即可。  
```js
BluetoothSocket {
    id: socket
    connected: true

    onSocketStateChanged: {
        switch (socketState) {
        case BluetoothSocket.Unconnected:
            //...
            break;
        case BluetoothSocket.Connected:
            console.log("Connected to server");
            btModel.running = false
            ld.source = "gamePage.qml"
            break
        }
    }
    onStringDataChanged: {
        let data = socket.stringData
        console.log(data)
        //...
    }
}
```
<br>

**定时器Timer**  
需要使用定时器是因为长按按钮需要不断向设备发送消息，而Qt无法处理。这时候可以改成，按下按钮时打开定时器，松开按钮时停止定时器。  
项目中一个定时器的代码如下所示。`triggeredOnStart`为一打开就生效，`repeat`为是否重复，`onTriggered`为每次需要做的内容。  
```js
Timer {
    id: timer_backward
    interval: 70
    repeat: true
    running: false
    triggeredOnStart: true
    onTriggered: {
        socket.stringData = "2"
    }
}
```
