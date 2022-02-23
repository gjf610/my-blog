## 手写发布订阅
```js
const eventHub = {
  map: {},
  on: (name, fn,) => {
    eventHub.map[name] = eventHub.map[name] || []
    eventHub.map[name].push(fn)
  },
  emit: (name, data) => {
    const q = eventHub.map[name]
    if (!q) return
    q.map(f => f.call(null, data))
  },
  off: (name, fn) => {
    const q = eventHub.map[name]
    if (!q) { return }
    const index = q.indexOf(fn)
    if (index < 0) { return }
    q.splice(index, 1)
  },
  once: (name, fn) => {
    function one() {
      fn.apply(this, arguments);
      eventHub.off(name, one)
    }
    eventHub.on(name, one)
  }
}

eventHub.on('click', console.log)
eventHub.once('ggg', (mine) => { console.log(mine) })

setTimeout(() => {
  eventHub.emit('click', 'frank')
}, 2000)
```