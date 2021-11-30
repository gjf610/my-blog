## vue 修饰符.sync
在Vue中时实现不了真正的“双向绑定”，需要通过$emit来通知对应的绑定事件。实现当一个子组件改变了一个 prop的值时，把变化同步到父组件中所绑定的值。
```html
<template>
  <div id="app">
    <h2>{{ title }}{{ count }}</h2>
    <Counter :money.sync="count" @update="count = $event"></Counter>

  </div>
</template>
<script>
import Vue from "vue";
Vue.component('Counter',{
  template:`<button @click="$emit('update', money - 800)">clicked -800.</button>`,
  props: ["money"],
})

export default {
  components: {
    Counter,
  },
  data() {
    return {
      count: 10000,
      title: "我现在有",
    };
  },
};
</script>
```
同时Vue推荐使用update:****** 的模式触发事件取代@update。因为它可以清晰表达触发事件而修改值的含义。因此在组件内可以用以下方法表达对其赋新值的意图：

```js
this.$emit('update:money', money - 800)
```
然后父组件可以监听那个事件并根据需要更新一个本地的数据
```html
<Counter :money="count" @update:money="count = $event"></Counter>
```

为此Vue提供了一个语法糖方便使用
```html
<Counter :money.sync="count"></Counter>
```


整理后得到一下代码
```html
<template>
  <div id="app">
    <h2>{{ title }}{{ count }}元</h2>
    <Counter :money.sync="count"></Counter>
  </div>
</template>
<script>
import Vue from "vue";
Vue.component('Counter',{
  template:`<button @click="$emit('update:money', money - 800)">clicked -800.</button>`,
  props: ["money"],
})

export default {
  components: {
    Counter,
  },
  data() {
    return {
      count: 10000,
      title: "我现在有",
    };
  },
};
</script>
```

