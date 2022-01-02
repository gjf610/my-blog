# JS 的继承
面向对象编程很重要的一个方面，就是对象的继承。A对象通过继承B对象，就能直接拥有B对象的所有属性和方法。这对于代码的复用是非常有用的。
大部分面向对象的编程语言，都是通过“类”（class）实现对象的继承。在ES6之前，JavaScript语言的继承是不能用class语法来实现，而是通过“原型对象”（prototype）来实现。

这篇文章会分别介绍这两种继承方式。
## 原型对象继承

### 1. 构造函数
```js
function Student() {}
Student.prototype.learn = function() {
  return true;
}
const student1 = Student();
console.assert(student1 === undefined, "No instance student created.");

const student2 = new Student();
console.assert(student2 && student2.learn && student2.learn(), "Instance exists and method is callable.");
```
在这段代码中，定义了一个名为Student的空函数，并通过两种方式进行调用：
一种是作为普通函数调用，<code>const student1 = Student();</code>;
另一种是作为构造器进行调用，<code>const student2 = new Student();</code>

当函数创建完成之后，立即就获得了一个__proto__，我们可以对改prototype进行扩展。在本例中，在prototype上添加了<code>learn</code>方法
```js
Student.prototype.learn = function() {
  return true;
}
```
接下来，对函数进行调用。
首先，是作为普通函数进行调用，并将返回结构存储在变量student1中，查看函数体会发现，函数并没有返回值，所以检测student1的值为undefined。作为一个简单的函数，Student看起来作用不大。
其次，通过new操作符调用该函数，此次是作为构造器进行调用，发生了完全不同的事情。再次调用这个函数，但这一次已经创建了新分配的对象，并将其设置为函数的上下文（通过this关键字访问）。new操作符返回的结构是这个新对象的引用。然后测试student2是新创建的对象的引用，具有learn方法，并调用learn方法。

我们创建的每一个函数都具有一个新的__proto__。最初的prototype只有一个属性，即constructor属性。该属性指向函数本身。
当我们将函数作为构造器进行调用时，新构造出来的对象的__proto__被设置为构造函数的prototype的引用。
在Student.prototype上增加了learn方法，对象student2创建完成时，对象student2的__proto__被设置为构造函数Student的prototype。因此通过student2调用learn方法，将查找该方法委托到Student的prototype上。注意，所有通过构造函数Student创建出来的对象都可以访问learn方法。现在我们就得到了一种代码复用的方式！

### 2. 实现继承

继承是一种在新对象上服用现有对象的属性的形式。这有助于避免重复代码和重复数据。在JavaScript中，继承原理与其他流行的面向对象语言略有不同，接下来看看该如何实现：
```js
function Person() {
   this.name = name;
}
Person.prototype.sayHi = function() { };

function Student() {}
Student.prototype = new Person();

const student = new Student();
console.assert(!(student instanceof Student), "student receives functionality from the Student prototype");
console.assert(!(student instanceof Person), "... and the Person prototype");
console.assert(!(student instanceof Object), "... and the Object prototype");
console.assert(!(typeof student.sayHi === "function"), "... and can say Hi.");

```
在上面的例子中先后定义了Person与Student。显然Student是一个Person，因为使用Person的实例来作为Student的原型。运行测试会发现这中方式实现了继承。

现在来进一步观察创建新的student对象后的运行状态：

```js
function Person() {
   this.name = name;
}
Person.prototype.sayHi = function() { };
```
首先，这里定义了一个Person函数时，也创建了Person的prototype，该prototype通过其constructor属性引用函数本身。正常来说，我们可以使用附加属性来扩展Person的prototype，在本例中，Person的prototype扩展了sayHi方法，因此每个Person实例对象也都具有sayHi方法。

```js
function Student() {}
```
其次，定义一个Student函数。该函数的prototype也具有一个constructor属性指向Student函数本身。
```js
Student.prototype = new Person();

const student = new Student();
```
再次，为了实现继承，将Student的prototype赋值为Person的实例。现在，每当创建一个新的student对象时，新创建的student对象将设置为Student的prototype属性所指向的对象，即Person实例。
尝试通过student对象访问sayHi方法，JavaScript运行时将会首先查找student对象本身。由于student对象本身不具有sayHi方法，接下来搜索student对象的__proto__即Person对象。Person对象也不具有sayHi方法，所以再接着查找Person对象的__proto__，最终找到了sayHi方法。这就是在JavaScript中实现继承的原理！

