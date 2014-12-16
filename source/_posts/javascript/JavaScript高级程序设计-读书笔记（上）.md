title: JavaScript高级程序设计-读书笔记（上）
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

<!-- more -->

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

###3.1  语法

####3.1.1   区分大小写

####3.1.2   标示符

标示符指的是变量、函数、属性的名字，函数的参数。

+   第一个字符必须是字母、_或$
+   其它字符可以是字母、下划线、美元符号或数字

####3.1.4   严格模式

    "use strict"

是一个编译指示（pragma），告诉支持的JavaScript引擎切换到严格模式。`函数内部也可以使用`

###3.4  数据类型

ECMAScript中有5种简单数据类型：Undefined、Null、Boolean、Number和String。一种复杂数据类型：Object（本质上是由一组无序的明值对组成）。

####3.4.1   typeof操作符

返回某个字符串

####3.4.2   Undefined类型

Undefined类型只有**一个值**，即特殊的**undefined**

    undefined==undefined    返回true

####3.4.3   Null类型

Null类型是第二个只有一个值的数据类型，是null。null值表示一个空对象指针，所以typeof检测null值会返回"object"的原因。

undefined值是派生自null值的

    null==undefined 返回true

####3.4.4    Boolean类型

各种数据类型及其对应的转换规则

####3.4.5   Number类型

2.  数值范围
    Number.MAX_VALUE
    Number.MIN_VALUE

超出数值范围的值，会自动换换成特殊的Infinity值（负数转换为-Infinity(负无穷)）

3.  NaN
NaN，即非数值，是一个特殊的数值，这个数值用于表示一个本来要返回数值的操作数未返回数值的情况。

两个特点：
+   任何涉及NaN的操作都返回NaN
+   NaN与任何值都不相等，包括NaN本身

4.  数值转换
略

####3.4.6   String类型

用于表示由0或多个16位Unicode字符组成的字符串序列，即字符串。

2.  字符串的特点

ECMAScript中的字符串是不可变的。

####3.4.7   Object类型

ECMAScript中的对象其实就是一组数据和功能的集合。

###3.5  操作符

####3.5.7   相等操作符

**相等(==)和不相等（!=）**：想转换再比较

**全等(===)和不全等(!==)**：仅比较而不转换

###3.6  语句

if  do-while    while   for for-in  label   break和continue  with    switch

###3.7  函数

####3.7.1   理解参数

ECMAScript函数不介意传递进来多少个参数，也不在乎传进来参数是什么类型。在函数体内可以通过`arguments`对象来访问这个参数数组。

####3.7.2   没有重载

ECMAScript函数没有签名，因为其参数是由包含0或多个值的数组来表示的。

##4 变量、作用域和内存问题

###4.1  基本类型和引用类型的值

JavaScript不允许直接访问内存中的位置，也就是说不能直接操作对象的内存空间。

####4.1.1   动态的属性

引用类型的值，我们可以为其添加属性和方法，也可以改变和删除其属性和方法。（基本类型不行）

####4.1.2   复制变量值

基本类型：如果一个变量想另一个变量复制基本类型的值，会在变量对象上创建一个新值。
引用类型：复制结束后，两个变量实际上将引用同一个对象。

####4.1.3   传递参数

ECMAScript中所有函数的参数都是按值传递的。（把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样）。

####4.1.4   检测类型

`typeof`在检测引用类型的值时，这个操作符的用处不大。所以有了`instanceof`，所有引用类型的值都是Object的实例。

###4.2  执行环境及作用域

每个执行环境都是一个与之关联的**变量对象**，环境中定义的所有变量和函数都保存在这个对象中。（编写的代码无法访问这个对象，但解析器在处理数据时会在后台使用它）

全局执行环境是最外围的一个执行环境。

每个函数都有自己的**执行环境**。当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。而在函数执行之后，栈将其环境弹出，把控制权返回给之前的执行环境。

