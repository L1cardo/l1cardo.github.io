---
title: macOS沙盒里，如何安全又优雅地运行沙盒外脚本？
date: '2025-10-06 15:47:47'
updated: '2025-10-06 15:50:29'
tags:
  - Swift
  - macOS
categories:
  - Swift
comments: true
toc: true
---



# macOS沙盒里，如何安全又优雅地运行沙盒外脚本？

> 参考自：https://github.com/lexrus/SwiftyMenu

## 1. 问题背景

在开发 macOS 应用时，苹果的沙盒（Sandbox）安全机制会带来一个挑战：为了保证用户安全，应用不能随意执行外部的脚本文件。本文档提供了一种可靠、灵活的解决方案。

## 2. 核心思路与流程

这个方案的核心是使用一个统一的辅助脚本 (`runner.sh`) 作为所有脚本任务的“调度中心”。应用本身只和这个调度中心打交道，由它来分发和执行具体的任务。

我们依赖苹果官方推荐的沙盒内执行工具 `NSUserUnixTask` 来调用我们的调度中心。

**总流程如下：**

```
App -> NSUserUnixTask -> runner.sh -> 最终要执行的脚本或命令
```

## 3. 核心调度脚本 `runner.sh` 详解

​`runner.sh` 是本方案的基石。理解它的工作原理至关重要。

### 3.1. 脚本源码

该脚本的功能是接收一系列参数，将第一个参数作为要执行的命令，并将其余所有参数传递给该命令。

```bash
#!/bin/sh
# 1. 将第一个参数赋值给变量 action
action="$1"

# 2. 将所有参数向左移动一位，$2 变成 $1，$3 变成 $2，以此类推
shift 1

# 3. 执行 action 变量中的命令，并将所有剩余参数传递给它
"$action" "$@"
```

### 3.2. 工作原理

​`runner.sh`​ 巧妙地利用 `shift` 命令分离了“执行者”和“执行参数”。

例如，当 Swift 代码通过 `NSUserUnixTask`​ 传递以下参数数组时：  
​`["/bin/sh", "/path/to/temp_script.sh", "arg1"]`

1. ​**​`action="$1"`​** ​：`action`​ 变量被赋值为 `"/bin/sh"`。
2. ​**​`shift 1`​**​：参数列表变为 `["/path/to/temp_script.sh", "arg1"]`。
3. ​ **​`"$action" "$@"`​** ​：最终执行的命令被解析为 `/bin/sh "/path/to/temp_script.sh" "arg1"`。

这样，`runner.sh` 就实现了作为一个通用调度代理的功能。

## 4. Swift 实现步骤

现在，让我们看看如何在 Swift 代码中实现这个流程。

### 4.1. 第一步：定义 `runner.sh`

为确保安全和一致性，我们将 `runner.sh` 的内容直接定义为 Swift 字符串。当然，您也可以将其作为文件打包在 App 中。

```swift
// runner.sh 的脚本内容
public enum PersistentHelperScript: String {
    case runner = """
        #!/bin/sh
        # 第一个参数是要执行的命令或脚本
        # 后续所有参数都传递给该命令
        action="$1"
        shift 1
        "$action" "$@"
        """
}
```

### 4.2. 第二步：安装 `runner.sh`

在 App 首次启动时，需将上述脚本内容写入磁盘上的固定位置，并赋予执行权限。这个位置通常在 `~/Library/Application Scripts/` 目录下，沙盒应用对此目录有权访问。

```swift
import Foundation

// 在 App 启动时调用，以确保 runner.sh 已安装
func installRunnerScript() {
    // getRunnerURL() 是一个你需要自己实现的函数，用来获取 runner.sh 的预定安装路径
    guard let runnerURL = getRunnerURL() else { return }
    let scriptContent = PersistentHelperScript.runner.rawValue
    let fileManager = FileManager.default
    
    // 确保目录存在
    let dirPath = runnerURL.deletingLastPathComponent()
    if !fileManager.fileExists(atPath: dirPath.path) {
        try? fileManager.createDirectory(at: dirPath, withIntermediateDirectories: true)
    }

    // 如果脚本文件不存在，就创建它
    if !fileManager.fileExists(atPath: runnerURL.path) {
        guard let data = scriptContent.data(using: .utf8) else { return }
        // 创建文件并设置权限为 0o755 (可读/可写/可执行)
        fileManager.createFile(atPath: runnerURL.path, contents: data, attributes: [
            .posixPermissions: 0o755
        ])
    }
}
```

