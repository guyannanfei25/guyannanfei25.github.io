---
layout: post
title: 链家二手房信息抓取
categories: [爬虫, html]
description: 链家抓取程序
keywords: golang, 爬虫
---

为什么会有这个想法呢？说起来也可悲，在北京刚毕业就见识到房价在高点又翻倍的惨状，
一个技术人员，不得已还要把部分精力放在研究房产上。一个国家被房地产整的乌烟瘴气，
百业凋敝。
扯远了，不知何时，链家把他们的房价走势下架了，可能不利于他们哄抬房价吧。想要分
析走势没有数据怎么能行。一直想写个爬虫抓取链家数据，一直没有成行。昨天媳妇加班
去了，一个人无事，静下心来花了半小时，核心代码不到60行就搞定了。`要做一件事最困
难的是你下决心去做的时候`，当你下定决心之后只需风雨兼程。

## HTML DOM

想要抓取网页信息，肯定离不开对html分析，查看链家二手房页面，还好都是html元素，
不是通过js加载的。我是个服务端，对前端css、js等研究不是很深入，有待去push自己一
把。HTML DOM 定义了访问和操作html文档的标准做法，将html文档表达为树结构。如：
<div align="center">
<img src="/images/blog/htmltree.gif" alt=""/><br />
（示例图）
</div>
HTML DOM 定义了所有HTML元素的*对象*和-属性-，以及访问它们的*方法*。换言之，**HTML
DOM 是关于如何获取，修改，添加或删除HTML的标准**。
* HTML DOM 中所有事物都是节点。
** 整个文档是文档节点
** 每个 HTML 元素是元素节点
** 元素内的文本是文本节点
** 每个 HTML 属性是属性节点
** 注释是注释节点

节点树中节点是由层级关系的，父、子、同胞等。

* 方法是我们可以在节点上执行的动作。
* 属性是该节点不同维度的值。
* 对DOM节点访问，可以通过tag、class等。
* HTML DOM 允许 js 对 HTML 事件作出反应。

## jQuery selector

通过 [jQuery selector](http://api.jquery.com/category/selectors/) 可以准确的选择
并操作我们所需要的html节点。jQuery selector 是借鉴的 [css selector](http://www.ruanyifeng.com/blog/2009/03/css_selectors.html). 
|选择器|含义|
|------|----|
|*|通用选择器，匹配所有元素|
|E|标签选择器，匹配所有使用E标签的元素|
|.info|class选择器，匹配所有class属性中包含info的元素|
|#foot|id选择器，匹配所有id属性等于foot的元素|

## 开工

做好那么多铺垫，可以开工。
1，分析链家二手房网页源码，经过查看源码，发现房屋信息在class="clear"的li节点中。
<div align="center">
<img src="/images/blog/lianjia.png" alt=""/><br />
（链家源码）
</div>
通过 selector 语法可以很轻松的获取所需元素的信息。li.clear
2，golang html 解析库推荐：`github.com/PuerkitoBio/goquery`，一个语言欢迎程度，
跟它的库丰富程度正相关。github上一大堆优秀的人无偿的编写了丰富的golang库，确实
要感谢伟大的开源思想，让现在科技那么进步，编程那么简单。另一方面，这也是以牺牲
了我们编程人员的劳动所得，很多优秀的程序员，境界高，技术牛，却没有得到应有的待遇。

讲到这里基本也差不多了， [项目地址](https://github.com/guyannanfei25/lianjia_spider)
> Usage of grab_ershou:
>   -n int
>         grab max pages (default 100)
>   -p string
>         data store path (default "/tmp/")
里面只有二进制文件，golang源码不超过60行，感兴趣的可以试下。
