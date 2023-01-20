---
title : Git基础整理
category : 学习
date : 20221213_周二
update : 
tags : 
- git
- linux
- mac
---

## 前期准备

#### mac 环境安装配置

	```sh
	// 安装git
	$ brew install git	
	// 查看安装版本
	$ git version
	git version 2.37.1 (Apple Git-137.1)	
	// 配置git用户与邮箱
	$ git config --global user.name mt
	$ git config --global user.email mt.@gmail.com	
	// 考虑配置分页器,默认为less,这里我设置为空,不管多少内容在终端直接输出
	$ git config --global core.pager ''	
	// 查看git配置信息,返回详情页使用q退出
	$ git config --global --list
	```

#### 配置ssh免密登陆

- [官方教程](https://docs.github.com/zh/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent)

- 配置该方法后,克隆以及添加远程仓库时,请选择ssh地址

	```sh
	// 确认密钥生成地址,输入密钥密码(一路回车,不要设置密码)
	$ ssh-keygen -t ed25519 -C "wangyi571001@outlook.com"
	
	// 2.在后台开启ssh密钥管理器
	$ eval "$(ssh-agent -s)"
	
	// 3.配置ssh的config
	$ vi ~/.ssh/config
	
	// 3.1 把下面内容拷贝到config中
	Host *.github.com
	  AddKeysToAgent yes
	  #前面未设置密码时该行不填 UseKeychain yes
	  IdentityFile ~/.ssh/id_ed25519
	
	// 4.将密钥添加到密钥管理器,同时将密码存储在密钥链中
	$ ssh-add --apple-use-keychain ~/.ssh/id_ed25519
	
	// 5. 读取拷贝公钥内容,并在github仓库setting中配置你的ssh key(注意拷贝的是.pub公钥中的内容而非本地私钥)
	$ cat ~/.ssh/id_ed25519.pub
	```
- 多账号免密推送,需要修改远程地址中的@github.com

	```sh
	# 有时可能需要删除.ssh下的known*文件,里面存放着之前的连接记录
	
	# 推送到WangyiG需要把@github.com修改为@WangyiG
	# 在克隆时就修改,lazygit推送时会自动使用修改后的
	Host WangyiG
	    HostName github.com
	    User wangyi571001
	    PreferredAuthentications publickey
	    IdentityFile ~/.ssh/WangyiG_github

	# 推送到miniwymt需要把@github.com修改为@miniwymt
	Host miniwymt
	    HostName github.com
	    User wangyimt
	    PreferredAuthentications publickey
	    IdentityFile ~/.ssh/miniwymt_github

	# gitee
	Host gitee.com
	    HostName gitee.com
	    User buzhongyao
	    PreferredAuthentications publickey
	    IdentityFile ~/.ssh/miniwymt_github

	# 只克隆不提交用
	Host github.com
	    HostName github.com
	    User wangyimt
	    PreferredAuthentications publickey
	    IdentityFile ~/.ssh/miniwymt_github
	```

#### 工作区创建

1. 新建 demo 目录并初始化

	```sh
	$ cd desktop && mkdir demo && cd demo && git init
	```

2. 写入文件并查看内容

- 并使用 _`echo`_ 命令向 a.txt 文件写入 1st 如无 a.txt 文件则新增
- 注意此处 _`>`_ 表覆盖写,另有 _`>>`_ 表追加写

	```sh
	$ echo 1st > a.txt
	
	$ cat a.txt
	1st
	```

## 概念须知

1. 文件流转的 4 个区域

	- `工作区`(本地文件目录)
	- `缓存区`
	- `本地版本库`
	- `远程仓库`

2. 文件存在的几个不同状态

	- `untracked`,未追踪状态,文件从未被 add 进`缓存区`过
	- 其余都属于`tracked`状态,即已被 Git 仓库追踪,细分为以下几个状态
		- `staged`,已缓存状态
		- `unstaged`,缓存过后取消缓存状态,进过缓存再取消与 untracked 状态不同
		- `commit`,已提交状态
		- `modified`,已修改状态,该状态可与 staged 状态或 commited 状态同时存在

3. _**HEAD**_

	- 进阶,了解基本命令之后再回头研究
	- 理解为一个指向当前分支当前版本的一个游离指针

	```sh
	// 返回所指向的当前分支
	$ cat .git/HEAD
	ref: refs/heads/master
	
	// 返回当前版本hash值,与reflog对比验证
	$ cat .git/refs/heads/master
	111e19a6766de94de7e715f12ac1c72177580264
	```

## 基础命令

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202212180124494.png)

