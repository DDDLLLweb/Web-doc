# 1.简介
Generator 函数是 ES6 提供的一种异步编程解决方案，语法行为与传统函数完全不同      

Generator 函数是一个普通函数，但是有两个特征。一是，function关键字与函数名之间有一个星号；二是，函数体内部使用yield表达式，定义不同的内部状态（yield在英语里的意思就是“产出”）。

Generator 函数的调用方法与普通函数一样，也是在函数名后面加上一对圆括号。不同的是，调用 Generator 函数后，该函数并不执行，返回的也不是函数运行结果，而是一个指向内部状态的指针对象，也就是上一章介绍的遍历器对象（Iterator Object）。下一步，必须调用遍历器对象的next方法，使得指针移向下一个状态。

总结：调用 Generator 函数，返回一个遍历器对象，代表 Generator 函数的内部指针。以后，每次调用遍历器对象的next方法，就会返回一个有着value和done两个属性的对象。value属性表示当前的内部状态的值，是yield表达式后面那个表达式的值；done属性是一个布尔值，表示是否遍历结束。
## yield 表达式
由于 Generator 函数返回的遍历器对象，只有调用next方法才会遍历下一个内部状态，所以其实提供了一种可以暂停执行的函数。yield表达式就是暂停标志。

遍历器对象的next方法的运行逻辑如下。

（1）遇到yield表达式，就暂停执行后面的操作，并将紧跟在yield后面的那个表达式的值，作为返回的对象的value属性值。

（2）下一次调用next方法时，再继续往下执行，直到遇到下一个yield表达式。

（3）如果没有再遇到新的yield表达式，就一直运行到函数结束，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为undefined。

需要注意的是，yield表达式后面的表达式，只有当调用next方法、内部指针指向该语句时才会执行，因此等于为 JavaScript 提供了手动的“惰性求值”（Lazy Evaluation）的语法功能。

# 2.next 方法的参数 
yield表达式本身没有返回值，或者说总是返回undefined。next方法可以带一个参数，该参数就会被当作上一个yield表达式的返回值。
# 3.for...of 
for...of循环可以自动遍历 Generator 函数时生成的Iterator对象，且此时不再需要调用next方法。
```js
function* foo() {
  yield 1;
  yield 2;
  yield 3;
  yield 4;
  yield 5;
  return 6;
}

for (let v of foo()) {
  console.log(v);
}
// 1 2 3 4 5
```
上面代码使用for...of循环，依次显示 5 个yield表达式的值。
`注意`:一旦next方法的返回对象的done属性为true，for...of循环就会中止，且不包含该返回对象，所以上面代码的return语句返回的6，不包括在for...of循环之中。
# 4.Generator.prototype.throw()******
# 5.Generator.prototype.return() 
Generator 函数返回的遍历器对象，还有一个return方法，可以返回给定的值，并且终结遍历 Generator 函数。

```js
function* gen() {
  yield 1;
  yield 2;
  yield 3;
}

var g = gen();

g.next()        // { value: 1, done: false }
g.return('foo') // { value: "foo", done: true }
g.next() 
```
遍历器对象g调用return方法后，返回值的value属性就是return方法的参数foo。并且，Generator 函数的遍历就终止了，返回值的done属性为true，以后再调用next方法，done属性总是返回true。

如果return方法调用时，不提供参数，则返回值的value属性为undefined。

如果 Generator 函数内部有try...finally代码块，那么return方法会推迟到finally代码块执行完再执行。
```js
function* numbers () {
  yield 1;
  try {
    yield 2;
    yield 3;
  } finally {
    yield 4;
    yield 5;
  }
  yield 6;
}
var g = numbers();
g.next() // { value: 1, done: false }
g.next() // { value: 2, done: false }
g.return(7) // { value: 4, done: false }
g.next() // { value: 5, done: false }
g.next() // { value: 7, done: true }
```
# 7.yield* 表达式
如果在 Generator 函数内部，调用另一个 Generator 函数，默认情况下是没有效果的。
```js
function* foo(){
    yield 'a';
    yield 'b';
}
function* bar(){
    yield 'x';
    foo();
    yield 'y';
}
for(let v of bar()){
    console.log(v);
}
//"x"
//"y"
```
上面代码中，foo和bar都是 Generator 函数，在bar里面调用foo，是不会有效果的。    
这个就需要用到yield*表达式，用来在一个 Generator 函数里面执行另一个 Generator 函数。
```js
function* bar() {
  yield 'x';
  yield* foo();
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  yield 'a';
  yield 'b';
  yield 'y';
}

// 等同于
function* bar() {
  yield 'x';
  for (let v of foo()) {
    yield v;
  }
  yield 'y';
}
for (let v of bar()){
  console.log(v);
}
// "x"
// "a"
// "b"
// "y
```
对比例子：
```js
function* inner() {
  yield 'hello!';
}

function* outer1() {
  yield 'open';
  yield inner();
  yield 'close';
}

var gen = outer1()
gen.next().value // "open"
gen.next().value // 返回一个遍历器对象
gen.next().value // "close"

function* outer2() {
  yield 'open'
  yield* inner()
  yield 'close'
}

var gen = outer2()
gen.next().value // "open"
gen.next().value // "hello!"
gen.next().value // "close"
```
定义：从语法角度看，如果yield表达式后面跟的是一个遍历器对象，需要在yield表达式后面加上星号，表明它返回的是一个遍历器对象。这被称为yield*表达式。

