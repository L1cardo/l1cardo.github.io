---
title: ELRS LED指示灯状态
date: '2025-02-21 20:42:08'
updated: '2025-02-21 21:29:37'
tags:
  - ELRS
categories:
  - ELRS
permalink: /post/elrs-led-indicator-status-1wn09q.html
comments: true
toc: true
---

![image](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/quick-start.png)

# ELRS LED指示灯状态

# 接收机/高频头 LED 状态

ExpressLRS 使用 LED 来表明 **接收机** / **高频头** 的状态。

 LED 条件和状态如下：

## 接收机

### 接收机单色 LED

||LED 指示灯|状态|
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------| --------------------| ------------------------------------|
|​​![CONNECTED](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LED_ON.gif)​|常亮|连接到高频头或进入 bootloader 模式|
|​​![LEDSEQ_BINDING](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQ_BINDING_10_10_10_100.gif)​|间断双闪|对频模式|
|​​![LEDSEQ_DISCONNECTED](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQ_DISCONNECTED_50_50.gif)​|500ms 慢闪|等待高频头连接|
|​​![LEDSEQ_MODEL_MISMATCH](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQ_MODEL_MISMATCH_10_10_10_10_10_100.gif)​|间断三闪|已连接到高频头但模型设置不匹配|
|​​![LEDSEQ_RADIO_FAILED](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQ_RADIO_FAILED_20_100.gif)​|100ms 中等速度慢闪|未检测到射频芯片|
|​​![LEDSEQ_WIFI_UPDATE](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQ_WIFI_UPDATE_2_3.gif)​|25ms 快闪|WiFi 模式|

### 接收机 RGB LED

||LED 指示灯|状态|
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------| -----------------| ------------------------------------|
|​![LEDRGB_STARTUP](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQRGB_STARTUP.gif)​|彩虹渐变效果|启动|
|​![LEDRGB_WIFI_UPDATE](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQRGB_WIFI_UPDATE.gif)​|绿色呼吸灯|网页更新模式已启用|
|​![LEDRGB_WAITING_FOR_CONNECTION](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQRGB_WAITING_TO_CONNECT.gif)​|500ms 慢闪|等待高频头连接|
|​![LEDRGB_RADIO_FAILED](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQRGB_RADIO_FAILED.gif)​|100 ms 红色闪烁|未检测到射频芯片|
|​![LEDRGB_BINDING_ENABLED](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQRGB_BINDING.gif)​|间断橙色双闪|对频模式|
|​![LEDRGB_MODEL_MISMATCH](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LEDSEQRGB_MODEL_MISMATCH.gif)​|间断橙色三闪|已连接到高频头但模型设置不匹配|
|​![LEDRGB_CONNECTED](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LED_RGB_ON.png)​|单色常亮|已连接到高频头，颜色代表数据包速率|
|​![BOOTLOADER_OR_OFF](https://raw.githubusercontent.com/l1cardo/l1cardo.github.io/blog/images/LED_RGB_OFF.png)​|无灯|关闭或处于 bootloader 模式|

|LED 颜色|2.4GHz 数据包速率 (Hz)|915/868MHz 数据包速率 (Hz)|
| ----------| ------------------------| ----------------------------|
|红|F1000|200|
|黄|F500||
|黄-绿|D500|100 Full|
|绿|D250|100|
|青|333 Full||
|浅蓝|500|50|
|蓝|250|25|
|蓝-紫|150||
|紫|100 Full|D50|
|品红|50||

## 高频头

### 高频头 RGB LED

|LED 指示灯|状态|
| -----------------| ------------------------------------|
|彩虹渐变效果|启动|
|绿色呼吸灯|网页更新模式已启用|
|蓝色呼吸灯|蓝牙摇杆模式已启用|
|100 ms 红色闪烁|未检测到射频芯片|
|橙色每秒闪烁|未检测到遥控器|
|单色常亮|已连接到接收机，颜色代表数据包速率|
|单色渐隐|未连接到接收机，颜色代表数据包速率|

|LED 颜色|2.4GHz 数据包速率 (Hz)|915/868MHz 数据包速率 (Hz)|
| ----------| ------------------------| ----------------------------|
|红|F1000|200|
|黄|F500||
|黄-绿|D500|100 Full|
|绿|D250|100|
|青|333 Full||
|浅蓝|500|50|
|蓝|250|25|
|蓝-紫|150||
|紫|100 Full|D50|
|品红|50||

<div>
<div style="display: flex;align-items: center;justify-content: space-evenly;padding-top: 40px;">
  <img src="https://ghproxy.licardo.vip/https://raw.githubusercontent.com/L1cardo/l1cardo.github.io/blog/themes/butterfly/source/img/notbyai_cn.png" alt="真人撰写" style="height: 42px;">
  <img src="https://ghproxy.licardo.vip/https://raw.githubusercontent.com/L1cardo/l1cardo.github.io/blog/themes/butterfly/source/img/notbyai_en.png" alt="written by human" style="height: 42px;">
</div>

‍
