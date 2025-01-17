---
title: 第1-3节 DDD 工程模型
lock: https://t.zsxq.com/iPuAe
---

# 《小型支付商城系统》第1-3节 DDD 工程模型

作者：小傅哥
<br/>博客：[https://bugstack.cn](https://bugstack.cn)

> 沉淀、分享、成长，让自己和他人都能有所收获！😄

**什么是系统的工程结构，工程框架的作用是什么？**

其实，工程结构的存在作用目的，是为了承载工程系统开发的模型划分，定义工程服务开发过程中实施标准。说白了，就是你在工程实现时，在哪个层访问数据库、哪个层使用缓存、哪个层调用外部接口、哪个层做功能实现，这就是工程框架结构定义的目的。

但在 `Service + 贫血模型` 的三层结构开发指导下，是没有细分出面向对象工程结构设计的趋于划分的。所以在通常意义的 MVC 下，开发过程所有需要的内容，都会堆砌到 Service 实现类中。这也是为什么 DDD 领域驱动设计的落地工程结构，会出现；洋葱架构、整洁架构、菱形架构、六边形架构等这些架构模型。因为我们需要更细致的划分，来承载 DDD 设计概念中映射的领域、仓储、适配、编排、触发，并重视面向对象过程。—— 其实你一上学，学Java就开始学面向对象了，只不过一点点在忘记它。

## 一、为啥需要架构

说到开发代码为啥需要架构，就想买了个房子，为啥要隔出厨房、客厅、卧室、卫生间一样，核心目的就是让不同的职责分配到不同的区域内。虽然在代码中是没有马桶要放卫生间、沙发要放客厅、床要放卧室。但他有一些列的科目信息要引入到工程。

**在工程开发时会涉及到的核心科目；**

<div align="center">
    <img src="https://bugstack.cn/images/roadmap/tutorial/ddd-easy-guide-03-01.png" width="550px">
</div>

如；统一的异常、数据库的连接、日志的打印、外部服务的调用、消息的监听、任务的轮训以及服务的实现等一些列的东西要处理，分配到不同的工程包下承载。在 DDD 之前，我们一直用 MVC 的分层结构承接这些内容；

<div align="center">
    <img src="https://bugstack.cn/images/roadmap/tutorial/ddd-easy-guide-03-02.png" width="550px">
</div>

通用的、配置的、组件的、持久化的、内部的、外部的，在以往的单体应用时代开发下，其实是没有这么多东西的，那时候的工程结构都偏向于 Service + 贫血模型实现。

但随着微服务的演进，越来越多的内容被填充到工程中，这个时候你细心的查看架构，就会发现原本的 MVC 结构其实已经变的非常混乱了。一个 Service 中为了实现自己的功能，要引入一堆的东西，这些原子的功能与 Service 自身的服务耦合在一块。也导致了工程的维护成本越来越大。

>这样的三层工程结构分配方式，对于要承载庞大的分布式技术栈体系显然是有点小马拉大车，三缸机带不动SUV一样。

## 二、工程结构设计

2004年，Eric Evans 在发表了一部名为《Domain Driven Design》的著作。2005年 Alistair Cockburn 提出的“六边形关系图”理论，2008年 Jeffrey Palermo 提出了洋葱架构。虽然这些架构并不是专门为 DDD 而出，但巧的是这些架构都在 DDD 一书发表之后陆续推出新的架构模型。同时这些架构的分层设计方式也都与 DDD 非常契合，在这些架构下也可以很好的落地 DDD 设计方法。

<div align="center">
    <img src="https://bugstack.cn/images/roadmap/tutorial/ddd-easy-guide-03-03.png" width="750px">
</div>
无论是六边形架构，还是洋葱架构，或是[张毅老师](http://zhangyi.xyz/)提到的南向网关/北向网关的菱形架构，他们的目标都是以领域服务为核心，隔离内部实现与外部资源的耦合。

在 DDD 分层架构下，以支撑 domain 核心领域实现拆分出基础设施（infrastructure），来承接对外部资源的调用。触发器（trigger）向外部提供服务。之后 app 为应用启动、api 为接口定义、types 为通用信息、case 为编排。

在这样一套结构下，用于开发工程的各项科目也可以被优雅的分配到各个分层结构了。相对于 Service + 数据模型的贫血模型结构，现在就主要以 domain 为核心开发业务功能，不会在 domain 工程模块下，引入其他各类外部组件了，这样就可以更加关心业务功能开发。

之后是这样的思想映射到工程中，常见的分层结构会有两套，一套是整洁分层，另外一套是六边形分层。