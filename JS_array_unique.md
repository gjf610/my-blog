## 数组去重
```js
//测试数据
var arr = [1,1,'true','true',true,true,15,15,false,false, undefined,undefined, null,null, NaN, NaN,'NaN', 0, 0, 'a', 'a',{toString:"xiaowang"},{toString:"xiaowang"}];
```
```js
/*
方案一：数组双循环比较
  依次拿数组的每一项（排除最后一项：后面没有需要比较的内容了）
  和当前拿出项后面的每一项进行比较
  如果发现重复，把这个重复项再原有数组中删除（splice）
  缺点：1.算法时间复杂度为O(n^2)
  
       2.无法去除NAN与引用类型。
  注：使用数组的indexOf() 和 includes()方法与此方法类似
*/
const unique = (array) => {            
  for (var i=0; i<arr.length; i++) {
    for (var j=i+1; j<arr.length; j++) {
      if (arr[i]===arr[j]) {         //第一个等同于第二个，splice方法删除第二个
        arr.splice(j,1)
        j--
      }
    }
  }
  return arr
}
```
```js
/* 
方法二：利用对象属性名不能重复和hasOwnProperty 判断是否存在对象属性
  缺点：不能有与对象原型的方法一致的对象属性
*/
const unique = (array) => {
  const obj = {}
  return array.filter((item) => {
    return obj.hasOwnProperty(typeof item + item) ? false : (obj[typeof item + item] = true)
  })
}
```
```js
/* 
方案三：利用Set数据结构去重
  缺点：无法去除引用类型。
*/
function unique(array) {
  return Array.form(new Set(array))  
}
function unique(array) {
  return [...new Set(array)] 
}
```
```js
/* 
方案四：利用Map数据结构去重
  创建一个空Map数据结构，遍历需要去重的数组，把数组的每一个元素作为key存到Map中。
  由于Map中不会出现相同的key值，所以最终得到的就是去重后的结果。
  缺点：无法去除引用类型。
*/
const unique = (array) => {
  const mark = new Map()
  return array.filter((item) => !mark.has(item) && mark.set(item, 1))
}
```

