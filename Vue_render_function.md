## 渲染函数

渲染函数是Vue里响应式渲染系统的另外一半。Vue的templates实际上是通过渲染函数渲染出来的。

### 初始渲染
template
-> 编译渲染函数
-> 返回虚拟DOM
-> 生成真实DOM

在Vue上下文中，当我们第一次渲染一个Vue应用时，会将template放到渲染函数进行编译，这回发生在你是用完整的编译模式，也就是你直接使用DOM模板或者直接给Vue实例传递一个模板字符串。但是你如果是使用Vue CLI 来构建项目用到webpack和vue-loader，vue-loader实际上在构建时预编译template，所以当浏览器访问的代码是不包含原始的template，而是提供了编译的纯JavaScript渲染函数代码。这类似于angular上下文中的ALT编译。这样做会节省很多编译时间，而且我们可以在没有编译器的情况下运行。因此在Vue中提供两种构建方式，一种是完整的构建，它包含一个编译器和运行时代码，另一种是只有运行时代码。

在编译成渲染函数后，渲染函数实际上是一个返回虚拟DOM的函数，Vue基于虚拟DOM生成真实DOM
在这个过程之后，我们产生虚拟DOM的过程，本质上是调用渲染函数。因为渲染函数和所有的data属性有依赖关系，在Vue实例中。同时这些data属性是具有响应性的，所有这些data属性会收集为组件渲染函数的依赖。如果这些依赖关系中的任何一个发生变化，将会再次调用渲染函数，在后续更新中，渲染函数会被再次调用，它会返回一个新的虚拟DOM，旧的虚拟DOM和新的虚拟DOM将被比较和区分，最后把最少量的更新应用到真实DOM中
### 后续更新
渲染函数
-> 返回新的虚拟DOM
-> 对比与旧虚拟DOM的差异
-> 将差异应用/更新到真实DOM中


## 虚拟DOM
真实DOM，很多人知道原生DOM API，例如document.createElement，我们可以创建一个真实的div节点表示在我们的文档流中加入一个div，它内部实际上是通过使用C++编写的浏览器引擎实现的，我们实际上并没有接触到这部分，它只是公开了JavaScript中的API以供我们使用。相比之下，Vue中的虚拟DOM会在每个实例this.$createElement返回一个虚拟节点，这个虚拟节点也表示一个div，但它是一个纯JavaScript对象。差异实际是非常大的，如果你在浏览器中打印div属性，你会看到一个真实的div节点会有很多属性，还有他的底层实现实际上很复杂。我们一般把这些称为原生代码的JavaScript接口，双方的调用成本很高，而且他们会比在JavaScript环境执行JavaScript代码更消耗资源，所以很多时候我们会说修改DOM是缓慢的，有的浏览器可能会做优化，但总的来说，对于经常在JavaScript端调用原生端效果不大.

Actual DOM
"[object HTMLDivElement]"
Browser Native Object(expensive)

Virtual DOM
{tag: 'div', data: {attrs: {}, ...}, children: []} 
Plan JavaStript Object (cheap)

就像你看到的代码，虚拟DOM div只是一个对象，它有一个标签，告诉我们它是一个div，还有一个数据对象，包含一些属性。如果他没有任何属性，就不需要数据对象，还有一个children列表，表示他还有很多虚拟节点。现在我们就有了一个通过虚拟节点组成的虚拟DOM树.

假设我们有1000个元素的列表，创建1000个JavaScript对象是非常节省的，也非常快。但是创建1000个真实div节点要昂贵得多？

Virtual DOM
本质上是轻量的JavaScript数据格式在特定时间来表示真实DOM。因为我们会在每次更新是生成虚拟DOM的副本，能这样做是因为虚拟DOM比真实DOM节省，假设我们使用innerHtml更新应用，我们需要丢弃以前真实的DOM节点，然后重新生成所有真实DOM节点，这个成本是比仅生成一个新的虚拟DOM快照更高昂，而且innerHtml也有一些问题。例如它会丢弃所有表单输入元素或者输入状态。但是常见的一个误解是虚拟DOM让这些框架变快，虚拟DOM只是解决原始DOM的局限性的一个方法，它提供了以声明方式构成你想要的DOM结构，这提供了很多可能，但两者间也存在这种差异。

