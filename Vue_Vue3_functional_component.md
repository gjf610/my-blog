## VUE3函数式组件写法(.tsx文件)
### 1普通函数式
```ts
export default () => <div>Tesla</div>
```

### 2 defineComponent() render Options API
缺点：和this交互
```ts

import { defineComponent } from "vue";
export default defineComponent({
  render() {
    return <div>Tesla</div>
  }
})
```

### 3defineComponent() setup Composition API
```ts
import { ref,defineComponent } from 'vue'
export default defineComponent({
  setup() {
    const count = ref<number>(0)
    return () => (
      <>
        <div>Tesla</div>
        <input type="text" v-model={count.value} />
      </>
    )
  }
})
```