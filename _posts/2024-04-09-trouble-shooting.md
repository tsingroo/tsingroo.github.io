---
author: tsingroo
date: 2024-04-09 11:41:00+08:00
layout: post
title: '如何在前后端项目中进行问题排查'
---

#### 个人知识的一个小结，不系统，但可能对你有些用处

### 通用的排查思路(以内存泄露问题为例)主要包含如下步骤
- 1.先看看内存泄露是否是暂时性的,如果是暂时性的可能是任务数过多的时候，GC没来得及释放。如果应用冷却下来后(或者前端页面不操作一段时间)内存还是没能降下来，那多半是内存泄露了。这种情况会造成内存一直上涨直到重启应用或者系统崩溃。
- 2.如果确认是内存泄露无法GC，就要使用工具(go语言使用pprof并打开web端口,前端使用Chrome Dev Tool)来确定大概的位置。关于确认内存泄露的大概逻辑的可以网上搜一下。以后我有可能会写一个简短的实例分析，也可能不写。
- 3.有时候工具(pprof)只能定位大概的代码位置，或者定位到了基础库位置，但是我们有多个地方调用了基础库，这个时候不好确认的时候就使用二分删除代码法，一部分一部分的删除，确认哪一部分的代码调用触发了内存泄露。方法虽然笨但是相当好用，屡试不爽。
- 4.定位到关键代码后，就需要查找一下相关资料，看看是不是触及到自己知识盲区了，然后找到问题并解决。当一个工具或者语言在面临复杂问题时，你原先对于这个工具或语言的知识可能就不够用了。


### 一个go语言内存问题的简单记录
- 待完成


### 一个前端页面的内存问题的简单记录
- 待完成