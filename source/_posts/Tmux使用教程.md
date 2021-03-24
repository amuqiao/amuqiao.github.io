---
title: Tmux使用教程
top: false
cover: false
toc: true
mathjax: true
date: 2021-03-22 16:46:57
img:
password:
summary:
tags:
- Tmux
categories:
- 工具

---
> Tmux 是一个终端复用器（terminal multiplexer），非常有用，属于常用的开发工具。

<!--more-->

### tmux的基本操作
`Prefix-Command`前置操作：所有下面介绍的快捷键，都必须以前置操作开始。tmux默认的前置操作是`CTRL+b`。例如，我们想要新建一个窗体，就需要先在键盘上摁下`CTRL+b`，松开后再摁下`n`键。

下面所有的`prefix`均代表`CTRL+b`

### 常用

```
d  退出 tmux（tmux 仍在后台运行）
t  窗口中央显示一个数字时钟
?  列出所有快捷键
:  命令提示符
```

### Session相关操作

| 操作                     | 快捷键                           |
| ------------------------ | -------------------------------- |
| 新建一个无名称的会话     | tmux                             |
| 新建一个名称为demo的会话 | tmux new -s demo                 |
| 默认进入第一个会话       | tmux a                           |
| 进入到名称为demo的会话   | tmux a -t demo                   |
| session 重命名           | tmux rename -t old_name new_name |
| 查看所有 session         | tmux ls                          |
| 查看/切换session         | prefix s                         |
| 离开Session              | prefix d                         |
| 重命名当前Session        | prefix $                         |

### Window相关操作

| 操作                   | 快捷键             |
| ---------------------- | ------------------ |
| 新建窗口               | prefix c           |
| 新建命名窗口           | prefix -s new_name |
| 切换到上一个活动的窗口 | prefix space       |
| 关闭当前窗口           | prefix &           |
| 使用窗口号切换         | prefix 窗口号      |
| 列出所有窗口           | prefix w           |
| 前一个窗口             | p                  |
| 后一个窗口             | n                  |
| 重命名当前窗口         | ,                  |
| 查找窗口               | f                  |

#### 调整窗口排序

| 操作                 | 快捷键                |
| -------------------- | --------------------- |
| 交换 3 号和 1 号窗口 | swap-window -s 3 -t 1 |
| 交换当前和 1 号窗口  | swap-window -t 1      |
| 移动当前窗口到 1 号  | move-window -t 1      |

### Pane相关操作

| 操作                  | 快捷键   |
| --------------------- | -------- |
| 切换到下一个窗格      | prefix o |
| 查看所有窗格的编号    | prefix q |
| 垂直拆分出一个新窗格  | prefix “ |
| 水平拆分出一个新窗格  | prefix % |
| 切换窗格最大化/最小化 | z        |
| 与上一个窗格交换位置  | {        |
| 与下一个窗格交换位置  | }        |
| 关闭窗格              | x        |
| 垂直分割              | %        |
| 水平分割              | "        |
#### 调整窗格尺寸
如果你不喜欢默认布局，可以重调窗格的尺寸。虽然这很容易实现，但一般不需要这么干。这几个命令用来调整窗格：

### 其他命令
```
# 列出所有快捷键，及其对应的 Tmux 命令
$ tmux list-keys

# 列出所有 Tmux 命令及其参数
$ tmux list-commands

# 列出当前所有 Tmux 会话的信息
$ tmux info

# 重新加载当前的 Tmux 配置
$ tmux source-file ~/.tmux.conf

# 关闭服务器，所有的会话都将关闭, 重启tmux生效配置
tmux kill-server 

# 在session中，先按C+b，然后输入：，进入命令行模式，在命令行模式下输入
source-file ~/.tmux.conf

# 关闭demo会话
tmux kill-session -t demo 



```

