---
title: iOS系统的沙盒机制
date: 2021-03-15 14:13:54
tags:
  - iOS
  - Developer
categories: iOS
---

# 一、iOS系统的沙盒机制

---

## 沙盒机制介绍

沙盒机制是iOS系统中的一种安全体系。每个iOS程序都有一个独立的文件系统（存储空间），而且只能在对应的文件系统中进行操作，此区域被称之为沙盒。所有非代码文件都保存在此，如属性文件plist、文本文件、图像、图标、媒体资源等。

并且不像安卓系统，iOS系统比较封闭，也没有提供内存卡扩展的功能，没有开放文件管理，所以在手机上是看不到文件目录的。

## 沙盒机制的一些规则

每个应用程序都有自己的存储空间
应用程序不能翻过自己的围墙去访问其他应用的存储空间内容
应用程序请求的数据需要通过权限检测,若不符合条件,则不会被放行
如果需要访问其他某些资源,比如相册、通讯录、麦克风、地理位置等，需要经过用户同意才可以获取。
每次应用程序重新开启时目录名称会改变。
沙盒目录的结构

主要包含4个目录 MyApp.app、Documents、Library、tmp

一个应用的沙盒目录结构

![s](/assets/ios_app_layout_2x.png)

### MyApp.app

该目录存放应用本身的数据，包括资源文件和可执行文件。该目录是只读的，如果改变这个目录，将会改变应用程序的签名，应用将会无法启动。不会被iTunes和iCloud同步。

### Documents

该目录主要存放用户产生的文件，该目录会被iTunes和iCloud同步。

### Library

苹果建议用来存放默认设置或其他状态信息，该目录除Caches子目录以外会被iTunes和iCloud同步。

### Library/Caches

主要存放缓存文件，比如音乐缓存，图片缓存等，用户使用过程中的缓存都可以保存在这个目录中，可用于保存可再生文件，应用程序也需要负责删除这些文件，该目录不会被iTunes和iCloud同步。

### Library/Preferences

存放应用偏好设置文件，该目录会被iTunes和iCloud同步。

### tmp

存放各种临时文件，保存应用再次启动时不需要的文件。当应用不再需要这些文件时应该主动将其删除。该目录的文件会被系统清理，比如系统磁盘空间不足的时候。该目录不会被iTunes和iCloud同步。

# 二、获取各种文件目录的路径

---

```

// 获取沙盒主目录路径
NSString *homeDir = NSHomeDirectory(); 

// 获取Documents目录路径
NSString *docDir = [NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES) firstObject];

// 获取Library的目录路径
NSString *libDir = [NSSearchPathForDirectoriesInDomains(NSLibraryDirectory, NSUserDomainMask, YES) lastObject];

// 获取Caches目录路径
NSString *cachesDir = [NSSearchPathForDirectoriesInDomains(NSCachesDirectory, NSUserDomainMask, YES) firstObject];

// 获取tmp目录路径
NSString *tmpDir =  NSTemporaryDirectory();

//应用程序程序包的路径
[[NSBundle mainBundle] bundlePath]
```

# 三、NSSearchPathForDirectoriesInDomains

---

方法用于查找目录，返回指定范围内的指定名称的目录的路径集合。有三个参数：

## directory

`NSSearchPathDirectory`类型的enum值，表明我们要搜索的目录名称，比如这里用`NSDocumentDirectory`表明我们要搜索的是`Documents`目录。如果我们将其换成`NSCachesDirectory`就表示我们搜索的是`Library/Caches`目录。

## domainMask

`NSSearchPathDomainMask`类型的enum值，指定搜索范围，这里的`NSUserDomainMask`表示搜索的范围限制于当前应用的沙盒目录。还可以写成`NSLocalDomainMask`（表示/Library）、`NSNetworkDomainMask`（表示/Network）等。

## expandTilde

BOOL值，表示是否展开波浪线。我们知道在iOS中的全写形式是`/User/userName`，该值为`YES`即表示写成全写形式，为NO就表示直接写成“~”。

# 引用链接

---

[iOS 沙盒目录结构及正确使用](https://www.jianshu.com/p/dd3f120eb249)

[File System Basics](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html)

[Function NSSearchPathForDirectoriesInDomains(::_:)](https://developer.apple.com/documentation/foundation/1414224-nssearchpathfordirectoriesindoma)
