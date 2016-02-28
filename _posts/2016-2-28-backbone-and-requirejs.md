---
layout: post
title: 初学Backbone+RequireJS
tags: 模块化 MVC Backbone RequireJS RESTful
categories: JavaScript
---

这个礼拜经人推荐面试，去了一家新公司，年后的生活算正式开启了。公司的项目主要采用的是Backbone+RequireJs，模板引擎用的是doT.js，用RESTful API与后端对接。试岗三天我稍微熟悉了一下这些技术，现在就来记录下学到的内容和遇到的挑战。

### Backbone

Backbone是一个构建JavaScript应用程序的框架。传统的MVC分为`Model`,`View`,`Controller`，即把应用程序中的数据、视图分开，由Model也就是模型存放数据，View管理视图，而Controller控制器作为连接Model与View的中间件。但在Backbone中，C代表的不是Controller而是`Collection`，Model存放单个数据，Collection用来存放数据集合，而操作Model的主要部分，Event事件放在了View中。Backbone依赖`underscore`和`jQuery`，underscore为Collection提供很多的集合方法，而jQuery可以为View提供方便的DOM操作。
