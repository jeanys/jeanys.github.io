---
layout: post
title: CSS未来公开课笔记
tags: CSS3 框架 中国首届CSS开发者大会
categories: HTML&CSS
---

## CSS未来

BortBos [PPT][PPT] [视频地址][video]

### 1. 目前CSS规范的状态

在CSS诞生时，考虑到需要向前兼容，CSS标准化使用 level 而不是 version[^level]。历史上，CSS Level 1 经历了 1994-1996 年，CSS Level 2 经历了 1997-1998 年。从1999年至今，CSS规范有很多部分需要更新，这个工作将是非常庞大的。于是W3C将 CSS Level 3 切分成了许多的 Module，这样已完成的部分就可以更早地推出规范。在 CSS Level 1 和 CSS Level 2 未出现过的模块，level 从1开始计，例如 `Transforms`、`Animations`，出现过的模块，将延续从前的 level 继续计数，例如 `Selectors`、`Fonts`。如下表所示。

| 1994–1996 | 1997–1998 | 1999–now     |
| :-------- | :-------- | :--------    |
| level 1   | level 2   | Selectors level 3 / Transforms level 1 ... |
| 1 spec.   | 1 spec.   | ~ 60 modules |
| 5K lines  | 54K lines | 201K lines   |

### 2. 开发中的模块

主要有以下四个领域（部分规范略，详情请看[PPT][PPT]）

+ 文字转语音，是给有阅读障碍的人准备的
+ 分页，是给电子书准备的领域
+ GUI布局，不仅有静态布局，还有关于用户交互等
+ 图形，包含变换、动画，还有阴影、透明等
    + [Filter Effects Module Level 1][filter] 滤镜，如反色、模糊等
    + [CSS Masking Module Level 1][mask] 遮罩蒙板
    + [Compositing and Blending Level 1][composit] 渐变与混合模式
    + [CSS Transitions][transition] 过渡，使样式逐渐变化
    + [CSS Transforms Module Level 1][transform] 图形变换，如缩放、拉伸等
    + [CSS Animations][animation] 动画

还有一些模块并不从属于某个领域，而是普遍适用的。

+ 选择器
    + [Selectors Level 3][selector3]
    + [Selectors Level 4][selector4]
+ 条件规则
    + [CSS Conditional Rules Module Level 3][conditional] `@supports` 检测浏览器对某属性的兼容性
+ 媒体查询
    + [Media Queries Level 4][media4] 新增了一些特性，比如检测你的背景光
+ 取值单位
    + [CSS Values and Units Module Level 3][units3] 新增了一些单位，如rem,ch,vw,vh. 值可以用calc()做简单的运算

以上模块在2015年底基本能完成，还有一些尚在筹划的模块，如嵌套动画等。
CSS从诞生到现在已经18年有余，但她仍然未显衰老。相反CSS开发者越来越多，CSS触及领域越来越广。CSS未来将接近电子书的阅读体验，提供更好的非西方语言的排版支持。CSS吸取了Word、Photoshop的优点，她的另一个目标就是未来在Web中能做出像桌面应用、系统组件的效果。
让我们拭目以待吧。

---

参考资料：

+ [中国首届CSS开发者大会-慕课网录播][imooc]
+ [首届大会w3ctech主页][w3ctech]

[video]: http://www.imooc.com/view/314
[PPT]: http://www.w3.org/Talks/2015/0110-CSS-Beijing/all

[filter]: http://www.w3.org/TR/filter-effects-1
[mask]: http://www.w3.org/TR/css-masking-1
[composit]: http://www.w3.org/TR/compositing-1
[transition]: http://www.w3.org/TR/css3-transitions
[transform]: http://www.w3.org/TR/css-transforms-1
[animation]: http://www.w3.org/TR/css3-animations

[selector3]: http://www.w3.org/TR/css3-selectors/
[selector4]: http://www.w3.org/TR/selectors4
[conditional]: http://www.w3.org/TR/css3-conditional/
[media4]: http://www.w3.org/TR/mediaqueries4
[units3]: http://www.w3.org/TR/css3-values

[imooc]: http://www.imooc.com/space/teacher/id/1214876
[w3ctech]: http://www.w3ctech.com/event/43/

[^level]: level表示每新一级的level都向下兼容，而version有取代旧版的意思。
