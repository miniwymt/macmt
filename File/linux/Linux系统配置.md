---
title : app
time : 2023-01-16 周一
tags : 
- apt-get
- 
- 
---
## 新建用户并赋权

#### 新建用户

```sh
# 新建mt用户,提示设置密码2次:wy571
adduser mt
```

#### 赋权

```sh
# su root 确保在 root 权限用户下操作
vim /etc/sudoers

# User privilege specification 参考 root 权限配置 mt 权限
root    ALL=(ALL:ALL) ALL
mt      ALL=(ALL:ALL) ALL
```

#### 删除用户

```sh
userdel -r mt
```

#### 设置mac免密远程访问服务器

1. 创建mac本地密钥

	```sh
	cd ~/.ssh
	# 生成名为ssh_guiyun的本地密钥对(私钥与公钥,其中公钥带.pub后缀)
	ssh-keygen -t rsa -f ssh_guiyun
	```

2. 将其中的公钥复制到云服务器上

	```sh
	# 注意这里复制的是公钥
	# 以 root 用户为例,ip 指服务器的公网 ip
	# 公钥会被存储在云服务器 ~/.ssh/authorized_keys 文件中
	ssh-copy-id -i ssh_guiyun.pub root@ip
	```
3. 将mac本地私钥添加到密钥管理器并将密码存储在密钥链中

	```sh
	# 每次访问去mac的密钥链进行验证
	ssh-add -K ssh_guiyun
	```
4. 配置mac本地config文件

	```sh
	vi ~/.ssh/config
	
	Host *.github.com  # git免密配置
	  AddKeysToAgent yes
	  IdentityFile ~/.ssh/id_ed25519
	
	Host root  # root用户
	HostName 103.133.177.123  # 远程主机ip
	User root  # 你在远程主机的用户名
	Port 22
	IdentityFile ~/.ssh/ssh_guiyun  # 本地root私钥
	
	Host mt  # mt用户
	HostName 103.133.177.123  # 远程主机ip
	User mt  # 你在远程主机的用户名
	Port 22
	IdentityFile ~/.ssh/mt_guiyun  # 本地mt私钥
	```
5. 食用

	```
	# 直接连接config中配置的Host即可
	ssh root
	```

#### 配置zsh搭配oh-my-zsh

1. 查看系统中是否已有zsh

	```sh
	# 检查系统shell列表中是否已有/bin/zsh
	cat /etc/shells
	```
2. 根据Linux系统发行版本的不同安装zsh

	```sh
	# ubuntu下(centos请使用yum)
	sudo apt-get update
	sudo apt-get install zsh
	```
3. 查看安装好的zsh路径并设置为默认shell

	```sh
	cat /etc/shells
	
	# 注意是把当前用户的默认shell设置为zsh,路径选带usr的 
	chsh -s /usr/bin/zsh
	
	# 设置好后退出系统重连启用zsh
	exit
	```
4. 配置[oh-my-zsh](https://ohmyz.sh/#install)

	```sh
	# 3设置zsh后没有.zshrc配置文件,这里使用oh-my-zsh配置
	# 之前配的py环境也没了,去.bashrc中拷到.zshrc中来
	sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
	```

#### 配置miniforge环境

1. [miniforge](https://gitcode.net/mirrors/conda-forge/miniforge?utm_source=csdn_github_accelerator)

```sh
# 1.1 下载安装
wget "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh"

# 1.2 添加到环境变量
bash Mambaforge-$(uname)-$(uname -m).sh

# 1.3 重新加载配置
source .zshrc
```

#### homebrew安装

1. 在云服务器上安装贼慢

```sh
# linux不支持在root用户下安装brew
su mt

# 更新apt-get,非root用户需要sudo提权
sudo apt-get update

# 安装依赖
sudo apt-get install build-essential curl file git

# homebrew安装,注意后续会提示有几个next step,参照运行即可
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2. 使用homebrew安装oh-my-zsh插件

	```sh
	# 以下几步都需要按提示将插件配置到.zshrc中,请仔细查看命令提示
	brew install zsh-syntax-highlighting
	# source /home/linuxbrew/.linuxbrew/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
	
	brew install zsh-autosuggestions
	# source /home/linuxbrew/.linuxbrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh
	
	brew install autojump
	# [ -f /home/linuxbrew/.linuxbrew/etc/profile.d/autojump.sh ] && . /home/linuxbrew/.linuxbrew/etc/profile.d/autojump.sh
	
	
	# 安装并配置到.zshrc后重新加载.zshrc
	source .zshrc
	```

2. 使用homebrew安装ranger

```sh
# 安装
brew install ranger

# 开启默认配置文件
ranger --copy-config=all
```



