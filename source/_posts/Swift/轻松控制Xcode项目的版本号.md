---
title: 轻松控制 Xcode 项目的版本号
date: '2025-11-08 20:28:53'
updated: '2025-11-08 21:45:34'
tags:
  - Apple
  - Swift
categories:
  - Swift
comments: true
toc: true
---



# 轻松控制 Xcode 项目的版本号

当在做原生 Apple 平台的开发的时候，Xcode 是我们必须要接触的工具，如果项目的 Target 比较少还好，我们可以直接在 Xcode 中的 Version 和 Build 中直接改变版本号。但如果项目的 Target 较多时，每次更新版本都要设置多个 Target 的版本号，这就比较麻烦。

还好，我们可以给 Xcode 项目添加构建配置文件，在每次构建时就从配置文件中读取版本号。这样每次更新版本就只需要在一个文件中更改，Xcode 会自动应用的所有 Target 的 Version 和 Build 中。

在 [Apple 的文档](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project)中“详细”说明了怎么给项目添加构建配置文件，但是奈何 Apple 的文档起始一点都不详细，往往会遗漏掉最重要的步骤！

### 创建 `.xcconfig` 配置文件

1. 首先这个文件我们是希望所有的 Target 都使用，所以要在项目的根目录创建，这里选择项目的根目录。
2. 然后按下键盘快捷键 `⌘ + N` 来创建新文件
3. 在新文件的创建模版中，我们搜索 `config`​，然后再搜索结果中选择 `Configuration Settings File`​，名字我建议取名为 `Build`，因为是和编译相关，当然了您可以创建自己喜欢的名称。实际上可以创建多个配置文件，来为每一个 Target 设置一个。
4. 在创建文件时 Targets 部分一定要全部取消选择，因为这个文件只是一个编译“配置文件”，我们不希望这个文件被编译进 App 中，或者作为 App 中的 Resource 文件。创建成功后也要去掉所有的 Target Membership

### 填写配置

本篇文章聚焦于版本号控制，所以我们只需要填入版本号相关字段，其他字段可参考 [Build settings reference](https://developer.apple.com/documentation/xcode/build-settings-reference)。

- ​`MARKETING_VERSION`​ 是 Version，对应 `Info.plist`​ 中的 `CFBundleShortVersionString`
- ​`CURRENT_PROJECT_VERSION`​ 是 Build，对应 `Info.plist`​ 中的 `CFBundleVersion`

那么最后您的 `.xcconfig` 文件看起来就是这样：

```swift
//
//  Build.xcconfig
//  Calunar
//
//  Created by Licardo on 2025/11/8.
//

// Configuration settings file format documentation can be found at:
// https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project

MARKETING_VERSION = 1.19.3
CURRENT_PROJECT_VERSION = 76

```

‍

‍