虚拟DOM的另一个好处是，它把渲染逻辑从真实DOM中解耦出来，我们先计算差异，然后将这些更改应用到DOM上，如果我们放弃最后一步，我们应用所有的更新逻辑实际上都是被执行，它不需要接触DOM，实际上，如果我们抽象出这些最终连接点操作DOM的API，然后把它们应用到别的地方，我们就可应用任何支持虚拟环境的JavaScript应用，但不一定是DOM，相反它可以是原生渲染引擎例如iOS或Android或者在服务器端，我们可以将虚拟DOM转换为字符串或字符串查找器，让虚拟DON进行原生渲染像Weeks，Native script这样的项目成为可能，它们的架构都是类似的。JavaScript应用实际上是嵌入到JavaScript引擎中运行。它只发送必要的消息，例如差异更改或任何实际的节点操作发送到渲染器。因此，这就是虚拟DOM的架构优势。显然虚拟DOM不是实现这一目标的唯一方法，但这是一个很好的方法。最后渲染函数只是返回虚拟DOM的函数，template在Vuejs是通过渲染函数编译的 。

把渲染函数和响应式整合起来，每个组件都有一个渲染函数。而这个渲染函数，它实际上包装在我们之前实现的autoRun函数。当渲染的时候，我们收集它们的依赖通过调用我们data属性的getters。我们有了这个额外的概念--观察者。每个组件都有一个负责监视的观察者收集依赖项，清理它们并通知所有内容。组件渲染函数返回虚拟DOM。你会看到这里是循环的，因为我们的渲染放在autoRun中，渲染将会反复调用，只要我们依赖的渲染属性发生变化。每个组件都有自己自动循环渲染。组件树由许多组件组成，每个组件都只负责自己的依赖。因此当你拥有巨大的组件树时，这实际上是一个优势，因为你可以更改数据依赖关系，你的数据可以在任何地方发生改变，因为每个组件都只负责自己的依赖在这个组件树中。我们确切地知道哪些组件受到哪些数据的影响。所以，它不会过度渲染，不会造成过多的组件重新渲染。因为我们有一个精确的依赖跟踪系统。因此这是一种架构优势，可摆脱一些优化工作。React使用的是基于自上而下的渲染模型，但是我们还付出了将这些数据转换为getter和setter的开销，这确实也有一点开销。因此这不是万灵药，双方都有利弊，但这两种方法都是可行的，React和V ue用这两种方法构建大规模应用都没什么问题，实际上差异指挥在非常极端的情况下才会出现


# Vue Mastery

## 模板编译和渲染函数
* 学习虚拟DOM
* 提高你debug技巧
* 编写自己的渲染函数
* 创造强大的抽象

组件模板渲染有两个步骤（这发生在响应式建立之后）
* Step 1 编译
template------>render function
编译我们的模板变成一个被称为渲染函数的东西
* Step 2 运行渲染函数
render function------>Virtual DOM------>Browser
运行渲染函数后将创建一个虚拟DOM节点，加载到浏览器显示我们看到的内容

## 渲染函数看起来是什么？什么是虚拟DOM？
我们的浏览器界面能更改屏幕上显示的内容
```html
<html>
  <head>
    <title>My title</title>
  </head>
  <body>
    <h1>A heading</h1>
  </body>
</html>
```
因此这个HTML映射到一系列我们可以操作的DOM节点。
如果我运行下面的js，他就会更新对应的文本内容在浏览器里
```js
let item = document.getElementByTagName("h1")[0];
item.textContent = "New Heading";
```
DOM树可以变得非常大并且有数千个节点
这就是为什么我们使用JavaScript框架像Vue
但是搜索和更新DOM还是很慢的，这就是为什么我们用虚拟DOM，这并不是Vue独有的，很多JavaScript框架都有一个虚拟DOM。这只是一种用Javascript对象来表示实际DOM的方法。
如果我有这样的div
```html
<div>Hello</div>
```
我们可以用Javascript对象来表示在VNode中使用类似的方法
```js
{
  tag: "div",
  children: [
    {
      text: "Hello"
    }
  ]
}
```
VNode只代表虚拟节点，Vue知道如何使用这个虚拟节点并创建一个真正的DOM节点。
当VNode开始改变时，这就是虚拟DOM开始真正扩展。
如果我们的VNode改为“Goodbye”而不是“Hello”，Vue知道如何比较旧的与新的VNode并进行更新，在我们的DOM中以最有效的方式。
另外一种理解虚拟DOM的方法是将其比喻为建筑图纸，而且将实际DOM比喻为建筑物。

