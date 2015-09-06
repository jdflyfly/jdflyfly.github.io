---
layout: post
title:  "Introduction"
date:   2015-09-06 19:11:00
categories: android
---


# 安卓入门

## 应用基础
每个安卓应用会被打包成一个apk文件。

安装到设备之后，每个安卓应用存活在**自己的安全沙箱**里：

* 每个安卓应用对应linux中**不同的用户**
* 默认情况，系统给每个应用分配一个**userID**，并设置应用内所有文件只有该应用有权限获取。
* 每个进程有自己的虚拟机，应用之间运行是相互独立的。
* 默认情况，每个应用都运行在自己的进程中。当任何一个应用的组件执行时，安卓启动进程。当组件已经无用或者系统缺乏资源时关闭进程。

应用之间共享数据或者应用获取系统服务的方式：

* 使得两个应用共享同一个linux userID，这样双方就可以互相获取对方的文件。 为了节省系统资源，同样userID的应用可以在同一个linux进程中运行，共享同一个虚拟机。
* 应用可以申请读取设备数据的权限，比如通讯录、短信、外部存储等，所有权限在用户安装应用时被授予。

### 应用组件

* **Activities**
每个activity代表与用户交互的一个界面。
* **Services**
在后台运行、执行长任务或者执行外部进程工作的组件，没有UI界面。
* **Content providers**
每个content provider管理着应用数据的一个共享集。其他应用通过content provider访问该应用共享的数据。
* **Broadcast receivers** 
每个broadcast receiver响应系统级别的广播声明。

安卓系统设计的独特一方面在于允许任何应用启动**其他应用**的组件。当系统启动一个组件时，启动的是该应用组件所在应用的进程。比如你的应用启动camera应用的一个activity，该activity是运行在camera应用的进程中，而不是在你的进程中。并且你的应用不能直接激活其他应用的组件，只能发送一个消息（intent）来制定某个特定应用的组件，系统替你激活。

### Manifest文件

* 定义应用的各个**组件**
* 声明应用需要的**系统权限**
* 声明应用支持的最低API Level
* 声明应用需要的**硬件和软件功能**，比如蓝牙、照相机等
* 声明应用需要的其他API库，比如Google Map Library
* 等等

注意：没有在manifest文件中申明的组件对于系统的**不可见的**，因此也不可能执行；另外其中broadcase receiver还可以通过程序注册。

### 应用资源
一个安卓应用不仅仅包涵代码，还有图片、声音、动画等。通过XML文件定义动画、菜单、风格、颜色等信息，避免了直接修改代码的麻烦并且可以用来适配设备、语言等。

## 设备兼容性

### 设备功能
可以通过在manifest文件中声明安装应用所需要的软硬件功能。

```xml
<manifest ... >
    <uses-feature android:name="android.hardware.sensor.compass"
                  android:required="true" />
    ...
</manifest>
```

### 平台版本

#### minSdkVersion
系统运行所需要的最低的sdk版本，低于该版本的设备不能安装此应用。

#### targetSdkVersion
这个属性通知系统，你已经针对这个指定的目标版本测试过你的程序，系统不必再使用兼容模式来让你的应用程序向前兼容这个目标版本。
由于Android不断向着更新的版本进化，一些行为甚至是外观可能会改变。然而，如果平台的API Level **高于**你的应用程序中的targetSdkVersion属性指定的值，系统会 **开启兼容行为**来确保你的应用程序继续以期望的形式来运行。你可以通过指定targetSdkVersion来匹配运行程序的平台的 API level来禁用这种兼容性行为。

#### build target
每个项目的根目录下都会有一个project.properties文件，这个文件中的内容用于告诉构建系统，怎样构建这个项目。打开这个文件，除了注释之外，还有以下一行：
target=android-XX
这句话就是指明build target，也就是根据哪个android平台构架这个项目。比如指明build target为android-18，就是使用sdk中platforms目录下android-18目录中的android.jar这个jar包编译项目。同样，这个android.jar会被加入到本项目的build path中。


### 屏幕配置
暂无

## 系统权限

#### 安全架构
安卓安全架构的核心设计点是：默认情况下，没有任何应用具有能够影响其他应用、操作系统和用户的权限，比如读写用户私有数据、读写其他应用文件、发送网络请求等等。应用必须声明除了基本沙箱权限之外的额外权限。

#### 应用签名
每个应用都必须经过签名，私钥由开发者保存。通过签名可以识别应用的作者。

#### 使用权限
使用设备被保护的功能时，必须在manifest文件中使用`<uses-permission>`声明应用需要的权限。

#### URI 权限
比如启动activity时，调用者可以设定flag来允许接受activity访问特定的URI。这种机制使得我们可以动态的授予一些细纬度的权限，而减少应用需要直接声明的权限。

参考：
[官方文档](http://developer.android.com/guide/index.html)