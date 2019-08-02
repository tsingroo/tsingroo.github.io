---
author: tsingroo
date: 2019-08-02 19:00:50+08:00
layout: post
title: '搭建ReactJs开发环境实践'
---

本文写于2019-08-02，CRA版本为3.0.1。目前开启的项目算是正式意义上的第一个ReactJs项目，把环境搭建的一些东西记录下来，供大家也供自己参考。

##### 本文涉及的内容包括:
* 一、在不eject官方脚手架配置的前提下，让react项目支持多种环境(staging,灰度.etc)
* 二、在不eject官方CRA配置的前提下，让react项目读取本地json文件(不放在public目录下，放在public目录下会导致发布的dist增加很多不必要的文件)
* 三、推荐的文件目录结构
* 四、commit之前的代码检查


##### 结论看起来简单，但是中间还是走了不少弯路的。目前这种方法不一定是最好的，但是绝对是考虑了一些场景后做的相对较优的。因为Eject出来配置后，后续CRA一更新可能产生更多问题，所以尽量不eject配置。

#### 一、如何让React支持新的环境(以staging为例)

根据React官方的介绍，官方通过支持```.env```文件的方式支持test/development/production三种。这个很像VueCli3以后的环境支持方式。两个框架都不约而同的选择了这种方式，简单一查。这是由```dotenv```这个npm包提供的一个能力，会在编译时将这个配置量插入到编译好的代码中。运行时这个变量并不存在。

经过反复尝试后，确实如React官方所说，NODE_ENV重写不了。但是可以写入REACT_APP_开头的一些量，，所以我们区分环境到这里也就只能以REACT_APP_NODE_ENV来区分。

如何新增这个staging环境：
* 1.创建env文件```touch .env.staging```
* 2.写入REACT_APP_NODE_ENV这个值```echo "REACT_APP_NODDE_ENV=staging" > .env.staging```
* 3.向package.json添加一条记录```"staging":"node -r dotenv/config ./node_modules/.bin/react-scripts start dotenv_config_path=.env.staging"```
* 4.现在程序里面取```process.env.REACT_APP_NODE_ENV```就是```staging```这个值了！


#### 二、如何让React读取本地JSON文件来Mock



#### 三、推荐的目录结构
```
mock/                       //mock数据json文件目录
public/                     //index.html目录
src/                        //源码目录
|--component/               //组件
|--pages/                   //页面-如果页面太多需要划分模块文件夹
|--utils/                   //工具
|--router/                  //路由
|--index.js                 //ReactJs入口
.env.staging                //新增环境配置
package.json                //项目配置文件
```


#### 四、Commit之前的代码检查
