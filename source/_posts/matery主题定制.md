---
title: matery主题定制
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-19
password:
summary:
tags:
- hexo
- hexo-theme-matery
categories:
- 前端

---
> 基于hexo-theme-matery做的个性化美化

<!-- more-->

### 限定预览文字字数
```
# 在预览文字后添加限定符号 <!-- more-->
Welcome to Hexo <!-- more--> This is your very first post. 
```
![限定预览文字](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/87j2QT.jpg)


### 去掉首页banner的背景颜色
打开`themes/matery/source/css/matery.css`文件(367行，也可以快速搜索`.bg-cover:after`进行定位):
然后注释掉这两行`css`

```
.bg-cover:after {
    /* -webkit-animation: rainbow 60s infinite;
    animation: rainbow 60s infinite; */
}
```

### 修改社交连接

首页第二个按钮跳转到自己的github

修改主题配置文件，修改链接

```
首页 banner 中的第二行个人信息配置，留空即不启用
indexbtn:
  enable: false
  name: Github
  icon: fab fa-github-alt
  url: https://github.com/blinkfox/hexo-theme-matery
  
# 首页 banner 中的第二行个人信息配置，留空即不启用
socialLink:
  github:  https://github.com/blinkfox
  email: 1181062873@qq.com
  facebook: # https://www.facebook.com/xxx
  twitter: # https://twitter.com/xxx
  qq: 1181062873
  weibo: # https://weibo.com/xxx
  zhihu: # https://www.zhihu.com/xxx
  rss: true # true、false
```
### 本地资源文件夹
> 资源（Asset）代表 source 文件夹中除了文章以外的所有文件，例如图片、CSS、JS 文件等。比方说，如果你的Hexo项目中只有少量图片，那最简单的方法就是将它们放在 source/images 文件夹中。然后通过类似于 ![](/images/image.jpg) 的方法访问它们。

添加[资源文件夹](https://hexo.io/zh-cn/docs/asset-folders.html)


### 指定文章特色图片
![森林的猫](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/%E6%A3%AE%E6%9E%97%E7%9A%84%E7%8C%AB.jpg)

修改文章 Front-matter选项
- 修改`img`为图片的`url`链接

```
img: https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/%E6%A3%AE%E6%9E%97%E7%9A%84%E7%8C%AB.jpg
```
- 修改为本地资源路径

```
img: images/森林的猫.jpg
```


### 添加文章特色图片
主题下配置文件
```
featureImages:
- /medias/featureimages/0.jpg
- /medias/featureimages/1.jpg
- /medias/featureimages/2.jpg
- /medias/featureimages/3.jpg
...
```
在`featureimages`新增图片后添加到配置中

### 添加视频链接

若不设置height和width则采用默认大小

```
<iframe src="https://vdn1.vzuu.com/SD/e3cc53d8-a9cb-11e8-9bd8-0242ac112a24.mp4?disable_local_cache=1&auth_key=1622534176-0-0-63692becd56e6a743c78d08f951eccd8&f=mp4&bu=http-com&expiration=1622534176&v=hw" </iframe>
```
<iframe src="https://vdn1.vzuu.com/SD/e3cc53d8-a9cb-11e8-9bd8-0242ac112a24.mp4?disable_local_cache=1&auth_key=1622534176-0-0-63692becd56e6a743c78d08f951eccd8&f=mp4&bu=http-com&expiration=1622534176&v=hw" </iframe>

html中的iframe标签

```
<iframe height=520 width=820 src="//player.bilibili.com/player.html?aid=502349665&bvid=BV1KK411w7rv&cid=315155499&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>
```

<iframe height=520 width=820 src="//player.bilibili.com/player.html?aid=502349665&bvid=BV1KK411w7rv&cid=315155499&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>