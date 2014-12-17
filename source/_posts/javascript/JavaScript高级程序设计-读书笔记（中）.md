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