当代码在一个环境中执行时，会创建变量对象的一个**作用域链**。（保证对执行环境有权访问的所有变量和函数的有序访问）

**需要补充**

//TODO

###4.3  垃圾收集

####4.3.1   标记清除

####4.3.2   引用计数

##5 引用类型

在ECMAScript中，**引用类型**是一种数据结构，用于将数据和功能组织在一起，它也常被称为**类**，不妥当（不具备传统面向对象语言所支持的类和接口等基本结构）。也称为**对象定义**，因为它们描述的是一类对象所具有的属性和方法。

对象时某个特定引用类型的**实例**。新对象是使用new操作符后跟一个**构造函数**来创建的。

###5.1  Object类型

创建方式：

    var person=new Object();
    person.name="ihww";

or(**对象字面量**)

    var person={
        name:"ihww"
    }

###5.2  Array类型

ECMAScript数组的每一项可以保存任何类型的数据，同时数组的大小可以动态调整(如果传递的是数值，则数组的大小固定)。

创建方式：

    var names=new Array("Greg")
    var names=Array("Greg")
    var names=[]

####5.2.1   检测数组

instanceof操作符的问题在于，它假定只有一个全局执行环境（如果网页中包含多个框架，那实际上就存在两个以上不同的全局执行环境，从而存在两个以上不同版本的Array构造函数）。

    Array。isArray(value)

####5.2.2   转换方法

toLocaleString()、toString()和valueOf()方法，默认情况一逗号分隔的字符串的形式返回数组想。而是用join()方法，则可以是用不同的分隔符来构建这个字符串。

`如果数组中某一项的值是null或者undefined，则用空字符串代替`

####5.2.3   栈方法

push()方法可以接收任意数量的参数，把它们逐个添加到数组末尾，并返回修改后数组的长度。
pop()方法则从数组末尾移除最后一项，减少数组的length值，然后返回移除的项。

####5.2.4   队列方法

shift()方法，取出第一项。
unshift()方法，它能在数组前端添加任意个项并返回新数组的长度。

####5.2.5   重排序方法

reverse()反转数组项的顺序。
sort()方法可以自定义排序方法。

####5.2.6   操作方法

concat()、slice()、splice()

####5.2.7   位置方法

indexOf()和lastIndexOf()

####5.2.8    迭代方法

略

####5.2.9   归并方法

reduce()和reduceRight()

###5.3  Date类型

日期格式化方法？
日期/时间组件方法？

###5.4  RegExp类型

    var expression= / pattern / flags;

g:表示全局模式，即模式将被应用于所有字符串
i:表示不区分大小写模式
m:表示多行模式


####5.4.1   RegExp实例属性

+   global：布尔值，表示是否设置了g标志。
+   ignoreCase：布尔值，表示是否设置了i标志
+   lastIndex：整数，表示开始搜索下一个匹配项的字符位置，从0算起
+   multiline：布尔值，表示是否设置了m标志
+   source：正则表达式的字符串表示，按照字面量形式而非传入构造函数的字符串模式返回

####5.4.2   RegExp实例方法

    var text="mom and dad and baby";
    var pattern=/mom( and dad( and baby)?)?/gi;
    var matches=pattern.exec(text);
    alert(matches.index);   //0
    alert(matches.input);   //mom and dad and baby
    alert(matches[0]);      //mom and dad and baby
    alert(matches[1]);      // and dad and baby
    alert(matches[2]);      // and baby

####5.4.3   RegExp构造函数属性

####5.4.4   模式的局限性

###5.5  Function类型

每个函数都是Function类型的实例，而且都与其他引用类型一样具有属性和方法。

    function sum(num1,num2){
        return num1+num2;
    }
    等同于
    var sum=function(num1,num2){
        return num1+num2;
    }
    等同于
    var sum=new Function("num1","num2","return num1+num2"); //不推荐，会导致解析两次代码（第一次解析常规ECMAScript代码，第二次解析传入构造函数中的字符串，影响性能）

    函数是对象，函数名是指针

