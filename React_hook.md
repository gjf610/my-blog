## React Hook 浅析

### 首先，我们要先了解为什么要用Hook:
Hook是 React 16.8 的新增特性。它可以让我们在不编写 class 的情况下使用 state 以及其他的 React 特性。

### Hook有几个特点：
* 完全可选的。 无需重写任何已有代码就可以在一些组件中尝试 Hook。
* 100% 向后兼容的。 Hook 不包含任何破坏性改动。
* Hook 不会影响对 React 概念的理解。 恰恰相反，Hook 为已知的 React 概念提供了更直接的 API：props， state，context，refs 以及生命周期。
了解了为什么要用Hook后（就是比class好用），下面开始上例子：

这是一个点击加1的计数器的例子

### useState
```js
import React, { useState } from 'react';
function Example() {
  // 声明一个叫 “count” 的 state 变量。
  const [count, setCount] = useState(0);
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
* 在这里，<code>useState</code>就是一个Hook。通过在函数组件里调用它来给组件添加一些内部 state。React 会在重复渲染时保留这个 state。
* <code>useState</code>会返回一对值：当前状态和一个让你更新它的函数(也就是[count, setCount])，你可以在事件处理函数中或其他一些地方调用这个函数。
* 它类似 class 组件的<code>this.setState</code>，但是它不会把新的 state 和旧的 state 进行合并。
当然，我们也可以声明不同的 state 变量：
```js
function ExampleWithManyStates() {
  // 声明多个 state 变量
  const [age, setAge] = useState(42);
  const [fruit, setFruit] = useState('banana');
  const [todos, setTodos] = useState([{ text: 'Learn Hooks' }]);
  // ...
}
```
解构赋值 的语法让我们在调用<code>useState</code>时可以给 state 变量取不同的名字。这些名字并不是<code>useState</code>API 的一部分。

### useEffect（副作用）
对环境的改变即为副作用，如修改innerText, 但我们不一定非要把副作用放在<code>useEffect</code>里。
实际上叫做afterRender更好，render后运行。

#### 先上一个class的例子
```js
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  componentDidMount() {    document.title = `You clicked ${this.state.count} times`;  }  
  componentDidUpdate() {    document.title = `You clicked ${this.state.count} times`;  }
  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```
注意，在这个 class 中，我们需要在两个生命周期函数中编写重复的代码。

这是因为很多情况下，我们希望在组件加载和更新时执行同样的操作。从概念上说，我们希望它在每次渲染之后执行 —— 但 React 的 class 组件没有提供这样的方法。

现在让我们来看看如何使用<code>useEffect</code>执行相同的操作。

注意，在这个 class 中，我们需要在两个生命周期函数中编写重复的代码。

这是因为很多情况下，我们希望在组件加载和更新时执行同样的操作。从概念上说，我们希望它在每次渲染之后执行 —— 但 React 的 class 组件没有提供这样的方法。即使我们提取出一个方法，我们还是要在两个地方调用它。

现在让我们来看看如何使用<code>useEffect</code>执行相同的操作。
```js
import React, { useState, useEffect } from 'react';
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {    document.title = `You clicked ${count} times`;  });
  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
#### useEffect做了什么？

通过使用这个 Hook，我们告诉 React 组件需要在渲染后执行什么操作。

React 会保存你传递的函数（我们将它称之为 “effect”），并且在执行 DOM 更新之后调用它。

在这个 effect 中，我们设置了 document 的 title 属性，不过我们也可以执行其他操作。

#### 为什么在组件内部调用useEffect？

将<code>useEffect</code>放在组件内部让我们可以在 effect 中直接访问countstate 变量（或其他 props）。

我们不需要特殊的 API 来读取它 —— 它已经保存在函数作用域中。Hook 使用了 JavaScript 的闭包机制，而不用在 JavaScript 已经提供了解决方案的情况下，还引入特定的 React API。

#### 总结用途
* 作为componentDidMount使用，[]作第二个参数
* 作为componentDidUpdate使用，可指定依赖
* 作为componentWillUnmount使用，通过return返回
* 以上三种用途可同时存在
#### 特点
如果同时存在多个<code>useEffect</code>，会按照出现次序执行

### 自定义Hook
这是React Hook中最有意思的一部分：

* 自定义 Hook 不需要具有特殊的标识。我们可以自由的决定它的参数是什么，以及它应该返回什么。
* 它就像一个正常的函数。但是它的名字应该始终以use开头，这样可以一眼看出其符合Hook 的规则。
#### 自定义 Hook 是一个函数，其名称以 “use” 开头，函数内部可以调用其他的 Hook。

> 我会在我的React项目中运用到自定义Hook的技巧。