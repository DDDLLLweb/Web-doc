## 原型与原型链
### 什么是原型
每一个Javascript对象(null除外), 在创建的时候就会与之关联另一个对象，这个对象就是我们所说的原型，每一个对象都会从原型“继承”属性。 

    __proto__ :每一个JavaScript对象(除了 null )都具有的一个属性，这个属性会指向该对象的原型  
constructor，每个原型都有一个 constructor 属性指向关联的构造函数。
```js
    function Person() {}
    var person = new Person();
    console.log(person.__proto__ === Person.prototype); // true
    person.constructor === Person.prototype.constructor //true 
    Person === Person.prototype.constructor    //true  
    // 顺便学习一个ES5的方法,可以获得对象的原型
    console.log(Object.getPrototypeOf(person) === Person.prototype) // true
```
### 原型对象
JavaScript 规定，每个函数都有一个prototype属性，指向一个对象。<span style ="color: pink">对于普通函数来说，该属性基本无用。但是，对于构造函数来说，生成实例的时候，该属性会自动成为实例对象的原型。</span>    
原型对象的作用，就是定义所有实例对象共享的属性和方法。这也是它被称为原型对象的原因，而实例对象可以视作从原型对象衍生出来的子对象。  
### 什么是原型链
Object.prototype 的原型是null，原型链的尽头是null   

    console.log(Object.prototype.__proto__ === null)  //true
- 定义： 相互关联的原型组成的链状结构就是原型链
- 定义：JavaScript 规定，所有对象都有自己的原型对象（prototype）。一方面，任何一个对象，都可以充当其他对象的原型；另一方面，由于原型对象也是对象，所以它也有自己的原型。因此，就会形成一个“原型链”（prototype chain）：对象到原型，再到原型的原型……
可结合二者来解释原型链

![原型链](https://github.com/mqyqingfeng/Blog/raw/master/Images/prototype5.png)

### 原型的原型
读取对象的某个属性时，JavaScript 引擎先寻找对象本身的属性，如果找不到，就到它的原型去找，如果还是找不到，就到原型的原型去找。如果直到最顶层的Object.prototype还是找不到，则返回undefined。如果对象自身和它的原型，都定义了一个同名属性，那么优先读取对象自身的属性，这叫做“覆盖”（overriding）。
### 补充
>原型中的继承
>>继承意味着复制操作，然而 JavaScript 默认并不会复制对象的属性，相反，JavaScript 只是在两个对象之间创建一个关联，这样，一个对象就可以通过委托访问另一个对象的属性和函数，所以与其叫继承，委托的说法反而更准确些

    