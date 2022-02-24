## 手写Promise.all
  ```js
Promise.all2 = (promiseList) => {

  return new Promise((resolve, reject) => {
    const result = []
    const length = promiseList.length
    let count = 0
    promiseList.map((promise, index) => {
      promise.then((data) => {
        result[index] = data
        count += 1
        if (count === length - 1) {
          resolve(result)
        }
      }, (r) => {
        reject(r)
      })
    })
  })
}

const promiseList = [Promise.resolve(1), Promise.reject(2), Promise.resolve(3)]
Promise.all2(promiseList).then(data => console.log(data), r => console.error(r))
```

