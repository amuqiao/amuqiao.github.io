---
title: matery主题定制
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-19 15:47:43
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