# 作用域
作用域是指程序源代码中定义变量的区域。    
作用域规定了如何查找变量，也就是确定当前执行代码对变量的访问权限。    
JavaScript 采用词法作用域(lexical scoping)，也就是静态作用域。    
# 静态作用域与动态作用域
JavaScript 采用的是词法作用域    
词法作用域关注函数在何处声明，动态作用域关注函数在何处调用 
通过一个例子说明二者之间的区别       
```js
    var value = 1;
    function foo() {
        console.log(value);
    }
    function bar() {
        var value = 2;
        foo();
    }
    bar(); //1
```
词法作用域让foo中的value通过RHS引用到了全局作用域中的value，因此会输出1    
执行 foo 函数，先从 foo 函数内部查找是否有局部变量 value，如果没有，就根据书写的位置，查找上面一层的代码，也就是 value 等于 1，所以结果会打印 1。
假设采用动态作用域，会输出2    
执行 foo 函数，先从 foo 函数内部查找是否有局部变量 value，如果没有，就顺着调用栈在调用foo（）的地方查找value，引擎会查到bar()函数的作用域，并在其中找到值为2的value    
# 思考
```js
    var scope = "global scope";
    function checkscope(){
        var scope = "local scope";
        function f(){
            return scope;
        }
        return f();
    }
    checkscope();
```
```js
    var scope = "global scope";
    function checkscope(){
        var scope = "local scope";
        function f(){
            return scope;
        }
        return f;
    }
    checkscope()();
```
两段代码都会打印：local scope    
JavaScript 函数的执行用到了作用域链，这个作用域链是在函数定义的时候创建的。嵌套的函数 f() 定义在这个作用域链里，其中的变量 scope 一定是局部变量，不管何时何地执行函数 f()，这种绑定在执行 f() 时依然有效。    

# 作用域链
## 前言
上下文：当JavaScript代码执行一段可执行代码(executable code)时，会创建对应的执行上下文(execution context)。
对于每个执行上下文，都有三个重要属性：

变量对象(Variable object，VO)   
作用域链(Scope chain)    
this   
## 作用域链 
定义：当查找变量的时候，会先从当前上下文的变量对象中查找，如果没有找到，就会从父级(词法层面上的父级)执行上下文的变量对象中查找，一直找到全局上下文的变量对象，也就是全局对象。这样由多个执行上下文的变量对象构成的链表就叫做作用域链。


