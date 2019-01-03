# 按值传递
什么是按值传递？   
- 把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。
举个栗子：

    var value = 1;
    function foo(v) {
        v = 2;
        console.log(v); //2
    }
    foo(value);
    console.log(value) // 1
很好理解，当传递 value 到函数 foo 中，相当于拷贝了一份 value，假设拷贝的这份叫 _value，函数中修改的都是 _value 的值，而不会影响原来的 value 值。
# 引用传递
所谓按引用传递，就是传递对象的引用，函数内部对参数的任何改变都会影响该对象的值，因为两者引用的是同一个对象。

举例： 
 例2
 ```js  
    var obj = {
        value: 1
    };
    function foo(o) {
        o.value = 2;
        console.log(o.value); //2
    }
    foo(obj);
    console.log(obj.value) // 2
```
例3
```js
    var obj = {
    value: 1
    };
    function foo(o) {
        o = 2;
        console.log(o); //2
    }
    foo(obj);
    console.log(obj.value) // 1
```
# 共享传递
共享传递是指，在传递对象的时候，传递对象的引用的副本。
上述例子中，修改 o.value，可以通过引用找到原值，但是直接修改 o，并不会修改原值。所以第二个和第三个例子其实都是按共享传递。
# 总结
参数如果是基本类型是按值传递，如果是引用类型按共享传递。

```js
function changAge(person){
    person.age = 25;
    person = {
        name:"jhon",
        age:50
    }
    return person;
}
var personObj1 = {
    name:"Alex",
    age:30
};
var personObj2 = changAge(personObj1);
console.log(personObj1);//{name: "Alex", age: 25}
console.log(personObj2);//{name: "jhon", age: 50}
```