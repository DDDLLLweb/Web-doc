# 类数组对象
拥有一个 length 属性和若干索引属性的对象即为类数组对象
例如：
```js
var array = ['name', 'age', 'sex'];

var arrayLike = {
    0: 'name',
    1: 'age',
    2: 'sex',
    length: 3
}
```
累数组对象的读写、获取长度、遍历都和数组一样，但是类数组对象不能使用数组的方法

# 类数组转为数组
```js
var arrayLike = {0: 'name', 1: 'age', 2: 'sex', length: 3 }
//1.slice
Array.prototype.slice.call(arrayLike);//["name", "age", "sex"]
//2.splice
Array.prototype.splice.call(arrayLike,0);//["name", "age", "sex"]
//3.Es6 Array.from
Array.from(arrayLike);//["name", "age", "sex"]
//4.apply
Array.prototype.concat.apply([],arrayLike);//["name", "age", "sex"]
```
# arguments 对象
Arguments 对象只定义在函数体中，包括了函数的参数和其他属性。在函数体中，arguments 指代该函数的 Arguments 对象。   
## 转数组
利用ES6的...运算符，可以将参数转为数组
```js
    function func(...arguments) {
        console.log(arguments); // [1, 2, 3]
    }
    func(1, 2, 3);
```

## 传递参数
将参数从一个函数传递到另一个函数
```js
// 使用 apply 将 foo 的参数传递给 bar
function foo() {
    bar.apply(this, arguments);
}
function bar(a, b, c) {
   console.log(a, b, c);
}

foo(1, 2, 3)
```

