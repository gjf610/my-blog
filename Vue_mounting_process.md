## Vue挂载过程

* mountComponent是在哪里调用的？
* 模板是在哪里编译为渲染函数的？这是如何工作的？
* _render做了什么？
* _update做了什么？

### initialMixin函数
当我们创建一个组件时，调用它，就会初始化一堆不同的东西。它有两个生命周期hook，beforeCreate和created。它检查组件options里是否有.el选项。如果有，它会调用$mount，在$mount里模板会被编译（如果需要），最后就到了mountComponent，接下来让我们来深入研究它。
```js
export function initMixin (Vue: Class<Component>) {
  Vue.prototype._init = function (options?: Object) {
    const vm: Component = this
    // ...
    vm._self = vm
    initLifecycle(vm)
    initEvents(vm)
    initRender(vm)
    callHook(vm, 'beforeCreate')
    initInjections(vm) // 在data/props前初始化inject
    initState(vm)
    initProvide(vm) // 在data/props后初始化provide
    callHook(vm, 'created')

    // 如果用户在实例化Vue.js时传递了el选项，则自动开启模板编译阶段与挂载阶段
    // 如果没有传递el选项，则不进入下一个生命周期流程
    // 用户需要执行vm.$mount方法，手动开启模板编译阶段与挂载阶段
  
    if (vm.$options.el) {
      vm.$mount(vm.$options.el)
    }
  }
}
```

### vm.$mount

Mount函数会基于运行环境而不同。如果我们查看/scripts/config.js里，你会发现 Vue 有许多不同的构建配置。其中一个是“runtime-only-build”，它只包含运行渲染函数的代码，然后是“web-full-dev-build”，里面包含将模板编译为渲染函数以及运行它们的代码。
```js
const mount = Vue.prototype.$mount
Vue.prototype.$mount = function (el?: string | Element, hydrating?: boolean): Component {
  el = el && query(el)
  const options = this.$options

  if (!options.render) { 
    let template = options.template
    if (template) {
      const { render, staticRenderFns } = compileToFunctions(template, {...}, this)
      options.render =  render
    }
  }
  return mount.call(this, el, hydrating)
}
```
首先查询el元素。如果没有渲染函数，这意味着可能需要编译一个template。检查template，如果存在则调用compileToFunctions，在这个函数中我们的template会被编译为渲染函数。设置渲染函数。最后调用先前定义的mount。接下来让我们看看compileToFunctions，看看它做了什么。

### compileToFunctions
这个函数的作用是获取模板并将其转换为渲染函数。
```js
template: `<h1>{{name}}</h1>`
```
如果我们有上面的模板，它将创建类似下面的一个匿名函数。此函数返回一个VNode。是怎么发现的呢？如果我们跳进浏览器控制台，输入app.$options.render，就可以查看这个匿名函数。
```js
render = () => {
  with(this){
    return _c("h1", [_v(_s(name))])
  }
}
```
此时你可能在想这些带有下划线的是什么玩意？如果我们往里面看render.js。在initRender中，我们可以看到哪里定义了_c，这就是createElement函数。
```js
export function initRender(vm: Component){
  ...
  vm._c = (a, b, c, d) => createElement(vm, a, b, c, d, false)
}

```
在core/instance/render-helpers.js中，可以看到_s就是toString、_v就是createTextVNode。这些快捷方式使渲染功能的体积尽可能小。将他们翻译过来，渲染函数就像下面的代码，这样读起来肯定比较容易。
```js
render = () => {
  with(this){
    return createElement("h1", [createTextVNode(toString(this.name))])
  }
}
```

### MountComponent函数
让我们回到MountComponent函数，这里有beforeMount，updateComponent函数，_render函数，还有Watcher。记住Watcher构造函数里调用updateComponent函数，一开始就运行了updateComponent函数，每当响应式数据属性更改时，updateComponent函数会再次运行。但是等一下，这是在调用_render()函数，并且我们存储了模板函数，内部只是正常渲染。那么_render()在做什么？
```js
export function mountComponent(vm: Component, el:? Element, hydrating?: boolean): Component {
  vm.$el = el
  ...
  callHook(vm, 'beforeMount')
  let updateComponent
  updateComponent = () => {
    vm._update(vm._render(), hydrating)
  }

  new Watcher(vm, updateComponent, noop, null, true /* isRenderWatcher */)
  hydrating = false
  return vm
}
```

### _render()
在_render函数里有几个常量render和_parentVNode，将_parentVNode设置为vnode，然后调用render函数。它会执行上面的匿名函数，在这期间它得到了“name”并调用dep.depend()来收集这个响应属性。这将返回一个vnode，设置parent并返回新的vnode
```js
Vue.prototype._render = function(): VNode{
  const vm: Component= this
  const { render, _parentVNode } = vm.$options

  vm.$vnode = _parentVNode
  let vnode = render.call(vm.renderProxy, vm.$createElement)

  vnode.parent = _parentVNode
  return vnode
}
```
### _update()
mounted之后，将调用beforeUpdate hook，设置preVnode和新的vnode，检查是否没有preVnode。如果没有就需要创建一个新的 DOM 节点并插入它。 否则，将之前的 Vnode 与生成的 Vnode 进行比较，并示更新次数最少。
```js
Vue.prototype._update = function (vnode: VNode, hydrating?: boolean) {
    const vm: Component = this
    if (vm._isMounted) {
        callHook(vm, 'beforeUpdate')
    }

    const preVnode = vm._vnode
    vm._vnode = vnode

    if (!preVnode) {
        vm.$el = vm.__patch__(vm.$el, vnode, hydrating, false, vm.$options._parentElm, vm.$options_refElm)
    } else {
        vm.$el = vm.__patch__(preVnode, vnode)
    }
}
```
好，我们再深入一层。这个__patch__函数是从哪里来的？这是Virtual DOM的一部分吗？
### __patch__

其实__patch__函数是在初始化时设置的，因为我们在浏览器中。在web/runtime/index.js，它检查是否在浏览器中，然后设置__patch__函数。patch是一个常量，它在runtime/patch.js里面声明，它调用createPatchFunction，它位于core/vdom/patch.js里面。和 ，有趣的是，这个函数以backend作为参数。在这里，发送与 DOM 交互的函数里面有nodeOps。那么，nodeOps从何而来？
```js
// runtime/index.js
Vue.prototype.__patch__ = inBrowser ? patch : noop
```
```js
// runtime/patch.js
export const patch: Function = createPatchFunction({nodeOps, modules})
```
```js
// core/vdom/patch.js
export function createPatchFunction(backend) {...}
```
### nodeOps
在runtime/node-ops.js里，可以 找到所有与 DOM 交互的函数。比如createElement，调用的是 document.createElement。又比如createTextNode，调用的是document.createTextNode。我们所有 DOM 函数都是在这里调用。我们将它们发送到虚拟DOM。它知道如何操作。它知道如何获取VNode并进行适当的调用来更新我们的DOM。所以如果我们想在另一个平台上使用虚拟 DOM，同样是没有问题的。

```js
export function createElement(tagName: String, vnode: VNode): Element{
    const elm = document.createElement(tagName)
}
export function createTextNode(text: steing): Text{
    return document.createTextNode(text)
}
```