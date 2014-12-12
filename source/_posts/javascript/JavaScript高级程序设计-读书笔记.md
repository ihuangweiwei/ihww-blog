title: JavaScript高级程序设计-读书笔记
date: 2014-12-12 16:30:13
tags:
    - javascript
---

##1 JavaScript简介

###1.2  JavaScript实现

+   核心（ECMAScript）
+   文档对象模型（DOM）
+   浏览器对象模型（BOM）

####1.2.1   ECMAScript（提供核心语言功能）

规定了该门语言的下列组成部分：语法、类型、语句、关键字、保留字、操作符、对象

####1.2.2   文档对象模型（DOM）（提供访问和操作网页内容的方法和接口）

是针对XML但经过扩展用于HTML的应用程序编程接口。

2.DOM级别

DOM1级有两个模块组成：DOM核心 和 DOM HTML.（思考两个模块干嘛的）
DOM2级的模块：DOM试图，DOM事件（事件和事件处理的接口），DOM样式（基于CSS为元素应用样式的接口），DOM遍历和范围（遍历和操作文档树的接口）

3.其他DOM标准

SVG、MathML、SMIL

####1.2.3   浏览器对象模型（BOM）（提供与浏览器交互的方法和接口）

一些扩展：

+   navigator（浏览器信息）
+   location（页面详细信息）
+   screen（显示器分辨率详细信息）
+   对cookies的支持
+   如XMLHttpRequest、ActiveXObject 自定义对象

##2 在HTML中使用JavaScript

####2.1.2   延迟脚本

    <script type="text/javascript" defer="defer" src="example1.js"></script>

告诉浏览器立即下载，但延迟执行（有浏览器忽略该属性）

####2.1.3   异步脚本

    <script type="text/javascript" async src="example1.js"></script>


###2.4  <noscript>元素

符合以下情况，元素内内容会显示

+   浏览器不支持脚本
+   脚本被禁用

##3 基本概念




