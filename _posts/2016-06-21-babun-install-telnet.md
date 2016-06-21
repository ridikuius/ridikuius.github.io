---
layout: post
title: "babun install telnet"
date: 2016-06-21
excerpt: "babun install telnet."
tags: [babun, install, telnet]
comments: true
---

   **最近开始用一个windows下模拟linux的集成环境babun，一个 Windows 上的开箱即用的壳程序。 但是安装telnet一直失败。**

```bash

{ ~ }  » pact telnet install
~ 1 Working directory is /setup Mirror is http://mirrors.kernel.org/sourceware/cygwin/
--2016-04-25 12:00:42--  http://mirrors.kernel.org/sourceware/cygwin//x86/setup.bz2 
Resolving mirrors.kernel.org (mirrors.kernel.org)... 198.145.20.143, 149.20.37.36, 2001:4f8:4:6f:0:1994:3:14, ... Connecting to mirrors.kernel.org (mirrors.kernel.org)
|198.145.20.143|:80... connected. HTTP request sent, awaiting response... 200 OK
Length: 2170765 (2.1M) [application/octet-stream] Saving to: ‘setup.bz2’
setup.bz2                100%[====================================>]   2.07M   190KB/s   in 71s
2016-04-25 12:01:55 (29.8 KB/s) - ‘setup.bz2’ saved [2170765/2170765] Updated setup.ini
Installing telnet Package telnet not found or ambiguous name, exiting

```

**后用 pact install inetutils**

```bash

 { ~ }  » pact install inetutils  

```

**成功安装telnet**

### 另附babun常用软件安装命令

```bash

pact install tmux           安装tmux
pact install screen         安装screen 有了这两个不用conEmu也可以了
pact install zip            安装zip
pact install subversion     安装svn相关的命令
pact install lftp           lftp命令
pact install p7zip          p7zip命令
pact install connect-proxy  基于openssh的socks https代理
pact install util-linux     安装linux基础命令行工具 more/col/whereis等等命令
pact install bind-utils     安装dig命令
pact install inetutils      安装Telnet等常用网络命令
pact install python         python环境
pact install python-crypto  python 环境

```

