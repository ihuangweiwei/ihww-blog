title: JavaScript高级程序设计-读书笔记（中）
date: 2014-12-17 16:30:14
tags:
    - javascript
---

##8 BOM

###8.1  window对象

BOM的核心对象时window，它表示浏览器的一个实例。在浏览器中，window对象既是通过JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。

####8.1.1   全局作用域

var语句添加window属性有一个名为[[Configurable]]的特性，这个值被设置为false。以及

    var abc='1';
    dd='2';
    delete abc;
    delete dd;
    alert(abc);//抛出错误 abc is not defined
    alert(dd);//2

但我们可以通过查询window对象，知道某个声明变量是否存在：

    //这里会抛出错误，因为oldValue未定义
    var newValue=oldValue;
    //这里不会抛出错误，因为这是一次属性查询
    //newValue的值是undefined
    var newValue=window.oldValue;

<!-- more -->
####8.1.2   窗口关系及框架

如果页面中包含框架，则每个框架都拥有自己的window对象，并且保存在frames集合中（可以通过数值索引或者框架名称来访问相应window对象）

![图8-1](/images/201412/javascript_8_1.png)

top对象始终指向最高（最外）层的框架，也就是浏览器窗口。使用它可以确保一个框架中正确地访问另一个框架

####8.1.3   窗口位置

IE、Safari、Opera和Chrome都提供了screenLeft（窗口相对于屏幕左边的位置）和screenTop（上边的位置）属性。Firefox则在screenX和screenY属性中提供。

    //将窗口移动到屏幕左上角（x,y坐标）
    window.moveTo(0.0);

    //moveBy()接收的是在水平和垂直方向上移动的像素值
    window.moveBy(0,100)

这两个方法可能会被浏览器禁用（Opera和IE7（及更高版本），默认禁用）。

####8.1.4   窗口大小

IE9+、Firefox、Safari、Opera和Chrome均为此提供了4个属性：innerWidth、innerHeight、outerWidth和outerHeight。注意不同浏览器返回的值可能是包含窗口本身尺寸。

IE、Firefox、Safari、Opera和Chrome中，`document.documentElement.clientWidth`和`document.documentElement.clientHeight`中保存了页面视图的信息。

使用resizeTo()和resizeBy()方法可以调整浏览器窗口的大小。

####8.1.5   导航和打开窗口

使用`window.open()`方法既可以导航到一个特定的URL，也可以打开一个新的浏览器窗口。**需要了解其参数**

####8.1.6   间歇调用和超时调用

超时调用

    //推荐的调用方式，设置超时调用
    var timeoutId=setTime(function(){
        alert("hello world");
    },1000);
    //取消
    clearTimeout(timeoutId);

间歇调用

    setInterval()
    clearInterval()

###8.2   location对象
|属性名|例子|说明|
| ------------- |:-------------:|:-------------:|
|hash|#contents|返回URL中的hash（#号后跟的字符）|
|host|blog.ihww.cn:8080|返回服务器名称和端口号|
|hostname|blog.ihww.cn|不带端口号的服务器名称|
|href|http://blog.ihww.cn|返回当前加载页面的完整URL|
|pathname|/query/|返回URL中的目录和文件名|
|port|8080|端口号|
|protocol|http|协议|
|search|?q=javascript|返回URL的查询字符串|

####8.2.1   查询字符串参数

####8.2.2   位置操作

+   `location.assign("http://blog.ihww.cn")`立即打开新URL并在浏览器的历史记录中生成一条记录。如果将location.href或window.location设置为一个URL，也会调用该方法。
+   通过修改location的属性，也可以改变当前加载的页面。

###8.3  navigator对象

已经成为识别客户端浏览器的事实标准。

//TODO 属性

####8.3.1   检测插件

对于非IE浏览器，可以使用plugins数组来达到这个目的。

+   name：插件的名字
+   description：插件的描述
+   filename：插件的文件名
+   length：插件会处理的MIME类型数量

####8.3.2   注册处理程序

`registerContentHandler()`和`registerProtocolHandler()`方法

###8.4  screen对象

