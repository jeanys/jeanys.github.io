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

创建便签集合的Collection，设置与服务器通信的url。

```javascript
var NoteList = Backbone.Collection.extend({
    model: NoteItem,
    url: '/index.php/note/'
});
```

所有便签的View首先要放在模板里，这样才能对它进行循环和渲染。（Backbone用的underscore渲染模版，语法真是丑哭惹，等会就换doT.js）

```html
<script type="text/template" id="item-template">
<% _.each(item,function(data) { %>
    <li>
        <pre class="cont"><%- data.content %></pre>
        <span class="info"><%- data.time %></span>
        <a href="javascript:;" class="remove iconfont" title="删除">&#xe6b4;</a>
    </li>
<% }); %>
</script>
```

获取模版内容，DOM操作当然用默认依赖的jQuery啦。

```javascript
_.template($('#item-template').html()),
```

创建Collection的实例和便签列表的视图。最重要的一步：对Collection的事件监听并渲染，这样集合发生的添加、修改、删除就能反馈给视图。然后就可以对Collection拉取了。拉取成功后，Backbone自动对Collection进行`add`操作，监听方法就会执行`this.render`，便签列表就可以显示出来了。

```javascript
var notelist = new NoteList;
var NoteView = Backbone.View.extend({
    el: '.note-list',                                               //获取ul元素
    template: _.template($('#item-template').html()),
    initialize: function() {
        this.listenTo(notelist,'add',this.render);                  //监听集合实例的自定义事件
        this.listenTo(notelist,'change',this.render);
        this.listenTo(notelist,'destroy',this.render);
        notelist.fetch();                                           //拉取
    },
    render: function(model) {
        this.$el.html(this.template({item: notelist.toJSON()}));    //渲染模版并插入ul
    }
});
```

这里有必要说下render这个操作，首次看到的时候，我实在对这个部分难以理解，怎么看怎么都绕来绕去的。仔细拆分之后就好懂多了，`this.template`就是获取到的模版内容，对它传入的对象模版那边可以接收到。当然了，对象的键名item要与模版中一致。`notelist.toJSON()`是Collection中的数据格式化为JSON，模版中的data对应的就是这个JSON数据，然后渲染的时候就可以访问到每个便签的具体属性了。接下来的增加、修改、删除都差不多，就不全部贴出了，只记录一下具体的重要操作。

```javascript
//添加
notelist.create({'content':this.notetext.val(),'time':dateFormat(new Date())},{
    success: function(model,response) {
        model.set('id',response);
    }
});
//删除
notelist.at(index).destroy();
//修改
this.modified.set('content',this.notetext.val());
this.modified.set('time',dateFormat(new Date()));
this.modified.save();
```

到这里，便签就完成了。其实View层我写的很烂。正确的做法是为单个便签创建一个View，为便签列表创建一个View，Model对应小View，Collection对应大View，单个便签的事件与集合的事件分别绑定，这样逻辑和代码都清晰的多，可参考官方例子Todo。

---

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
