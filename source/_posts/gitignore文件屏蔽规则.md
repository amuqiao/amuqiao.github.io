---
title: gitignore文件屏蔽规则
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-31 19:27:48
img:
password:
summary: 屏蔽不想提交的的文件
tags:
- Git
- gitignore
categories:
- 开发工具

---


在使用git仓库时，有些特殊文件我们希望能够进行屏蔽，而有些文件我们希望能够保留在版本库中，此时就用到了git的.gitignore 文件。

在项目根路径创建`.gitignore`文件

### gitignore 文件规范
`.gitignore `文件格式规范如下：

- 所有空行或#开头的行都会被忽略；
- 可以使用标准的 glob 模式匹配；
- 文件或目录前加 / 表示仓库根目录的对应文件；
- 匹配模式最后跟反斜杠 / 说明要忽略的是目录；
- 要特殊不忽略某个文件或目录，可以在模式前加上取反 ! 。

其中 glob 模式是指 shell 所使用的简化了的正则表达式。

- 星号 * 匹配零个或多个任意字符；
- [abc]匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；- - 问号 ? 只匹配一个任意字符；
- 如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。

下面是一个 `.gitignore` 文件例子，注释上附录有说明：
```
*.a                 # 所有以 '.a' 为后缀的文件都屏蔽掉
# Office 缓存文件
~'$'*.docx
~'$'*.ppt
~'$'*.pptx 
~'$'*.xls

tags                # 仓库中所有名为 tags 的文件都屏蔽
core.*              # 仓库中所有以 'core.' 开头的文件都屏蔽

tools/              # 屏蔽目录 tools
log/*               # 屏蔽目录 log 下的所有文件，但不屏蔽 log 目录本身

/log.log            # 只屏蔽仓库根目录下的 log.log 文件，其他目录中的不屏蔽
readme.md           # 屏蔽仓库中所有名为 readme.md 的文件
!/readme.md         # 在上一条屏蔽规则的条件下，不屏蔽仓库根目录下的 readme.md 文件
```

例子中的最后两条的顺序很重要，必须要先屏蔽所有的，然后才建立特殊不屏蔽的规则！


### 在`github`上创建代码仓库时忘记添加`.gitignore`文件或修改了`.gitignore`该怎么办？

```
#清除本地缓存(改变成未track状态)
#git rm -r --cached . 表示清除项目中所有文件的本地缓存
#xxx表示不想版本控制的文件，比如小编可以输入test.o
#.gitignore中的忽略规则应该与之相对应
git rm -r --cached xxx

git add .   #添加除了忽略文件外的所有文件
git commit -m "此处可以描述你提交的信息"
git push -f #强制推送
```