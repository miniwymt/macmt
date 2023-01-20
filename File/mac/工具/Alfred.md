---
title : Alfred
time : 2023-01-12 周四
tags : 
- homebrew
- 
- 
---

## 安装

```sh
brew install --cask alfred
```

## 热键

#### 常用热键

| 热键      | 功能                        |
| --------- | --------------------------- |
| C-C       | 打开 Alfred                 |
| S-C-j     | 打开 Iterm 并跳转到文件目录 |
| ctrl-ctrl | 打开剪切板历史              |
|           |                             |

#### 关键字

| 关键字 | 功能                                                                  |
| ------ | --------------------------------------------------------------------- |
| >code  | 打开终端并运行 code                                                   |
| d 空格 | 调用 mac 内置字典                                                     |
| 空格   | 查找文件(设置 openfile 为空),查找到后 shift 预览,→ 方向键查看可用操作 |
| f 空格 | 查找文件路径(设置 revealingfiles 为 f)                                |
| bm     | 查找书签                                                              |

## Features

#### 配置网页搜索

```md
以 github 为例,设置热键为 gh
https://github.com/search?q={query}
```

#### 配置默认搜索可选项

- 在 Default Results 中设置 fallbacks

![600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202301122038593.png)

#### 配置 Iterm2

- 热键为 _**>**_

```text
on alfred_script(q)
	if application "iTerm2" is running or application "iTerm" is running then
		run script "
			on run {q}
				tell application \"iTerm\"
					activate
					try
						select first window
						set onlywindow to true
					on error
						create window with default profile
						select first window
						set onlywindow to true
					end try
					tell the first window
						create tab with default profile
						tell current session to write text q
					end tell
				end tell
			end run
		" with parameters {q}
	else
		run script "
			on run {q}
				tell application \"iTerm\"
					activate
					try
						select first window
					on error
						create window with default profile
						select first window
					end try
					tell the first window
						tell current session to write text q
					end tell
				end tell
			end run
		" with parameters {q}
	end if
end alfred_script

```