####5.5.1   没有重载

将函数名想象为指针

####5.5.2   函数声明与函数表达式

解析器率先读取函数声明，并使其在执行任何代码之前可用

####5.5.3   作为值的函数

####5.5.4   函数内部属性

两个特殊的对象：arguments和this。
argument:有一个名叫callee的属性，该属性石一个指针，指向拥有这个arguments对象的函数。

    function factorial(num){
        if(num<=1){
            return 1;
        }else{
            return num*factorial(num-1)
        }
    }
    等同于
    function factorial(num){
        if(num<=1){
            return 1;
        }else{
            return num*arguments.callee(num-1)//严格模式下，运行出错
        }
    }

this：this引用的是函数据以执行的环境对象

####5.5.5   函数属性和方法

每个函数都包含两个属性：length和prototype。
length：表示函数希望接收的命名参数的格式（方法参数个数）
prototype：对于ECMAScript中的引用类型而言，prototype是保存它们所有实例方法的真正所在。prototype属性是不可枚举的，因此使用for-in无法发现。

每个函数都包含两个非继承而来的方法：
apply():接收两个参数：一个是在其中运行函数的作用域，另一个是参数数组（也可以是Array的实例）
call():和apply作用相同，区别仅在于接收参数的方式不同，第一个参数this没有变化，变化的是其余参数都直接传递给函数

    function sum(num1,num2){
        return num1+num2;
    }
    function callSum1(num1,num2){
        return sum.apply(this,arguments);
        //return sum.apply(this,[num1,num2])
    }
    function callSum2(num1,num2){
        return sum.call(this,num1,num2);
    }

bind():创建一个函数的实例，其this值会被绑定到传给bind()函数的值

    window.color="red";
    var o={color:"blue"}

    function sayColor(){
        alert(this.color);
    }
    var objectSayColor=sayColor.bind(o);
    objectSayColor();

###5.6  基本包装类型

3个特殊的引用类型：Boolean、Number和String。（基本类型值不是对象，逻辑上它们不应该有方法）

###5.7  单体内置对象

内置对象：由ECMAScript实现提供的、不依赖于宿主环境的对象，这些对象在ECMAScript程序执行之前就已经存在了。

####5.7.1   Global对象

isNan()、isFinite()、parseInt()以及parseFloat()，都是Global对象的方法。

1.encodeURI()、encodeURIComponent()编解码url

2.eval()方法

    eval("alert('hi')");

3.Global对象的属性

?

4.window对象

Web浏览器都是将这个全局对象作为window对象的一部分加以实现的。

####5.7.2   Math对象

略

##6 面向对象的程序设计

对象：无序属性的集合，其属性可以包含基本值、对象或者函数（无非就是一组名值对，其中值可以是数据或函数）。每个对象都是基于一个引用类型创建的。


###6.1  理解对象

####6.1.1   属性类型

ECMAScript中有两种属性：数据属性和访问器属性。

**1.数据属性**
+   [[Configurable]]：表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。
+   [[Enumerable]]：表示能否通过for-in循环返回属性。
+   [[Writable]]：表示能否修改属性的值。
+   [[Value]]：包含这个属性的数据值。

要修改属性默认的特性，必须使用`Object.defineProperty()`方法

**2.访问器属性**
+   [[Configurable]]：表示能否通过delete删除属性从而重新定义属性，能否修改属性的特性，或者能否把属性修改为访问器属性。
+   [[Enumerable]]：表示能否通过for-in循环返回属性。
+   [[Get]]：在读取属性时调用的函数。
+   [[Value]]：在写入属性时调用的函数。

####6.1.2   定义多个属性

`Object.defineProperties()`方法

####6.1.3   读取属性的特性

`Object.getOwnPropertyDescriptor()`方法

