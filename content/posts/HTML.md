+++
title = "HTML"
date = "2021-12-27T19:08:19+08:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["", ""]
keywords = ["", ""]
description = "一些HTML的理解和注意点"
showFullContent = false
+++

## 理解

超文本标记语言，作为“结构”存在。其作用为语义化页面内容

默认的呈现样式由浏览器添加，为了保证页面的可读性，不需要理会，实际开发时会进行重置（不带任何样式）

标签和属性都由对应功能的英文单词缩写而来，可做了解，加深理解和记忆

属性是当前标签天生具备的能力

编写页面思路：在确定了页面内容后，为每个部分添加适合的标签。标签之间可以并列和嵌套，形成DOM树

## 需要注意的标签及属性

### 0. 路径问题

- 相对路径
- 绝对路径
- 锚

### 1. img

只定义宽或高时，另一个属性自适应

### 2. a

其`href`属性可以为锚点，也可以是JS代码段。值为`#`时，跳转到当前页面首屏；为`javascript:void(0);`时，无任何行为。

### 3. ul dl ol

定义某种列表，只能嵌套自己的列表项标签

### 4. table

- 合并单元格：`rowspan`, `colspan`
- 合并边框：`border-collapse`
- 表格标题标签：`caption`
- 表格结构化标签：`thead` `tbody` `tfoot`

### 5. form

自身的属性：
- `action`: 处理当前表单的URL
- `enctype`: 加密类型
- `method`: 提交表单的HTTP方法

表单相关的标签有：
- `input`: `type`的各种值，`radio`必须`name`相同生效，`file`可以添加`multiple`多选，`submit`等按钮需要在`form`中生效
- `textarea`: 常用`rows`和`cols`属性限定范围，CSS的`resize: none;`禁止缩放
- `select(name)>option(value)`: 默认选中为`selected`而非`checked`，`name`在`select`上，`value`在`option`上
- `label`: 两种用法：嵌套和`for id`，后者更灵活，避免嵌套

相关属性：
- `name` `value`: `name`作为上传数据的key，`value`为key对应的值
- `placeholder`: 可以用CSS的`::placeholder`伪元素定义样式

### 6. 布局（容器）

无语义容器标签：`div` `span`
语义化容器标签：`header` `footer` `article` `section` `aside` `nav` 等等，常用语移动端（兼容性好）

### 7. 字符实体

在页面中直接显示特殊字符，需要转化为字符实体，否则会被浏览器处理或解析。格式为`&FOO;`，如空格为`&nbsp;`
