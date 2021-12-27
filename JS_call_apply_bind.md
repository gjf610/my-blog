## call、apply、bind 的用法分别是什么？

## call()方法
使用一个指定的this值和单独给出的一个或多个参数来调用一个函数。
### 语法
```
func.call([thisArg[, arg1, arg2, ...argN]])
```
### 参数
* thisArg|可选项。
  在<code>func</code>函数运行时使用的<code>this</code>值。
  在某些情况下，<code>thisArg</code>可能不是该方法看到的实际值。
  如果该函数是非严格模式下的函数，则将<code>null</code>和<code>undefined</code>替换为全局对象，并将原始值转换为对象。
* arg1, arg2, ...argN|可选项。
  函数的参数。

### 描述
call() 允许为不同的对象分配和调用属于一个对象的函数/方法。

call() 提供新的 this 值给当前调用的函数/方法。你可以使用 call 来实现继承：写一个方法，然后让另外一个新的对象来继承它（而不是在新对象中再写一次这个方法）。

### 例子

1. 使用 call 方法调用函数并且指定上下文的 'this'

```js
function greet() {
  const reply = [this.animal, 'typically sleep between', this.sleepDuration].join(' ');
  console.log(reply);
}

const obj = {
  animal: 'cats',
  sleepDuration: '12 and 16 hours'
};

greet.call(obj);  // cats typically sleep between 12 and 16 hours
```

2. 使用 call 方法调用函数并且不指定第一个参数（argument）

```js
var sData = 'Wisen';

function display() {
  console.log('sData value is %s ', this.sData);
}

display.call();  // sData value is Wisen
```

## apply()方法
调用一个具有给定this值的函数，以及以一个数组（或类数组对象）的形式提供的参数。

### 语法

```
func.apply(thisArg, [argsArray])
```

### 参数
* thisArg|必选项。
  在<code>func</code>函数运行时使用的<code>this</code>值。
  在某些情况下，<code>thisArg</code>可能不是该方法看到的实际值。
  如果该函数是非严格模式下的函数，则将<code>null</code>和<code>undefined</code>替换为全局对象，原始值会被包装。
* arg1, arg2, ...argN|可选项。
  一个数组或者类数组对象，其中的数组元素将作为单独的参数传给<code>func</code>函数。如果该参数的值为<code>null</code>或<code>undefined</code>，则表示不需要传入任何参数。从ECMAScript 5 开始可以使用类数组对象。

### 描述
在调用一个存在的函数时，你可以为其指定一个 this 对象。 this 指当前对象，也就是正在调用这个函数的对象。 使用 apply， 你可以只写一次这个方法然后在另一个对象中继承它，而不用在新对象中重复写该方法。

apply 与 call() 非常相似，不同之处在于提供参数的方式。apply 使用参数数组而不是一组参数列表。apply 可以使用数组字面量（array literal），如<code>fun.apply(this, ['eat', 'bananas'])</code>，或数组对象， 如<code>fun.apply(this, new Array('eat', 'bananas'))</code>。

### 例子

1. 用 apply 将数组各项添加到另一个数组

```js
var array = ['a', 'b'];
var elements = [0, 1, 2];
array.push.apply(array, elements);
console.info(array); // ["a", "b", 0, 1, 2]
```

2. 使用apply和内置函数
对于一些需要写循环以便历数组各项的需求，我们可以用apply完成以避免循环。

```js
const numbers = [5, 6, 2, 3, 7];
const max = Math.max.apply(null, numbers);
console.log(max);
const min = Math.min.apply(null, numbers);
console.log(min);
```

## bind()方法

创建一个新的函数，在<code>bind()</code>被调用时，这个新的函数的<code>this</code>被指定为<code>bind()</code>的第一个参数，而其余参数将作为新函数的参数，供调用时使用。

### 语法

```js
func.bind(thisArg[thisArg[, arg1, arg2, ...argN]])
```

### 参数
* thisArg|可选项。
  调用绑定函数时作为<code>this</code>参数传递给目标函数的值。 如果使用new运算符构造绑定函数，则忽略该值。当使用<code>bind</code>在<code>setTimeout</code>中创建一个函数（作为回调提供）时，作为<code>thisArg</code>传递的任何原始值都将转换为<code>object</code>。如果<code>bind</code>函数的参数列表为空，或者<code>thisArg</code>是<code>null</code>或<code>undefined</code>，执行作用域的<code>this</code>将被视为新函数的<code>thisArg</code>。
* arg1, arg2, ...argN|可选项。
  当目标函数被调用时，被预置入绑定函数的参数列表中的参数。
### 例子
1. 创建绑定函数
<code>bind()</code>最简单的用法是创建一个函数，不论怎么调用，这个函数都有同样的<code>this</code>值。很多人经常犯的一个错误是将一个方法从对象中拿出来，然后再调用，期望方法中的<code>this</code>是原来的对象（比如在回调中传入这个方法）。
如果不做特殊处理的话，一般会丢失原来的对象。基于这个函数，用原始的对象创建一个绑定函数，

```js
this.x = 9;    // 在浏览器中，this 指向全局的 "window" 对象
var module = {
  x: 81,
  getX: function() { return this.x; }
};

module.getX(); // 81

var retrieveX = module.getX;
retrieveX();
// 返回 9 - 因为函数是在全局作用域中调用的

// 创建一个新函数，把 'this' 绑定到 module 对象
// 新手可能会将全局变量 x 与 module 的属性 x 混淆
var boundGetX = retrieveX.bind(module);
boundGetX(); // 81
```