1. _**init**_

	- 初始化 git 仓库
	- 新增.git 子目录
	- 此时文件属于`untracked`未追踪状态

2. _**add**_

	- 将`工作区`文件 _**add**_ 到`缓存区`
	- 此时文件属于`tracked`追踪状态之中的 staged 已缓存状态
	- _-A_ 参数包括所有的更改操作--新建,更改,删除
	- _._ 参数只包括 新建 ,修改操作;无删除
	- _-u_ 参数只包括修改,删除操作;无新建
	- 计算`校验和`与创建文件快照

1. _**commit**_

	- 将`缓存区`文件提交到`本地版本库`
	- 此时文件属于`commited`状态
	- 组织快照树&&创建提交对象
	- _--amend_ 选项使用新的提交取代上一个提交
	- _-m_ 参数声明提交注释,如果不使用会进入默认 vim 编辑器编辑注释
		- i 进入 vim 编辑模式
		- ESC 退出 vim 默认的编辑模式
		- :wq 写入并退出 vim 命令模式

2. _**log**_ 与 _**reflog**_

	- `log` 查询`commit` 提交记录
	- `reflog` 查询`commit` 操作记录
	- 区别在于`reflog` 支持查询被删除或重置的`commit` 记录,而`log` 不支持

3. _**status**_

	- 查询文件在`工作区`与`缓存区`的状态
	- _-s_ 参数用于对状态进行简短化输出
		- _??_ 表示文件处于 `untracked` 状态
		- _A_ 表示文件处于 `staged` 已缓存状态
		- _M_ 表示文件处于 `modified` 状态
		- _D_ 表示`工作区`文件已被删除

4. _**diff**_

	- 查询`缓存区`与`工作区`的文件具体区别
	- _--cached_ 选项查询`本地版本库`与`缓存区`文件的具体区别
	- _HEAD_ 指针对比查询当前版本`commited` 状态文件与`工作区`文件的具体区别
	- _--stat_ 选项支持以摘要形式显示具体区别

5. _**reset**_

	- _filename_ 取消`缓存区`文件 staged 状态到`unstaged`状态
	- _HEAD~n_ (当前版本的前 n 次版本)或 _commitID_ (具体提交 ID 版本) 参数将`本地版本库`与`缓存区`重置为参数对应`commited`状态文件
	- _--soft_ 选项支持仅重置`本地版本库`而不重置`缓存区`
	- _--hard_ 选项支持将`工作区`也重置为对应`commited`状态文件

6. _**rm**_

	- _filename_ 将文件从`缓存区`和`工作区`删除
	- _--cached_ 选项支持保留`工作区`文件,_此时保留的工作区文件为`untracked`状态_

7. _**restore**_

	- _filename_ 从`缓存区`恢复文件到`工作区`
	- _-S_ 或 _--staged_ 从`本地版本库`恢复文件到`缓存区`
	- _-W_ 或 _--worktree_ 从`本地版本库`恢复文件到`工作区`
	- `git rm` 删除的文件属 `untracked`状态无法通过 _**restore**_ 恢复但可通过 _**reset**_ 恢复

