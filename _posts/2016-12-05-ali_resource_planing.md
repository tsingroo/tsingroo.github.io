---
author: tsingroo
date: 2016-12-05 19:53:50+08:00
layout: post
title: '阿里巴巴容量规划'
---

 > 内容来源于ArchSummit2016北京站小邪演讲,并结合了一部分自己的认知

问题：原有业务快速增长、大促等活动需要预知各种资源(CPU、IO、消息队列)，需要知道何时扩容/缩容。

如何解决？：容量规划

容量规划三个阶段：
 * **经验判断**：系统架构未进行服务化改造之前，系统是一个单块系统，大家几百人坐到一起，拿出电脑，一起测试，完后看看自己负责的模块有没有什么问题。这个时候的容量规划完全是凭经验判断
 * **线下，线上压测**：淘宝进行服务化分布式改造之后，开始压测是线下(不是真实环境)，后来又在线上(真实淘宝线上环境)进行测试。淘宝是一个分了很多层的分布式系统，整体的压测不能发现某一点的问题，例如双11主推商品，但双12主推店铺，用户在这两个活动中使用的服务和对系统模块造成的压力都是不一样的
 * **场景化压测**：淘宝的核武器-全链路场景化压测

如何做场景化压测？
 * 平台架构:
   * 1.执行程序exec负责执行，并负责阀值监控，停止压测，异常处理等工作。
   * 2.模型引擎engine负责模拟用户操作，复制用户和操作等工作。
   * 3.线上应用app接收引擎的操作,产生各种日志和数据。
   * 4.模型引擎engine收集系统数据，性能数据，业务数据。
   * 5.生成压测报告和容量水位(各种用户量下系统负载情况)
 * 数据制作：来自用户真实场景的日志，经过hadoop分析后转成压测可用的数据,然后用压测引擎执行这部分数据
 * 压测引擎集群:
   * 压测引擎部署到各个运行商的CDN机房，同时执行，访问阿里巴巴数据中心。
   * 各种协议，包括internet和wap。
   * 引擎可以对压测产生流量进行控制。
 * 流量染色：针对测试流量和真实流量进行标记，区别出两者
 * 流量隔离：识别出测试用户，针对测试用户，通过引擎隔离出资源，针对隔离出来的资源，进行拉平.
 * 整体流程：构造数据->环节准备->压测结果

真实大促中还会用的的一些手段：
 * 服务降级(为不重要的服务分配更少的资源)
 * 服务下线(大促期间，屏蔽评论等模块操作)
 * 快速失败(避免阻塞和死锁)

场景化压测实际表现:
 * 每周一个大测试，全年4000+次
 * 13年、14年、15年全链路压测分别发现了700+、500+、400+问题，系统越来越大，但问题越来越少。
 * 精准发现容量瓶颈

**阿里在这方面的优势：集团技术栈和基础服务比较统一，有利于全集团共享一种技术。**

**CTO提出的新要求：现在已经自动化，但是还不够智能化，中间件团队正在这条路上探索。**







