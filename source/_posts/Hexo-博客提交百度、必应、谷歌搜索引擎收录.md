---
title: Hexo 博客提交百度、必应、谷歌搜索引擎收录
top: false
cover: false
toc: true
mathjax: true
date: 2021-05-19 17:39:47
img:
password:
summary:
tags:
- hexo
- seo
categories:
- 前端

---

> 让搜索引擎收录你的博客

<!--more-->

SEO优化
> SEO（Search Engine Optimization）：汉译为搜索引擎优化。是一种方式：利用搜索引擎的规则提高网站在有关搜索引擎内的自然排名。 ——百度百科


### 百度收录站点
>GitHub是不允许百度的Spider（蜘蛛）爬取GitHub上的内容的，所以任何部署在GitHub上的静态博客都是不能百度爬取到的！

首先来搞百度的，刚建完站在百度上是不可能搜索到我们的网站的，可以先在百度上搜索site:<你的域名>，一般是搜不到的
![搜索站点](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/fKHiND.png)

1. 注册、登陆百度搜索资源平台（ https://ziyuan.baidu.com/ ）
2. 用户中心，站点管理，添加网站
3. 输入网站->站点属性->验证网站，选择HTML标签验证，复制HTML标签，在主题目录下`hexo-theme-matery/layout/_partial/head.ejs`中添加HTML标签(matery主题默认添加了该HTML标签)，完成验证
3. 搜索服务->普通收录->API提交，记下下方接口调用地址的【百度站长平台中注册的域名，密匙】：http://data.zz.baidu.com/urls?site=https://amuqiao.github.io&token=密钥，这里即【https://amuqiao.github.io&token=密钥】

### 使用插件向搜索引擎推送网站
> 使用[hexo-submit-urls-to-search-engine](https://github.com/cjh0613/hexo-submit-urls-to-search-engine)插件向搜索引擎推送网站【[说明文档](https://cjh0613.com/20200603HexoSubmitUrlsToSearchEngine.html)】

1. 获取各站长平台密钥
1. 安装并配置hexo-submit-urls-to-search-engine插件
1. hexo clean && hexo g && hexo d，并查询推送结果
1. 如果推送成功，前往 [Github 地址](https://github.com/cjh0613/hexo-submit-urls-to-search-engine)点击 Star 按钮以支持

安装插件

请在 hexo 根目录运行
```
npm install --save hexo-submit-urls-to-search-engine
```

编辑hexo的_config.yml
```
hexo_submit_urls_to_search_engine:
  submit_condition: count #链接被提交的条件，可选值：count | period 现仅支持count
  count: 10 # 提交最新的10个链接
  period: 900 # 提交修改时间在 900 秒内的链接
  google: 0 # 是否向Google提交，可选值：1 | 0（0：否；1：是）
  bing: 1 # 是否向bing提交，可选值：1 | 0（0：否；1：是）
  baidu: 1 # 是否向baidu提交，可选值：1 | 0（0：否；1：是）
  txt_path: submit_urls.txt ## 文本文档名， 需要推送的链接会保存在此文本文档里
  baidu_host: https://cjh0613.github.io ## 在百度站长平台中注册的域名
  baidu_token: 请按照文档说明获取 ## 请注意这是您的秘钥， 所以请不要把它直接发布在公众仓库里!
  bing_host: https://cjh0613.github.io ## 在bing站长平台中注册的域名
  bing_token: 请按照文档说明获取 ## 请注意这是您的秘钥， 所以请不要把它直接发布在公众仓库里!
  google_host: https://cjh0613.github.io ## 在google站长平台中注册的域名
  google_key_file: Project.json #存放google key的json文件，放于网站根目录（与hexo _config.yml文件位置相同），请不要把json文件内容直接发布在公众仓库里!
  google_proxy: http://127.0.0.1:8080 # 向谷歌提交网址所使用的系统 http 代理，填 0 不使用
  replace: 0  # 是否替换链接中的部分字符串，可选值：1 | 0（0：否；1：是）
  find_what: http://cjh0613.github.io/blog
  replace_with: https://cjh0613.com

# deplpy 增加配置
deploy:
- type: git
  repo: 
    coding: git@xxx
  branch: master 
  
#添加本插件的配置项 注意有-：
- type: cjh_google_url_submitter
- type: cjh_bing_url_submitter
- type: cjh_baidu_url_submitter
```

执行`hexo g`后在根目录生成`public\submit_urls.txt`文件，需要推送的链接会保存在此文本文档里

执行`hexo d`同时向搜索引擎推送网站

### 推送成功返回信息
必应
```
Bing response:  { d: null }
```
百度

```
Baidu response:  {"remain":2999,"success":1}
```
谷歌
```
Google response:  { urlNotificationMetadata:
   { url:
      'https://cjh0613.github.io',
     latestUpdate:
      { url:
         'https://cjh0613.github.io',
        type: 'URL_UPDATED',
        notifyTime: '2020-06-12T05:37:25.701779228Z' } } }
```

推送成功后并没有立即收录



### 参考文档

- [hexo-submit-urls-to-search-engine 搜索引擎收录](https://github.com/cjh0613/hexo-submit-urls-to-search-engine)
- [hexo-baidu-url-submit 百度收录](https://github.com/huiwang/hexo-baidu-url-submit)