---
title: '[在线书籍管理平台] - 0 - 环境配置与说明'
tags:
  - online-book-management
categories:
  - project
  - go
date: 2021-01-02 02:48:33
---

## 摘要

前端项目地址: [https://github.com/Jecosine/obm-front-end.git](https://github.com/Jecosine/obm-front-end.git)

本文讲述了开发过程中用到的软件版本与开发工具, 已经具备相关环境的同学可以确认前后端软件版本之后, 直接阅读 [下一章]()

## 前端

| Software | Version  |
| -------- | -------- |
| NVM      | v1.1.7   |
| Node     | v12.16.0 |
| NPM      | v6.13.4  |

## 后台

Go  v1.15.6

Gin [框架地址](https://github.com/gin-gonic/gin)

## 开发环境

- Window 10 
- Visual Studio Code
- Goland (也许用到了)
- Google Chrome Version 87.0.4280.88 (Official Build) (64-bit)



## 前端所需环境配置

### NVM

nvm 允许你在同一台机器上安装与管理多个版本的node. 也许有的同学听过一个叫n的管理工具, 如果要在两者之间进行比较的话:

> node 版本管理工具还有一个是 TJ大神的 [n](https://github.com/tj/n) 命令，n 命令是作为一个 node 的模块而存在，而 nvm 是一个独立于 node/npm 的外部 shell 脚本，因此 n 命令相比 nvm 更加局限。
>
> 由于 npm 安装的模块路径均为 **/usr/local/lib/node_modules**，当使用 n 切换不同的 node 版本时，实际上会共用全局的 node/npm 目录。 因此不能很好的满足『按不同 node 版本使用不同全局 node 模块』的需求。
>
> *引用自 [菜鸟教程](https://www.runoob.com/w3cnote/nvm-manager-node-versions.html)*



#### Window 下安装

到 [此处](https://github.com/coreybutler/nvm-windows/releases) 下载安装包安装即可. 

**※ 请最好先把已有的node版本卸载掉**



#### 切换国内源

**Windows**

新建 **系统变量** `NVM_NODEJS_ORG_MIRROR`,  设置值为

```http
https://npm.taobao.org/mirrors/node
```

**Linux**

```shell
export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
```

若需要设置成对 **当前用户** 永久生效, 请写进`~/.bashrc`中 (如果你用的是bash的话)

此处也可以顺便把npm与node的源一起换了

```powershell
nvm node_mirror https://npm.taobao.org/mirrors/node/
```

```powershell
nvm npm_mirror https://npm.taobao.org/mirrors/npm/
```



#### 简单使用 (Window 为例)

- 列出可用的版本
  ```powershell
  nvm ls available
  ```
	**※ 在Linux中是 `nvm ls-remote`**

- 安装node版本

  ```powershell
  nvm install 12.16.0
  ```

- 使用某版本的node

  ```powershell
  nvm use 12.16.0
  ```

- 卸载版本为啊`a.b.c`的node

  ```powershell
  nvm uninstall a.b.c
  ```



## 后台环境配置

#### Go环境配置

从 [此处](https://golang.google.cn/dl/) 下载对应版本的 go 直接安装即可

如果你需要做go的多版本管理, 可以试试使用 [gvm](https://github.com/moovweb/gvm)



#### **go的下载太慢?**

可以尝试一下 [Goproxy.cn](https://goproxy.cn/) 

