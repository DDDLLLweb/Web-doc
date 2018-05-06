# 基本语法
## 标签(label)
JavaScript 语言允许，语句的前面有标签（label），相当于定位符，用于跳转到程序的任意位置，标签的格式如下。

    label:
    语句
标签可以是任意的标识符，但不能是保留字，语句部分可以是任意语句。
标签通常与break语句和continue语句配合使用，跳出特定的循环。
```js
    top:
    for (var i = 0; i < 3; i++){
        for (var j = 0; j < 3; j++){
        if (i === 1 && j === 1) break top;
        console.log('i=' + i + ', j=' + j);
        }
    }
    // i=0, j=0
    // i=0, j=1
    // i=0, j=2
    // i=1, j=0
```
上面代码为一个双重循环区块，break命令后面加上了top标签（注意，top不用加引号），满足条件时，直接跳出双层循环。如果break语句后面不使用标签，则只能跳出内层循环，进入下一次的外层循环。

<span style="color:pink">continue</span>语句也可以与标签配合使用。
```js
top:
  for (var i = 0; i < 3; i++){
    for (var j = 0; j < 3; j++){
      if (i === 1 && j === 1) continue top;
      console.log('i=' + i + ', j=' + j);
    }
  }
// i=0, j=0
// i=0, j=1
// i=0, j=2
// i=1, j=0
// i=2, j=0
// i=2, j=1
// i=2, j=2
```
continue命令后面有一个标签名，满足条件时，会跳过当前循环，直接进入下一轮外层循环。如果continue语句后面不使用标签，则只能进入下一轮的内层循环。    
# 标准库
## 对象
JavaScript 原生提供Object对象（注意起首的O是大写）       
JavaScript 的所有其他对象都继承自Object对象，即那些对象都是Object的实例。       
Object对象的原生方法分成两类：Object本身的方法与Object的实例方法。
1. Object本身的方法
”本身的方法“就是直接定义在Object对象的方法。

    Object.print = function (o) { console.log(o) };
2. Object的实例方法
实例方法就是定义在Object原型对象Object.prototype上的方法。它可以被Object实例直接使用。    
```js
    Object.prototype.print = function () {
    console.log(this);
    };

    var obj = new Object();
    obj.print() // Object
```
凡是定义在Object.prototype对象上面的属性和方法，将被所有实例对象共享

