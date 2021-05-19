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

### SEO优化
> SEO（Search Engine Optimization）：汉译为搜索引擎优化。是一种方式：利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。 ——百度百科


#### 百度收录
GitHub是不允许百度的Spider（蜘蛛）爬取GitHub上的内容的，所以任何部署在GitHub上的静态博客都是不能百度爬取到的！

让百度收录你的站点
首先来搞百度的，刚建完站在百度上是不可能搜索到我们的网站的，可以先在百度上搜索site:<你的域名>，一般是搜不到的，然后点击 提交网址 将自己的网站提交给百度。
![zsOfGJ](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/zsOfGJ.png)

添加自己的站点到百度
点击`提交网址`
![dIPnoa](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/dIPnoa.png)


参考文档

- [Hexo进阶之各种优化](https://blog.sky03.cn/posts/42790.html#toc-heading-17)