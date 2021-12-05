## 事件目标处理
### target V.S. currentTarget
区别
* e.target-用户操作的元素
* e.currentTarget-程序员监听的元素

### 取消冒泡
捕获不可以取消，但冒泡可以
用 stopPropagation() 当在事件对象上调用该函数时，它只会让当前事件处理程序运行，但事件不会在冒泡链上进一步扩大，因此将不会有更多事件处理器被运行(不会向上冒泡)。
```js
video.addEventListener('click', (e)=>{
  e.stopPropagation();
  video.play();
});
```
### 不可阻止默认动作
有些事件不能阻止默认动作
* [scroll event](https://developer.mozilla.org/en-US/docs/Web/API/Document/scroll_event)，看到Bubbles和Cancelable
* Bubbles是该事件是否冒泡，所有冒泡都可以取消
* Cancelable是开发者是否可以阻止默认事件
### 插曲：如何阻止滚动
#### scroll事件不可阻止默认动作
* 阻止scroll默认动作没用，因先有滚动才有滚动事件
* 阻止滚动，可以阻止wheel和touchstart的默认动作
* 注意你需要找准滚动条所在的元素
```js
x.addEventListener('wheel', (e)=>{
  console.log(2)
  e.preventDefault()
})
```
* 但是滚动条还能用，可用CSS让滚动条<code>width: 0</code>
#### CSS也行
* 使用overflow: hidden可以直接取消滚动条
* 但此时JS依然可以修改scrollTop

### 开发者自定义事件
```js
button_1.addEventListener('click', ()=>{
  const event = new CustomEvent('ling', {
    "detail": {
      name: 'ling', 
      age: 27
    }
  })
  button_1.dispatchEvent(event)
})
button_1.addEventListener('ling', (e)=>{
  console.log('xiaowang')
  console.log(e)
})
```

资料来源： &copy;饥人谷