screen对象基本上只是用来表明客户端的能力，包括浏览器窗口外部的显示器信息

###8.5  history对象

history对象保存着用户上网的历史记录，从窗口被打开的那一刻算起。`length`属性保存历史记录的数量

    history.go()
    //后退一页
    history.back()
    //前进一页
    history.forward()

##9 客户端检测

略

##10    DOM

###10.1 节点层次

####10.1.1  Node类型

DOM1级定义了一个Node接口，该接口将有DOM中的所有节点类型实现。每个节点都有一个nodeType属性，用于表明节点的类型。节点类型由在Node类型中定义的下列12个数值常量来表示：

+   Node.ELEMENT_NODE(1)
+   Node.ATTRIBUTE_NODE(2)
+   Node.TEXT_NODE(3)
+   Node.CDATA_SECTION_NODE(4)
+   Node.ENTITY_REFERENCE_NODE(5)
+   Node.ENTITY_NODE(6)
+   Node.PROCESSING_INSTRUCTION_NODE(7)
+   Node.COMMENT_NODE(8)
+   Node.DOCUMENT_NODE(9)
+   Node.DOCUMENT_TYPE_NODE(10)
+   Node.DOCUMENT_FRAGMENT_NODE(11)
+   Node.NOTATION_NODE(12)

**1.nodeName和nodeValue属性**

    if(someNode.nodeType==1){
        value=someNode.nodeName;    //nodeName的值是元素的标签名
    }

**2.节点关系**

每个节点都有一个childNodes属性，其中保存着一个NodeList对象（数组，用于保存一组有序的节点）

    var firstChild=someNode.childNodes[0];
    var secondChild=someNode.childNodes.item(1);
    var count=someNode.childNodes.length;

**3.操作节点**

`appendChild()`用于向childNodes列表的末尾添加一个节点。如果需要放在childNodes列表指定位置需要用`insertBefore()`（接受两个参数：要插入的节点和做为参考的节点）

`replaceChild()`:要插入的节点和要替换的节点
`removeChild()`:要移除的节点

**4.其他方法**

+   `cloneNode()`，用于创建调用这个方法的节点的一个完全相同的副本。接受一个布尔值，表示是否执行深复制。
+   `normalize()`，唯一的作用就是处理文档数中的文本节点

####10.1.2  Document类型

在浏览器中，document对象时HTMLDocument（继承自Document类型）的一个实例，表示整个HTML页面。Document节点具体有下列特征：

+   nodeType的值为9
+   nodeName的值为“#document”；
+   nodeValue的值为null
+   parentNode的值为null
+   ownerDocument的值为null
+   其子节点肯是一个DocumentType（最多一个）、Element（最多一个）、ProcessingInstruction或Comment

**1.文档的子节点**

略

**2.文档信息**

**3.查找元素**

getElementById()和getElementsByTagName()

**4.特殊集合**

这些集合都是HTMLCollections对象

+   document.anchors，包含文档中所有带name特性的<a>元素
+   document.applets
+   document.forms，包含文档中所有的<form>元素
+   document.Images
+   document.links

**5.DOM一致性检测**

由于DOM分为多个级别，也包含多个部分，因此检测浏览器实现DOM的那些部分就十分必要了。`document.implementation`属性

    var hasXmlDom=document.implementation.hasFeature("XML","1.0");


**6.文档写入**

write()、writeln()、open()和close()

####10.1.3  Element类型

Element提供了对元素标签名、子节点及特性的访问。具有以下特征：

+   nodeType的值为1
+   nodeName的值为元素的标签名
+   nodeValue的值为null
+   parentNode可能是Document或Element
+   其子节点可能是Element、Text、Comment、ProcessingInstruction、CDATASection、EntityReference

要访问元素的标签名，可以使用nodeName属性，也可以使用tagName属性。

    element.tagName.toLowerCase() == 'div' //html标签始终全部大写，XML则原样输出

**1.HTML元素**

每个HTML元素中都存在下列标准特性。

