---
author: tsingroo
date: 2020-09-02 11:00:00+08:00
layout: post
title: '如何在古老项目(类似页面通过script引入jquery的项目)中加入TypeScript及Webpack等支持'
---

文章写于2020-09-02.

目前仍然有很多公司(尤其是二三线城市)的很多项目，都是由html + 页面script引入方式编写而成，其中既有技术传播的原因，也有一些历史项目的原因。目前我就在参与这么一个jquery的项目。现在TypeScript已经普及的情况下，对于ts好坏暂且不说，但是确实有必要改造一下这个项目了。

### 参考资料
* [TS Cli参数](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
* [ts声明文件的编写](https://www.typescriptlang.org/docs/handbook/declaration-files/introduction.html)


### 当前项目情况
* JS通过手动编写，没有基础的语法校验，也没有语法转换
* 编写JS的一些低级错误(方法名不小心写错，字段不小心写错等)，编辑器也无法检查出错误
* 各个环境都是手动通过sftp放文件到服务器，可能因为手误、眼花等原因导致误操作，生产环境没有备份出问题就比较棘手


### 想要改造后的情况
* 通过webpack及typescript,支持TS以及一些ES的高级语法
* 通过dotenv,支持编译以及传入环境变量,各个环境统一拉取git发布

### 改造步骤

#### Step1.支持Typescript以及高级的ES语法
* 0.使用target参数即可生成目标ES的代码，所以目前看起来并不需要加入webpack套件
* 1.使用npm初始化为node项目后，安装typescript,```npm install -D typescript```
* 2.安装jquery和类型声明,```npm install -S jquery```, ```npm install -D @types/jquery```
* 3.对于其它的公共库，也是需要安装库和types文件
* 4.这个时候，对于绝大多数的通过npm安装的库，在ts文件里面已经都可以给出提示和语法纠错了

#### Step2.对于自己的js文件需要编写```xxx.d.ts```类型声明文件,并将原有的js改成TS
* 0.这一步工作量比较多，但是不难。可以一步一步来，通过指定编译文件来分步迁移TS，具体参考types的编写文档
* 1.完成这一步之后，自己编写的ts也有语法提示和语法纠错了。下一步就是将TS编译到js文件，并有指定的文件名

#### Step2.传入环境变量，不同环境差别编译
* 