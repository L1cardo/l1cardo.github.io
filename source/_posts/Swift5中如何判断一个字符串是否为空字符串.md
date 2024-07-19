---
title: Swift5中如何判断一个字符串是否为空字符串
categories:
  - Swift
tags:
  - Swift
  - iOS
  - macOS
date: 2019-11-03 00:00:00
cover: https://42f2671d685f51e10fc6-b9fcecea3e50b3b59bdc28dead054ebc.ssl.cf5.rackcdn.com/illustrations/progressive_app_m9ms.svg
---

# Swift5中如何判断一个字符串是否为空字符串

> 2019-11-03
> 
> 转自[掘金社区-Lebus](https://juejin.im/post/5d6916bbf265da039d32e457)

### Swift中判断字串是否为空有两种方法：

```swift
"xx".isEmpty     // 推荐
"xx".count == 0  // 不推荐，因为需要遍历，费资源
```

但isEmpty无法判断这种情况：

```swift
"   ".isEmpty  // false 
```

这种全部是空格的字串也被判断成了非空，也就是说Swift认为`" "`不是空字串。

在实际开发中我们一般不希望这样。

### 所以可以给`String`加个扩展计算属性：

```swift
extension String { 
    var isBlank: Bool { 
        let trimmedStr = self.trimmingCharacters(in: .whitespacesAndNewlines)
        return trimmedStr.isEmpty
    }
}

"  ".isBlank  // true
```

### 用到的两个东西解释一下：

#### 1. trimmingCharacters

```swift
"xx".trimmingCharacters(...)
```

顾名思义，截取字符串。

把字符串中的一些东西截掉，然后扔掉

截掉哪些东西呢？--在括号里面的参数中规定

#### 2. CharacterSet

```swift
"xx".trimmingCharacters(in: CharacterSet.xxx)
```

参数是个`CharacterSet`类型，顾名思义：字符集，也就是一堆字符的集合。

就是说把我们平常见到的单个字符按照一定的条件进行了分类，比如：

```swift
CharacterSet.whitespacesAndNewlines  // 空格和换行符
CharacterSet.letters                 // 所有英文字母的集合...
```

他里面有很多静态方法，上面两个就是，所以我们可以直接用CharacterSet.xx

大家可以去文档寻找更多用法： [developer.apple.com/documentati…](https://developer.apple.com/documentation/foundation/characterset)

### 回正题：

让`trimmingCharacters`截掉哪些字串呢，在这里我们是要截掉所有的空格和换行。

然后再把截掉后的字串用isEmpty来判断，就可以完美的排除用户输入空格的情况了。

## 当然这是我自己总结的方法，如果大家还有更好的方法欢迎留言

<div style="display: flex;align-items: center;justify-content: space-evenly;">
  <img src="https://mirror.ghproxy.com/https://raw.githubusercontent.com/L1cardo/l1cardo.github.io/blog/themes/butterfly/source/img/notbyai_cn.png" alt="真人撰写" style="height: 42px;">
  <img src="https://mirror.ghproxy.com/https://raw.githubusercontent.com/L1cardo/l1cardo.github.io/blog/themes/butterfly/source/img/notbyai_en.png" alt="written by human" style="height: 42px;">
</div>
