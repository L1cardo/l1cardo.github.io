---
title: Hexo 搭建个人博客详细教程
categories:
  - Hexo
tags:
  - Hexo
date: 2019-09-30 00:00:00
cover: https://42f2671d685f51e10fc6-b9fcecea3e50b3b59bdc28dead054ebc.ssl.cf5.rackcdn.com/illustrations/blogging_vpvv.svg
---

# Hexo 搭建个人博客详细教程

> 2019-09-30

- 本文的对象是仅会电脑开机的小伙伴，必须没有一点关于网络和代码的知识。如果你已知道一些知识，可能本文不适合你，因为本文将会是史上最详细的 Hexo 教程。

### 1. Hexo 介绍

本文会详细的介绍如何使用 [Hexo](https://hexo.io/) 搭建个人博客。Hexo 是基于 Node.js 的快速、简洁且高效的博客框架，拥有超快速度、一键部署、支持 Markdown、丰富的插件等优秀的特性。

以上是官方对于 Hexo 的描述，出了以上的描述，利用 Hexo 搭建个人博客还有以下特点：

1. 全是静态文件，访问速度快

2. 免费方便，不用花一分钱就可以搭建一个自由的个人博客，不需要服务器不需要后台

3. 可以随意绑定自己的域名

4. 数据绝对安全，基于github、coding等的版本管理，想恢复到哪个历史版本都行

5. 博客内容可以轻松打包、转移、发布到其它平台
   
   等等...

#### 1.1 准备工作

在使用 Hexo 搭建个人博客之前，你得要做一些准备工作，这些工作是必须的，相当于 Hexo 搭建并且运行的基石，你不能跳过

1. 一个 [GitHub](https://github.com/) 账户（[Coding](https://coding.net/) 或者 [码云](https://gitee.com/) 也可以，但由于 GitHub 是最主流的平台，所以本文以 GitHub 为例，如对其他平台有疑问，欢迎提问)

2. 安装 [Node.js](https://nodejs.org/en/)，Node.js 是多平台的，去官网选择相应的系统版本下载安装即可

3. 安装 [Git](https://git-scm.com/)，Git 也是多平台的，去官网选择相应的系统版本下载安装即可。上文提到的 GitHub、Coding、码云都是基于 Git 的代码托管平台

### 2. 设置 GitHub 博客

#### 2.1. 创建 GitHub 仓库

新建一个名为`你的用户名.github.io`的仓库。比如说，如果你的 GitHub 用户名是`L1cardo`，那么你就新建`l1cardo.github.io`的仓库（必须是你的用户名，其它名称无效），将来你的网站访问地址就是 http://l1cardo.github.io

有几个需要注意的地方

1. 注册的邮箱一定要验证，否则不会成功
2. 仓库名字必须是：`username.github.io`，其中`username`是你的用户名

#### 2.2. 绑定域名（可选）

当然，你不绑定域名肯定也是可以的，就用刚刚创建的的 `你的用户名.github.io` 来访问。如果你想更个性一点，想拥有一个属于自己的域名，那也是OK的。就像我的博客地址，默认是 http://l1cardo.github.io ，但是你也可以使用 https://licardo.cn 来访问。

首先你要注册一个域名，`腾讯云` 、`阿里云` 等国内的域名商也是可以的，而且新用户会有一定的折扣。

注册好域名，接下来就是域名解析了（域名解析就是把你的域名版绑定到 GitHub 提供的域名的意思）。如果你不会填写解析记录，那么我建议你直接按照我的来填写：

![](https://i.loli.net/2019/09/30/uF8BVDfGC6ZOm4U.png)

在你绑定了新域名之后，原来的`你的用户名.github.io`并没有失效，只是现在多了一种方式来访问你的博客。

### 3. 配置

#### 3.1 配置 SSH Key

因为你提交代码肯定要拥有你的 GitHub 权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用 SSK key 来解决本地和服务器的连接问题。

打开你的终端，敲入下面的命令

```bash
cd ~/. ssh
```

这个是检查你是否有 SSH key 的命令，如果提示 `No such file or directory` 说明你是第一次使用 Git，很好，你符合我们最开始说的本教程的阅读对象

接下来敲入下面的命令

```bash
ssh-keygen -t rsa -C "邮件地址"
```

这里的 `邮件地址` 最好是你之前注册 GitHub 的地址，这样方便以后的管理。敲入代码后连续按3次回车，最终会生成一个文件在用户目录下，打开用户目录，找到`.ssh\id_rsa.pub`文件，记事本打开并复制里面的内容。打开你的 GitHub 主页，进入个人设置 -> SSH and GPG keys -> New SSH key，并将刚刚复制的内容填入 Key 那里。Title 你可以随便起。最后别忘了保存。就像下面的图片一样：

![](https://i.loli.net/2019/09/30/NL61XFjYIDeZTur.png)

#### 3.2. 测试 SSH key是否成功

在终端里面敲入一下命令行并回车：

```bash
ssh -T git@github.com
```

如果提示`Are you sure you want to continue connecting (yes/no)?`，输入`yes` 并回车，然后会看到：

```bash
Hi L1cardo! You've successfully authenticated, but GitHub does not provide shell access.
```

当你看到这个消息，那么恭喜你，SSH key 配置成功啦！

#### 3.3. 配置 Git

在终端里面敲入一下命令行并回车：

```bash
git config --global user.name "你的 GitHub 用户名"
git config --global user.email  "你的 GitHub 注册邮箱"
```

就像我，我的 GitHub 用户名是 `L1cardo` ，我的 GitHub 注册邮箱是 `xxxx@qq.com` ，那么我就要这样填写：

```bash
git config --global user.name "L1cardo"
git config --global user.email "xxxx@qq.com"
```

### 4. 搭建 Hexo 博客

#### 4.1. 安装 Hexo

在终端里面敲入一下命令行并回车：

```bash
npm install hexo-cli -g
```

#### 4.2. 初始化 Hexo

在电脑的某个地方新建一个存放你博客文件的文件夹（名字可以随便取），比如我的是`/Users/licardo/数据/博客`，由于这个文件夹将来就作为你存放代码的地方，所以最好不要随便放。

然后在终端里面执行下面的命令：

```bash
cd /Users/licardo/数据/博客
hexo init
```

Hexo 会自动下载一些文件到这个目录，目录结构如下图：

![](https://i.loli.net/2019/09/30/z495wZ7WkKheLyI.png)

然后要让 Hexo 生成博客文件，在终端里面执行：

```bash
hexo g
```

这样，你的博客就生成了，Hexo 就会在 `public` 文件夹生成相关 `html` 文件，这些文件将来都是要提交到 GitHub 去的。

![](https://i.loli.net/2019/09/30/Q7Ek8Vw6P9YRy1j.png)

接下来我们就可以上传的 GitHub 了，但是我们还什么都没有写，也不知道博客部署完了以后会是什么样子，所以我们最好在本地启动预览服务来看看我们的博客的效果，在终端里执行以下命令：

```bash
hexo s
```

打开浏览器访问 http://localhost:4000 即可看到内容。默认的主题比较丑，打开时就是这个样子：

![](https://i.loli.net/2019/09/30/JbwqsmlhtYC8IDd.png)

#### 4.3. 修改主题

既然默认主题很丑，那我们别的不做，首先来替换一个好看点的主题。这是 [官方主题](https://hexo.io/themes/) 库，里面有很多好看的、实用的主题。

个人比较喜欢的主题是：[hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery) ，我的博客也是利用这个主题搭建的，可以看看最终的效果 https://licardo.cn

首先下载这个主题，在终端里面执行下面的命令：

- 请在稳定版与最新版中二选一

```bash
cd /Users/licardo/数据/博客/themes
git clone https://github.com/blinkfox/hexo-theme-matery.git            # 稳定版
git clone -b develop https://github.com/blinkfox/hexo-theme-matery.git # 最新版
```

下载的主题你可以在 `themes` 文件夹里面看到了：

![](https://i.loli.net/2019/09/30/xVgZr9tAfwYOpsz.png)

然后修改 Hexo 的配置文件，让 Hexo 使用我们下载的主题来生成博客。找到 `博客` 根目录下的 `_config.yml` 文件，将其中的 `theme: landscape`改为`theme: hexo-theme-matery` ，就像下面一样：

![](https://i.loli.net/2019/09/30/I2H8QsDLGqdmcbF.png)

然后在终端里面执行下面的命令：

```bash
hexo clean
hexo g
hexo s
```

然后打开浏览器访问 http://localhost:4000 即可看到更改主题之后的样子了。

#### 4.4. 上传到 GitHub

首先要配置好 Hexo 的配置文件，让 Hexo 知道要上传到哪里

修改博客文件夹根目录下的 `_config.yml` 中有关 deploy 的部分：

![](https://i.loli.net/2019/09/30/i2rnqUNVxaIkc3t.png)

将上面的 `L1cardo` 换成你自己的 GitHub 用户名就可以了。

然后安装 Hexo 上传到 GitHub 的插件，终端里面执行：

```bash
npm install hexo-deployer-git --save
```

万事具备了，距离将你的博客部署到 GitHub 上只差一步了，接下来屏住呼吸，在终端里执行下面的命令：

```bash
hexo d
```

博客已经上传到 GitHub 了，那么我们距离访问你的博客只剩下最后一步了。

#### 4.5. 设置 GitHub Pages

GitHub Pages 是 GitHub 为我们提供的静态页面服务，Hexo 就是一个静态博客框架。我们只有开始 GitHub Pages 才能使我们的博客生效并且能够被访问。

打开你的 GitHub 博客仓库进行相关设置，设置步骤如下：

![](https://i.loli.net/2019/09/30/LYOvyHE1di9W3D4.png)

![](https://i.loli.net/2019/09/30/YacMl2PynKgjtoA.png)

在这个页面开启 GitHub Pages 服务即可。

其中 ② 是我们自己之前注册过的域名，我们不仅可以通过`你的用户名.github.io` 来访问，也可以通过自定义域名来访问我们的博客。

其中 ③ 是强制开启 `HTTPS` 的意思，这样的话 GitHub 会强制时候 `HTTPS` 来启动你的博客，`HTTPS` 相比较于 `HTTP` 来说更安全，所以建议开启。

现在你可以用 `你的用户名.github.io` 或者用你自己注册的域名来访问你的博客了！

#### 4.6. Hexo 相关命令

常见命令：

```bash
hexo new "postName"      # 新建文章
hexo new page "pageName" # 新建页面
hexo generate            # 生成静态页面至public文件夹
hexo server              # 开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy              # 部署到GitHub
hexo help                # 查看帮助
hexo version             # 查看Hexo的版本
```

缩写：

```bash
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```

组合命令：

```bash
hexo s -g # 生成并本地预览
hexo d -g # 生成并上传
```

#### 4.7. _config.yml 文件

这里面都是一些 Hexo 的全局配置，每个参数的意思都比较简单明了，所以就不作详细介绍了。

需要特别注意的地方是，冒号后面必须有一个空格，否则可能会出问题。

### 5. 写博客

定位到博客根目录地址，在博客根目录下打开终端，然后执行命令：

```bash
hexo new 'my-first-blog'
```

Hexo 会帮我们在 `/source/_posts` 文件夹下生成相关 `md`  格式的文件：

![](https://i.loli.net/2019/09/30/lNDtLrvfdg2HVa5.png)

我们只需要打开这个文件就可以开始写博客了，默认生成如下内容：

![](https://i.loli.net/2019/09/30/b9kmR8sDqKcznPE.png)

注意，这里面的默认内容一定不能删除，也就是两个 `---` 里面的内容不能删除，这个是帮助 Hexo 识别文章的内容，Hexo 能根据里面的内容自动帮你把文章归类，打标签等。两个 `---` 里面的内容我们称之为 `Front-matter` ，其实他有很多属性，但是最常用的就是上面的几个了，更详细的属性你可以在 [这里](https://hexo.io/zh-cn/docs/front-matter) 进行查看

当然你也可以不用命令来创建新的博客文件，只需在 `/source/_posts` 文件夹内新建 `md` 格式的文件进行写博客即可。但是一定要注意，必须要包含 `---` 内的信息！也就是说必须要有 `Front-matter` ！

每次写完博客都要使用 Hexo 的相关命令 来生成相关的博客页面，并且部署到 GitHub 上面。

写博客的工具？

因为 Hexo 的博客都是用 `md` 格式文件来写的，`md` 格式是 Markdown 文件的格式，所以只要使用规范的 Markdown 语法进行书写即可。

当然也是有好的 Markdown 编辑器的，在这里我推荐 [Mark Text](https://github.com/marktext/marktext) ，一个跨平台的 Markdown 编辑器，开源免费，而且实时渲染的特性使得即使不会 Markdown 语法也能进行优美的 Markdown 写作。我的所有的 Markdown 文件都是用它来书写的。

### 6. 最终效果

可以访问我的博客来查看效果：https://licardo.cn

当然你也可以使用其他主题，动手能力强的话，你也可以自己修改一些主题的内容，去创建属于你自己的博客！

### 7. 参考

http://blog.haoji.me/build-blog-website-by-hexo-github.html

<div style="display: flex;align-items: center;justify-content: space-evenly;padding-top: 40px;">
  <img src="https://raw.githubusercontent.com/L1cardo/l1cardo.github.io/blog/themes/butterfly/source/img/notbyai_cn.png" alt="真人撰写" style="height: 42px;">
  <img src="https://raw.githubusercontent.com/L1cardo/l1cardo.github.io/blog/themes/butterfly/source/img/notbyai_en.png" alt="written by human" style="height: 42px;">
</div>
