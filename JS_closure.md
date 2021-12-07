## JS闭包

### 定义
> 一个函数A内声明并返回一个函数B供外部使用，函数B内用到了A内部的局部变量或者形参。外界对A函数内变量的引用导致A作用域不能被释放，构成一个闭包。
```js
funcation f1(){
  let a = 1
  funcation f2() {
    let a = 2
    funcation f3 (){
      console.log(a)
    }
    a = 22
    f3()
  }
  console.log(a)
  a = 100
  f2()
}
f1()
```
### 更宽泛的闭包
> 函数B不一定直接返回，只要B能够再次被调用，都构成闭包。比如A返回的是一个对象，而函数B是对象的属性。或者函数B能在setTimeout延时结束时的触发。
#### 返回一个对象
```js
function Counter() {
  let num = 0;
  return {
    add() {
      num++
      console.log(num)
    }
  }
}

let counter = Counter()
counter.add()
counter.add()
```
#### 什么都不返回
```js
function Counter() {
  let num = 0
  function add() {
    num++
    console.log(num)
  }
  setTimeout(add, 1000)
}

let counter = Counter()
```

### 闭包的缺点
1. 引用的变量可能发生变化
```js
var result = [];
for (var i = 0; i<10; i++){
  result[i] = function () {
    console.info(i)
  }
};
result['0']()
result['1']()
result['2']()
result['3']()
result['4']()
result['5']()

```
2. this指向问题
```js
const object = {
  name: 'object',
  getName() {
    return function() {
      console.info(this.name)
    }
  }
}
object.getName()()    // underfined
```
3. 内存泄漏问题
```js
function  showId() {
  const element = document.getElementById("app")
  element.onclick = () => {
    alert(element.id)   // 这样会导致闭包引用外层的el，当执行完showId后，el无法释放
  }
}

// 改成下面
function  showId() {
  const element = document.getElementById("app")
  const { id }  = element
  element.onclick = () =>{
    alert(id)   // 这样会导致闭包引用外层的el，当执行完showId后，el无法释放
  }
  element = null    // 主动释放el
}
```
### 闭包的用途
1. 用来暂存"当时"的状态;
2. 用来"封装"一些数据;
#### 封装数据、模块模式、IIFE
```js
const cache = (() => {
  const store = {}
  return {
    get(key) {
      return store[key]
    }
    set(key, val) {
      store[key] = val
    }
    remove(key) {
      delete store[key]
    }
  }
})()
console.log(cache)
cache.set('name', '仿佛锋')
cache.get('name')
cache.remove('name')
```
#### 暂存数据、高阶函数、Curry
```js
const makeUrl = domain => path => `https://${domain}${path}`
const makeBDUrl = makeUrl('baidu.com')
const makeGGUrl = makeUrl('google.com')
const url1 = makeBDUrl ('/index')
const url2 = makeGGUrl ('/help')
```
