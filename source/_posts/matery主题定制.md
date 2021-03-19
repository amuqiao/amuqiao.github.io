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
- matery主题
categories:
- Hexo

---

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


#### 修改文章特色图片
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