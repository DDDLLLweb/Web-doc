# 概述
Promise 对象是 JavaScript 的异步操作解决方案，为异步操作提供统一接口。它起到代理作用（proxy），充当异步操作与回调函数之间的中介，使得异步操作具备同步操作的接口。Promise 可以让异步操作写起来，就像在写同步操作的流程，而不必一层层地嵌套回调函数。
首先，Promise 是一个对象，也是一个构造函数。
## 设计思想
Promise 的设计思想是，所有异步任务都返回一个 Promise 实例。Promise 实例有一个then方法，用来指定下一步的回调函数。
```js
function f1(resolve, reject) {
  // 异步代码...
}
//Promise构造函数接受一个回调函数f1作为参数，f1里面是异步操作的代码。然后，返回的p1就是一个Promise 实例。
var p1 = new Promise(f1);
//f1的异步操作执行完成，就会执行f2。
p1.then(f2);
```
传统的写法可能需要把f2作为回调函数传入f1，比如写成f1(f2)，异步操作完成后，在f1内部调用f2。Promise 使得f1和f2变成了链式写法。不仅改善了可读性，而且对于多层嵌套的回调函数尤其方便.
```js
// 传统写法
step1(function (value1) {
  step2(value1, function(value2) {
    step3(value2, function(value3) {
      step4(value3, function(value4) {
        // ...
      });
    });
  });
});

// Promise 的写法
(new Promise(step1))
  .then(step2)
  .then(step3)
  .then(step4)
  .then(
    console.log,
    console.error
  );
```
console.log只显示step4的返回值，而console.error可以显示p1、step1、step2、step3,step4之中任意一个发生的错误。
如果step1的状态变为rejected，那么step2和step3，step4都不会执行了（因为它们是resolved的回调函数）。Promise 开始寻找，接下来第一个为rejected的回调函数，在上面代码中是console.error。这就是说，Promise 对象的报错具有传递性。
# Promise 对象的状态
Promise 对象通过自身的状态，来控制异步操作。Promise 实例具有三种状态。
>异步操作未完成（pending）    
>异步操作成功（fulfilled）    
>异步操作失败（rejected）    

这三种的状态的变化途径只有两种。

>从“未完成”到“成功”    
>从“未完成”到“失败”    

一旦状态发生变化，就凝固了，不会再有新的状态变化
Promise 的最终结果只有两种。

>异步操作成功，Promise 实例传回一个值（value），状态变为fulfilled。    
>异步操作失败，Promise 实例抛出一个错误（error），状态变为rejected。    
# 微任务
Promise 的回调函数属于异步任务，会在同步任务之后执行。
```js
new Promise(function (resolve, reject) {
  resolve(1);
}).then(console.log);

console.log(2);
// 2
// 1
```
上面代码会先输出2，再输出1,因为console.log(2)是同步任务，而then的回调函数属于异步任务，一定晚于同步任务执行。  
但是，Promise 的回调函数不是正常的异步任务，而是微任务（microtask）。它们的区别在于，正常异步任务追加到下一轮事件循环，微任务追加到本轮事件循环。这意味着，微任务的执行时间一定早于正常任务。 
```js
setTimeout(function() {
  console.log(1);
}, 0);

new Promise(function (resolve, reject) {
  resolve(2);
}).then(console.log);

console.log(3);
// 3
// 2
// 1
```
上面代码的输出结果是321。这说明then的回调函数的执行时间，早于setTimeout(fn, 0)。因为then是本轮事件循环执行，setTimeout(fn, 0)在下一轮事件循环开始时执行。

# 面试问题
 一个promise有多个then，如果第一个then出错，后面的还会执行吗，如何捕获异常。 如果第一个then出错了，我还想要后面的继续执行，应该怎么做？