###6.2  创建对象

####6.2.1   工厂模式

工厂模式虽然解决了创建多个相似对象的问题，但却没有解决对象识别的问题（即怎么知道一个对象的类型）

####6.2.2   构造函数模式

    function Person(name,age){
        this.name=name;
        this.age=age;
        sayName=function(){
            alert(this.name);
        }
    }
    var person1=new Person("ihww",27);

    person1.constructor == Person //true

**以这种方式定义的构造函数是定义在Global对象中的**

**1.将构造函数当做函数**

    //当做构造函数使用
    var person1=new Person("ihww",27);
    person.sayName(); //ihww

    //作为普通函数调用
    Person("abc",27);
    window.sayName();//abc

    //在另一个对象的作用域中调用
    var o=new Object();
    Person.call(o,'bbb',27)
    o.sayName() //bbb

**2.构造函数的问题**

每个方法都要在每个实例上重新创建一遍。

        function Person(name,age){
            this.name=name;
            this.age=age;
            sayName=new Function("alert(this.name)");//与声明函数在逻辑上是等价的
        }

从这个角度上看构造函数，每个Person实例都包含一个不同的Function实例的本质。（这种方式创建函数，会导致不同的作用域链和标识符解析，但创建Function新实例的机制仍然相同）。

    alert(person1.sayName == person2.sayName) //false   不同实例上的同名函数是不相等的

把sayName提取到全局域，导致一下问题：

+   全局域中定义的函数实际只是被某个对象调用，名不副实
+   如果对象需要定义很多方法

解决方法**原型模式**

####6.2.3   原型模式

我们创建的每个函数都有一个prototype(原型)属性，该属性是一个指针，指向一个对象（该对象的用途是包含可以由特定类型的所有实例共享的属性和方法）。

**1.理解原型对象**

所有原型对象都会自动获得一个constructor（构造函数）属性，这个属性包含一个指向prototype属性所在函数的指针。

Person.prototype.constructor指向Person。而通过这个构造函数，我们还可以继续为原型对象添加其他属性和方法。

创建了自定义的构造函数之后，原型对象默认只会取得constructor属性，其他方法都是从Object继承而来的。当调用构造函数创建一个新实例后，该实例的内部将包含一个指针（内部属性），指向构造函数的原型对象（`_proto_`也可以表示为[[Prototype]]）。这个连接存在于实例与构造函数的原型对象之间，不是存在于实例与构造函数之间。

![图6-1](/images/201412/prototype_1.png)

解释：Person.prototype指向原型对象，而Person.prototype.constructor又指回了Person。原型对象中除了包含constructor属性，还有其他属性。Person的实例都包含一个内部属性（_proto_），该属性仅仅指向了Person.prototype;

通过`isPrototypeOf()`方法来确定对象之间是否存在这种关系。

    alert(Person.prototype.isPrototypeOf(person1))；//true

ECMAScript5增加了一个新方法，叫Object.getPrototypeOf(),这个方法返回_proto_的值

    alert(Object.getPrototypeOf(person1) == Person.prototype); //true
    alert(Object.getPrototypeOf(person1).name); //Nicholas

**每当代码读取某个对象的属性时，都会执行一次搜索，目标是具体给定名字的属性。搜索首先从对象实例本身开始，然后原型对象**

**实例对象能访问原型中的值，但不能修改，如果我们在实例中添加一个原型同名属性，则该属性将会屏蔽原型中的那个属性，但可以使用delete操作符完全删除实例属性**

使用`hasOwnProperty()`方法检测一个属性是否存在实例中，还是存在原型中（Object继承来的）。

**2.原型与in操作符**

两种方式使用in操作符：

+   单独使用（通过对象能够访问给定属性时返回true）
+   for-in循环中使用

`hasPrototypeProperty()`检测是否是原型中属性，如果实例覆盖了原型中属性则返回false，取得对象上所有可枚举的实例属性：`Object.keys()`

