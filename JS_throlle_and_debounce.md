## 手写节流防抖
### 节流
```js
const d = () => {
  console.log('闪现')
}

const throttle = (f, time) => {
  let timer = null;
  return (...args) => {
    if (timer) { return }
    f.call(null, ...args)
    timer = setTimeout(() => {
      timer = null
    }, time)
  }
}

const d2 = throttle(d, 2000)
```
### 防抖
```js
const f = () => {
  console.log('回城成功')
}

const debounce = (f, time) => {
  let timer = null

  return (...args) => {
    if (timer) {
      clearTimeout(timer)
    }
    timer = setTimeout(() => {
      f.call(null, ...args)
      timer = null
    }, time)
  }
}

const tp = debounce(f, 3000)
```