8. _**checkout**_

	- _filename_ 将`工作区`文件复原为`缓存区` `staged`状态文件
	- _-f_ 参数将`缓存区`与`工作区`文件均复原成`commited`状态文件
	 - _branchname_ 切换到指定分支
	- _-b_ 参数并切换分支,同 _**switch**_ _-c_ ,build to create

> _**checkout**_ 承受太多,既复原文件又切换分支,遂在新版`git`中引入了 _**restore**_ 与 _**switch**_

11. _**blame**_

	- _file_ 追踪指定文件的历史改动,通常返回
	    - 校验和
	    - (提交用户 提交时间)
	    - 提交内容
	- _-L m,n_ 指定查看第 m 行至第 n 行的历史改动
	- _-e_ 返回内容中提交用户名指定为提交用户邮箱信息
	- _branch file_ 追踪指定分支下指定文件的历史改动

#### 忽略文件.gitignore

1. 对于无需纳入版本管理的文件,避免每次在`untracked`列表中出现,可以创建 _**.gitignore**_ 文件来声明需要被忽略的文件

2. _**.gitignore**_ 文件格式规范

	- 空行以及#开头的注释行会被忽略
	- 匹配模式可以以`/x`开头防止递归(只忽略当前目录下的 x 文件, 而不忽略子目录下的 x 文件)
	- 匹配模式可以以`x/`结尾来指定目录(忽略任何目录下的 x 目录及其内容)
	- 标准的 glob 模式匹配(shell 所使用的简化版正则表达式),递归的应用在整个工作区 ^3eafb8
	    - _\*_ 号匹配 0 或 n 个任意字符
	    - 匹配任何一个列在方括号中的字符,如 _[abc]_ 匹配 a 或匹配 b 或匹配 c
	    - _?_ 号只匹配一个任意字符
	    - 匹配字符范围内的字符,如 _[0-9]_ 匹配 0 到 9 闭区间中的任意一个数
	    - _\*\*_ 双星号表示匹配任意中间目录,如 a/\*\*/z 可以匹配 a/z,a/b/z,a/b/c/d/z
	    - 使用 _!_ 取反,通常用于取消某类忽略中的部分,如先忽略所有的\*.txt 文件,但其中的!a.txt 不忽略

## 分支管理

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202212172212359.png)

#### Git 的数据保存

1. Git 保存的不是文件的变化或差异,而是一系列不同时刻的快照

2. 执行 _**add**_ 暂存操作时

	- 计算校验和
	- 将该文件快照保存到 git 仓库中(Git 中使用 blob 对象来保存快照)
	- 将校验和加入到暂存区域等待提交

3. 执行 _**commit**_ 提交操作时

	- 计算每个子目录的校验和
	- 组织树对象(记录目录结构和 blob 对象索引)
	- 创建提交对象(节点),该对象包含
		- _指向校验和树对象的指针_
		- _指向父对象(父节点)的指针_
		- _提交信息(提交注释,作者用户名邮箱等)_

#### 分支的新建删除与切换对比

1. _**branch**_

	- 查看分支
	    - 需要新分支中存在 commit 对象
	    - _\*_ 标识当前分支
	    - _--merged_ 选项过滤已合并的分支
	    - _--no-merged_ 选项过滤未合并的分支
	- _branchname_ 创建分支,注意:创建新分支后,并未切换至新分支
	- _-m oldname newname_ 修改分支名
	- _-d_ 删除分支,分支中暂存区文件需要提交完成才可 _-d_ 删除
	- _-D_ 强制删除

2. _**switch**_

	- _branchname_ 切换分支
	- _-c branchname_ create 并切换分支

3. _**diff**_

	- _otherbranch_ 对比当前分支暂存区与其它分支最近一次提交的区别
	- _HEAD otherbranch_ 对比当前分支最近一次提交与其它分支最近一次提交的区别

