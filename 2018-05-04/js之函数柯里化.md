# 柯里化
## 定义
在数学和计算机科学中，柯里化是一种将使用多个参数的一个函数转换成一系列的使用一个参数的函数的技术    
简单示例：    
```js
    //函数定义
    function add(x,y){
        return x + y;
    }
    //函数调用
    add(3,4);//5

    //函数表达式定义
    var add = function(x){
        return function(y){
            return x + y;
        }
    };
//函数调用
add(3)(4);
```
## 用途
柯里化(Currying)具有：延迟计算、参数复用、动态生成函数的作用