+   id，元素在文档中的唯一标示符
+   title，有关元素的附加说明信息，一般通过工具提示条显示出来
+   lang，元素内容的语言代码，很少使用
+   dir，语言的方向，值为“ltr”，“rtl”
+   className，与元素的class特性对应，即为元素指定的CSS类。

**2.取得特性**

`getAttribute()`,`setAttribute()`,`removeAttribute()`

**3.设置特性**

`setAttribute()`：方法接受两个参数：要设置的特性名和值。

**4.attributes属性**

Element类型是使用attributes属性的唯一一个DOM节点类型。attributes属性中包含一个NamedNodeMap，与NodeList类似，动态集合。

NamedNodeMap对象拥有下列方法：

+   getNamedItem(name)：返回nodeName属性等于name的节点
+   removeNamedItem(name)：从列表中移除nodeName属性等于name的节点
+   setNamedItem(node)：向列表中添加节点，以节点的nodeName属性为索引
+   item(pos)：返回位于数字pos位置处的节点

**5.创建元素**

    var div=document.createElement('div')
    div.di="myNewDiv"
    div.className="box"
    document.body.appendChild(div)

在IE中可以这样

    var div=document.createElement("<div id='myNewDiv' class='box'>");

**6.元素的子节点**

`childNodes`属性中包含了它的所有子节点

####10.1.4  Text类型

文本节点由Text类型表示，包含的是可以照字面解释的纯文本内容。Text节点具有以下特性：

+   nodeType的值为3
+   nodeName的值为“#text”
+   nodeValue的值为节点所包含的文本
+   parentNode是一个Element
+   不支持子节点
+   appendData(text)：将text添加到节点的末尾
+   deleteData(offset,count)：从offset指定的位置开始删除count个字符
+   insertData(offset,count,text)：在offset指定位置插入text
+   replaceData(offset,count,text)：用text替换从offset指定的位置开始到offset+count为止处的文本
+   splitText(offset)：从offset指定的位置将当期文本节点分成两个文本节点
+   substringData(offset,count)：提取从offset指定位置开始到offset+count为止处的字符串

*没有内容，也就没有文本节点*

    createTextNode("<strong>hello</strong>")

####10.1.5  Comment类型

Comment节点具有下列特性：

+   nodeType的值为8
+   nodeName的值为“#comment”
+   nodeValue的值是注释的内容
+   parentNode可能是Document或Element
+   不支持子节点

    <div id='myDiv'><!--A comment --></div>
    document.createComment("A comment");

####10.1.6  CDATASection类型

只针对基于XML的文档，表示的是CDATA区域。继承自Text类型，CDATASection节点具有下列特性：

+   nodeType的值为4
+   nodeName的值为“#cdata-section”
+   nodeValue的值是CDATA区域中的内容
+   parentNode可能是Document或Element
+   不支持子节点

    <div id='myDiv'>![CDATA[This is some content]]</div>
    document.createCDataSection();

####10.1.7  DocumentType类型

DocumentType类型在Web浏览器中并不常用，仅有Firefox、Safari和Opera支持它。它具有下列特性

+   nodeType的值为10
+   nodeName的值为doctype的名称
+   nodeValue的值为null
+   parentNode是Document
+   不支持子节点

####10.1.8  DocumentFragment类型

略

####10.1.9  Attr类型

元素的特性在DOM中以Attr类型来表示。在所有浏览器中，都可以访问Attr类型的构造函数和原型。从技术角度讲，特性就存在于元素的attributes属性中的节点。特性节点具有以下特征：

+   nodeType的值为2
+   nodeName的值是特性的名称
+   nodeValue的值是特性的值
+   parentNode的值为null
+   在HTML中不支持子节点
+   在XML中子节点可以使Text或EntityReference

###10.2 DOM操作技术

####10.2.1  动态脚本

    function loadScript(url){
        var script=document.createElement("script");
        script.type="text/javascript";
        script.src=url;
        document.body.appendChild(script);
    }

*无法知道脚本是否加载完成*

####10.2.2  动态样式

    function loadStyles(url){
        var link=document.createElement("link");
        link.rel="stylesheet";
        link.type="text/css";
        link.href=url;
        var head=document.getElementByTagName("head")[0];
        head.appendChild(link);
    }

