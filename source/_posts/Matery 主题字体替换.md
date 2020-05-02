---
title: Matery 主题字体替换
categories:
  - Hexo
tags:
  - Hexo
  - Matery
abbrlink: 16590
date: 2019-10-04 00:00:00
cover: https://42f2671d685f51e10fc6-b9fcecea3e50b3b59bdc28dead054ebc.ssl.cf5.rackcdn.com/illustrations/fashion_blogging_w9ol.svg
---

# Matery 主题字体替换

> 2019-10-04

群里的一个小伙伴前几天问怎么替换 Matery 主题的字体，想改的更加个性化一点，我当时给他讲了一下方法。后来一想，可能更多的人会需要这个教程，于是今天就抽出时间谢谢这个教程吧。其实不难的，步骤很少。

### 1. 全局字体自定义

1. 在 Hexo 根目录下的 `source` 文件夹内创建一个名为 `font` 的文件夹，即文件夹路径为 `/source/font/` ，用来统一存放你要用到的字体

2. 将你要用到的字体放入上述创建的文件夹内，字体名称最好为英文，如 `/source/font/myFont.ttf` 

3. 找到主题文件夹下的 `my.css` 文件，路径为 `/themes/hexo-theme-matery/source/css/my.css` ，并将下面的命令填入的文件中
   
   ```css
   @font-face{
       font-family: 'myFont';
       src: url('../font/myFont.ttf');
   }
   
   body{
       font-family: 'myFont';
   }
   ```
   
   将上面的 `myFont` 改成你自己的字体名称即可 

### 2. 局部字体自定义

如果有点 HTML 或者 CSS 基础的同学能看出来，我们在全局字体自定义中给整个网页的 `body` 给了一个 `font-family` 的属性。

那么不难想到，局部字体自定义就是自定义一个属性，然后让这个属性在局部起效就好，而并非整个 `body` 都起效。

1. 与全局字体自定义一样，我们需要创建 `font` 文件夹，将需要的字体放入，与上面的第1、2步一样，可以参考一下

2. 找到主题文件夹下的 `my.css` 文件，路径为 `/themes/hexo-theme-matery/source/css/my.css` ，并将下面的命令填入的文件中
   
   ```css
   @font-face{
    font-family: 'myFont';
    src: url('../font/myFont.ttf');
   }
   .diyFont{
    font-family: 'myFont';
   }
   ```
   
   这里是创建了一个 `diyFont` 的类，注意与全局字体自定义相区别

3. 找到你要自定义的区域，如我要自定义博客主页的标题字体，那么我要找到相应的文件，也就是 `/themes/hexo-theme-matery/layout/_partial/bg-cover-content.ejs` ，在相应的地方加入我刚刚自定义的 `diyFont` 类。如将下面的：
   
   ```javascript
   <div class="row">
       <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
           <div class="brand">
               <div class="title center-align">   
   ```
   
   改为：
   
   ```javascript
   <div class="row">
       <div class="col s10 offset-s1 m8 offset-m2 l8 offset-l2">
           <div class="brand">
               <div class="title center-align diyFont">
   ```
   
   也就是在对应的 `class` 里面填入你自定义的类 `diyFont`

### 3. 参考

https://www.bwxyz.top/posts/29975/
