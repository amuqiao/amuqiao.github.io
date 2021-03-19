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