```
PREFIX : resize-pane -D          当前窗格向下扩大 1 格
PREFIX : resize-pane -U          当前窗格向上扩大 1 格
PREFIX : resize-pane -L          当前窗格向左扩大 1 格
PREFIX : resize-pane -R          当前窗格向右扩大 1 格
PREFIX : resize-pane -D 20       当前窗格向下扩大 20 格
PREFIX : resize-pane -t 2 -L 20  编号为 2 的窗格向左扩大 20 格
```

### 安装
```
$ brew install tmux
```

### 修改tmux配置

#### [鼠标滚动查看上下文](https://github.com/NHDaly/tmux-better-mouse-mode)
```
vi ~/.tmux.conf
# 开启鼠标模式
set -g mouse on

```

#### 修改`Prefix-Command`
```
# 修改前置快捷键
set -g prefix C-z
unbind C-b

```
#### 窗口自定义命名
```
# 如果喜欢给窗口自定义命名，那么需要关闭窗口的自动命名
set-option -g allow-rename off
```
#### 鼠标选中复制
```
# 鼠标选中复制
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
bind -n C-WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
bind -T copy-mode-vi    C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-vi    C-WheelDownPane send-keys -X halfpage-down
bind -T copy-mode-emacs C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-emacs C-WheelDownPane send-keys -X halfpage-down

# To copy, left click and drag to highlight text in yellow,
# once you release left click yellow text will disappear and will automatically be available in clibboard
# # Use vim keybindings in copy mode
setw -g mode-keys vi
# Update default binding of `Enter` to also use copy-pipe
unbind -T copy-mode-vi Enter
bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "pbcopy"
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "pbcopy"
```
#### 其他配置（待整理）
```
# 鼠标支持 - 设置为 on 来启用鼠标(与 2.1 之前的版本有区别，请自行查阅 man page)
* set -g mouse on

# 设置默认终端模式为 256color
set -g default-terminal "screen-256color"

# 启用活动警告
setw -g monitor-activity on
set -g visual-activity on

# 居中窗口列表
set -g status-justify centre

# 最大化/恢复窗格
unbind Up bind Up new-window -d -n tmp \; swap-pane -s tmp.1 \; select-window -t tmp
unbind Down
bind Down last-window \; swap-pane -s tmp.1 \; kill-window -t tmp


# C-b 和 VIM 冲突，修改 Prefix 组合键为 Control-Z，按键距离近
set -g prefix C-z

set -g base-index         1     # 窗口编号从 1 开始计数
set -g display-panes-time 10000 # PREFIX-Q 显示编号的驻留时长，单位 ms
set -g mouse              on    # 开启鼠标
set -g pane-base-index    1     # 窗格编号从 1 开始计数
set -g renumber-windows   on    # 关掉某个窗口后，编号重排

setw -g allow-rename      off   # 禁止活动进程修改窗口名
setw -g automatic-rename  off   # 禁止自动命名新窗口
setw -g mode-keys         vi    # 进入复制模式的时候使用 vi 键位（默认是 EMACS）
```

### [`TMUX`插件管理器](https://github.com/tmux-plugins/tpm)
#### 安装
```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
添加以下信息到配置文件中`~/.tmux.conf`($XDG_CONFIG_HOME/tmux/tmux.conf works too):
```
# 插件列表
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'

# 其他示例:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com:user/plugin'
# set -g @plugin 'git@bitbucket.com:user/plugin'

