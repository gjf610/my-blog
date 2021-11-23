## 如何使用Vue

### 关于引入Vue的两个版本
* 完整版（Full->vue.js）：同时包含编译器和运行时的版本。
* 运行时版本（Runtime->vue.runtime.js）：用来创建 Vue 实例、渲染并处理虚拟 DOM 等的代码。基本上就是除去编译器的其它一切。

ps: 编译器（Compiler）：用来将模板字符串编译成为 JavaScript 渲染函数的代码。
### 如何使用完整版和运行时版本

为了保证良好的开发体验，开发者可以传入一个HTML字符串给 template 选项，或挂载到一个元素上并以其 DOM 内部的 HTML 作为模板。再使用编译器获得 JavaScript 渲染函数的代码，然后运行，即使用完整版：

```js
// 需要编译器
new Vue({
  template: `
    <div id="app">
        {{ n }}
        <button @click="add">+1</button>
    </div>
  `,
  data: {
    n: 0
  },
  methods: {
    add() {
      this.n += 1
    }
  }
})
```

为了保证用户体验，使用户下载的应用更小体积，就是用运行时版本（直接在render()里写编译好的语法）：

```js
// 不需要编译器
new window.Vue({
  el: '#app',
  render(createElement) {
    const h = createElement;
    return h('div', [this.n, h('button', { on: { click: this.add } }, '+1')])
  },
  data: {
    n: 0
  },
  methods: {
    add() {
      this.n += 1
    }
  }
})

```

### 如何才能做到最佳实践？
*  总是使用运行时版本，然后配合vue-loader和vue文件。
1. 让编译模板的工作交给vue-loader，把HTML字符串编译成为render函数的代码。
2. 开发者只编写vue文件里的代码。


### 在codesandbox里写Vue代码
在学习Vue时，我们可以通过codesandbox方便快捷创建项目和浏览项目效果

1. 访问[codesandbox](https://codesandbox.io/)
2. 点击“start coding for free”
3. 选择Vue的模板，然后网站就会创建好项目和预览效果
4. 在左侧files栏中选中要编辑的文件，中间就会有对应文件的代码，修改好代码后右侧预览效果会实时更新
5. 在获得想要的项目后，点击左侧files栏中的“Export to ZIP”就会下载对应的代码到本地中
6. 解压后，下载对应的node_modules，就可以运行项目