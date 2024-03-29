## 函数调用栈、递归

### 什么是函数调用
函数调用就是运行一个函数，具体使用方式是使用函数名称跟着一对小括号。下面我们看个简单的示例代码：
``` js
debugger
var a = 2
function add(){
  var b = 10
  return  a + b
}
add()
```
### 创建执行上下文
当一段代码被执行时，JavaScript 引擎先会对其进行编译，并创建执行上下文。但是并没有明确说明到底什么样的代码才算符合规范。 
那么接下来我们就来明确下，哪些情况下代码才算是“一段”代码，才会在执行之前就进行编译并创建执行上下文。一般说来，有这么三种情况：
1. 当 JavaScript 执行全局代码的时候，会编译全局代码并创建全局执行上下文，而且在整个页面的生存周期内，全局执行上下文只有一份。
2. 当调用一个函数的时候，函数体内的代码会被编译，并创建函数执行上下文，一般情况下，函数执行结束之后，创建的函数执行上下文会被销毁。
3. 当使用 eval 函数的时候，eval 的代码也会被编译，并创建执行上下文。

在执行到函数 add() 之前，JS 引擎会为上面这段代码创建全局执行上下文，包含了声明的函数和变量。
* 首先，从全局执行上下文中，取出 add 函数代码。
* 其次，对 add 函数的这段代码进行编译，并创建该函数的执行上下文和可执行代码。
* 最后，执行代码，输出结果。

### 什么是调用栈
js引擎在调用一个函数前，需要把函数所在的执行上下文push到一个数组里，这个数组就叫做调用栈

等函数执行完了，就会吧环境弹pop出来，然后return到之前的环境，继续执行后续代码。

### 举例
```js
  debugger
  console.log(1)
  console.log('1+1的结果为' + add(1, 1))
  console.log(2)
  function add(a, b) {
    return a + b
  }
```

### 递归函数
#### 阶乘
```js
debugger
function f(n) {
  return n !== 1 ? n * f(n - 1) : 1
}
console.log(f(4))
```

#### 理解递归
```
f(4)
= 4 * f(3)
= 4 * (3 * f(2))
= 4 * (3 * (2 * f(1)))
= 4 * (3 * (2 * 1))
= 4 * (3 * 2)
= 4 * (6)
= 24
```
先递进，再回归。
### 番外. 在开发中，如何利用好调用栈
1. 利用浏览器查看调用栈的信息

打开“开发者工具”，点击“Source”标签，选择 JavaScript 代码的页面，然后在第 3 行加上断点，并刷新页面。你可以看到执行到 add 函数时，执行流程就暂停了，这时可以通过右边“call stack”来查看当前的调用栈的情况
2. console.trace()

除了通过断点来查看调用栈，你还可以使用 console.trace() 来输出当前的函数调用关系，比如在示例代码中的 add 函数里面加上了 console.trace()，你就可以看到控制台输出的结果
``` js
var a = 2
function add(b,c){
  console.trace()
  return b+c
}
function addAll(b,c){
  var d = 3
  result = add(b,c)
  return  a+result+d
}
addAll(1,9)
```
