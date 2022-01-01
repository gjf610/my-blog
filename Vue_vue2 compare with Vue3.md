## Vue2与Vue3的使用比较
目前使用的是非setup语法
### 使用组件
* 使用ref来声明一个响应式数据
* 需要引入defineComponent，将组件抛出的代码包裹起来。这样模板里才不会报错
```ts
import { defineComponent，ref } from 'vue'
export default defineComponent({
  components: {Switch},
  setup() {
    const menuVisible = ref<boolean>(true)
    const toggleMenu = () => {

    }
    return { menuVisible, toggleMenu }
  },
})
```
```html
<div class="menu" v-if="menuVisible"></div>
<span class="toggleAside" @click="toggleMenu"></span>
```
### v-model
* 新 v-model 代替以前的 v-model 和 .sync
* 新增 context.emit，与 this.$emit 作用相同
父组件
```xml
<template>
  <Switch v-model:value="bool"/>
</template>
```
```ts
import { defineComponent, ref } from 'vue'
import Switch from "../lib/Switch.vue";
export default defineComponent({
  components: {Switch},
  setup() {
    const bool = ref<boolean>(false)
    return { bool }
  },
})
```
子组件
```xml
<template>
  <button :class="{checked: value}" @click="toggle"></button>
</template>
```
```ts
import {  defineComponent } from 'vue'
export default defineComponent({
  props: {
    value: Boolean
  },
  setup(props, context) {
    const toggle = () => {
      context.emit('update:value', !props.value)
    }
    return {toggle}
  }
})
```

### 属性绑定
* context.attrs 获取所有属性
* 使用 const {size, level, ...xxx} = context.attrs 将属性分开

