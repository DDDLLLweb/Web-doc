# 什么是浅拷贝
如果数组元素是基本类型，就会拷贝一份，互不影响，而如果是对象或者数组，就会只拷贝对象和数组的引用，这样我们无论在新旧数组进行了修改，两者都会发生变化。这种赋值引用的拷贝方法被称为浅拷贝。    
- 定义：浅拷贝只是对另外一个变量的内存地址的拷贝，这两个变量指向同一个内存地址的变量值。 
浅拷贝的特点：    
- 公用一个值；
- 这两个变量的内存地址一样；
- 对其中一个变量的值改变，另外一个变量的值也会改变；
# 什么是深拷贝
深拷贝就是指完全的拷贝一个对象，即使嵌套了对象，两者也互相分离，修改一个对象的属性，也不会影响另外一个。
定义：一个变量对另外一个变量的值拷贝。    
深拷贝的特点：

- 两个变量的内存地址不同；
- 两个变量各有自己的值，且互不影响；
- 对其任意一个变量的值的改变不会影响另外一个

# 数组的浅拷贝
1. slice、concat 返回一个新数组的特性来实现拷贝。
```js
    var arr = ['old', 1, true, null, undefined];
    var new_arr = arr.slice(); //或var new_arr = arr.concat();
    new_arr[0] = 'new';
    console.log(arr) // ["old", 1, true, null, undefined]
    console.log(new_arr) // ["new", 1, true, null, undefined]
```
但是如果数组嵌套了对象或者数组的话，比如：   
```js
    var arr = [{old: 'old'}, ['old']];
    var new_arr = arr.concat();
    arr[0].old = 'new';
    arr[1][0] = 'new';
    console.log(arr) // [{old: 'new'}, ['new']]
    console.log(new_arr) // [{old: 'new'}, ['new']]
```
此时不论是对新数组还是旧数组进行修改，会导致二者都被修改。

2. map ：返回一个由原数组中的每个元素调用一个指定方法后的返回值组成的新数组。
```js
    var arr = ["one","two","three"];
    var arrto = arr.map(ele=>ele);
    console.log(arr, arrto); // ["one","two","three"] ["one","two","three"]
    arrto[0] = 1;
    console.log(arr, arrto); // ["one","two","three"] [1,"two","three"
```
# 数组的深拷贝
```js
    var arr = ['old', 1, true, ['old1', 'old2'], {old: 1}]
    var new_arr = JSON.parse( JSON.stringify(arr) );
    console.log(new_arr);
```
注意：此方法适用于数组和对象，但不能拷贝函数
```js
    var arr = [function(){
        console.log(a)
    }, {
        b: function(){
            console.log(b)
        }
    }]

    var new_arr = JSON.parse(JSON.stringify(arr));

    console.log(new_arr); //[null, Object]
```
# 对象的拷贝
Object.assign，第一级属性深拷贝，以后级别属性浅拷贝
```js
    var obj = {'a':{'ac':'xixi'},'b':'haha'};
    var newobj = Object.assign({}, obj);
    newobj.a = 'heihei';
    console.log(newobj);// {a: "heihei", b: "haha"}
    console.log(obj);// {a:{'ac':'xixi'},b:'haha'};

    var newobj1 = Object.assign({}, obj);
    newobj1.a.ac = 'lalala';
    console.log(newobj1); // {a: {ac: "lalala"}, b: "haha"}
    console.log(obj); // {a: {ac: "lalala"}, b: "haha"}
```

# 浅拷贝的实现
遍历对象，然后把属性和属性值都放在一个新的对象
```js
    var shallowCopy = function(obj) {
    // 只拷贝对象
    if (typeof obj !== 'object') return;
    // 根据obj的类型判断是新建一个数组还是对象
    var newObj = obj instanceof Array ? [] : {};
    // 遍历obj，并且判断是obj的属性才拷贝
    for (var key in obj) {
        if (obj.hasOwnProperty(key)) {
            newObj[key] = obj[key];
        }
    }
    return newObj;
}
```
# 深拷贝的实现
拷贝的时候判断一下属性值的类型，如果是对象，递归调用深拷贝函数
```js
    var deepCopy = function(obj) {
        if (typeof obj !== 'object') return;
        var newObj = obj instanceof Array ? [] : {};
        for (var key in obj) {
            if (obj.hasOwnProperty(key)) {
                newObj[key] = typeof obj[key] === 'object' ? deepCopy(obj[key]) : obj[key];
            }
        }
        return newObj;
    }
```