### 4.3. 第三步：创建统一的执行函数

应用中所有的脚本任务，都通过下面这个 `execute` 函数来统一调用。

```swift
// 所有脚本任务的统一出口
func execute(arguments: [String], completion: ((Error?) -> Void)? = nil) {
    guard let runnerURL = getRunnerURL() else { return }

    guard FileManager.default.isExecutableFile(atPath: runnerURL.path) else {
        completion?(/* ... 创建一个错误 ... */)
        return
    }

    do {
        // 使用 NSUserUnixTask 执行我们的 runner.sh
        let task = try NSUserUnixTask(url: runnerURL)
        task.execute(withArguments: arguments) { error in
            DispatchQueue.main.async {
                completion?(error)
            }
        }
    } catch {
        completion?(error)
    }
}
```

### 4.4. 第四步：不同场景的调用方法

通过构造不同的参数列表，我们可以指挥 `runner.sh` 完成各种任务。

 *(注：以下示例依赖一个辅助函数* *​`createTempFile`​*​ *来创建临时脚本，请参考您提供的原文实现该函数。)*

#### 场景 A：执行一个基本的 Shell 脚本

**目标**：运行一个临时的 Shell 脚本文件。

```swift
// --- 调用代码 ---
let scriptContent = "echo '任务完成.'"
guard let tempScriptPath = createTempFile(content: scriptContent) else { return }

// 构造参数列表
let arguments = [
    "/bin/sh",        // 第一个参数：要 runner.sh 执行的命令
    tempScriptPath    // 第二个参数：传递给 /bin/sh 的参数
]

execute(arguments: arguments) { error in
    try? FileManager.default.removeItem(atPath: tempScriptPath) // 执行后删除临时文件
    if error == nil { print("Shell 脚本执行成功。") }
}
```

#### 场景 B：执行 AppleScript

**目标**：运行一段临时的 AppleScript。

```swift
// --- 调用代码 ---
let scriptContent = "display dialog \"来自 AppleScript 的消息。\""
guard let tempAppleScriptPath = createTempFile(content: scriptContent, ext: "applescript") else { return }

// 构造参数列表
let arguments = [
    "/usr/bin/osascript", // 第一个参数：直接使用 osascript 命令
    tempAppleScriptPath   // 第二个参数：要执行的 AppleScript 文件
]

execute(arguments: arguments) { error in
    try? FileManager.default.removeItem(atPath: tempAppleScriptPath) // 执行后删除
    if error == nil { print("AppleScript 执行成功。") }
}
```

#### 场景 C：执行复杂的终端命令

**目标**：在 iTerm2 中运行一条命令。对于这种复杂操作，我们可以在运行时动态创建一个临时的辅助脚本来处理。

```swift
// 1. 将复杂的终端控制脚本定义为字符串
let terminalExpertScriptContent = """
    #!/usr/bin/env bash
    这里定义一些复杂的脚本，都是没问题的
    """

// --- 调用代码 ---
// 2. 运行时，创建这个临时的专家脚本
guard let tempExpertPath = createTempFile(content: terminalExpertScriptContent) else { return }

// 3. 构造参数列表，让 runner.sh 去执行这个临时脚本
let arguments = [
    "/bin/bash",      // 第一个参数：使用 bash 来运行我们的专家脚本
    tempExpertPath,   // 第二个参数：我们刚刚创建的临时脚本的路径
    
    // 后续参数：这些将全部传递给我们的临时专家脚本
    "com.googlecode.iterm2",
    "/Users/Shared",
    "echo '复杂任务执行成功!'"
]

execute(arguments: arguments) { error in
    try? FileManager.default.removeItem(atPath: tempExpertPath) // 执行后删除
    if error == nil { print("终端命令执行成功。") }
}
```

## 5. 结论与优势总结

这个方案通过 **​`App -> NSUserUnixTask -> runner.sh -> 用户脚本`​** 的链条，在完全遵循 macOS 沙盒规则的前提下，实现了强大而灵活的脚本执行功能。

其优点非常明确：

- **最小化安装**：只在用户系统上安装一个必要的 `runner.sh` 文件。
- **高度灵活**：既能直接调用系统命令，也能通过创建临时脚本来处理任意复杂的逻辑。
- **安全可靠**：执行流程清晰，入口统一，临时文件用完即删，非常符合沙盒的设计理念。
