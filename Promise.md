## Promise

### 1. Promise的用途
原有的异步回调方法的缺点：
* 命名不规范，例如：success+error，success+fail，done+error
* 容易出现回调地狱，代码难以读懂
* 很难进行错误处理

 Promise 的优点

* 规范回调的名字和顺序
* 链式调用，拒绝回调地狱
* 方便捕获错误

### 2. 如何创建一个 new Promise 
```js
return new Promise((resolve, reject)=> {})
```

### 3.如何使用 Promise.prototype.then 和 Promise.prototype.catch 

#### then

所有 Promise 都有 then 方法，它接受两个参数：第一个是当 Promise 的状态变为 fulfilled 时要调用的函数，与异步操作相关的附加数据都会传递给这个完成函数；第二个时当 Promise 的状态变为 rejected 时要调用的函数，其与完成时调用的函数类似，所有与失败状态相关的附加数据都会传递给这个拒绝函数。

 then 的两个参数都是可选的，所以可以按照任意组合的方式来监听 Promise ，执行完成或被拒绝都会被响应。

```js
let promise = new Promise((resolve, reject) => {
    
})

promise.then((contents)=> {
    //完成
    console.log(contents);
}, (err)=>{
    //拒绝
    console.error(err.message);
})

promise.then((contents)=> {
    //完成
    console.log(contents);
})

promise.then(null, (err)=>{
    //拒绝
    console.error(err.message);
})
```

上面这3次 then 调用操作的是同一个 Promise 。第一个同时监听了执行完成和执行拒绝；第二个只监听了执行完成，错误时不报告；第三个只监听了执行拒绝，成功时不报告。

####  catch 

 Promise 还有一个 catch 方法，相当于只给其传入拒绝处理程序的 then 方法。例如，下面的 catch 方法和 then 方法是等价的：

```js
promise.catch((err)=> {
    //拒绝
    console.error(err.message);
})

promise.then(null, (err)=>{
    //拒绝
    console.error(err.message);
})
```

 then 方法和 catch 方法一起使用才能更好地处理异步操作结构。这套体系能够清楚地指明操作结果是成功还是失败，比事件和回调函数更好用。如果使用事件，在遇到错误时不会主动出发；如果使用回调函数，则必须要记得每次都检查错误参数。如果不给 Promise 添加拒绝处理程序，那所有失败就自动被忽略了，所以一定要添加拒绝处理程序，即使只在函数内部记录失败的结果也行。

### 4.如何使用 Promise.all 

 Promise.all 方法只接受一个参数并返回一个 Promise ，该参数是一个含有多个受监视 Promise 的可迭代对象，只有可迭代对象中所有 Promise 都被解决后返回的 Promise 才会被解决，只有当可迭代对象中所有 Promise 都被完成后返回的 Promise 才会被完成，

```js
let p1 = new Promise((resolve, reject) => {
    resolve(11);
});

let p2 = new Promise((resolve, reject) => {
    resolve(12);
});

let p3 = new Promise((resolve, reject) => {
    resolve(13);
});

let p4 = Promise.all([p1, p2, p3]);

p4.then((value) => {
    console.log(Array.isArray(value));// true
    console.log(value[0]); // 11
    console.log(value[1]); // 12
    console.log(value[2]); // 13
})
```

在这段代码中，每个 Promise 解决时传入一个数字，调用 Promise.all 方法创建Promise p4，最终当Promise p1、p2和p3都处于完成状态后p4才被完成。传入p4完成处理程序的结果是一个包含每个解决之（11，12，13）的数组，这些值按照传入参数数组中 Promise 的顺序存储，所以可以根据每个结果来匹配对应的 Promise 。

所有传入 Promise.all 方法的 Promise 只要有一个被拒绝，那么返回的 Promise 没等所有 Promise 都完成就立即被拒绝。

```js
let p1 = new Promise((resolve, reject) => {
    resolve(21);
});

let p2 = new Promise((resolve, reject) => {
    reject(22);
});

let p3 = new Promise((resolve, reject) => {
    resolve(23);
});

let p4 = Promise.all([p1, p2, p3]);

p4.catch((value) => {
    console.log(Array.isArray(value)); // false
    console.log(value); // 22
})
```
在这个示例中，p2被拒绝并传入值22，没等p1或p3结束执行，p4的拒绝处理程序就立即被调用。拒绝处理程序总是接受一个值而非数组，改制来自被拒绝 Promise 的拒绝值。

### 5.如何使用 Promise.race 
 Promise.race 方法监听多个 Promise 的方法稍有不同：它也接受多个受监视 Promise 的可迭代对象作为唯一参数并返回一个 Promise ，但只要有一个 Promise 被解决返回的 Promise 就被解决，无须等到所有 Promise 都被完成。一旦数组中的某个 Promise 被完成， Promise.race 方法也和 Promise.all 方法一样返回一个特定的 Promise ，例如：

```js
let p1 = Promise.resolve(31);

let p2 = new Promise((resolve, reject) => {
    reject(32);
});

let p3 = new Promise((resolve, reject) => {
    resolve(33);
});

let p4 = Promise.race([p1, p2, p3]);

p4.then(value=>{
    console.log(value); // 31
})
```

在这段代码中，p1创建时便处于已完成状态，其他 Promise 用于编排任务。随后，p4的完成处理程序被调用并传入值31，其他 Promise 则被忽略。实际上，传给 Promise.race 方法的 Promise 会进行竞选，以决出哪一个先被解决，如果先解决的是已完成 Promise ，则返回已完成 Promise ；如果先解决的是已拒绝 Promise ，则返回已拒绝 Promise 

```js
let p1 = new Promise((resolve, reject) => {
    setTimeout(()=>{
        resolve(31);
    }, 0);
});

let p2 = Promise.reject(32);

let p3 = new Promise((resolve, reject) => {
    resolve(33);
});

let p4 = Promise.race([p1, p2, p3]);

p4.catch(value=>{
    console.log(value); // 32
})
```
此时，由于p2已处于被拒绝状态，因而当 Promise.race 方法被调用时，p4也被拒绝了，尽管p1和p3最终被完成，但由于是发生在p2被拒后，因此它们的结果被忽略掉。
