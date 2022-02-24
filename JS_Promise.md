## 手写Promise
```js
class Promise2 {
  #status = 'pending'
  constructor(fn) {
    this.q = []
    const resolve = (data) => {
      this.#status = 'fulfilled'
      const f1f2 = this.q.shift()
      if (!f1f2 || !f1f2[0]) return
      const x = f1f2[0].call(undefined, data)
      if (x instanceof Promise2) {
        x.then((data) => {
          resolve(data)
        }, () => {
          reject(reason)
        })
      } else {
        resolve(x)
      }
    }
    const reject = (reason) => {
      this.#status = 'rejected'
      const f1f2 = this.q.shift()

      if (!f1f2 || !f1f2[1]) return
      const x = f1f2[1].call(undefined, reason)
      if (x instanceof Promise2) {
        x.then((data) => {
          resolve(data)
        }, () => {
          reject(reason)
        })
      } else {
        resolve(x)
      }
    }
    fn.call(undefined, resolve, reject)
  }
  then(f1, f2) {
    this.q.push([f1, f2])
    return new Promise2()
  }
}

const p = new Promise2(function (resolve, reject) {
  setTimeout(() => {
    reject('hi')
  }, 3000)
})

p.then((data) => { console.log(data) }, (err) => { console.log(err) })
```
