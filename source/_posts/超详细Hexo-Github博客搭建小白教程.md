---
title: 超详细Hexo+Github博客搭建小白教程
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-19 11:02:24
password:
summary:
tags: 
- hexo
- 个人博客搭建
categories: 
- 博客搭建
---

### 前言


### 安装Node.js

首先下载稳定版Node.js[[下载地址](https://nodejs.org/zh-cn/download/)]，选择相应的系统版本。

mac用户下载macos安装包，点击下一步安装即可。
```
This package has installed:
	•	Node.js v14.16.0 to /usr/local/bin/node
	•	npm v6.14.11 to /usr/local/bin/npm
Make sure that /usr/local/bin is in your $PATH.
```

安装好之后，打开命令终端，输入node -v和npm -v，如果出现版本号，那么就安装成功了。

#### 添加国内镜像源
> 使用阿里的国内镜像进行加速
```
npm config set registry https://registry.npm.taobao.org
```


### 安装Git

为了把本地的网页文件上传到github上面去，我们需要用到分布式版本控制工具—Git[[下载地址](https://git-scm.com/downloads)]。

安装完成后在命令提示符中输入git --version验证是否安装成功。

### 注册Github账号

> 打开[github](https://github.com/)，新建一个项目,输入自己的项目名字，后面一定要加`.github.io`后缀README初始化也要勾上。名称一定要和你的github名字完全一样，比如你`github`名字叫`abc`，那么仓库名字一定要是`abc.github.io`。

例如我的github名称为`amuqiao`
![FvRygZ](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/FvRygZ.png)


### 安装Hexo
安装
- [[官方安装文档](https://hexo.io/zh-cn/docs/index.html)]
- 在合适的地方新建一个文件夹，用来存放自己的博客文件，比如我的博客文件都存放在`/Users/wangqiao/NutstoreFiles/blog-new`目录下。

按照官方文档说明完成安装、建站操作。
```
$ npm install -g hexo-cli
$ hexo init <folder>
$ cd <folder>
$ npm install
```

输入`hexo s`打开本地服务器，然后浏览器打开[http://localhost:4000/](http://localhost:4000/)，就可以看到我们的博客啦，
```
➜  blog-new hexo server
INFO  Validating config
INFO  Start processing
INFO  Hexo is running at http://localhost:4000 . Press Ctrl+C to stop.
```
![bIoB82](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/bIoB82.png)


### 连接Github与本地

#### 添加github ssh秘钥
```
ssh-keygen  # 直接enter即可，不用选择yes or no
cat ~/.ssh/id_rsa.pub
```
在github中配置ssh公钥
![BBYeWY](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/BBYeWY.png)


打开博客根目录下的_config.yml文件，这是博客的配置文件，在这里你可以修改与博客相关的各种信息。

修改最后一行的配置：


```
deploy:
  type: git
  repository: https://github.com/amuqiao/amuqiao.github.io.git
  branch: master
```

repository修改为你自己的github项目地址。


### 写文章、发布文章
你可以执行下列命令来创建一篇新文章或者新的页面。
```
# 您可以在命令中指定文章的布局（layout），默认为 post，可以通过修改 _config.yml 中的 default_layout 参数来指定默认布局。
$ hexo new [layout] <title>  
```

然后打开`blog-new/source/_posts/`的目录，可以发现下面多了一个文件夹和一个.md文件，一个用来存放你的图片等数据，另一个就是你的文章文件。

编写完markdown文件后，
```
hexo g # 生成静态网页
hexo s # 可以本地预览效果
```
这时打开你的[http://localhost:4000](http://localhost:4000)就能看到发布的文章啦。

###  一键部署
安装 [hexo-deployer-git](https://github.com/hexojs/hexo-deployer-git).
```
$ npm install hexo-deployer-git --save
```

在 _config.yml（如果有已存在的请删除）添加如下配置：

```
deploy:
  type: git
  repo: git@github.com:amuqiao/amuqiao.github.io.git
  branch: main
```
运行 `hexo clean` && `hexo deploy` 。

查看 `username.github.io` 上的网页是否部署成功。

### 博客备份

有时候我们想换一台电脑继续写博客，这时候就可以将博客目录下的所有源文件都上传到github上面。

首先在github博客仓库下新建一个分支hexo，然后git clone到本地，把.git文件夹拿出来，放在博客根目录下。

```
git clone git@github.com:amuqiao/amuqiao.github.io.git
cd amuqiao.github.io
# mac shift+command+. 显示隐藏文件夹
mv .git/ ../blog-new  # 将.git目录移动到博客根目录

git checkout hexo # 切换到hexo分支
git add . # 
git commit -m "xxx" 
git push origin hexo # 推送到远端分支
```
在`github`上可以看到，`main`分支只有静态网页文件，`hexo`分支包含所有源文件


### 更换主题
博客主题: [hexo-theme-matery](https://github.com/blinkfox/hexo-theme-matery/blob/develop/README_CN.md)，它的文档写得也非常的详细，还有中英文两个版本，作者回复也很及时。


### 文章模板设置
首先为了新建文章方便，建议将/scaffolds/post.md修改为如下代码：

```
--- 
title: {{ title }} 
date: {{ date }} 
top: false 
cover: false 
password: 
toc: true mathjax: true 
summary: 
tags: 
categories: 
---
```
新建文章后不用你自己补充了，修改信息就行。

### 添加百度统计和谷歌统计代码
首先打开[百度站长平台](https://ziyuan.baidu.com/site/index)，然后点击添加网站，输入网址并选择领域。

接下来要验证网站所有权，有三种方式，推荐采用HTML标签验证，最简单，将代码复制出来。

打开themes/matery/layout/_partial/head.ejs，修改下面两行：

```
<meta name="baidu-site-verification" content="xxx" />
<meta name="google-site-verification" content="xxx" />
```
其中content内容改成你自己刚刚复制出来的就行了。


### 参考
- [超详细Hexo+Github博客搭建小白教程](https://godweiyang.com/2018/04/13/hexo-blog/)