---
title: ChatGPT注册攻略
top: false
cover: false
toc: true
mathjax: true
date: 2023-02-14 10:59:19
img:
password:
summary: 分享ChatGPT注册攻略
tags:
- ChatGPT
categories:
- AI

---

### 准备
* 首先能能访问 Google（前置条件，不能明确说，懂得都懂）
* 你得有一个国外手机号，GV 号肯定不行。
    * 如果你没有国外手机号，推荐 sms-activate.org

### 注册短信平台并充值
- 先行注册 sms-activate.org
- 注册好之后进行对应的充值(支持支付宝)
![n4x6tR](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/n4x6tR.png)


### 注册OpenAI账号

- 打开beta.openai.com/signup 页面进行相应的注册。
    - 这里同样需要你能访问Google且 ip 不是香港，最好是美国、新加坡等等，不然会提示不能在当前国家服务。
- 注册成功进入下面填写手机号的页面
    - 下面记得切换下国家区号，这里的区号默认是你代理的。
    

![QT6qj0](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/QT6qj0.jpg)


### 准备接码
> 这里需要注意下的就是，部分地区可能收到码，我选择的是巴西

![Opk2cg](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Opk2cg.png)


![Uehg4b](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Uehg4b.png)


![JGISFI](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/JGISFI.png)


然后再刚刚填写手机号码的页面填入申请的手机号

### 开始使用ChatGPT
注册完后，我们去ChatGPT网站去登陆。chat.openai.com/auth/login
![YeHrD8](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/YeHrD8.jpg)
![Fe12re](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/Fe12re.jpg)


### 可能碰到的问题
#### 接不到码
- 接不到码，可以在那个有效期内退回换个号试试。
- 有人直接把发的电话输入框里，没去掉区号也可以收到
- 这个接码网站很全面，我演示充值是 1 美元，你也可以冲一个 0.18美元
- 目前完全支持的是印度+巴西。你也可以选择其他国家的 any+other 选择合适的费用即可。

#### 无法访问

![E6N9kY](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/E6N9kY.png)
若出现ChatGPT官网拒绝访问的情况
一般你出现这种问题，就是因为你的代理没有全局，或者位置不对。香港或中国的的代理是100%无法通过的。

只要你出现了这个提示，那么你接下来怎么切换代理，都是没用的。需要清空浏览器缓存。
快速轻松浏览器缓存，先复制下面这段代码
```
window.localStorage.removeItem(Object.keys(window.localStorage).find(i=>i.startsWith('@@auth0spajs')))
```
按f12进入开发者模式，在控制台进行粘贴
![8uxplO](https://aamuqiao.oss-cn-beijing.aliyuncs.com/uPic/8uxplO.png)





### 参考资料
- [https://juejin.cn/post/7173447848292253704](https://juejin.cn/post/7173447848292253704)
- [https://www.bilibili.com/read/cv20340415](https://www.bilibili.com/read/cv20340415)
- [https://uzbox.com/ai/sms-activate-chatgpt.html](https://uzbox.com/ai/sms-activate-chatgpt.html)
