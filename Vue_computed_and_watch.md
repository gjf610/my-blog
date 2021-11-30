## computed 和 watch的区别

### computed
1. computed是计算属性
2. computed是用来计算一个值的，不需要加括号，可以在模板内直接调用，当做vue的属性来用
3. 它会根据依赖是否变化来缓存，如果依赖不变它的值不会重新计算
### watch 
1. watch是监听的意思，一旦监听的属性变化了，就执行函数
2. deep含义，监听一个对象里面属性的变化；immediate含义，如果表示是否在第一次渲染的时候执行函数
3. 可以通过options.watch和this.$watch来设置