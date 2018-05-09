# Symbol
## 概述
ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。    
它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

Symbol函数前不能使用new命令，否则会报错。这是因为生成的 Symbol 是一个原始类型的值，不是对象。也就是说，由于 Symbol 
```js
let s = Symbol();

typeof s
// "symbol"
```
 - --Symbol 值可以显式转为字符串，Symbol 值也可以转为布尔值，但是不能转为数值。
 ```js
    //转为字符串
    let sym = Symbol('My symbol');
    String(sym) // 'Symbol(My symbol)'
    sym.toString() // 'Symbol(My symbol)'
    //转为布尔值
    let sym = Symbol();
    Boolean(sym) // true
    !sym  // false
    //不能转为数值
    Number(sym) // TypeError
    sym + 2 // TypeError

 ```
## 作为属性名
每一个 Symbol 值都是不相等的，这意味着 Symbol 值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性.    
```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```
`注意`
- Symbol 值作为对象属性名时，不能用点运算符。
- 在对象内部，使用Symbol值定义属性时，Symbol值必须被放在方括号之中
## 属性名的遍历
Symbol 作为属性名，该属性不会出现在for...in、for...of循环中，    
也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。    
但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名。    
另：Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。    

## Symbol.for()，Symbol.keyFor()
Symbol.for方法接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建并返回一个以该字符串为名称的 Symbol 值。

Symbol.for()与Symbol()这两种写法，都会生成新的 Symbol。它们的区别是，前者`Symbol()会被登记在全局环境中`供搜索，后者不会。

Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值。


Symbol.keyFor方法返回一个已登记的 Symbol 类型值的key




