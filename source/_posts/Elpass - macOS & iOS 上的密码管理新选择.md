---
title: Elpass - macOS & iOS 上的密码管理新选择
categories:
  - App 推荐
tags:
  - App
  - macOS
date: 2021-01-12 12:00:00
cover: https://i.loli.net/2021/01/12/jDzXF2NqQdYbL3P.png
---

# Elpass - macOS & iOS 上的密码管理新选择

说到`Surge`, 大家应该都不陌生，距离其作者发布的密码管理工具`Elpass`的上架也已经过去一年多。我最近也是百般无聊，想要试试这款软件到底有多么强。之前一直使用的 Apple 自带的密码管理和`1Password` ，`1Password` 的便捷性不必多说，功能也是很完善，多端同步更是他的主要卖点。但是我总感觉 `1Password` 很臃肿，Apple 自带的又不能填充第三方 App。所以萌生出想要实时`Elpass`的想发。

# 界面

![p4giwtXCl7HQdcq](https://i.loli.net/2021/01/12/p4giwtXCl7HQdcq.png)

界面没有特别新奇的特点，从上到下依次为 All Items、Favorites、Logins、Bank Cards、Identifications、Secure Notes、Passwords、Archived，这些分类已经可以满足记录所需了，比 1Password 少了两个分类，信用卡（Elpass 已经整合在 Bank Cards 中）和软件许可（这个其实可以用Secure Notes来代替）

密码是默认不可见的，1Password 需要在下拉菜单点击显示，Elpass 只需要在密码右侧点击 Reveal 就可以显示出来，方便快捷

Elpass 的删除规则也非常的贴切，1Password 中我们删除是直接放到了废纸篓里，这个废纸篓如果我们不主动清空他是永远不会被删除的，而 Elpass 则直接叫做归档，我们在归档里删除才是真正的删除。

![LPf1w7sAogHOFzp](https://i.loli.net/2021/01/12/LPf1w7sAogHOFzp.png)

新建条目的时候设置密码也没有 1Password 那样繁琐的设置，1Password 设置密码的时候设置好长度还要设置数字、符号需要几位。

Elpass 选择好密码需要包含的字符就可以了，十分简单。

# 安全性

![NJOgbMd3InLZYSE](https://i.loli.net/2021/01/12/NJOgbMd3InLZYSE.png)

Elpass 密码分三个等级，可以让我们灵活的自主选择什么时候需要密码，什么时候不需要密码。

Elpass 的加密模块已经开源，有兴趣的可以[点击跳转](https://github.com/surge-networks/Elpass-Core)看一下代码。

目前只支持 iCloud 和 Dropbox 两个网盘的同步，其实支持 iCloud 就足够了，毕竟我的密码我也不希望放到别人那里。

# 方便

![UhRnrSGeIy87A6u](https://i.loli.net/2021/01/12/UhRnrSGeIy87A6u.png)

Elpass 可以设置为登录系统后自动解锁，省去繁琐的主密码输入阶段。而且也支持通过 Apple Watch 进行解锁，这一点也是极其方便的。

![zNjS5rP7D9vYed3](https://i.loli.net/2021/01/12/zNjS5rP7D9vYed3.png)

Elpass 已经有了对应 Safari、Chrome、Firefox 的插件，方便网页的自动填充（不会自动提交，方便账号修改），当我们登录一些有二次验证密码的网站的时候，Eplass 也会同时弹出一个二次密码的框，并有 copy 和 auto type 两个功能，

点击 auto type Eplass 会自动把数字填入目标框，如果目标框是不支持自动填入或者粘贴的，也可以方便的手动对照输入，这一点比 1Password 方便，1Password 只是把二次密码放进了剪贴板，后续操作则不在管。

# 原生 App 填充 (Mac App Store 版本不支持)

![2RwAahefUVqklMI](https://i.loli.net/2021/01/12/2RwAahefUVqklMI.png)

这个功能算是 Elpass 的大亮点了，不管我们用 1Password 还是系统的 keychain，这些密码管理在第三方 App 上都是不会自动填充的。

Eplass 会在响应对象为密码框的时候显示一个已保存对应 app 的所有密码备选，这个识别内容是通过 App 的 Bundle ID 来识别的，而我们需要做的只是在 Elpass 中相应的密码绑定到 App 即可。

![7UZftIL1uwgzvCX](https://i.loli.net/2021/01/12/7UZftIL1uwgzvCX.png)


# 目前的不足

1. 只支持 Apple 设备可能就是 Elpass 最大的弱点
2. $1.99/月 或 $19.99/年 对于大多数人可能还是不太能接受