假设我更改了29楼的一些数据，我改变了家具的布局，并增加了一个新的厨房岛台。
我有两种方法来更新它。首先，我可以拆除29楼的所有东西，重新修建。或者，我可以使用新的建筑图纸，比较新旧图纸并进行更新。只做最少的工作。

显然是第二种更有效，虚拟DOM就像这样来工作。

让我们来看看组件渲染过程的举例
模板创建渲染函数，渲染函数创建VNode，VNode更新DOM
```html
<div id=app></div>
<script src="vue.js"></script>
<script>
new Vue({
  el: 'app',
  template: '<div>hello</div>'
})
</script>

```
当它运行时，它会创建一个渲染函数。如果我们在浏览器中加载这个，然后在控制台转换成应用程序选项的值。我们会看到类似下面的代码，他运行createElement函数里面有div和hello参数
```js
app.$options.render = (createElement) => {
  return createElement('div', 'hello')
}
```
这个函数里返回下面的VNode，它只是一个普通的JS对象。Vue接受这个并创建一个真正的DOM对象，
```js
{
  tag: "div",
  children: [
    {
      text: "Hello"
    }
  ]
}
```

那如我们如何写渲染函数和跳过模板编译

```html
<div id=app></div>
<script src="vue.js"></script>
<script>
new Vue({
  el: 'app',
  render(createElement){
    return createElement('div', 'hello')
  }
})
</script>

```

VNode何时插入DOM？
当我们初始化组件并插入DOM
* 初始化事件和生命周期
- beforeCreate
* 初始化injections和响应式（状态）
- created
* 如果有模板存在，我们将他编译成渲染函数
- beforeMount
* 开始挂载 
- mounted
这就是我们获取VNode并将其插入DOM的地方。
我们会在浏览器上看到“hello”

在这些不同项目之间我们有组件生命周期钩子。我们有beforeCreate，created，beforeMount，mounted
在mount里面发生了什么？
在整个声明周期中与组件渲染有关。
还记得响应式课程中，我们观察/监听了响应式属性的代码。如果他们被访问了，我们就会想起这些被设置的属性，则监听的代码将再次运行它。
类似的，在Vue代码库中生命周期的mountComponent函数中，有callHook(vm, 'beforeMount')，然后我们有一个组件渲染函数。将匿名方法发送给观察者
```js
export function mountComponent() {
  callHook(vm, 'beforeMount')
  let updateComponent = () => {
    vm._update(vm._render(), hydrating)
  }
  new Watcher(vm, updateComponent, noop, null, true)
}
```

让我们看看一个简单的Vue应用程序,看看我们在mount开始时发生什么。你可以看到我们有一个响应式属性就是name,我们简单地渲染一个div和name。
```html
<div id=app></div>
<script src="vue.js"></script>
<script>
new Vue({
  el: 'app',
  data: {
    name: "Gregg"
  },
  render(h){
    return h('div', this.name)
  }
})
</script>

```
首先，在beforeMount里设置我们的target，调用render函数。
```js
vm._update(vm._render(), hydrating)
```
在render函数中它访问一个响应属性，getter去调用this.name。并且调用dep.depend()来收集target。
```js
this.name getter called=> dep.depend()
```
然后render函数返回一个VNode，我们把它放到update方法里。这个update方法与实际的DOM交互。然后我们就看到“Gregg”出现在屏幕上。现在我们就挂载完成了（mounted）
```js
vm._update(VNode, hydrating)
```

当响应式数据发生变化是会发生什么？例如，如果我们进入控制台我们把name的值改为Adam
```js
app.name = "Adam"
```
这将调用一个响应式的setter，它将调用dep.notify
```js
this.name setter called=> dep.notify()
```
这有一次运行我们的target，调用render函数，返回一个新的VNode
```js
vm._update(VNode, hydrating)
```
然后调用beforeUpdate，Vue来比较旧的VNode和新的VNode，找出差异，并修补已更改的DOM片段，一旦完成，我们就更新完成了（updated）。这会更新我们的浏览器，然后运行updated里的代码。


Vue的虚拟DOM最初是基于snabbdom来实现的。Vue当前的实现基本上是fork了snabbdom。snabbdom的好处在于修补算法模块部分