## 数据响应式（Reactivity）

响应式是Vue的一个核心特性，用于监听视图中绑定的数据，当数据发生改变时视图自动更新。

### 响应性问题

假设我们有个需求，价格（price）是5，数量（quantity）是2，求总价（total）是多少。如果使用命令式编程，可以很简单实现，就像下面的示例：

```js
let price = 5
let quantity = 2
let total = price * quantity;
console.log(`total is ${total}`) // 10
```
问题是我们把数量（quantity）设置成4时，价格（price）还是等于10。因为它是命令式的，不会实现数据的同步更新。
```js
quantity = 4
console.log(`total is ${total}`) // 10
```

那怎么让他们保持同步呢？为了得到最新总价（total）是多少？那我们需要重新求总价（total）的值，像下面代码：
```js
total = price * quantity
console.log(`total is ${total}`) // 20
```
这样每次更新价格（price）或数量（quantity）的时候，都需要手动更新总价（total）。这种方式显然太麻烦了，我希望仅重新赋值价格（price）或数量（quantity）一次，使得总价（total）就可以做到更新。

### 存储重复的运行代码

我们可以看到<code>total = price * quantity </code>重复了两遍。因此能不能想办法将这段代码存储起来，再放到合适的地方运行。

为此我将<code>total = price * quantity </code>放到target函数里以方便调用，并且声明storage数组将target函数存储到数组中。当我们遍历函数storage数组时，就可以运行里面存储的函数。
```js
let target = () => { total = price * quantity }
let storage = [];
storage.push(target)
storage.forEach(run => run())
```
然后我们来整理一下代码，用record()将target存储到storage中,用replay()遍历执行storage里的函数。
```js
let price = 5;
let quantity = 2;
let total = 0;
let target = null;
let storage = []

target = () => { total = price * quantity }

function record() {
  storage.push(target)
}
function replay() {
  storage.forEach(run => run())
}

record() // 调用record来保存target，它会把target放到storage里
target() // 然后运行target

console.log(total); // 10
price = 20;
console.log(total); // 10

replay() //当我们改变一个变量时，我们需要运行保存在storage里的所有target
console.log(total); // 40
```
运行target()后，我们看到打印出10。接着修改price，还是打印出10。但调用replay()之后,打印出40。总数和我们预期的一样重新计算了。

### 依赖跟踪（dependency tracking）

如果把上面代码运用到其他总数或者折扣的计算，我们可以看到storage、record()和replay()是不会变化的。也许我们可以创建一个构造函数或类来存储，收集，运行target()。

为此就实现一个Dep类，里面有实例化的subscribers数组用来存储target()；有depend()，用于记录（record）target()/依赖项（表达式、计算（computed）、函数（methods））；另外还有一个notify()，用于重新触发（replay）依赖项的执行。
```js
class Dep { // 代表有依赖关系的类
  constructor() {
    this.subscribers = [] // storage存储target
  }
  depend() {
    if (target && !this.subscribers.includes(target)) { // if there is a target & we don't already have it.
      this.subscribers.push(target) //添加 target 到 subscribers 中
    }
  }
  notify() {
    this.subscribers.forEach(sub => sub()) // 运行 subscribers 里的 target
  }
}
```
整理代码。首先我们使用Dep类，并且创建一个Dep实例。其次声明价格和数量以及target函数。然后调用dep.depend()来记录target，运行target()。
```js
class Dep { 
  constructor() {
    this.subscribers = []
  }
  depend() {
    if (target && !this.subscribers.includes(target)) { 
      this.subscribers.push(target)
    }
  }
  notify() {
    this.subscribers.forEach(sub => sub())
  }
}

const dep = new Dep()

let price = 5
let quantity = 2
let total = 0

let target = () => { total = price * quantity }

dep.depend()
target()

console.log(price) // 5
console.log(total) // 10

price = 20
console.log(total) // 10

dep.notify()
console.log(total) // 40
```
我们看到price打印出5，total打印出10。接着修改price，total还是没有改变（10）。因为要调用dep.notify()，我们再次total打印，可以看到我们是正确运行（40）。

### 观察者模式（Observer pattern）
如果每个变量有不同的target/依赖项，就会产生不同的target*变量，每次手动调用dep.depend()。因此我们需要一种方法来封装被监视或记录target。

我们就创建一个观察者函数。在我们的watcher里，我们传入一个匿名函数fn，并赋值给target，调用dep.depend()来记录watcher里面的watcher， 运行target，然后target设置为null。

```js
function watcher(fn) {
    target = fn
    dep.depend()
    target()
    target = null
}

watcher(() => {
  total = price * quantity
})
```
整理代码。首先我们使用Dep类，并且创建一个Dep实例。其次声明价格和数量以及target函数。watcher函数用来设置target，收集target，运行target，最后重置target。

