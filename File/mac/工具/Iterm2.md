---
title : iterm2
time : 2023-01-12 周四
tags : 
- homebrew
- nerd-fonts
- oh-my-zsh
---

## iterm2 安装

_**[hombrew](https://brew.sh)**_

## iterm2 主题

#### 下载

_**[color_schemes](https://github.com/mbadolato/iTerm2-Color-Schemes)**_ 中的 schemes 目录

#### 配置

1. Appearance

   - General 下的 Theme 设置为紧凑:Compact

2. `Profiles`

   - Colors 右下角 Color Presets 导入下载好的 schemes

   - Text

     - Cursor 设置为 Vertical bar 并选中 Blinking cursor

     - 选择 Hack Nerd Font Mono

       - 需要下载 _**[nerd-fonts](https://github.com/ryanoasis/nerd-fonts#option-4-homebrew-fonts)**_ 字体库,hombrew 下载即可

   - window

     - _**[背景图](https://uigradients.com/#Sylvia)**_

     - 配置如下

       ![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202301121315097.png)

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202301122321252.png)



## _**[oh-my-zsh](https://ohmyz.sh/#install)**_

#### oh-my-zsh 主题

```sh

#查看系统安装的所有Shell
cat /etc/shells

#修改默认Shell为zsh,需要输入电脑密码
chsh -s /bin/zsh

#查看当前使用的Shell
echo $Shell

# 查看oh-my-zsh主题
ls ~/.oh-my-zsh/themes

# 修改oh-my-zsh的配置文件.zshrc中的ZSH_THEME="xxx"
# 主题网站:https://github.com/ohmyzsh/ohmyzsh/wiki/Themes
vim ~/.zshrc

```

#### oh-my-zsh 插件

```sh
# hombrew 安装

# 1. 声明高亮插件zsh-syntax-highlighting,根据安装后的提示,将类似以下的命令配置添加到.zshrc中
source /usr/local/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# 2. 自动填充建议插件zsh-autosuggestions,根据安装后的提示,将类似以下的命令配置添加到.zshrc中
source /usr/local/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# 3. 快速跳转插件autojump,根据安装后的提示,将类似以下的命令配置添加到.zshrc中
[ -f /usr/local/etc/profile.d/autojump.sh ] && . /usr/local/etc/profile.d/autojump.sh

# 无论是主题还是插件配置,记住只要变更过配置文件.zshrc,都应重载一下配置文件
source ~/.zshrc
```

## 快捷键

1. 关闭iterm

	- cmd + w

2. 打开item并跳转到当前目录

	- shift + cmd + j

	- 该快捷键需要通过访达-服务进行配置

