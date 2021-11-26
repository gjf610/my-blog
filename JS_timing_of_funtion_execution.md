## JS 函数的执行时机
### 一、定义函数
1. 具名函数
```js
function [函数名]([形式参数1], [形式参数2]){
  [语句]
  return [返回值]
}
```
2. 匿名函数
上面叫具名函数，去掉函数名就是匿名函数
```js
let sum = function(x, y){return x + y}
```
也叫函数表达式
3. 箭头函数
```js
let f1 = x => x*x
```
传一个参数
```js
let f2 = (x, y) => x*y
```
传一个以上参数，圆括号不能省
```js
let f3 = (x, y) => { 
  console.log(x, y)
  return x+y
}
```
有操作语句时，花括号不能省
```js
let f4 = (x, y) => ({
  name: x, age: y
})
```
直接返回对象，需要在花括号外加圆括号
4. 构造函数
```js
let f = new Function('x','y', 'return x+y')
```
- 所有函数都是Function构造出来的
- 包括Object/Array/Function
### 二、函数自身V.S.函数调用 （ fn V.S. fn() ）
#### 函数自身
```js
let fn = () => console.log('hi')
fn
```
不会有任何结果，因为fn没有执行
#### 函数调用
```js
let fn = () => console.log('hi')
fn()
```
打印出hi，有圆括号才是调用
#### 再进一步
```js
let fn = () => console.log('hi')
let fn2 = fn
fn2()
```
- fn保存了匿名函数的地址
- 这个地址被复制给了fn2
- fn2调用的匿名函数
- fn和fn2都是储存匿名函数的引用而已
- 真正的函数既不是fn也不是fn2
### 三、调用时机
setTimeout()是指在本次函数调用栈执行完，最后在去执行里面的函数。
1. 这段代码是先用let声明i的值为0，然后for循环执行了6遍（i等于6时才退出执行for循环，0-1-2-3-4-5），最后将i打印出来。
```js
let i = 0
for(i = 0; i<6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```
2. 这段代码是for和let一起用的时候给i独立形成了一个块级作用域，每次循环都会多创建一个i。
```js
for(let i = 0; i < 6; i++){
  setTimeout(()=>{
    console.log(i)
  },0)
}
```
3. 这段代码是for和自执函数一起用的时候给形式参数i独立形成了一个函数作用域，每次循环都会多创建一个i。
```js
for(var i = 0; i < 6; i++){
  (function (i) {
    setTimeout(function () {
      console.log(i)
    }, 0)
  }(i))
}
```
4. 这段代码是for和output函数一起用的时候给形式参数i独立形成了一个函数作用域，每次循环都会多创建一个i。
```js
function output(i){
  setTimeout(function () {
    console.log(i)
  }, 0)
} 
for (var i = 0; i < 6; i++) {
  output.call(undefined, i)
}
```
5. 这段代码是利用Generator函数，使用setTimeout()在生成器对象形成后, 再执行迭代。
```js
function* idMaker(){
  let index = 0;
  while(true)
    yield index++;
}

let gen = idMaker();
setTimeout(()=>{
  for(var i = 0; i < 6; i++){
    console.log(gen.next().value); 
  }
})
```

部分资料来源： &copy;饥人谷