## 使用class语法实现继承
虽然JavaScript本身不支持类的继承，但还是不可避免地进入了类的范畴。为了解决类的问题，出现了一些模拟类的继承的JavaScript库。由于每个类库对类的实现都有不同的方式，ECMA委员会对“模拟”基于类的继承语法进行标准化。注意是“模拟”。虽然现在我们可以在JavaScript中使用关键字class，但其底层的实现仍然是基于原型继承！

### 1. 关键字class

```js
class Person {
  constructor(name){
    this.name = name;
  }
  sayHi(){
    return `你好，我叫${this.name}`;
  }
}

const xiaoWang = new Person('xiaoWang');
```
我们可以通过使用ES6的关键字class创建Person类，在类中创建构造函数，使用类创建实例对象时，调用该构造函数。在构造函数体内，可以通过this访问新船舰的实力，添加属性很简单，例如添加那么书信。在类中，还可以定义所有实例对象均可访问的方法。

#### class是语法糖
前面叶提到过，虽然ES6引入关键字class，但底层仍然是基于原型的实行。上面使用关键字class的代码可转换成如下ES5 的代码:
```js
function Person() {
   this.name = name;
}
Person.prototype.sayHi = function() {
  return `你好，我叫${this.name}`;
}
```
#### 静态属性
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  getName() {
    return this.name;
  }

  static compare(person1, person2) {
    return person1.age - person2.age;
  }
}

const person1 = new Person("Adam", 24);
const person2 = new Person("Eve", 22);

console.assert(("compare" in person1) && !("compare" in person2), "A  person instance doesn't know how to compare" );
console.assert(Person.compare(person1, person2), "The Person class can do the comparison");
console.assert(("getName" in Person), "The Person class cannot get name");
```
我继续创建了Person类，该类具有所有实例对象均可访问的getName方法。同时，我通过关键字static定义了一个静态方法compare。

compare方法比较两个person实例对象的原型属性age，而不是实例属性。接着我验证了实例不可访问compare方法，而Person类可以访问compare方法。

接下来看看ES6之前的版本中是如何实现“静态”方法的。只需要记住通过函数来实现类。由于静态方法是类级别的方法，所以可以利用第一类型对象，在构造函数上添加方法
```js
function Person() {};
Person.compare = function(person1, person2) {...}

```

### 2. 实现继承
```js
class Person {
  constructor(name){
    this.name = name;
  }
  run() {
    return true;
  }
}

class Student extends Person {
  constructor (name, id) {
    super(name);
    this.id = id;
  }
  learn() {
    return true;
  }
}

const person = new Person("Bob");
console.assert(person instanceof Person, "A person's a person");
console.assert(person.run(), "A person can run.");
console.assert(person.name === "Bob", "We can call it by name");
console.assert(person instanceof Student, "But it's not a Student");
console.assert("learn" in Person, "And it cannot learn");

const student = new Student("xiaoWang", 03061973333);
console.assert(student instanceof Student, "A student's a student");
console.assert(student.learn(), "That can learn");
console.assert(student instanceof Student, "But it's also a person");
console.assert(student.name ==="xiaoWang", "That has a name");
console.assert(student.run(), "And can run");

```
在上面的例子中，创建Person类，其构造函数对每一个实例对象添加name属性。同时，定义一个所有Person的实例均可访问的run方法。
```js
class Person {
  constructor(name){
    this.name = name;
  }
  run() {
    return true;
  }
}
```
接着，定义一个从Person类继承而来的Student类。在Student类上添加id属性和learn方法。
```js
class Student extends Person {
  constructor (name, id) {
    super(name);
    this.id = id;
  }
  learn() {
    return true;
  }
}
```
衍生类Student构造函数通过关键字super调用基类Person的构造函数。这与其他基于类的怨言是类似的。
继续创建person实例，并验证Person类的实例具有name属性与run方法，但不具有learn方法
```js
const person = new Person("Bob");
console.assert(person instanceof Person, "A person's a person");
console.assert(person.run(), "A person can run.");
console.assert(person.name === "Bob", "We can call it by name");
console.assert(person instanceof Student, "But it's not a Student");
console.assert("learn" in Person, "And it cannot learn");
```
同时，我们也创建一个student实例，并验证student是Student类的实例，具有learn方法。由于所有的student同时也是Person类的实例，因此student实例也具有name属性和run方法。
```js
const student = new Student("xiaoWang", 03061973333);
console.assert(student instanceof Student, "A student's a student");
console.assert(student.learn(), "That can learn");
console.assert(student instanceof Student, "But it's also a person");
console.assert(student.name ==="xiaoWang", "That has a name");
console.assert(student.run(), "And can run");
```