4. _**stash**_

	- 急需切换分支进行另一工作,但分支中有暂存且未提及的文件,无法切换分支
	- 使用 _**stash**_ 暂存`staged`文件并清空`modified` (使用 cat 查看工作区文件,会发现变更消失)
	- _apply_ 恢复 _**stash**_ 到`staged`状态
	- _list_ 查看 _**stash**_ 列表
	- _show -p stash@{n}_ 查看第 n 个 _**stash**_ 的具体内容

#### 分支的合并

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202212180011325.png)

1. _**merge**_

	- _branch_ 合并另一个分支
	
	- 无冲突的合并
		- 通常是将分支的共同祖先与不同分支的最近提交做一次三方合并
	- 有冲突的合并
	
	    - `conflicts` 发生于不同分支在相同文件相同位置有不同修改
	    - _**mergetool**_ 或 手工调整 或 第三方工具修复冲突
	    - _**status**_ 确认冲突解决且文件已暂存
	    - _**commit**_ _-a_ 做合并提交,使用 _-a_ 防止文件未暂存,未使用 _-am_ 以便在提交信息中编辑冲突解决说明

2. 手工调整冲突文件

	- 等号的上半部分标识当前分支 a.txt 文件的冲突内容为 1st
	- 等号下半部分标识 dev 分支的 a.txt 的冲突内容为 2nd
	- 去除等号与上述标识内容,保留 1st 或 2nd 或其它希望保留的内容

	```md
	<<<<<<< HEAD:a.txt
	`1st`
	=======
	`2nd` 
	>>>>>>> dev:a.txt
	```

2. _**rebase**_

	- _branch_ 将当前分支变基到目标分支最新提交之后
	
	- 无冲突变基
	    - 直接将当前分支变基到目标分支最新提交之后
	
	- 有冲突变基
	    - `vim file` 手工解决冲突
	    - _**add**_ 暂存文件
	    - _--continue_ 告诉`Git`冲突已解决,可以继续无冲突变基了
	    - _**rebase**_ 会要求修改当前分支最近一次提交的提交信息而 _**merge**_ 会在三方合并后新增 1 个合并提交

3. 可视化变基示图

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/pc/202212191452855.png)

## 远程仓库

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202212190410841.png)

1. _**clone**_

	- _url_ 克隆远程仓库
	    - 默认使用远程地址中最后一个\/后的名字做为目录名创建一个目录存放远程仓库内容
	    - _url dir_ 自定义目录名
	    - 默认会为远程仓库地址创建一个别名为 origin 的简名
	    - _url dir -o remote_ 自定义远程仓库别名
	- 远程仓库不一定是远程,也可以是本机上另一仓库

2. _**remote**_

	- 查看远程仓库
	- _show remote_ 查看某一远程仓库的详情
	- _add remote url_ 添加远程仓库并设置别名
	    - 添加远程仓库后,可使用 _**branch** -r_ 查看远程分支

3. _**push**_

	- _remote_ 默认推送到 master 分支
	- 推送的是 commit 对象,所以请务必牢记将本地文件变更进行暂存和提交操作
	- _remote branch_ 推送到指定分支
	- _-f_ 强制推送
	- _-u remote branch_ 关联当前分支到远程分支,后续直接使用 _**push**_

4. _**pull**_

	- _remote_ 拉取远程仓库到本地
	- _remote r_branch:l_branch_ 指定远程分支拉取到本地分支
	- _-f_ 强制拉取

5. _**fetch**_

	- 抓取克隆(或上一次抓取)后远程仓库中新推送的所有工作暂存至本地版本库
	- 该命令不会自动合并远程分支到本地,需要手工合并
	    - ***clone***  会自动设置本地 master 分支跟踪克隆的远程仓库的  `master`  分支
	    - _**diff origin/branch**_ 对比远程仓库最近一次提交与本地最近一次提交
	    - 如果设置了跟踪远程分支, _**pull**_ 会尝试合并远程分支到本地分支
	    - 如果在 pull 之前修改了本地文件需要先 _**stash**_ 再 _**pull**_ 然后 _**stash** pop_



