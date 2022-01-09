+++
title = "CSS"
date = "2021-12-28T11:00:22+08:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["", ""]
description = "CSS知识点的梳理和理解"
showFullContent = false
readingTime = false
+++

## 前言

记录CSS的常用样式、难点以及理解

## 基础知识

- CSS: cascading style sheets 层叠样式表。作为“样式”存在
- `selector { property: value; ...; /* commnet */ }`
- 通过选择器找到需要的元素，然后为其设置样式
- 引入方式
  - 内嵌：在`style`标签中编写
  - 外联：通过`link`标签的`href`属性引入，注意`rel`属性设置为`stylesheets`表示两者关系为样式表。常用，模块化利于代码复用
  - 行内：写在标签的`style`属性中，通常通过JS动态设置，优先级仅次于`!important`。
- 颜色值: 常用的是`rgba()`和十六进制
- 标准流：网页默认的排版方式
- CSS推荐书写顺序：布局 盒模型 背景 文本 字体

## 三大特性

- 继承：font 和 text 系列的属性会继承。继承的优先级小于浏览器的默认样式
- 层叠：同一元素的相同属性会覆盖；不同属性会同时生效
- 优先级：优先级高的生效，如果优先级相同则后被执行的样式生效

元素 伪元素(001) < Class 伪类 属性(010) < Id(100) < 行内 < !important

可以简单概括为：选择器的范围与优先级成反比

Tips: 不推荐使用`!important`，不便于开发调试，而且会影响JS修改的行内样式，如果必须提高优先级，可以重复选择器，如`#id#id {}`

## 选择器

- 元素选择器 `tagname`
- 类选择器 `.classname`。使用思路：定义 -> 调用
- ID选择器 `#id`。ID通常为JS服务，便于唯一选择某个元素，不可为了样式给元素设置ID
- 通配符选择器 `*`。会遍历文档中所有标签，性能较低，一般不用，或用来消除浏览器给标签添加的默认样式
- 伪元素选择器 `::before` `::after` 创建一个伪元素
- 结构伪类选择器 `:FOO-child` `:FOO-of-type`。FOO可以是`first` `last` `nth`，为`nth`时，可以写表达式，n为自然数
- 伪类选择器 `:hover`
- 复合选择器： ` ` `>` `,` `~` `+`
- 属性选择器 `[attribute="value"]` 含有正则匹配

## Box

content + padding + border + margin

实际占用空间包含这四部分，而可视区域不包含margin

- 嵌套块级元素，上外边距会塌陷
- 相邻块级元素之间的外边距为两者的最大值
- `inline`元素垂直方向的内外边距都无效

块级元素居中：`margin: 0 auto;`

内外边距的值始终遵循“上右下左”的顺序

上左外边距会移动元素自身，而右下外边距会挤开其他元素

页面由各种各样的“盒子”组成

## Float

早期用于图文环绕，现在多用于布局

浮动流：排列方式和标准流的行内块一样，浮动的元素也都转化为行内块元素的特点

备注：用`inline-block`布局时，HTML代码中标签换行会在页面中生成一个空格间隙，影响布局

缺点：
1. 浮动的元素为半脱离标准流，不占用标准流的空间，不会撑开父元素，会覆盖“下方”元素，但无法覆盖其中的文本
2. 无法用`text-align: center;`和`margin: 0 auto;`居中了

清除浮动的影响：
1. 直接给父元素设置`height`
2. 利用`::after`在父元素最后生成伪元素，应用`clear: both;`。
3. BFC: `overflow: hidden;`

通常使用以下方式清除浮动的同时，也可以解决嵌套块级元素上外边距塌陷的问题

```css
.clearfix::after, .clearfix::before {
  content: "";
  display: table;
}
.clearfix::after {
  clear: both;
}
```


## 常用样式分组

### 1. 字体

`font: style weight size/hight family;` 不可更换顺序，`size`和`family`不能省略

### 2. 文本

`text-indent: 2em;` `text-align: center;` `text-decoration: none;`

### 3. 背景

`background: color image repeat attachment position;`

无固定顺序，但推荐这样写
