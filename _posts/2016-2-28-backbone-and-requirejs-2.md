---
layout: post
title: 初学Backbone(2)
tags: doT.js RequireJS 模块化
categories: JavaScript
---

接上文：[初学Backbone(1)](http://jeanys.me/2016-02-28/backbone-and-requirejs/)

### doT.js

doT.js是一个强调快速简洁的JavaScript模版引擎，官方文档很简单，事实上用过之后发现它也确实很简单且易上手。doT.js有六种语法，下面来一一说说。（it为默认的接收对象表示）

+ 插值      `{{=it.name}}`
+ 循环
    + 开始标记 `{{ for(var prop in it) { }}`
    + 结束标记 `{{ } }}`
    + 插入键名 `{{=prop}}`
    + 插入键值 `{{=it[prop]}}`
+ 条件
    + 开始标记 `{{? 条件表达式}}`
    + elseif   `{{?? 条件表达式}}`
    + else     `{{??}}`
    + 结束语句 `{{?}}`
+ 数组
    + 开始标记 `{{~it.array :value:index}}`
    + 结束标记 `{{~}}`
    + 插入键值 `{{=value}}`
    + 插入索引 `{{=index}}`
+ 编码      `{{!it.html}}`
+ 定义
    + 开始标记 `{{##def.snippet:`
    + 片段结束 `#}}`
    + 结束标记 `{{#def.snippet}}`

利用之前的便签Demo将模版引擎替换实践。先将官方下载好的doT.js引入到Backbone之后，然后修改HTML部分：

```html
<script type="text/x-dot-template" id="item-template">
{{~it.item :value:index}}
    <li>
        <pre class="cont">{{=value.content}}</pre>
        <span class="info">{{=value.time}}</span>
        <a href="javascript:;" class="remove iconfont" title="删除">&#xe6b4;</a>
    </li>
{{~}}
</script>
```

JavaScript中的获取模版部分：

```javascript
doT.template($('#item-template').html()),
```

这样就可以了。是不是很简单呢~

---

### RequireJS

模块化学问还是很深的，这里我只是粗粗了解了下。它的好处还是蛮多的，统一配置引用库、解决相互依赖、模块化代码、异步加载等~使用官方的r.js配合打包还可以合并压缩减少请求。还是刚才的Demo来实践一下，先新建一个js用来相关配置：

```javascript
require.config({
    paths: {                                //不用写.js后缀
        'jquery': 'jquery-1.12.1.min',      //引用库的相对路径
        'underscore': 'underscore',
        'backbone': 'backbone',
        'doT': 'doT.min'
    },
    shim: {                                 //不支持AMD规范的库
        'underscore': {
            exports: '_'
        },
        'backbone': {
            deps: ['underscore','jquery'],  //依赖的库
            exports: 'Backbone'             //向外暴露的调用接口
        },
        'doT': {
            deps: ['underscore'],
            exports: 'doT'
        }
    }
});
require(['noteapp'],function(value){
    //这里写加载完noteapp.js之后要执行的，value接收到的是return返回值
});
```

在noteapp.js里面，第一个参数数组放依赖的库，与第二个函数中参数写的调用接口要一一对应：

```javascript
define([
    'jquery',
    'underscore',
    'backbone',
    'doT'
],function($,_,Backbone,doT){
    //核心代码，return的结果可以在上面接受到
});
```

这样配置完之后，就完成了初步的实践。当然RequireJS更好的使用应该是将之前的Model、Collection、View分别放到不同的js文件中，让define来互相引用，这样才算真正地实现了模块化。最后一步下载官网中的r.js，安装好Node.js并进行一下配置，在命令行中就可以把互相依赖的js合并到一起去了，大大的减少了HTTP请求和体积，r.js还有更多的用法，可以参考下网络上的文章。

---

最后来梳理下我看过的API和博文，在我学习的过程中都有很大的帮助。

+ [Backbone.js API][1]
+ [underscore.js API][2]
+ [Backbone.js API中文镜像][3]
+ [underscore.js API中文镜像][4]
+ [Backbone.js入门教程第二版][5]
+ [Backbone-mobile 移动WebAPP项目骨架][6]

+ [doT.js API][7]
+ [doT.js API中文镜像][8]
+ [doT.js模板引擎的使用][9]
+ [doT.js爱好者][10]

+ [RequireJS API][11]
+ [RequireJS API中文镜像][12]
+ [require.js的用法][13]
+ [requirejs模块化编程][14]
+ [巧用 RequireJS Optimizer 给传统的前端项目打包][15]

[1]: http://backbonejs.org/
[2]: http://underscorejs.org/
[3]: http://www.css88.com/doc/backbone/
[4]: http://www.css88.com/doc/underscore/
[5]: https://github.com/the5fire/backbonejs-learning-note "By the5fire"
[6]: https://github.com/linksgo2011/backbone-mobile "By 少个分号"

[7]: http://olado.github.io/doT/
[8]: http://jinlong.github.io/doT/
[9]: http://www.fantxi.com/blog/archives/dot-template/ "By Kairyou"
[10]: http://dotjs.cn/
[11]: http://www.requirejs.org/
[12]: http://www.requirejs.cn/
[13]: http://www.ruanyifeng.com/blog/2012/11/require_js.html "By 阮一峰"
[14]: https://segmentfault.com/a/1190000003409854 "By 智远try"
[15]: http://jiongks.name/blog/build-any-web-project-with-requirejs-optimizer "By 勾三股四"
