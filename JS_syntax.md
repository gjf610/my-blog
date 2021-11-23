## JS 语法

### 一、语句和表达式

语句（statement）是为了完成某种任务而进行的操作，比如下面就是一行赋值语句。

```js
var a = 8 + 8;
```

<code>8 + 8</code> 叫做表达式（expression），指一个为了得到返回值的计算式。
还有函数表达式<code>add(8,8)</code>

#### 语句和表达式的区别

- 前者主要为了进行某种操作，一般情况下不需要返回值
- 后者则是为了得到返回值，一定会返回一个值。
- 凡是 JavaScript 语言中预期为值的地方，都可以使用表达式。比如，赋值语句的等号右边，预期是一个值，因此可以放置各种表达式。

### 二、空格

大部分空格没有实际意义
<code>var a = 1</code> 和 <code>var a=1</code> 没有区别
加回车大部分时候也不影响结果，除了<code>return</code>后面

### 三、标识符

标识符（identifier）指的是用来识别各种值的合法名称。最常见的标识符就是变量名，以及后面要提到的函数名。
规则如下：

- 第一个字符，可以是 Unicode 字母（包括英文字母和其他语言的字母），以及美元符号（\$）和下划线（\_）。
- 后面的字符，除了 Unicode 字母、美元符号和下划线，还可以用数字 0-9。

变量名也是标识符。

### 四、区块 block

把代码包在一起

```js
{
  let a = "a";
  let b = "b";
}
```

常与 if/for/while 合用

### 五、判断语句

#### if else 语句

- 语法

if(表达式){语句 1}else{语句 2}

{}在语句只有一句的时候可以省略，不建议这样做

- 变态情况

  表达式里可以非常变态，如<code>a = 1</code>;

  语句 1 里可以非常变态，如嵌套的 ifelse;

  语句 2 里可以非常变态，如嵌套的 ifelse;

  缩进也可以很变态，如

  ```js
  a = 3;
  if (a === 2) console.log("a");
  console.log("a等于2");
  ```

#### switch 语句

- 语法

```js
switch (fruit) {
  case "banana":
    // ...
    break;
  case "apple":
    // ...
    break;
  default:
  // ...
}
```

- break

  大部分时候，不可以省略 break

  少部分时候，可以利用 break

#### 问号冒号表达式（三元运算符）

“条件表达式?表达式 1:表达式 2”

#### &&短路逻辑

A&&B&&C&&D 取第一个假值或 D
并不会取 true/false

#### ||短路逻辑

A||B||C||C 取第一个真值或 D
并不会取 true/false

### 六、循环语句

#### while 循环

- 语法

while(表达式){语句}
判断表达式的真假
当表达式为真，执行语句，执行完在判断表达式的真假
当表达式为假，执行后面的语句

#### for 循环

- 语法糖
  for 是 while 循环的方便写法
- 语法
  for(语句 1;表达式 2;语句 3){
  循环体
  }
  先执行语句 1
  然后判断表达式 2
  如果为真，执行循环体，然后执行语句 3
  如果为假，直接退出循环，执行后面的语句

#### break 和 continue

退出所有循环/退出当前一次循环

### 七、label 语句

- 语法

```js
foo: {
  console.log(1);
  break foo;
  console.log('本行不会输出')；
}
console.log(2)
```
结果：1，2
- 面试

```js
{
  foo: 1
}
```
Chrome会返回对象（加分号才返回1），firefox会返回1

部分资料来源： &copy;饥人谷和阮一峰的[JavaScript 的基本语法](https://wangdoc.com/javascript/basic/grammar.html)