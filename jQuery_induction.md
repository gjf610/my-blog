## 经典回顾，初探jQuery设计

jQuery作为网页开发中最长寿的库，在今天的互联网世界里里依然有70%~%80%的网站使用到[jQuery](https://trends.builtwith.com/javascript/jQuery)。

在以Angular,React,Vue等框架\库为主流网页开发的今天，对于网页开发者来说，jQuery并非是必要的使用工具。但是jQuery流行时间之长，使用范围之广，必然有其经典的设计。比如闭包，原型链，DOM API，设计模式（Promise、伪重载、装饰器模式等），DOM 事件...

### 一、获取元素
jQuery的基本设计思想和主要用法，就是"选择某个网页元素，然后对其进行某种操作"，并且将选择网页元素的操作封装起来。

使用jQuery的第一步，往往就是将一个选择表达式，放进构造函数jQuery()（简写为$），然后得到被选中的元素。
```js
　　$(document) //选择整个文档对象

　　$('#test') //选择ID为test的元素

　　$('div.red') // 选择class为red的div元素

　　$('input[placeholder]') // 选择placeholder属性的input元素
```
### 二、链式操作
选中网页元素之后，可以对它进行一系列操作，并且所有操作可以连接在一起，以链条的形式写出来。比如:
<code>$('div').find('.red').addClass('border');</code>
分解开来，就是下面这样：

```js
　　$('div') // 找到div元素
　　　.find('.red') // 选择其中的class为red的元素
　　　.addClass('border') // 添加border的class
```
它的原理在于每一步的jQuery操作，返回的都是一个jQuery对象，所以不同操作可以连在一起。

还有.end()方法，让操作的节点后退一步。

```js
　　$('div')
　　　.find('.red')
      .eq(0)//选中第1个元素
　　　.html('Hello')
　　　.end() //退回到选中class为red元素的那一步
　　　.eq(1) //选中第2个元素
　　　.html('World'); //将它的内容改为World
```

### 三、创建元素
创建新元素的方法，只要把新元素直接传入jQuery的构造函数就行了。
```js
　　$('<span>xiao wang</span>');
　　$('<div class="red">Ruby</div>');
　　$('ul').append('<li>item</li>');
```
### 四、移动元素
jQuery提供两种方法，来操作元素的位置移动。第一种方法是<code>.insertAfter()</code>，内容在方法前面，它将被放在参数里元素的后面。第二种方法是<code>.after()</code>，选择表达式在函数的前面，参数是将要插入的内容。

原始页面内容：
```html
<div class="container">
  <h2>Greetings</h2>
  <div class="inner">Hello</div>
  <div class="inner">Goodbye</div>
</div>
```
#### .insertAfter()
在页面上选择一个元素然后插在另一个元素后面：
```js
$('h2').insertAfter($('.container'));
```
如果一个被选中的元素被插在另外一个地方，它将被移动到目标元素的后面，注意是移动而不是复制
```html
<div class="container">
  <div class="inner">Hello</div>
  <div class="inner">Goodbye</div>
</div>
<h2>Greetings</h2>
```
我们也可以创建内容然后同时插在好几个元素后面：
```js
$('<p>Test</p>').insertAfter('.inner');
```
结果如下：
```html
<div class="container">
  <h2>Greetings</h2>
  <div class="inner">Hello</div>
  <p>Test</p>
  <div class="inner">Goodbye</div>
  <p>Test</p>
</div>
```

#### .after()
在页面上选择一个元素然后插在另一个元素后面：
```js
$('.container').after($('h2'));
```
如果一个被选中的元素被插在另外一个地方，这是移动而不是复制：
```html
<div class="container">
  <div class="inner">Hello</div>
  <div class="inner">Goodbye</div>
</div>
<h2>Greetings</h2>
```
可以创建内容然后同时插在好几个元素后面：
```js
$('.inner').after('<p>Test</p>');
```
每个内部的<code>div</code>元素得到新的内容：
```html
<div class="container">
  <h2>Greetings</h2>
  <div class="inner">Hello</div>
  <p>Test</p>
  <div class="inner">Goodbye</div>
  <p>Test</p>
</div>
```
### 五、修改元素的属性
#### .addClass()
为每个匹配的元素添加指定的样式类名
```js
$("p").addClass("myClass yourClass");
```
#### .removeClass()
每个匹配元素移除的一个或多个用空格隔开的样式名。
```js
$('p').removeClass('myClass yourClass')
```
.addClass()通常和.removeClass()一起使用，用来切换元素的样式,
```js
$("p").removeClass("myClass noClass").addClass("yourClass");
```
#### .css()
获取匹配元素集合中的第一个元素的样式属性的值  或  设置每个匹配元素的一个或多个CSS属性。
* .css( propertyName )
.css()方法可以非常方便地获取匹配的元素集合中第一个元素的样式属性值
```js
$("div").css("background-color");
```
* .css( propertyName, value )
.css()方法使得设置元素的CSS属性快速而又简单。这个方法可以使用任何一个CSS属性名和用空格隔开的值，或者一个“键/值”对对象(JavaScript 对象符号)作为参数。
```js
$("div").css( ["width", "height", "color", "background-color"] );
```