####10.2.3  操作表格

了解HTMLDOM为<table>、<tbody>和<tr>元素添加的一些熟悉和方法

####10.2.4  使用NodeList

理解NodeList及其“近亲”NamedNodeMap和HTMLCollection，是从整体上透彻理解DOM的关键所在。都是“动态的”，每当文档结构发生变化，他们都会更新。


##13    事件

事件，就是文档或浏览器窗口中发生的一些特定的交互瞬间。可以使用**侦听器**来预订事件，以便事件发生时执行相应的代码。

###13.1 事件流

**事件流**描述的是从页面中接收事件的顺序。

####13.1.1  事件冒泡

IE的事件流叫做事件冒泡，即事件开始时有最具体的元素接收，然后逐级向上传播到较为不具体的节点（文档）。

####13.1.2  事件捕获

事件捕获的思想是不太具体的节点应该更早接收到事件，而最具体的节点应该最后接收到事件。事件捕获的用意在于事件到达预订目标之前捕获它。

####13.1.3  DOM事件流

“DOM2级事件”规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段和事件冒泡阶段。

###13.2 事件处理程序

事件处理程序的名字以“on”开头，因此click事件处理程序就是onclick。

####13.2.1  HTML事件处理程序

某个元素支持的每种事件，都可以使用一个与相应事件处理程序同名的HTML特性来指定。

    <input type="button" value="Click Me" onclick="alert(this.value)">

####13.2.2  DOM0级事件处理程序

    var btn=document.getElementById("myBtn");
    btn.onclick=function(){
        alert("clicked");
    }

####13.2.3  DOM2级事件处理程序

定义了两个方法：`addEventListener()`和`removeEventListener()`

    var btn=document.getElementById("myBtn");
    btn.addEventListener("click",function(){
        alert(this.id);
    },false);

####13.2.4  IE事件处理程序

IE实现了与DOM中类似的两个方法：`attachEvent`和`detachEvent`，都接受相同的两个参数：事件处理程序名称和事件处理程序函数。

    var btn=document.getElementById("myBtn");
    btn.attachEvent("onclick",function(){
        alert("Clicked");
    });

####13.2.5  跨浏览器的事件处理程序

兼容性处理方案

###13.3 事件对象

####13.3.1  DOM中的事件对象

在触发DOM上的某个事件时，会产生一个事件对象event，这个对象中包含着所有与事件相关的信息


####13.3.1  DOM中的事件对象

兼容DOM的浏览器会将一个event对象传入到事件处理程序中。

    var btn=document.getElementById("myBtn");
    btn.onclick=function(event){
        alert(event.type);
    }
    btn.addEventListener("click",function(event){
        alert(event.type);
    },false);

//TODO 了解事件可用的属性和方法

在事件内部this始终等于currentTarget的值

####13.3.2  IE中的事件对象

在访问DOM中的event对象不同，要访问IE中的event对象有几种不同的方式，取决于指定事件处理程序的方法。在DOM0级方法中，event对象作为window对象的一个属性存在。

    var btn=document.getElementById("myBtn");
    btn.onclick=function(){
        var event=window.event;
        alert(event.type);
    }

####13.3.3  跨浏览器的事件对象

IE中event对象的全部信息和方法DOM对象中都有，只不过实现方式不同。有谈兼容性


##13.4  事件类型

Web浏览器中可能发生的事件有很多类型。

+   UI事件
    load、unload、resize、scroll

+   焦点事件
    blur、DOMFocusIn（focusin）、DOMFocusOut（focusout）、focus
+   鼠标事件
    click、dblclick、mousedown、mouseenter、mouseleave、mousemove、mouseout、mouseover、mouseup
+   滚轮事件
+   文本事件
+   键盘事件（熟悉键码）
    keydown、keypress、keyup
+   合成事件
+   变动事件
+   变动名称事件

####13.4.7  HTML5事件

####13.4.8  设备事件

###13.5 内存和性能

###13.6 模拟事件