**3.更简单的原型语法**

    function Person(){
    }
    Person.prototype={
        name:"Nicholas",
        age:27
    }

如果在原型中声明`constructor`属性，可能导致该特性的[[Enumerable]]为true，即可枚举。

**4.原型的动态性**

由于原型中查找值的过程是一次搜索，因此原型对象所做修改立即从实例上反映出来。

**5.原生对象的原型**

    alert(typeof Array.prototype.sort);//function

通过原生对象的原型，可以定义新方法。

**6.原型对象的问题**

引用类型值的属性被实例属性可以修改，导致其它实例值修改。

###6.2.4    组合使用构造函数模式和原型模式

构造函数模式用于定义实例属性，而原型模式用于定义方法和共享属性。

###6.2.5    动态原型模式

可以通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型。

    function Person(name,age){
        this.name=name;
        this.age=age;
        if(typeof this.sayName!="function"){
            Person.prototype.sayName=function(){
                alert(this.name);
            }
        }
    }

*不能使用对象字面量重写原型。如果在已经创建了实例的情况下重写原型，那么就会切断现有实例与新原型之间的联系*

###6.2.6    寄生构造函数模式

寄生（parasitic）构造函数模式，这种模式的基本思想是创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象

    function Person(name, age, job){
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function(){
            alert(this.name);
        };
         return o;
    }
    var friend = new Person(“Nicholas”, 29, “Software Engineer”);
    friend.sayName(); //”Nicholas”

这个模式可以在特殊的情况下用来为对象创建构造函数。如果我们想创建一个具有额外方法的特殊数组。

    function SpecialArray(){
        //create the array
        var values = new Array();
        //add the values
        values.push.apply(values, arguments);
        //assign the method
        values.toPipedString = function(){
            return this.join(“|”);
        };
        //return it
        return values;
    }
    var colors = new SpecialArray(“red”, “blue”, “green”);
    alert(colors.toPipedString()); //”red|blue|green”


###6.2.7    稳妥构造函数模式

指的是没有公共属性，而且其方法也不引用this的对象。稳妥对象最适合在一些安全的环境中（禁用this和new），或者在防止数据被其他应用程序（如Mashup程序）改动时使用。

稳妥构造函数遵循与寄生构造函数类似的模式，但有两点不同：

+   新创建对象的实例方法不引用this
+   不使用new操作符调用构造函数

改造下前面的Person构造函数：

    function Person(name, age, job){
        //create the object to return
        var o = new Object();
        //optional: define private variables/functions here
        //attach methods
        o.sayName = function(){
            alert(name);
        };
        //return the object
        return o;
    }

创建的对象，除了使用sayName()方法之外，没有其它办法访问name的值。

##6.3   继承

许多OO语言都支持两种继承方式：接口继承和实现继承。接口继承只继承方法签名，而实现继承则继承实际的方法。（由于函数没有签名，在ECMAScript中无法实现接口继承，只支持实现继承，实现继承主要是依靠原型链来实现的）。

###6.3.1    原型链

基本思想是利用原型让一个引用类型继承另一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

构造函数、原型和实例的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

    function SuperType(){
        this.property = true;
    }
    SuperType.prototype.getSuperValue = function(){
        return this.property;
    };
    function SubType(){
        this.subproperty = false;
    }
    //继承了SuperType
    SubType.prototype = new SuperType();
    SubType.prototype.getSubValue = function (){
        return this.subproperty;
    };
    var instance = new SubType();
    alert(instance.getSuperValue()); //true

![6-4](/images/201412/prototype_2.png)

***1.别忘了默认的原型*

![6-5](/images/201412/prototype_2.png)

**2.确认原型和实例的关系**

第一种方式：

    instanceof

第二中方式：

    Object.prototype.isPrototypeOf(instance)

**3.谨慎地定义方法**

