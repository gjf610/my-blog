## JS 对象
### 对象
  定义
- 无序的数据集合
- 键值对的集合
  写法
- <code>let obj = {'name': 'jimson', 'age': 18}</code>
- <code>let obj = new Object({'name': 'jimson'})</code>
  细节
- 键名是字符串，不是标识符，可以包含任意字符
- 引号可省略，省略之后就只能写表示符
- 就算引号省略了，键名页还是字符串（重要）
### 属性<font color="red">名</font>
每个key都是对象的属性名(property)
### 属性<font color="blue">值</font>
每个value都是对象的属性值
### 属性名
所有的属性名会自动变成字符串
```js
let obj = {
  1: 'a',
  3.2: 'b',
  1e2: true,
  1e-2: false,
  .234: true,
  0xFF: true
}
Object.keys(obj)
==>(6) ["1", "100", "255", "3.2", "0.01", "0.234"]
```
### 变量作属性名
<code>let p1 = 'name'</code>
<code>var obj = {p1: 'jimson'} </code>这样写，属性名为'p1'
<code>var obj = {[p1]: 'jimson'} </code>这样写，属性名为'name'
  对比

- 不加[]的属性名会自动变成字符串
- 加了[]则会当做变量，取变量值
- 值如果不是字符串，会自动变成字符串
### 对象的<font color=#FFD900>隐藏</font>属性
隐藏属性
- JS中每一个对象都有一个隐藏属性
- 这个隐藏属性储存着其<font color=#FFD900>共有属性组成的对象</font>的地址
- 这个<font color=#FFD900>共有属性组成的对象</font>叫原型
- 也就是说，隐藏属性储存着原型的地址
代码示例
```js
var obj = {}
obj.toString() //不会报错
```
因为obj的隐藏属性<font color=#FFD900>对应的对象</font>上有toString()
### 增删改查属性
#### 删除属性
- delete obj.xxx 或 delete obj['xxx']
  - 即可删除obj的xxx属性
  - 请区分“属性值为undefined”和“不含属性名”
- 不含属性名
  - <code>'xxx' in obj === false </code>
- 含有属性名，但是指为undefined
  - <code>'xxx' in obj&&obj.xxx===undefined</code>
- 注意obj.xxx === undefined
  - 不能断定'xxx'是否为obj的属性
#### 查看所有属性
- 查看自身所有属性
  - <code>Object.keys(obj)</code>
- 查看自身+共有属性
  - <code>console.dir(obj)</code>
- 判断一个属性是自身的还是共有的
  - <code>obj.hasOwnProperty('toString')</code>
#### 原型
每个对象都有原型
- 原型里存着对象的共有属性
- 比如obj的原型就是一个对象
- <code>obj.__proto__</code>存着这个对象的地址
- 这个对象里有toString/constructor/valueOf等属性
对象的原型也是对象
- 所有对象的原型也有原型
- obj={}的原型即为所有对象的原型
- 这个原型包含所有对象的共有属性，是<font color=#FFD900>对象的根</font>
- 这个原型也有原型，是null
#### 查看属性
两种方法查看属性
- 中括号语法：obj['key']
- 点语法：obj.key
- 迷惑语法：obj.[key] (变量key值一般不是字符串'key')
#### <font color="red">obj.name等价于obj['name']</font>
#### <font color="red">obj.name不等价于obj[name]</font>
#### 这里的<font color="red">name是字符串</font>，不是<font color="red">变量</font>
#### let name = 'jimson'
#### obj[name] 等价于obj['jimson']
#### 而不是obj['name']和obj.name
#### 修改或增加属性（写属性）
直接赋值
```js
let obj = {name:'jimson'};
obj.name = 'jimson';
obj['name'] = 'jimson';
```
<code>~~obj[name] = 'jimson';~~</code>
```js
obj['na'+'me'] = 'jimson';
let key = 'name';
let key = 'name';obj.key='jimson'
obj.key!==obj['name]
```
批量赋值
<code>Object.assign(obj, {age:18, gender: 'man'})</code>
### 修改或增加共有属性
无法通过自身修改或增加共有属性

- <code>let obj = {}, obj2 = {}</code> 共有toString
- <code>obj.toString = 'xxx'</code>只会改obj自身属性
- obj2.toString还是在原型上

偏要修改或增加原型上的属性
- <code>obj.__proto__.toString ='xxx'</code>不推荐使用__proto__
- <code>Object.prototype.toString = 'xxx'</code>
- 一般来说，不要修改原型，会引起很多问题
#### 修改隐藏属性
不推荐使用__proto__
```js
let obj = {name: 'jimson'}
let obj2 = {name: 'ling'}
let common = {kind: 'human'}
obj.__proto__ = common
obj2.__proto__ = common
```
推荐使用Object.create：创建一个新对象，使用现有的对象来提供新创建的对象的__proto__
```js
const obj = Object.create(common)
obj.name = 'jimson'
const obj2 = Object.create(common)
obj2.name = 'ling'
```
### 判断对象是否拥有某个属性in 运算符 和 Object.hasOwnProperty()的区别
1. Object.hasOwnProperty()判断对象自身属性中是否有指定的属性
```js
const obj = {
  name: 'jimson'
}
console.log(obj.hasOwnProperty('name')) // true
console.log(obj.hasOwnProperty('toString')) //false
```
在这个例子中，测试obj对象存在的name属性的时候，调用这个方法才返回true，而测试每个对象实例的原型链上存在toString方法，返回false。说明这个方法只是判断实例对象的自身属性，不包括原型链上的属性。

2. in 运算符 判断对象或其原型链中是否有指定的属性
```js
const car = { make: 'Honda' };
console.log('make' in car) // true
console.log('toString' in car) // true
```
在这个例子中，car对象make属性。然后分别打印<code>'make' in car</code>和<code>'toString' in car</code>，他们的结果都返回true。原因就是对象的原型链上存在toString方法，所以in操作不管是不是原型链上，只要存在这个属性，返回的就是true。

以上资料来源： &copy;饥人谷。