# 初始化tmux插件管理器 (将这行添加到配置文件最底部)
run '~/.tmux/plugins/tpm/tpm'
```

重启`TMUX`环境,使`TPM`生效(添加其他插件也需要执行该命令):
```
# 如果tmux正在运行，在终端执行该命令
$ tmux source ~/.tmux.conf
```
#### 安装插件

- 添加插件到配置文件`~/.tmux.conf`中，例如:set -g @plugin '...'
- 按 prefix + I (如Install中的大写字母I) 安装插件.

稍等片刻，插件会被下载到`~/.tmux/plugins/`文件夹中

#### 卸载插件
- 在插件列表中移除该插件.
- 输入`prefix + alt + u` (小写u进行卸载) 来移除插件.

#### 快捷键
`prefix + I`

- 从`github`或者其他仓库中安装在插件列表中的插件
- 刷新`TMUX`环境

`prefix + U`
- 更新插件
prefix + alt + u
- 移除/下载不在插件列表中的插件


#### 更新配置信息
若对`~/.tmux.conf`做出修改，执行`tmux source-file ~/.tmux.conf`命令使配置生效

#### [TMUX插件中心](https://github.com/tmux-plugins/list)
tmux支持下列插件

- extrakto - Allows you to select text from your window by fuzzy matching it through a set of filters with fzf. Look ma, no mouse!
- tmux-battery - Plug and play battery percentage and icon indicator.
- tmux-colours-superhero - A superhero themed tmux colour theme.
- tmux-continuum - Continuous saving of tmux environment. Automatic restore when tmux is started. Automatic tmux start when computer is turned on.
- tmux-copycat - Enhances tmux search.
- tmux-copytk - Multi utility rapid copy toolkit.
- tmux-cpu - Plug and play cpu percentage and icon indicator.
- tmux-fpp - Quickly open any path on your terminal window in your $EDITOR of choice!
- tmux-jump - Vimium/Easymotion like navigation for tmux.
- tmux-keyboard-layout - Show current keyboard layout in your status bar
- tmux-logging - Easy logging and screen capturing.
- tmux-maildir-counter - Plugin that counts files on a specific mail directory.
- tmux-mem-cpu-load - CPU, RAM, and load monitor for use with tmux.
- tmux-net-speed - Tmux plugin to monitor upload and download speed of one or all interfaces.
- tmux-online-status - Tmux plugin that displays online status of your computer.
- tmux-open - Tmux key bindings for quick opening of a highlighted file or url.
- tmux-pain-control - Standard pane key-bindings for tmux.
- tmux-peacock - Per session color and style based on session name
- tmux-piavpn - Keep track of your Private Internet Access VPN status.
- tmux-pomodoro - Use Pomodoro technique with timer showing in tmux status bar.
- [tmux-prefix-highlight](https://github.com/tmux-plugins/tmux-prefix-highlight) 当你按下tmux前缀键时突出显示的插件.
- [tmux-resurrect](https://github.com/tmux-plugins/tmux-resurrect) - 在系统重启时持续tmux环境.
- tmux-sensible - Basic tmux settings everyone can agree on.
- tmux-sessionist - Lightweight utils for manipulating sessions.
- tmux-sidebar - A sidebar with the directory tree for the current path. Tries to make tmux more IDE like.
- tmux-ticker - Keep a track of popular market indexes and stock price.
- tmux-update-display - When attaching to tmux session, update $DISPLAY for each tmux pane in that session.
- tmux-urlview - Quickly open any url on your terminal window!
- tmux-weather - Display weather information in your terminal.
- tmux-yank - 用于复制到系统剪贴板。适用于MacOS, Linux和Cygwin.
- tpm - Tmux plugin manager.

### 窗口持久化插件[`tmux-resurrect`](https://github.com/tmux-plugins/tmux-resurrect)
Tmux 虽好，但是一旦重启，之前的窗口环境都会消失得无影无踪，幸好有了这个插件
有了它，你可以通过简单的两个快捷键随时备份或恢复窗口环境。

使用 [Tmux插件管理器](https://github.com/tmux-plugins/tpm)(推荐)，安装插件后，在配置文件中插件列表的位置加入这一行
```
set -g @plugin 'tmux-plugins/tmux-resurrect'
```
点击`prefix + I`来获取插件使之生效。现在你应该可以使用这个插件了

现在你可以随时使用 prefix + Ctrl-s 保存，使用 prefix + Ctrl-r 恢复。

值得注意的是，恢复后，原窗口中的进程会被杀掉。如果你是重启后恢复，那自然没什么问题，但是平时试验的时候可要留心有没有重要的任务正在运行中。

[Resurrect save dir](https://github.com/tmux-plugins/tmux-resurrect/blob/e4825055c92e54b0c6ec572afc9b6c4723aba6c8/docs/save_dir.md)

By default Tmux environment is saved to a file in ~/.tmux/resurrect dir. Change this with:
```
set -g @resurrect-dir '/some/path'
```
Using environment variables or shell interpolation in this option is not allowed as the string is used literally. So the following won't do what is expected:

```
set -g @resurrect-dir '/path/$MY_VAR/$(some_executable)'
```
Only the following variables and special chars are allowed: $HOME, $HOSTNAME, and ~.


### 我的配置文件`~/tmux.conf`
```
# -----------------------------------------------------------------------------
# Tmux 基本配置 - 要求 Tmux >= 2.3
# 如果不想使用插件，只需要将此节的内容写入 ~/.tmux.conf 即可
# -----------------------------------------------------------------------------

