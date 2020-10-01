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
nvm: `1.1.7`  
npm: `6.13.4`
node: `12.16.0`
yarn: `1.22.0`

#### 安装nvm
Windows下在[此处](https://github.com/coreybutler/nvm-windows/releases)下载最新nvm并安装。其他平台请参考此处[https://www.runoob.com/w3cnote/nvm-manager-node-versions.html](https://www.runoob.com/w3cnote/nvm-manager-node-versions.html)。安装成功后，在控制台输入`nvm version`应该可以看到版本号
```
> nvm version
1.1.7
```
使用nvm对不同版本的npm与node进行管理，首先配置淘宝镜像。控制台中输入`nvm root`查看nvm安装目录(此处是**我的**安装目录)
```
> nvm root

Current Root: D:\nvm

```
找到里面的`settings.txt`，在最后加入这两行：
```
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```
最后的文件内容（可能与读者的略有不同）
```
root: D:\nvm
path: D:\Program Files\nodejs
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```
然后重新打开一个cmd， 输入`nvm install [your-node/npm-version]`即可安装对应版本的node与npm
```
> nvm install 12.16.0
```
安装成功后，切换node版本到12.16.0
```
> nvm use 12.16.0
```

### 模块
**cnpm**
国内使用cnpm是一个比较方便的选择，安装方法如下
```
> npm install cnpm -g --registry=https://registry.npm.taobao.org
```
**yarn**
使用npm安装
```
> cnpm install -g yarn
```
使用淘宝镜像
```
> yarn config set registry https://registry.npm.taobao.org --global
> yarn config set disturl https://npm.taobao.org/dist --global
```
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
**解决方案**
**ant-design**
```
yarn add ant-design-vue
```
使用[官网](https://www.antdv.com/docs/vue/introduce-cn/)推荐的babel引入方式
```js
// .babelrc or babel-loader option
{
  "plugins": [
    ["import", { "libraryName": "ant-design-vue", "libraryDirectory": "es", "style": "css" }] // `style: true` 会加载 less 文件
  ]
}
```
坑1-2 `cnpm run dev`时出现错误`ReferenceError: Unknown plugin "***"`
安装`babel-plugin-***`即可
```
> cnpm install babel-plugin-import -D
```
坑1-3 运行electron出现
```
┏ Electron -------------------

  Failed to fetch extension, trying 4 more times

┗ ----------------------------
...
```
实际上是网络问题，第一次运行时会下载vue tool插件，使用科学的方法之后可√，无视也可。

### Winpcap的配置

## 项目实现



### C++调用Winpcap

### Electron桌面应用开发

### Socket通信传输抓包信息

### 整合与打包

