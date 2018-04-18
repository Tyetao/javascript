# javascript
> 原型

定义：原型是function对象的一个属性（Person.prototype），它定义了构造函数制造出的对象的公共祖先。
通过构造函数产生的对象，可以继承该原型的属性和方法。原型也是对象
```js
  // Person.prototype --原型
  // Person.prototype = {} -- 祖先
  person.lastname = '祖先'
  function Person(name) {
    // 隐式的声明，系统自己帮我们弄得。 let this = {__proto__: Person.prototype}
    this.name = name
  }
  let person = new Person('name')
  person.lastname // 组先
  person.constrcutor // 返回构造函数的构造器 function Person(){}
```
__proto__ 是一个指针指向对象的原型 person.prototype

> 原型链
定义：当构造函数的prototypr属性指向一个构造函数实例时，这个实例和它指向的这个实例就构成了一条原型链，
通过__proto__逐级向上查找（就近原则），最顶层Object.__proto__。
```js
Grand.prototype.lastname = 'Grand'
function Grand() {}
let grand=new Grand()

Father.prototype = grand
function Father() {}
let father=new Father()

Son.prototype = father
function Son() {}
let son=new Son()
son.lastname // Grand
```

> call/apply
定义：改变this指向。区别：传参不一样。
```js
function Person(name, age) {
  //this = obj
  this.name = name
  this.age = age
}

// 本来son函数没有name和age调用Person.call(this,name, age)之后，son就拥有了person的功能
function Son(height,name, age) {
  Person.call(this,name, age) // Person.call() == Person() 借用别人的函数实现自己的功能 obj = {name:'tan',age:24,height:122}
  this.height = height
}
let son = new Son(100, 'tan', 24)
```
