# 1. 简介
ES6 提供了更接近传统语言的写法，引入了 Class（类）这个概念，作为对象的模板。通过class关键字，可以定义类
生成实例对象的传统方法是通过构造函数，如下：
```js
function Point(x, y) {
  this.x = x;
  this.y = y;
}

Point.prototype.toString = function () {
  return '(' + this.x + ', ' + this.y + ')';
};

var p = new Point(1, 2);
```
用class改写
```js
class Point {
    constructor (x,y){
        this.x = x;
        this.y = y;
    }
    toString(){
        return '(' + this.x + ', ' + this.y + ')';
    }
}
let p = new Point(1,2);
p.toString();//"(1,2)"
```
注意，定义“类”的方法的时候，前面不需要加上function这个关键字，直接把函数定义放进去了就可以了。另外，方法之间不需要逗号分隔，加了会报错    

类的数据类型就是函数，类本身就指向构造函数,如下：

```js
    class Point {
    // ...
    }

    typeof Point // "function"
    Point === Point.prototype.constructor // true
```
构造函数的prototype属性，在 ES6 的“类”上面继续存在。事实上，类的所有方法都定义在类的prototype属性上面。    
类的内部所有定义的方法，都是不可枚举的（non-enumerable）,与es5不一致
# 2. 严格模式
类和模块的内部，默认就是严格模式，不需要在用`use strict`指定运行模式
# tconstrucor方法
constructor方法是类的默认方法，通过new命令生成对象实例时，自动调用该方法。一个类必须有constructor方法，如果没有显式定义，一个空的constructor方法会被默认添加。
```js
class Point {
}

// 等同于
class Point {
  constructor() {}
}
```
constructor方法默认返回实例对象（即this），完全可以指定返回另外一个对象。
```js
class Foo {
  constructor() {
    return Object.create(null);
  }
}
//constructor函数返回一个全新的对象，结果导致实例对象不是Foo类的实例。
new Foo() instanceof Foo
// false
```

注意：类必须使用new调用，否则会报错。这是它跟普通构造函数的一个主要区别，后者不用new也可以执行
# 类的实例对象
与 ES5 一样，实例的属性除非显式定义在其本身（即定义在this对象上），否则都是定义在原型上（即定义在class上）

与 ES5 一样，类的所有实例共享一个原型对象。

    var p1 = new Point(2,3);
    var p2 = new Point(3,2);

    p1.__proto__ === p2.__proto__
    //true
# class 表达式
Class 表达式，可以写出立即执行的 Class
```js
let person = new class {
  constructor(name) {
    this.name = name;
  }

  sayName() {
    console.log(this.name);
  }
}('张三');

person.sayName();
```

# 6.不存在变量提升
# 7. new.target 属性
new是从构造函数生成实例对象的命令。ES6 为new命令引入了一个new.target属性，该属性一般用在构造函数之中，返回new命令作用于的那个构造函数。如果构造函数不是通过new命令调用的，new.target会返回undefined，因此这个属性可以用来确定构造函数是怎么调用的。
```js
function Person(name) {
  if (new.target !== undefined) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

// 另一种写法
function Person(name) {
  if (new.target === Person) {
    this.name = name;
  } else {
    throw new Error('必须使用 new 命令生成实例');
  }
}

var person = new Person('张三'); // 正确
var notAPerson = Person.call(person, '张三');  // 报错
```
Class 内部调用new.target，返回当前 Class。需要注意的是，子类继承父类时，new.target会返回子类。


利用new.target会返回子类这个特点，可以写出不能独立使用、必须继承后才能使用的类
```js
class Shape {
  constructor() {
    if (new.target === Shape) {
      throw new Error('本类不能实例化');
    }
  }
}

class Rectangle extends Shape {
  constructor(length, width) {
    super();
    // ...
  }
}
```
# 8.CLASS的继承
## extend
## super关键字
super可以当函数，也可以当对象用
-  super当函数用
uper作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。
作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错。
注意，super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B，因此super()在这里相当于A.prototype.constructor.call(this)。
```js
class A {
  constructor() {
    console.log(new.target.name);
  }
}
class B extends A {
  constructor() {
    super();
  }
}
new A() // A
new B() // B
```
-  super当对象用
super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

- 类的 prototype 属性和__proto__属性
每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。Class 作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。

（1）子类的__proto__属性，表示构造函数的继承，总是指向父类。

（2）子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。
这两条继承链，可以这样理解：作为一个对象，子类（B）的原型（__proto__属性）是父类（A）；作为一个构造函数，子类（B）的原型对象（prototype属性）是父类的原型对象（prototype属性）的实例。