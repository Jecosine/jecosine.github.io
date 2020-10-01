---
title: 使用Electron与C++开发一个网络抓包工具
date: 2020-09-30 17:03:44
tags:
- computer network
- electron
- nodejs
- vue
- ant-design
categories:
- project
- nodejs
---
## 项目概述

## 项目配置

### node 与 npm 的配置
### 模块

**electron**
**vue-cli**
**electron-vue**
**坑 1-1 运行electron时出现`ReferenceError`**
```
  ERROR in Template execution failed: ReferenceError: process is not defined

  ERROR in   ReferenceError: process is not defined

    - index.ejs:11 eval
      [.]/[html-webpack-plugin]/lib/loader.js!./src/index.ejs:11:2

    - index.ejs:16 module.exports
      [.]/[html-webpack-plugin]/lib/loader.js!./src/index.ejs:16:3

    - index.js:284
      [network_snipper]/[html-webpack-plugin]/index.js:284:18

    - runMicrotasks

    - task_queues.js:97 processTicksAndRejections
      internal/process/task_queues.js:97:5
```
**ant-design**
```
yarn add ant-design-vue
```
### Winpcap的配置

## 项目实现



### C++调用Winpcap

### Electron桌面应用开发

### Socket通信传输抓包信息

### 整合与打包

