---
layout: post
title: Markdown 基本语法
tags: markdown
categories: 字体排印
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
~~删除线~~
注脚[^footnote]

![Jeanys's Blog]({{"/upload/placeholder.png" | prepend: site.imgrepo }} "图片插入")

---

> 区块引用 Blockquotes
> > 区块嵌套

+ 无序列表 ul
+ 无序列表 ul
+ 无序列表 ul

1. 有序列表 ol
2. 有序列表 ol
3. 有序列表 ol

```javascript
function markdown() {
    //代码区块
}
```

| 向左对齐  | 向右对齐 | 居中 |
| :-------- | --------:| :--: |
| 计算机    |  6000 ￥ | 0.5  |
| 手机      |  3000 ￥ | 1.62 |
| 移动电源  |   200 ￥ | 66.5 |

---

参考文章：

+ [Markdown——入门指南][1]
+ [Markdown，你只需要掌握这几个][2]
+ [Markdown 语法说明（简明版）][3]
+ [Markdown 语法说明（完整版）][4]
+ [Markdown 中文网][5]

[1]: http://www.jianshu.com/p/1e402922ee32/ "By @Te_Lee"
[2]: https://www.zybuluo.com/AntLog/note/63228 "By @作业部落"
[3]: http://wowubuntu.com/markdown/basic.html "By @riku"
[4]: http://wowubuntu.com/markdown/index.html "By @riku"
[5]: http://www.markdown.cn

[^footnote]: 用来阐释注脚
