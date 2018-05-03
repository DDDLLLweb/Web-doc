## 原型与原型链
### 什么是原型
     每一个Javascript对象(null除外), 在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型“继承”属性。
    __proto__ :每一个JavaScript对象(除了 null )都具有的一个属性，这个属性会指向该对象的原型
```js
    function Person() {}
    var person = new Person();
    console.log(person.__proto__ === Person.prototype); // true
    person.constructor === Person.prototype.constructor
```
    constructor，每个原型都有一个 constructor 属性指向关联的构造函数。
### 什么是原型链
    相互关联的原型组成的链状结构就是原型链
    ![原型链](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype5.png)
### 补充
    原型中的继承
     继承意味着复制操作，然而 JavaScript 默认并不会复制对象的属性，相反，JavaScript 只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些

    