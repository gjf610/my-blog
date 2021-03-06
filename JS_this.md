## this 的值到底是什么?

### 函数调用
首先需要从函数的调用开始讲起。

JS（ES5）里面有三种函数调用形式：

```js
func(p1, p2) 
obj.child.method(p1, p2)
func.call(context, p1, p2) // 先不讲 apply
```
一般，初学者都知道前两种形式，而且认为前两种形式「优于」第三种形式。然而第三种调用形式，才是正常调用形式：
```js
func.call(context, p1, p2)
```
其他两种都是语法糖，可以等价地变为 call 形式：
```js
func(p1, p2) //等价于
func.call(undefined, p1, p2)
obj.child.method(p1, p2) //等价于
obj.child.method.call(obj.child, p1, p2)
```

请记下来。（我们称此代码为「转换代码」，方便下文引用）

至此我们的函数调用只有一种形式：
```js
func.call(context, p1, p2)
```
### 这样，this 就好解释了
this，就是上面代码中的 context。就这么简单。

this 是你调用一个函数时传的 context，由于你从来不用<code>call</code>形式的函数调用，所以你一直不知道。

### 先看 func(p1, p2) 中的 this 如何确定：

当你写下面代码时
```js
function func(){
  console.log(this)
}

func()
```

用「转换代码」把它转化一下，得到
```js
function func(){
  console.log(this)
}

func.call(undefined) // 可以简写为 func.call()
```

按理说打印出来的 this 应该就是 undefined 了吧，但是浏览器里有一条规则：

> 如果你传的 context 是 null 或 undefined，那么 window 对象就是默认的 context（严格模式下默认 context 是 undefined）
因此上面的打印结果是 window。

如果你希望这里的 this 不是 window，很简单：
```js
func.call(obj) // 那么里面的 this 就是 obj 对象了
```

### 再看 obj.child.method(p1, p2) 的 this 如何确定
```js
var obj = {
  foo: function(){
    console.log(this)
  }
}

obj.foo() 
```

按照「转换代码」，我们将 obj.foo() 转换为
```js
obj.foo.call(obj)
```

好了，this 就是 obj。搞定。

### [ ] 语法
```js
function fn (){ console.log(this) }
var arr = [fn, '1']
arr[0]() // 这里面的 this 又是什么呢？
```
我们可以把 arr[0]( ) 想象为arr.0( )，虽然后者的语法错了，但是形式与转换代码里的 obj.child.method(p1, p2) 对应上了，于是就可以愉快的转换了： 

    arr[0]() 假想为 arr.0()

    然后转换为 arr.0.call(arr)

    那么里面的 this 就是 arr 了

### 总结
1. this 就是你 call 一个函数时，传入的第一个参数。（请务必背下来「this 就是 call 的第一个参数」）
2. 如果你的函数调用形式不是 call 形式，请按照「转换代码」将其转换为 call 形式。