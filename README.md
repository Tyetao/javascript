# javascript入门到放弃
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

Fanter.prototype = grand
function Fanter() {}
let father=new Fanter()

Son.prototype = fanter
function Son() {}
let son=new Son()
son.lastname // Grand
```

![Alt text](原型链.png)

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
> 继承  
- 使用原型链继承。  
  缺点：过多的继承无用的属性。
 ```js
Grand.prototype.lastname = 'Grand'
function Grand() {}
let grand = new Grand()

Father.prototype = grand
function Father() {}
let father = new Father()

Son.prototype = Father
function Son() {}
let son = new Son()
son.lastname // Grand
 ```
- 使用构造函数（call/apply）继承。  
  缺点： 1.不能继承借用构造函数函数的原型。2.每次构造函数都要走一个函数  
 ```js
 function Person(name, age) {
  //this = obj
  this.name = name
  this.age = age
}
function Son(height,name, age) {
  Person.call(this,name, age)
  this.height = height
}
 ```
- 共享原型
  缺点：1.不能随便改动自己的原型。2.不能继承实例的东西
```js
  Fanter.prototype.lastname = 'fanter'
  function Father() {
    this.name = 'name'
  }
  function Son() {
  }
  Son.prototype = Father.prototype(如果改自己的原型，别人的原型也跟着改。因为指向同一个空间)
  let son = new Son()
  son.name // undefined
```
- 圣杯模式
```js
  function inherit(Target, Origin) {
    function F() {} // 中间层  
    F.prototype = Origin.prototype
    Target.prototype = new F()
    Target.prototype.constructor = Target // 归位子类constructor
    Target.prototype.super = Origin.prototype // 标记继承于谁
  }
  function Father() {}
  function Son() {}
  inherit(Son, Father)
  let son = new Son()
  let father = new Father()
```
> 命名空间
