---
layout: post
title: 初学Backbone+RequireJS
tags: 模块化 MVC Backbone RequireJS RESTful
categories: JavaScript
---

这个礼拜经人推荐面试，去了一家新公司，年后的生活算正式开启了。公司的项目主要采用的是Backbone+RequireJs，模板引擎用的是doT.js，用RESTful API与后端对接。试岗三天我稍微熟悉了一下这些技术，现在就来记录下学到的内容和遇到的挑战。

### Backbone

Backbone是一个构建JavaScript应用程序的框架。传统的MVC分为`Model`,`View`,`Controller`，即把应用程序中的数据、视图分开，由Model也就是模型存放数据，View管理视图，而Controller控制器作为连接Model与View的中间件。但在Backbone中，C代表的不是Controller而是`Collection`，Model存放单个数据，Collection用来存放数据集合，而操作Model的主要部分，Event事件放在了View中。Backbone依赖`underscore`和`jQuery`，underscore为Collection提供很多的集合方法，而jQuery可以为View提供方便的DOM操作。

Backbone出生很早，较轻量、纯净是它的优点，但时至今日用的人已经远远没有AngularJS多了，国内又有了Vue和AvalonJS。不活跃的社区加上各种手动，学的时候总想吐槽它，但不可否认的是它很适合我现在。它做的事越少就越考验我自己的coding能力，手动绑定，手动渲染，可替换的模板引擎，都让我对MVC式的框架有了更深入的理解。

在知乎看到有句话说的非常好，Backbone的难点就在于对数据的抽象化。你可以将Model看作数据库里表的一行，Collection看作整张表。Backbone的大体流程就是，用户的增删改会造成数据的变化，数据的变化会触发Backbone的自定义事件，我们对自定义事件的监听就可以找到相应的视图做出更新，而视图更新具体的操作就是找到对应的前端模版渲染。说了这么多概念，下面来上例子吧。

我所做的例子是一个云便签，就和Backbone官方经典Demo——TodoList差不多，不过更简单。这个便签的思路如下：

1. 在页面载入的时候，向服务器发送GET请求，拉取所有的便签数据存入Collection。
2. 将Collection渲染到View，也就是HTML的ul中。
3. 点击添加按钮出现一个弹窗，填写内容后确认，将新数据添加到Collection并渲染。向服务器发送POST请求提交Textarea中的内容。
4. 服务器接收到POST请求处理完成后返回新便签的ID，修改Collection中对应Model的ID。
5. 点击便签li可修改内容，确认后修改Collection中对应Model的content。向服务器发送PUT请求提交被修改的便签ID与内容。
6. 点击便签上的垃圾桶图标可删除该条，销毁Collection中对应的Model。向服务器发送DELETE请求提交被删除的便签ID。

通过阅读Backbone的API了解到，Backbone的Model拥有`fetch`,`save`,`destroy`方法，能直接完成向服务器拉取当前Model数据并覆盖、保存Model并发送、销毁Model并发送这三件事情。Collection也同样拥有`fetch`方法，可一次性向服务器拉取所有的Model集合，`create`方法可以在Collection中添加Model并保存到服务器。理清了思路下一步就实现细节吧。

创建便签的Model并设置默认值。

```javascript
var NoteItem = Backbone.Model.extend({
    defaults: {
        content: '',
        time: '1955-02-25'
    }
});
```