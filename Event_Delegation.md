## 事件委托
在JavaScript世界中，事件委托是最经常使用的方法论之一。事件委托使您可以避免将事件侦听器（Event Listener）添加到特定节点；而是将事件侦听器添加到一个父级元素。该事件侦听器分析冒泡事件以找到子元素的匹配项。下面然我来演示事件委托的工作过程。
### 场景一：
假设我们有一个div元素里带有若干个button子元素：
```html
<div id="parent">
  <span>span 1</span>
  <button>click 1</button>
  <button>click 2</button>
  <button>click 3</button>
  <button>click 4</button>
  <button>click 5</button>
  <button>click 6</button>
  <button>click 7</button>
</div>
```
当单击每个子元素时需要发生一些事情。你可以为每个单独的button元素添加一个单独的事件侦听器。但是这样button元素的事件侦听器就会频繁地从列表中添加和移除，添加和移除事件侦听器将是一场噩梦，尤其是如果添加和移除事件侦听器的代码位于应用程序中的不同位置。那我们该如何优化？更好的解决方案是将事件侦听器添加到父div元素。但是如果将事件侦听器添加到父级，UL里还有其他后代元素，那么你如何知道单击了哪个元素？

简单来说：当事件冒泡到div元素时，你可以检查事件对象的target属性以获得对实际单击的节点的引用。这是一个非常基本的JavaScript代码段，用于说明事件委托：
```js
//获取元素，添加一个click listener ... 
const parent = document.getElementById("parent")
parent.addEventListener("click", (e)=> {
  // e.target点击了element
  // 如果是 button 子项
  const t = e.target
  if(t && t.tagName.toLowerCase() === "button") {
    console.log('button 被点击了')
    console.log('button内容是' t.textContent);
  }
});
```

### 场景二：
要监听目前不存在的元素的点击事件,假设要监听1个按钮，但是按钮需要一秒钟之后才会生成.
```html
<div id=div1>

</div>
```
```js
setTimeout(()=>{
  const button = document.createElement('button')
  button.textContent = 'click 1'
  div1.appendChild(button)
}, 1000)

div1.addEventListener('click', (e)=>{
  const t = t.target
  if(t && t.tagName.toLowerCase() === "button"){
    console.log('button 被 click')
  }
})
```
这里button元素一开始并没有在HTML里，是不能直接监听button元素。只有通过监听祖先，等点击的时候看看是不是想要监听的元素即可。

### 优点：
1. 节省监听的数量，可以省内存
2. 可以监听动态元素

部分资料来源： &copy;饥人谷