```js
class Dep { 
  constructor() {
    this.subscribers = []
  }
  depend() {
    if (target && !this.subscribers.includes(target)) { 
      this.subscribers.push(target)
    }
  }
  notify() {
    this.subscribers.forEach(sub => sub())
  }
}

const dep = new Dep()
let price = 5;
let quantity = 2;
let total = 0;
let salePrice = 0;
let target = null

function watcher(fn) {
  target = fn
  dep.depend()
  target()
  target = null
}

watcher(() => {
  total = price * quantity
})
watcher(() => {
  salePrice = price * 0.9
})
console.log(total); // 10
price = 20;
console.log(total); // 10
```
total打印出10。接着修改price，total还是没有改变（10）。因为要调用dep.notify()，我们再次total打印，可以看到我们是正确运行（40）。

### 数据劫持

接下来能不能让代码更自动化一些，不需要我们去手动调用了dep.notify()。我们需要一种方法来识别属性何时更新。如果我们能够识别何时更新（或设置）属性，我们可以触发dep.notify() 在其 Dep 类上调用。之前我们必须运行dep.notify()，但我们需要一种方法。

当价格的时候改变时，将会调用price的dep实例上的dep.notify()。这就是ES5的[Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)的作用了。它提供监听属性变更的功能，get()覆盖.取值行为，而set()覆盖赋值行为。
```js
let data = { price: 5, quantity: 2 }
Object.defineProperty(data, "price", {// For just price on the data object
  get() {// Our new get function
    console.log(`I was accessed`)
  },
  set(newVal) {// Our new set function
    console.log(`I was changed`)
  }
})
data.price // Call data.price get()
data.price = 20 // Call data.price set()
```
问题是我们把这个值存在哪里呢？现在我们通过创建一个内部值变量，它将存储初始值。在get()里返回internalValue，在set()里把新值赋给internalValue。
```js
let data = { price: 5, quantity: 2 }
let internalValue = data.price // 存储初始值

Object.defineProperty(data, "price", {
    get() {
        console.log(`Getting price: ${internalValue}`)
        return internalValue;// Return the current value
    },
    set(newVal) {
        console.log(`Setting price to: ${newVal}`)
        internalValue = newVal// Set the current value
    }
})
```
我们要为每个属性调用Object.defineProperty重写它们的getter、setter。

```js
let data = { price: 5, quantity: 2 }
Object.keys(data).forEach(key => { // Get the keys and iterate through them
    let internalValue = data[key] // Set initial value.
    Object.defineProperty(data, key, { // For current property
        get() {
            console.log(`Getting ${key}: ${internalValue}`)
            return internalValue;// Return the current value
        },
        set(newVal) {
            console.log(`Setting ${key} to: ${newVal}`)
            internalValue = newVal// Set the current value
        }
    })
})
let total = data.price * data.quantity;
data.price = 20;

console.log(total); // Getting price: 10
console.log(data.price); // 20
```
计算total时会打印
* Getting price: 5,
* Getting quantity: 2。

接着修改data.price，打印出
* Setting price to:20。
### 响应式
最后将数据劫持和依赖跟踪结合起来产生响应式。把dep.depend()和dep.notify()放到get()和set()里。

设置data，获取data里面的keys，设置internalValue，还有每一个都需要自己的dep实例。对每个属性重新设置getter和setter。当调用get()时，将当前的target添加到subscribers里，返回internalValue；当调用set()时，将新值设置给internalValue并且调用dep.notify()后运行所有subscribers里的target
```js
let data = { price: 5, quantity: 2 }
Object.keys(data).forEach(key => {
    let internalValue = data[key]

    const dep = new Dep() // Each key gets its own Dep class

    Object.defineProperty(data, key, {
        get() {
            dep.depend()// When accessed (get) add current target to subscribers
            return internalValue;
        },
        set(newVal) {
            internalValue = newVal// When set run all subscribers
            dep.notify()
        }
    })
})
```
最后得出一个简易的数据响应式系统

```js

let data = { price: 5, quantity: 2 }
let target, total, salePrice

class Dep {
    constructor() {
        this.subscribers = []
    }
    depend() {
        if (target && !this.subscribers.includes(target)) {
            this.subscribers.push(target)
        }
    }
    notify() {
        this.subscribers.forEach(sub => sub())
    }
}

Object.keys(data).forEach(key => {
    let internalValue = data[key]

    const dep = new Dep()

    Object.defineProperty(data, key, {
        get() {
            dep.depend()
            return internalValue;
        },
        set(newVal) {
            internalValue = newVal
            dep.notify()
        }
    })
})

function watcher(fn) {
    target = fn
    target()
    target = null
}

watcher(() => {
    total = data.price * data.quantity
})

watcher(() => {
    salePrice = data.price * 0.9
})

```
通过上面的代码我们将会学到
* 如何创建一个收集target（依赖项）并重新运行所有依赖项的 Dep 类（通知）。 
 
* 如何创建一个观察程序来管理我们正在运行的代码，该代码可能需要添加（target）作为依赖项。

* 如何使用 Object.defineProperty() 创建 getter 和 setter。 