# 浅析MVC

## MVC的含义
* M - Model （模型层）用来获取、存放所有的对象数据，包含数据、业务逻辑和验证逻辑
* V - View  （视图层）用户所见以及交互的UI界面
* C - Controller (控制层) 应用程序的入口点， 接受来自UI控件的信号

```js
class Model {
    data: {},// 存放数据
    update() {} // 更新数据
}
class View {
    el, // 挂载节点
    template,  // 展示模板
    render(){} // 渲染
}
class Controller{
    init(){}, // 初始化
    events, // 事件
    methods, // 执行方法
    autoBindEvents, // 将方法绑定到事件上
}
```

### EventBus 有哪些 API，是做什么用的，给出伪代码示例

在Model和View中都有关于数据监听和更新的内容。因此抽象出EventBus类，让Model类和View类继承EventBus类

```js
class EventsBus {
    on(eventName, fn) {} // 监听事件
    trigger(eventName, data) {} // 触发事件
    off(eventName, fn) {} // 关闭事件
}
```

### 表驱动编程

《代码大全》对表驱动编程的描述：

>表驱动方法是一种使你可以在表中查找信息，而不必用逻辑语句（if 或 case）来把他们找出来的方法。事实上，任何信息都可以通过表来挑选。在简单的情况下，逻辑语句往往更简单而且更直接。但随着逻辑链的复杂，表就变得越来越富于吸引力了。

而在前端可以将执行的事件从多个监听事件用哈希表来管理

```html
<button id="add">+</button>
<button id="minus">-</button>
<button id="mul">×</button>
<button id="divide">÷</button>
```

* 原始代码：
每当我新增一个函数就要重复写监听函数，获取数据值，更新结果

```js
add.addEventListener("click", () => {
    const num1 = parseInt($("#num1").val())
    const num2 = parseInt($("#num2").val())
    const result = num1 + num2;
    $("#result").text(result);
})
minus.addEventListener("click", () => {
    const num1 = parseInt($("#num1").val())
    const num2 = parseInt($("#num2").val())
    const result = num1 - num2;
    $("#result").text(result);
})
```

* 哈希表管理：
使用哈希表管理就只需要增加对应的事件函数和events表属性就可以

```js
events: {
    'click #add1': 'add',
    'click #minus1': 'minus',
},
add() {
    m.update({ result: m.data.num1 + m.data.num2 })
},
minus() {
    m.update({ result: m.data.num1 - m.data.num2 })
},
autoBindEvents() {
    // 绑定事件
    for (let key in c.events) {
        const value = c[c.events[key]]
        const spaceIndex = key.indexOf(' ')
        const action = key.slice(0, spaceIndex)
        const ids = key.slice(spaceIndex + 1)
        v.el.on(action, ids, value)
    }
}
```

### 我是如何理解模块化的

随着代码的增多，人们就将具有相同逻辑但功能不同的代码块进行划分，把它们分成小的，耦合相对松散的代码块，这些代码块称为模块。

* 在使用时模块保持最少知识原则，与其协作模块进行交互但无需了解它们的内部结构。
* 在模块内部保持相同处理逻辑（M+V+C）
* 有相同的逻辑就抽象出对应的类来处理