yield*后面的 Generator 函数（没有return语句时），等同于在 Generator 函数内部，部署一个for...of循环。
```js
function* concat(iter1, iter2) {
  yield* iter1;
  yield* iter2;
}

// 等同于

function* concat(iter1, iter2) {
  for (var value of iter1) {
    yield value;
  }
  for (var value of iter2) {
    yield value;
  }
}
```
任何数据结构只要有 Iterator 接口，就可以被yield*遍历
```js
//遍历数组  yield*后面跟着一个数组，由于数组原生支持遍历器，因此就会遍历数组成员。
function* gen(){
  yield* ["a", "b", "c"];
}

gen().next();// { value:"a", done:false }                       **************
//////////////////
let g = gen();
g.next();//{ value:"a", done:false }
g.next();//{value: "a", done: false}
g.next();//{value: "c", done: false}
g.next();//{value: undefined, done: true}

```

# 8.Generator 函数的this
# 9.Generator 与上下文 
JavaScript 代码运行时，会产生一个全局的上下文环境（context，又称运行环境），包含了当前所有的变量和对象。然后，执行函数（或块级代码）的时候，又会在当前上下文环境的上层，产生一个函数运行的上下文，变成当前（active）的上下文，由此形成一个上下文环境的堆栈（context stack）。    
这个堆栈是“后进先出”的数据结构，最后产生的上下文环境首先执行完成，退出堆栈，然后再执行完成它下层的上下文，直至所有代码执行完成，堆栈清空。

Generator 函数不是这样，它执行产生的上下文环境，一旦遇到yield命令，就会暂时退出堆栈，但是并不消失，里面的所有变量和对象会冻结在当前状态。等到对它执行next命令时，这个上下文环境又会重新加入调用栈，冻结的变量和对象恢复执行。

# Generator 函数的异步应用
## 传统方法
- 回调函数
- 事件监听
- 发布/订阅
- Promise 对象
## 异步
异步，简而言之就是一个任务不是连续完成的，可以理解为该任务被认为分成两段，先执行第一段，然后转而执行其他任务，等做好了准备，再回过头去执行第二段。     
比如，有一个任务是读取文件进行处理，任务的第一段是向操作系统发出请求，要求读取文件。然后，程序执行其他任务，等到操作系统返回文件，再接着执行任务的第二段（处理文件）。这种不连续的执行，就叫做异步。

相应地，连续的执行就叫做同步。由于是连续执行，不能插入其他任务，所以操作系统从硬盘读取文件的这段时间，程序只能干等着
 ## 回调函数
 JavaScript 语言对异步编程的实现，就是回调函数。     
 所谓回调函数，就是把任务的第二段单独写在一个函数里面，等到重新执行这个任务的时候，就直接调用这个函数。回调函数的英语名字callback，直译过来就是"重新调用"。   

## Promise
## Generator 函数
### 协程
"协程"（coroutine），意思是多个线程互相协作，完成异步任务，协程有点像函数，又有点像线程。它的运行流程大致如下：
- 第一步，协程A开始执行。
- 第二步，协程A执行到一半，进入暂停，执行权转移到协程B。
- 第三步，（一段时间后）协程B交还执行权。
- 第四步，协程A恢复执行
上面流程的协程A，就是异步任务，因为它分成两段（或多段）执行
举例来说，读取文件的协程写法如下。
```js
function* asyncJob() {
  // ...其他代码
  var f = yield readFile(fileA);
  // ...其他代码
}
```
上面代码的函数asyncJob是一个协程，它的奥妙就在其中的yield命令。它表示执行到此处，执行权将交给其他协程。也就是说，yield命令是异步两个阶段的分界线。

协程遇到yield命令就暂停，等到执行权返回，再从暂停的地方继续往后执行。它的最大优点，就是代码的写法非常像同步操作，如果去除yield命令，简直一模一样。

异步任务的封装
下面看看如何使用 Generator 函数，执行一个真实的异步任务。
```js
var fetch = require('node-fetch');

function* gen(){
  var url = 'https://api.github.com/users/github';
  var result = yield fetch(url);
  console.log(result.bio);
}
```
上面代码中，Generator 函数封装了一个异步操作，该操作先读取一个远程接口，然后从 JSON 格式的数据解析信息。就像前面说过的，这段代码非常像同步操作，除了加上了yield命令。

执行这段代码的方法如下。
```js
var g = gen();
var result = g.next();

result.value.then(function(data){
  return data.json();
}).then(function(data){
  g.next(data);
});
```

上面代码中，首先执行 Generator 函数，获取遍历器对象，然后使用next方法（第二行），执行异步任务的第一阶段。由于Fetch模块返回的是一个 Promise 对象，因此要用then方法调用下一个next方法。

可以看到，虽然 Generator 函数将异步操作表示得很简洁，但是流程管理却不方便（即何时执行第一阶段、何时执行第二阶段）。
## Thunk 函数
## co 模块 


### 温习
  - for...of循环可以自动遍历 Generator 函数时生成的Iterator对象，且此时不再需要调用next方法。
  一旦next方法的返回对象的done属性为true，for...of循环就会中止，且不包含该返回对象，
  - 所谓异步： 不连续执行的任务

手写ajax
```js
var xhr = new XMLHttpRequest();
xhr.onreadystatechange= function(){
  if(xhr.readystate === 4){
      if(xhr.status === 200){
         console.log(xhr.responseText);
      }else{
          console.log(xhr.statusText);
      }
  }
  xhr.onError = function(){
    console.error(xhr.statusText);
  }
  xhr.open('get',url,true);
  xhr.send();

}
