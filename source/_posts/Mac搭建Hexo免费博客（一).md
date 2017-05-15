---
layout : post
title: Mac搭建Hexo免费博客（一）
date: 2014-05-21 15:53:51
tags: Blog
excerpt: "根据hexo搭建博客-步骤1"
comments: true
---

## HEXO博客配置

#### 前言

由于项目忙完，需要搭建一个Blog记录下工作经历，这里使用的是Hexo。一个方便快捷的搭建Blog工具
 
#### 关于Hexo

Hexo是一个快速、简洁且高效的博客框架。Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。 [Hexo官网](https://hexo.io/zh-cn/)

#### 环境搭建
硬件 
&emsp;此版本为Mac版👽
软件
    &emsp;node.js
    &emsp;npm
    &emsp;hexo
    &emsp;github账号

#### 安装node.js与npm

下载node.js 有多种方法：使用 [Homebrew](https://brew.sh/index_zh-cn.html) 下载 或者直接下载安装包。 建议 node.js 直接下载 安装包，因为使用 brew 有可能失败，会被墙掉。

[node.js下载地址](https://nodejs.org/en/download/)


node.js 下载完成后 安装到电脑上就可以了。安装成功后显示出来安装路径，可以看到 安装node.js 的时候 npm 也安装了。

检测安装是否成功 终端输入 -v , 成功则显示版本号


```
$ node -v
v4.3.1
$ npm -v
2.14.12
```
#### 安装Hexo

文档里给出了详细的安装方法，只需要按其一步步来就好.

```
$ sudo npm install -g hexo-cli
```
安装成功，使用hexo 命令的时候，可能会出现以下错误

```
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }    
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```
这个错误也没有造成任何影响，所以忽略，Go on。 完成后，验证下是否安装成功.

```
hexo -v
```

#### Hexo的使用
###### 1.创建存放博客的文件夹

```
$ mkdir myblog
```

##### 2.执行以下命令，Hexo会在目标文件夹建立博客所需要的文件

```
hexo init
```
输出以下信息

```
INFO  Cloning hexo-starter to ~/Documents/mytest
Cloning into '/Users/Quncao/Documents/mytest'...
```
成功之后文件夹的样子 
![](http://7xr7f9.com1.z0.glb.clouddn.com/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202016-02-26%20%E4%B8%8B%E5%8D%884.13.00.png)

_config.yml : Hexo和站点的配置文件，里面可以设置博客的名字、标题、作者、链接格式等相关项
scaffolds : 脚手架，用于存放我们创建文章时的模版
source : 用于存放我们用markdown编写的博文原文件、其他静态资源文件
themes : 用于存放主题，里面有我们博客的默认主题landscape
##### 3. 执行以下命令，进行依赖包的安装

```
sudo npm install
```
node_modules: 关联保存了将会使用到的hexo依赖包

##### 4. 安装相关插件
插件会安装至node_modules文件夹下，如果已经安装好的可以直接忽略.

- 安装便于自动部署到Github上的插件

```
$ npm install hexo-deployer-git --save
```
- 安装atom生成插件，便于感兴趣的小伙伴们订阅

```
$ npm install hexo-generator-feed --save
```

- 安装博客首页生成插件
- 
```
$ npm install hexo-generator-index --save
```

- 安装归档生成插件

```
$ npm install hexo-generator-archive --save
```

- 安装tag生成插件

```
$ npm install hexo-generator-tag --save
```

- 安装category生成插件

```
$ npm install hexo-generator-category --save
```

- 安装Sitemap文件生成插件

```
$ npm install hexo-generator-sitemap --save
```

- 安装百度Sitemap文件生成插件，因为普通的Sitemap格式不符合百度的要求

```
$ npm install hexo-generator-baidu-sitemap --save
```

---

*本篇主要讲述了使用Hexo进行博客的搭建与配置，以及如何部署到github上，至此我们所有创建博客的工作已经完成。之后便是自己博客的编写与经营了，一些更深入的配置移步[第二篇](https://leoantu.github.io/2014/04/25/Mac%E6%90%AD%E5%BB%BAHexo%E5%85%8D%E8%B4%B9%E5%8D%9A%E5%AE%A2%EF%BC%88%E4%BA%8C%EF%BC%89/)吧*