子类型有时候需要覆盖超类型的某个方法，或者需要添加超类型中不存在的某个方法。但不管怎样，给原型添加方法的代码一定要放在替换原型的语句之后。

**4.原型链的问题**

还是引用类型的问题，穿件子类型的实例是，不能向超类型的构造函数中传递参数。

####6.3.2   借用构造函数

为了解决引用类型值所带来的问题。在子类型构造函数的内部调用超类型构造函数，

    function SubType(){
        //继承了SuperType
        SuperType.call(this); //同样可以传递参数
    }

//TODO 后续小章节需要补充

##7 函数表达式

###7.2  闭包

闭包是指有权访问另一个函数作用域中的变量的函数。创建闭包的常见方式，就是在一个函数内部创建另一个函数。

    function createComparisonFunction(propertyName) {
        return function(object1, object2){
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];
            if (value1 < value2){
                return -1;
            } else if (value1 > value2){
                return 1;
            } else {
                return 0;
            }
            };
    }

*闭包会携带包含它的函数的作用域，因此会比其他函数占用更多的内存*


####7.2.1   闭包与变量

闭包只能取得包含函数中任何变量的最后一个值（作用域链的配置机制引起的）。用数组分别保存。

####7.2.2   关于this对象

this对象是在运行时基于函数的执行环境绑定的：在全局函数中，this等于window，而当函数被当做某个对象的方法调用时，this等于那个对象。不过，匿名函数的执行环境具有全局性，因此其this对象通常指向window。

    var name="The window";
    var object={
        name:"My Object",
        getNameFunc:function(){
            return function(){
                return this.name;
            }
        }
    };
    alert(object.getNameFunc()()); //The Window(非严格模式下)

通常做法

    var name="The window";
    var object={
        name:"My Object",
        getNameFunc:function(){
            var that=this;
            return function(){
                return that.name;
            }
        }
    };
    alert(object.getNameFunc()()); //My Object

*arguments存在同样的问题*

####7.2.3   内存泄漏

闭包中循环引用

###7.3  模仿块级作用域

    (function(){
        //这里是快级作用域
    })();

###7.4  私有变量

任何在函数中定义的变量，都可以认为是私有变量，因为不能再函数的外部访问这些变量

    function MyObject(){
        //私有变量和私有函数
        var privateVariable=10;
        function privateFunction(){
            return false;
        }

        //特权方法
        this.publicMethod=function(){
            privateVariable++;
            return privateFunction();
        }
    }


####7.4.1    静态私有变量

    (function(){
        var name = “”;
        Person = function(value){
            name = value;
        };
        Person.prototype.getName = function(){
            return name;
        };
        Person.prototype.setName = function (value){
            name = value;
        };
    })();
    var person1 = new Person(“Nicholas”);
    alert(person1.getName()); //”Nicholas”
    person1.setName(“Greg”);
    alert(person1.getName()); //”Greg”
    var person2 = new Person(“Michael”);
    alert(person1.getName()); //”Michael”
    alert(person2.getName()); //”Michael”

变量name就变成了一个静态的，由所有实例共享的属性

####7.4.2   模块模式

模块模式是为了单例创建私有变量和特权方法。

    var singleton={
        //私有变量和私有函数
        var privateVariable=10;
        function privateFunction(){
            return false;
        }

        return {
            publicProperty:true,
            publicMethod:function(){
                privateVariable++;
                return privateFunction();
            }
        }
    }

在Web应用程序中，经常需要使用一个单例来管理应用程序级的信息。


####7.4.3   增强的模块模式

    var singleton={
        //私有变量和私有函数
        var privateVariable=10;
        function privateFunction(){
            return false;
        }
        //创建对象
        var object=new CustomType();
        //添加特权/共有属性和方法
        object.publicProperty=true;
        object.publicMethod=function(){
            privateVariable++;
            return privateFunction();
        }
        //返回这个对象
        return object;
    }

