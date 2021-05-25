---
title: 关于exportOptions.plist的说明
date: 2021-04-13 10:51:14
tags: 
	- iOS 
	- Developer
categories: iOS
---

# 一、 简介

我们在使用xcodebuild命令行工具进行自动化构建过程中，会用到-exportOptionsPlist这个命令指定一个配置文件，xcodebuild会使用该文件导出合适的ipa文件。在这里就简单说明一下这个配置文件的内容。

# 二、 说明

## compileBitcode

参数类型：`Bool`

该参数告诉Xcode是否需要通过bitcode重新编译，需要与app中的Enable Bitcode配置一致。

## destination 

参数类型：`String`

该参数用来确认当前app是本地导出还是上传到Apple的服务器。可以填写的值为`export`、`upload`，默认值为`export`。

## distributionBundleIdentifier

参数类型：`String`

该参数用来格式化包内可用目标的bundle identifier。

## embedOnDemandResourcesAssetPacksInBundle

参数类型：`Bool`

该参数在非app store的导出类型下有效。如果app使用了On Demand Resources功能，该选项为YES时，app将会加载所有的资源，可以在没有服务器支持下使用该app。如果没有配置`onDemandResourcesAssetPacksBaseURL`选项，则默认值为YES。

## generateAppStoreInformation

参数类型：`Bool`

该参数在app store的导出类型下有效。在iTMSTransporter上传时判断是否生成App Store的相关信息。默认值为NO。

## iCloudContainerEnvironment

参数类型：`String`

## manifest

参数类型：`Dictionary`

该参数在非app store的导出类型下有效。该参数用于web上安装测试应用包使用。该字典包含appURL、displayImageURL、fullSizeImageURL，如果使用了On Demand Resources，还需要配置assetPackManifestURL。

## method

参数类型：`String`

该参数确定Xcode该如何导出应用包。可用的选项为：app-store、validation、ad-hoc、package、enterprise、development,、developer-id和mac-application。默认值为development。

## onDemandResourcesAssetPacksBaseURL

参数类型：`String`

该参数在非app store的导出类型下有效。如果app使用了On Demand Resources，并且embedOnDemandResourcesAssetPacksInBundle配置不是YES，则需要配置该字段。该配置确定app如何下载On Demand Resources资源。

## provisioningProfiles

参数类型：`Dictionary`

该参数在手动配置签名下生效。指定包内所有可执行文件的描述文件。其中key为可执行文件对应的bundle identifier，value为描述文件的文件名或UUID。

## signingCertificate

参数类型：`String`

该参数在手动配置签名下生效。可以配置为证书名称、SHA-1 Hash或者自动选择。其中自动选择允许Xcode自动选择最新可以使用的证书，可选值为："iOS Developer"、"iOS Distribution"、"Developer ID Application"、"Apple Distribution"、"Mac Developer"和"Apple Development"。默认值和导出类型相关。

## signingStyle

参数类型：`String`

该选项用来确定签名方式，可选值为manual和automatic。如果app配置为自动签名，打包前可以修改此配置，否则该配置会被忽略。

## stripSwiftSymbols

参数类型：`Bool`

该参数用来确认是否需要对swift库进行裁剪。默认值为YES。

## teamID

参数类型：`String`

该参数表明导出包使用的开发者ID。

## thinning

参数类型：`String`

该参数在非app store的导出类型下有效。使用该参数可以打包出指定设备的精简包。可选项为<none>（不精简）、<thin-for-all-variants>（生成一个通用包和所有精简包）或者指定设备的标识符（如："iPhone7,1"）。默认值为<none>。

## uploadBitcode

参数类型：`Bool`

该参数在app store的导出类型下有效。用来配置导出的包是否包含bitcode。默认值为YES。

## uploadSymbols

参数类型：`Bool`

该参数在app store的导出类型下有效。用来配置导出的包是否包含符号表。默认值为YES。

# 三、 参考

使用命令`xcodebuild -help`。
