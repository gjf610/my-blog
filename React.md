## React

## React两种组件
一、函数组件
```js
function Welcome(props){
    return <h1>Hello, {props.name}</h1>
}
使用方法：<Welcome name="jim"/>
```
二、类组件
```js
class Welcome extends React.Component{
    constructor(props) {
        super(props)
    } 
    return (<h1>Hello, {this.props.name}</h1>)
}
使用方法：<Welcome name="jim"/>
```
### 类组件注意事项
#### this.state.n += 1无效？
其实n已经改变了，只不过UI不会自动更新而已，调用setState才会触发UI更新（异步更新）。

ps:因为React没有像Vue监听data一样监听state
#### setState会异步更新UI
setState之后，state不会马上改变，立即读state会失败。更推荐的方式是setState((state)=>({}))来修改state
#### 不推荐this.setState(this.state)
React希望我们不要修改就state（数据不可变原则）

常用代码：setState({n: this.state.n + 1})
### 函数组件注意事项
#### 修改数据
```js
const [n, setN] = useState(0)
```
使用useState来设置数据和修改数据的方法

第一个参数n代表设置的数据，第二个参数setN代表修改数据和更新UI
#### 跟类组件不同的地方
没有this，一律用参数和变量

### 复杂state
#### 如果state里不止有n怎么办
* 类组件的setState会自动合并第一层属性，但是不会合并第二场及以下的属性，需要使用Object.assign或者...操作符
* 函数组件的setX则完全不会合并，要自己使用Object.assign或者...操作符来合并属性
### 两种编程模型
#### Vue的编程模型
* 一个对象，对应一个虚拟DOM
* 当对象的属性改变时，把属性相关的DOM节点全部更新
注：Vue为了其他考量，也引入了虚拟DOM和DOM diff
#### React的编程模型
* 一个对象，对应一个虚拟DOM，另一个对象，对应另一个虚拟DOM
* 对比两个虚拟DOM，找不同（DOM diff）最后局部更新DOM
#### 为什么React与Vue在DOM调用函数时this会不一样？
* 在React里编译后是DOM.call(null, fn)，这是就要手动帮fn绑定this。
* 而在Vue里方法会挂在到整个Vue对象上，调用时就会自动找到this
### React V.S. Vue
* 都是对视图的封装，React是用类和函数来表示一个组件，而Vue是通过构造option选项构造一个组件
* 都提供了createElement的XML简写，React提供的是JSX语法，而Vue是提供的模板语法
* React是把HTML放在JS里写（HTML in JS），而Vue是把JS放在HTML里写（JS in HTML）
