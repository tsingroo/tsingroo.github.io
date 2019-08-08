---
author: tsingroo
date: 2019-08-07 12:00:50+08:00
layout: post
title: 'Webpack核心概念'
---

本文涉及的内容包括：
* 什么是Webpack
* Webpack的核心:plugin,loader,entry,output,webpack-dev-server


### 什么是Webpack

先看一下官方解释： webpack is a static module bundler for modern JavaScript applications。有时候直接看英文比看翻译过来的中文更好理解，这句话就是这样。static是说在编译期处理(发布之后不会携带webpack相关内容)，Module使用的UMD规范,bundler是打包器。用更好理解的话来说就是：webpack是一个在编译期使用UMD规范进行模块化打包的打包器。

打出来的包叫做bundle。打包之前bundle的输入叫做module。默认webpack只可以处理js和json两种module，通过loaders可以扩展webpack处理module的种类。

webpack是一个工程化的工具，将标准化的流程框架定义出来，然后各个框架(Vue/Angular等)可以以webpack定义的流程为基础扩展自己的能力。包括前期开发时候的webpack-dev-server，各个环境支持，通过loaders对scss,less等的支持，后期的通过plugin的统一处理等。



### Webpack核心概念

配置文件为```wepack.config.js```

#### Entry

编译入口文件，可以有多个，但是多个入口不能相互引用，否则构建depedency grraph就有问题。


#### Output

输出的文件目录和文件名，静态html会使用这个名字来输出编译后的入口文件。

#### Loader

默认webpack只能处理js和json文件，当有其他文件的时候，需要install对应的loader然后在rules里面配置。


#### Plugin

前期loaders处理完成之后，plugin再对这些bundle进行处理(bundle优化，环境变量注入等)

#### webpack-dev-server

本地运行的一个http-server，配合HMR在修改文件后页面立即刷新。