# 开启鼠标模式
set -g mouse on

# 修改前置快捷键
set -g prefix C-z
unbind C-b

# 鼠标选中复制
bind -n WheelUpPane if-shell -F -t = "#{mouse_any_flag}" "send-keys -M" "if -Ft= '#{pane_in_mode}' 'send-keys -M' 'select-pane -t=; copy-mode -e; send-keys -M'"
bind -n WheelDownPane select-pane -t= \; send-keys -M
bind -n C-WheelUpPane select-pane -t= \; copy-mode -e \; send-keys -M
bind -T copy-mode-vi    C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-vi    C-WheelDownPane send-keys -X halfpage-down
bind -T copy-mode-emacs C-WheelUpPane   send-keys -X halfpage-up
bind -T copy-mode-emacs C-WheelDownPane send-keys -X halfpage-down

# To copy, left click and drag to highlight text in yellow,
# once you release left click yellow text will disappear and will automatically be available in clibboard
# # Use vim keybindings in copy mode
setw -g mode-keys vi
# Update default binding of `Enter` to also use copy-pipe
unbind -T copy-mode-vi Enter
bind-key -T copy-mode-vi Enter send-keys -X copy-pipe-and-cancel "pbcopy"
bind-key -T copy-mode-vi MouseDragEnd1Pane send-keys -X copy-pipe-and-cancel "pbcopy"

# -----------------------------------------------------------------------------
# 使用插件 - via tpm
# -----------------------------------------------------------------------------

# 插件列表
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-resurrect'
set -g @plugin 'tmux-plugins/tmux-prefix-highlight'

# 其他示例:
# set -g @plugin 'github_username/plugin_name'
# set -g @plugin 'git@github.com:user/plugin'
# set -g @plugin 'git@bitbucket.com:user/plugin'

# tmux-prefix-highlight 插件配置
set -g status-right '#{prefix_highlight} #H | %a %Y-%m-%d %H:%M'
set -g @prefix_highlight_show_copy_mode 'on'
set -g @prefix_highlight_copy_mode_attr 'fg=white,bg=blue'

# 初始化tmux插件管理器 (将这行添加到配置文件最底部)
run '~/.tmux/plugins/tpm/tpm'

# -----------------------------------------------------------------------------
# 结束
# -----------------------------------------------------------------------------
```

### 参考文档
- [Tmux教程](http://cenalulu.github.io/linux/tmux/)
- [Tmux使用手册](http://louiszhai.github.io/2017/09/30/tmux/)
- [十分钟学会 tmux](https://www.cnblogs.com/kaiye/p/6275207.html)
- [Tmux 快捷键 & 速查表](https://gist.github.com/ryerh/14b7c24dfd623ef8edc7)
- [tmux使用及个性化配置](https://blog.csdn.net/qushaming/article/details/90712886)
- [Tmux 快捷键 & 速查表 & 简明教程](https://gist.github.com/ryerh/14b7c24dfd623ef8edc7)
- [tmux 插件管理器和插件](http://blog.codepiano.com/2018/06/11/tmux-tpm)