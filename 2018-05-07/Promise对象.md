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

//5.14新增
# es6中的Promise
## Promise对象的特点
- （1）对象的状态不受外界影响。Promise对象代表一个异步操作，有三种状态：pending（进行中）、fulfilled（已成功）和rejected（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。
- （2）一旦状态改变，就不会再变，任何时候都可以得到这个结果。Promise对象的状态改变，只有两种可能：从pending变为fulfilled和从pending变为rejected。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（已定型）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到
## Promise对象的缺点
- 首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。
- 其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。
- 第三，当处于pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。

## 基本用法
Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署.       
resolve函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；reject函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。 

Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。
```js
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve("完成啦");
});

promise.then(function(data) {
  console.log('resolved.',data); //resolved. 完成啦
});
```
### Promise.prototype.then()
Promise实例具有then方法，thent方法是定义在原型对象上的,then方法的第一个参数是resolved状态的回调函数，第二个参数（可选）是rejected状态的回调函数
then返回的是一个Promise实例，因此可以采用链式写法，即then方法后面再调用另一个then方法。
### Promise.prototype.catch()
```js
// 写法一
const promise = new Promise(function(resolve, reject) {
  try {
    throw new Error('test');
  } catch(e) {
    reject(e);
  }
});
promise.catch(function(error) {
  console.log(error);
});

// 写法二
const promise = new Promise(function(resolve, reject) {
  reject(new Error('test'));
});
promise.catch(function(error) {
  console.log(error);
});
```
比较上面两种写法，可以发现reject方法的作用，等同于抛出错误。

一般总是建议，Promise 对象后面要跟catch方法，这样可以处理 Promise 内部发生的错误。catch方法返回的还是一个 Promise 对象，因此后面还可以接着调用then方法。
>注意：如果 Promise 状态已经变成resolved，再抛出错误是无效的
一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。
```js
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
### Promise.prototype.finally()
finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。该方法是 ES2018 引入标准的.

finally方法的回调函数不接受任何参数，这意味着没有办法知道，前面的 Promise 状态到底是fulfilled还是rejected。这表明，finally方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果。

finally本质上是then方法的特例
```js
promise.finally(()=>{
  //要执行的语句
})
//等同于
promise.then(
  result=>{
    //要执行的语句
    return result;
  },
  error=>{
    //要执行的语句
    return error;
  }
)
```
  如果不使用finally方法，同样的语句需要为成功和失败两种情况各写一次。有了finally方法，则只需要写一次
#### finally的实现

```js
Promise.prototype.finally = function (callback) {
  let P = this.constructor;
  return this.then(
    value  => P.resolve(callback()).then(() => value),
    reason => P.resolve(callback()).then(() => { throw reason })
  );
};
```
 ### finally 
 ```js
 promise.finally(() => {
  // 语句
 });
 // 等同于
 promise
  .then(
   result => {
     // 语句
     return result;
   },
   error => {
     // 语句
     throw error;
   }
  );
  ```
  ### Promise.all()
  - all 方法
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。并发执行多个异步方法，当所有的异步方法都是fullfilled，此promise对象才是fullfilled，否则就是reject。
```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);


const p2 = new Promise((resolve, reject) => {
  resolve("world");
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
//["hello", "world"]
```
如果作为参数的 Promise 实例，自己定义了catch方法，那么它一旦被rejected，并不会触发Promise.all()的catch方法。
```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]
```
  ### Promise.race()
  - race方法
  Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。
  ```js
  const p = Promise.race([p1, p2, p3]);
  ```
上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。

Promise.race方法的参数与Promise.all方法一样，如果不是 Promise 实例，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。

 ## promise 操作流程
 -  异步执行成功 resolve 处理结果集
  ```js
  new Promise((resolve,reject)=>{
    setTimeout(function(){
      console.log("====模拟异步方法执行，比如ajax=====");
      resolve({a:113});
      },3000)})
      .then((data)=>{
        console.log(data);
        })
      .catch((error)=>{
        console.log(error);
      });
  }
```
异步执行失败 reject 处理错误信息 catch捕获异常
  ```js
 new Promise((resolve,reject)=>{
     setTimeout(function(){
       console.log("====异步方法执行，比如ajax");
       reject(new Error('ajax fail'));
     },3000)
 })
 .then((data)=>{
   console.log(data)
 })
 .catch((error)=>{
   console.log(error);
 });
 ```
### 模拟ajax
  ```js
  new Promise((resolve,reject)=>{
      console.log("====模拟异步方法执行，比如ajax=====");
      $.ajax({
        url:'api',
        data:{},
        success:function(data) {
           resolve(data);
        },
        error: function(error) {
          reject(error);
        }
      });
  }).then((data)=>{
        console.log(data);
        })
      .catch((error)=>{
        console.log(error);
      });
```
# 面试问题
1.一个promise有多个then，如果第一个then出错，后面的还会执行吗，如何捕获异常。 如果第一个then出错了，我还想要后面的继续执行，应该怎么做？
  （1）在第一个then后边写catch，然后在catch后边继续写then
  （2）finally方法执行你想要执行的动作


2.说出下列代码的打印顺序以及原理
```js
setTimeout(function() {
  console.log(1);
}, 0);
new Promise(function (resolve, reject) {
  console.log(2);//同步任务代码
  setTimeout(function() {
    resolve(3);
  }, 0);
  
}).then((res)=>{
  console.log(res);
});
console.log("4");//同步任务代码 
//2
//4
//1
//3
```
以上代码中 ：
- 首先setTimeout，异步任务放入宏任务队列中
- new Promise同步代码立即执行`console.log(2)`,
- 又有一个setTimeout,继续放入宏任务队列中，
- 由于resolve方法被放在setTimeout中了，微任务then将在setTimeout执行的时候才会执行
- 接下来执行同步代码`console.log(4)`,
- 这时候主线程的主任务（也就是宏任务中的同步任务）执行完毕
- 开始事件循环，先执行宏任务，从任务队列中按照先进先出执行setTimeout,先打印1 后打印3

3.实现一个方法 delay(timer).then(...)
```js
  function delay(t){
    return new Promise((resolve,reject)=>{
      setTimeout(resolve,t,'done');
    })
  }
  delay(1000).then((data)=>{
    console.log(data);
  })
```
3.一个promise多个then，其中一个then报错，后边的就不会在执行，若想继续执行就要在出错的then后边写catch
或者将想要执行的动作写在finally方法中
```js
  var promise = new Promise((resolve,reject)=>{
    resolve(1);
  });
  promise
  .then(res=>console.log(res))
  .then(()=>{
    x+2;
  }).then(()=>{
    console.log('3');
  }) //打印1 然后报错
```
```js
  var promise = new Promise((resolve,reject)=>{
    resolve(1);
  });
  promise.then(res=>console.log(res))
  .then(()=>{
    x+2;
  })
  .catch((error)=>{console.log(error);})
  .then(()=>{console.log('4');});
  //打印1 打印错误 打印4
//finally
   var promise = new Promise((resolve,reject)=>{
    resolve(1);
  });
  promise.then(res=>console.log(res))
  .then(()=>{
    x+2;
  }).finally(()=>{
    console.log('3');
  }).then(()=>{console.log('4');})
  .catch((e)=>{console.log(e);});
  //打印 1，3 ，error
```

4.如何利用promise实现一个图片的异步加载
```js
  function loadImg(url){
    return new Promise(function(resove,reject){
      const image = new Image();
      image.onload = function(){
        resolve(image);
      };
      image.onerror = function(){
        reject(new Error("error"));
      }
      image.src = url;
    })
  }
```
5.实现promise.all方法
```js

```