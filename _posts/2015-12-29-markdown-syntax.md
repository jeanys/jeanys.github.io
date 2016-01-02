---
layout: post
title: Markdown 基本语法
tags: markdown
categories: study
---

# Header 1
## Header 2
### Header 3
#### Header 4
##### Header 5
######  Header 6

普通段落

Markdown 是一个 Web 上使用的文本到HTML的转换工具，可以通过简单、易读易写的文本格式生成结构化的HTML文档。目前 github、Stackoverflow 等网站均支持这种格式。

[超链接](http://jeanys.github.io/ "link")
**强调加粗**
*强调倾斜*
`行内代码`

--------------------------------------

> 区块引用 Blockquotes
> > 区块嵌套

+ 无序列表 ul
+ 无序列表 ul
+ 无序列表 ul

1. 有序列表 ol
2. 有序列表 ol
3. 有序列表 ol

~~~javascript
function markdown() {
    //代码区块
}
~~~

- [ ] 支持以 PDF 格式导出文稿
- [ ] 改进 Cmd 渲染算法，使用局部渲染技术提高渲染效率
- [x] 新增 Todo 列表功能
- [x] 修复 LaTex 公式渲染问题
- [x] 新增 LaTex 公式编号功能

```flow
st=>start: Start
op=>operation: Your Operation
cond=>condition: Yes or No?
e=>end

st->op->cond
cond(yes)->e
cond(no)->op
```

| 项目       | 价格    |  数量   |
| --------   | -----:  | :----:  |
| 计算机     | \$1600  |   5     |

