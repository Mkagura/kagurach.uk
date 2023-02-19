---
title: "Cloudflared隧道简单入门"
date: 2023-02-19T21:26:12+08:00
draft: false 
description: "cloudflare tunnel在外部访问内网服务"
tags:  ["服务器秒寄术"]  
isCJKLanguage: true
featured_image: ""
---

# 提示！
 - 将本地服务暴露在公网上有***巨大***风险，请小心对待
 - 此服务在一些国家/地区可用性差

# 0. 是什么
先看[官方说明](https://www.cloudflare.com/zh-cn/products/tunnel/)，Cloudflare Tunnel将你的原始服务器安全的连接到Cloudflare,然后别人在访问你的时候经过了CF的防火墙，这帮你减少了被DDoS/CC的风险  

# 1. 有什么用
 - 让你的机器可以在公网被访问
 - 加快你源服务器的访问速度
 - ...

# 2. 开始配置
## 2.1 下载
[GitHub Releases](https://github.com/cloudflare/cloudflared/releases)或者[CF Docs](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-setup/installation#linux)，支持Windows,MacOS,Linux
## 2.2 在cloudflare上添加你的网站
请自行搜索相关教程  
> 另，cloudflared提供了试用模式，请看[#3.1](./#31-%e8%af%95%e7%94%a8)
## 2.3 配置cloudflared
通过  
```cloudflared tunnel login```  
登陆你的cloudflare账号，并授权网站。你会打开一个网页/在终端看到一个链接。
之后会在（默认位置） ```~/.cloudflared/``` 下生成证书
> 注意，你只能创建在你授权的网站的子域名上
## 2.4 建立隧道
```cloudflared tunnel create <NAME>```建立隧道，此时会
 - 生成一个此隧道的UUID（要用）
 - 生成隧道的证书
 - 生成 ```<随机>.cfargotunnel.com```的域名
> 你可以通过 ```cloudflared tunnel list``` 查看你授权的域名下所有的隧道
## 2.5 写配置文件
默认的配置文件是```～/.cloudflared/config.yml```，内容为  
```
url: http://localhost:<port>
tunnel: <Tunnel-UUID>
credentials-file: /root/.cloudflared/<Tunnel-UUID>.json
```
 - &lt;port&gt; 你要经过代理的端口
 - &lt;Tunnel-UUID&gt; 2.4中建立隧道时给出的UUID
 - credentials-file 需要一个绝对路径，这里是以你在root下进行操作的
 - 在Windows下请使用 '\\\\' 避免麻烦
## 2.6 解析
```cloudflared tunnel route dns <UUID or NAME> <hostname>```
 - &lt;UUID or NAME&gt;是隧道的名字或UUID之一
 - &lt;hostname&gt;你希望的子域名，会自动CNAME解析到2.4生成的随机域名下
# 3. 开始运行
使用默认配置```cloudflared tunnel run <UUID or NAME>```  
使用指定配置```cloudflared tunnel --config /path/your-config-file.yaml run```
 - &lt;UUID or NAME&gt;是隧道的名字或UUID之一
## 3.1 试用
```cloudflared tunnel --url http://localhost:<port>```
 - &lt;port&gt; 你要经过代理的端口
之后下面会给出你的试用链接
