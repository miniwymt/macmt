---
title : Clashx
time : 2023-01-11 周三
tags : 
- 科学
- jms
- 
---

## [jms](https://justmysocks3.net/members/sunshine.php?serviceId=472917&page=home)

1. 服务器在外网

2. 购买服务后找到4个关键配置信息

![jms|600](https://mtpc-1313122433.cos.ap-nanjing.myqcloud.com/obsidian/mac/202301112143752.png)

## 下载ClashX

1. 下载地址

	- homebrew
	
	- [github](https://github.com/yichengchen/clashX/releases)
	
	- [v2ray](https://v2xtls.org/v2ray-mac客户端下载/)


## 配置文件

1. 下载[[clashx_jms.yaml]]模版文件

2. 根据 `jms` 的4个关键信息配置模版文件的如下部分

```yaml

# Shadowsocks

- name: "jms-s1"

type: ss

server: c81s1.jjvip8.com

port: 19198

cipher: aes-256-gcm

password: "zBYfHGewsH8zEcmf"

  

# Shadowsocks

- name: "jms-s2"

type: ss

server: c81s2.jjvip8.com

port: 19198

cipher: aes-256-gcm

password: "zBYfHGewsH8zEcmf"

  

# VMess

- name: "jms-s3"

type: vmess

server: c81s3.jjvip8.com

port: 19198

uuid: f6d65c6b-0a10-4887-8541-b86d85c31d09

alterId: 0

cipher: aes-128-gcm

  

# VMess

- name: "jms-s4"

type: vmess

server: c81s4.jjvip8.com

port: 19198

uuid: f6d65c6b-0a10-4887-8541-b86d85c31d09

alterId: 0

cipher: aes-128-gcm

  

# VMess

- name: "jms-s5"

type: vmess

server: c81s5.jjvip8.com

port: 19198

uuid: f6d65c6b-0a10-4887-8541-b86d85c31d09

alterId: 0

cipher: aes-128-gcm

  

# VMess

- name: "jms-s801"

type: vmess

server: c81s801.jjvip8.com

port: 19198

uuid: f6d65c6b-0a10-4887-8541-b86d85c31d09

alterId: 0

cipher: aes-128-gcm

```