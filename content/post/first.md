---
title: "Window下安装HUGO"
date: 2019-10-25T18:15:25+08:00
draft: false
---

### 第一篇文章献给HUGO

#### 【介绍】

hugo是用GO语言写的一个静态网站生成器，静态页面的生成速度很快；

通过 Hugo 你可以快速搭建你的静态网站，比如博客系统、文档介绍、公司主页、产品介绍等等。相对于其他静态网站生成器来说，
Hugo 具备如下特点：

- 极快的页面编译生成速度。（ ~1 ms 每页面）
- 完全跨平台支持，可以运行在  Mac OS X,  Linux,  Windows, 以及更多!
- 安装方便 Installation
- 本地调试 Usage 时通过 LiveReload 自动即时刷新页面。
- 完全的皮肤支持。
- 可以部署在任何的支持 HTTP 的服务器上。

#### 【安装】

hugo的安装方法有很多，具体在官方或者百度、谷歌都能找到安装方法；

1. 第一种方法，也是我用的方法，这个的前提是机子有安装GO语言，建议对GO有了解的

   - 直接用git将hugo仓库clone下来，然后解压放到$GOPATH下，到其目录下执行go install；
   - 也可以到$GOPATH目录下直接用以下命令安装
		```
		go get -v github.com/spf13/hugo
		```
   - 可以使用 -u 参数执行 go get 用来更新 Hugo 的所有依赖。
		```
		go get -u -v github.com/spf13/hugo
		```

2. 第二种方法，安装hugo二进制文件， 这个安装最简单，直接到[这个网站](https://github.com/gohugoio/hugo/releases)下载你对应平台的二进制文件，然后安装就行了；

#### 【安装主题】

在HUGO中安装主题很简单，可以到[主题列表](https://www.gohugo.org/theme/)浏览主题，然后将主题仓库clone到themes文件夹中
``` Bash
git clone https://github.com/olOwOlo/hugo-theme-even.git themes/even
```
然后在config.toml 文件中修改默认主题
```
theme = "even"
```
有些主题可能需要一些特殊的配置，需要在配置文件里修改，具体如何修改，可以在主题介绍那边查看；

#### 【命令执行报错】

``` Bash
hugo server --theme=even --buildDrafts
```

报错：
``` Bash
Building sites … ERROR 2019/10/26 10:17:18 render of "taxonomyTerm" failed: 
"D:\code\hugoblog\blog\themes\simple\layouts\_default\terms.html:19:8": execute 
of template failed: template: _default\terms.html:6:5: executing "_default\\terms.html" 
at <partial "head.html" .>: error calling partial: 
"D:\code\hugoblog\blog\themes\simple\layouts\partials\head.html:19:8": 
execute of template failed: template: partials/head.html:19:8: executing "partials/head.html" 
at <$.Scratch.Add>: error calling Add: can't apply the operator to the values
```
这是因为在config.toml 参数配置中没有配置\[params\] 参数，它下面的值将会构成模板里的 .Site.Params 变量：

一般安装完主题之后，主题文件夹里会有一个<exampleSite>文件夹，里面有该主题的config.toml配置文件和一些主题默认的文章，所以安装完主题之后，只需要把里面的文件
复制替换根目录下的文件即可，再根据自己的需要修改对应的配置内容。

#### 【新增文章】

使用命令
``` Bash
hugo new post/newpost.md
```
来创建一篇新文章，其中 post 是指文件夹，也就是先创建文件夹post再创建newpost.md文件；创建好的文件是在content目录里面；
新增的文章，默认有一下内容：
```
---
title: "About"
date: 2017-07-28T20:54:37+08:00
draft: true
---
```
这些内容默认是在archetypes/default.md 文件里维护，可以修改；
title 是新增文章时的文件名，作为文章的标题；***draft=true***表示这是一篇草稿的意思，HUGO默认不会渲染草稿状态的博客，使用命令
``` Bash
hugo undraft post/first.md
```
命令会将draft状态改成false，或者直接在文件中修改也行。

#### 【启动HUGO】

在博客的根目录下，运行
``` Bash
hugo server --buildDrafts --theme=even
```
如果第一次安装主题没在配置文件中指定theme那么可以加上--theme参数，--buildDrafts 参数是渲染所有的文章，相当于热部署的意思，就是启动完之后修改了文章保存之后
就可以立即渲染，不需要重新停止再启动；

#### 【关于部署】

将HUGO部署到Github Pages：
首先要在github上面创建一个仓库，仓库命名为ballboom.github.io 其中ballboom改成你的用户名，
在站点根目录执行HUGO命令,生成最终页面
``` Bash
hugo --theme=even --buildDrafts --baseUrl="https://ballboom.github.io"
```
一切顺利的话，所有的静态页面都会生成到public文件夹里，将public目录全部推送到github master分支
``` Bash
cd public
git init
git remote add origin git@github.com:BallBoom/BallBoom.github.io.git
git add -A
git commit -m "first commit"
git pull -u origin master
```
不出意外的话 在浏览器输入BallBoom.github.io 地址就能访问到你的博客了

后续新增文章话
``` Bash
--生成静态页面
hugo --buildDrafts --baseUrl="https://BallBoom.github.io"
---发布
cd public 
git add .
git commit -m "new blog added"
git push origin master
```
