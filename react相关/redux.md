#redux适用场景
如果UI层非常简单，没有很多互动，Redux 就是不必要的，用了反而增加复杂性。

- 用户的使用方式非常简单
- 用户之间没有协作
- 不需要与服务器大量交互，也没有使用 WebSocket
- 视图层（View）只从单一来源获取数据

多交互、多数据源,可以用redux

- 用户的使用方式复杂
- 不同身份的用户有不同的使用方式（比如普通用户和管理员）
- 多个用户之间可以协作
- 与服务器大量交互，或者使用了WebSocket
- View要从多个来源获取数据

从组件角度来看，如果你的应用有以下场景，可以考虑使用 Redux。
- 某个组件的状态，需要共享
- 某个状态需要在任何地方都可以拿到
- 一个组件需要改变全局状态
- 一个组件需要改变另一个组件的状态

# redux介绍
## 什么是redux

Redux 是 JavaScript 状态容器，提供可预测化的状态管理     
## 设计思想
（1）Web 应用是一个状态机，视图与状态是一一对应的。

（2）所有的状态，保存在一个对象里面。
## 动机
随着 JavaScript 单页应用开发日趋复杂，JavaScript 需要管理比任何时候都要多的 state （状态）。 这些 state 可能包括服务器响应、缓存数据、本地生成尚未持久化到服务器的数据，也包括 UI 状态，如激活的路由，被选中的标签，是否显示加载动效或者分页器等等。     
Redux 试图让 state 的变化变得可预测。
## 核心概念
action 就像是描述发生了什么的指示器       
Action 就是普通对象而已，因此它们可以被日志打印、序列化、储存、后期调试或测试时回放出来。                
为了把 action 和 state 串起来，开发一些函数，这就是 reducer，reducer 只是一个接收 state 和 action，并返回新的 state 的函数。 对于大的应用来说，不大可能仅仅只写一个这样的函数，所以我们编写很多小函数来分别管理 state 的一部分。

## 三大原则
### 单一数据源
整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。       
好处:
- 来自服务端的state可以在无需编写更多代码的情况下被序列化并注入到客户端中。
- 由于是单一的state tree ，调试也变得非常容易
- 在开发中可以把state保存在本地，从而加快开发速度
- 轻而易举实现“撤销/重做”这类功能       
### state是只读的
唯一改变state的方法就是触发action，action是一个用于描述已经发生的事件的普通对象。 
### 使用纯函数来执行修改
为了描述action如何改变state tree，需要编写reducers。        
Reducer只是一些纯函数，他接收先前的state和action，并返回新的state。      

# 基础
## action
action 是把数据从应用（译者注：这里之所以不叫 view 是因为这些数据有可能是服务器响应，用户输入或其它非 view 的数据 ）传到 store 的有效载荷。它是 store 数据的唯一来源。
### Action 创建函数
 Redux 中的 action 创建函数只是简单的返回一个 action:
 ```js
    function addTodo(text) {
        return {
        type: ADD_TODO,
        text
        }
    }
 ```
 Redux 中只需把 action 创建函数的结果传给 dispatch() 方法即可发起一次 dispatch 过程。
 ```js
    dispatch(addTodo(text))
    dispatch(completeTodo(index))
 ```
 或者创建一个 被绑定的 action 创建函数 来自动 dispatch：
```js
    const boundAddTodo = text => dispatch(addTodo(text))
    const boundCompleteTodo = index => dispatch(completeTodo(index))
```
然后直接调用它们：
```js
    boundAddTodo(text);
    boundCompleteTodo(index);
```

## Reducer
reducer指定了应用状态的变化如何响应actions并发送到store的，记住actions只是描述了有事情发生了这一事实，并没有描述应用如何更新state。
不要在reducer里做这些操作：        
- 修改传入参数；
- 执行有副作用的操作，如 API 请求和路由跳转；
- 调用非纯函数，如 Date.now() 或 Math.random()。

## Store
Store 就是保存数据的地方，你可以把它看成一个容器。整个应用只能有一个 Store。
Redux 提供createStore这个函数，用来生成 Store。

```js
import { createStore } from 'redux';
const store = createStore(fn);
```
Store 就是把action和Reducer联系到一起的对象。Store 有以下职责：

维持应用的 state；
提供 getState() 方法获取 state；
提供 dispatch(action) 方法更新 state；
通过 subscribe(listener) 注册监听器;
通过 subscribe(listener) 返回的函数注销监听器。

Redux 应用只有一个单一的 store。当需要拆分数据处理逻辑时，你应该使用 reducer 组合 而不是创建多个 store。

## 工作流程
首先，用户发出 Action。

    store.dispatch(action);
然后，Store 自动调用 Reducer，并且传入两个参数：当前 State 和收到的 Action。 Reducer 会返回新的 State 。
    let nextState = todoApp(previousState, action);
State 一旦有变化，Store 就会调用监听函数。

    // 设置监听函数
    store.subscribe(listener);
listener可以通过store.getState()得到当前状态。如果使用的是 React，这时可以触发重新渲染 View。
```js
function listerner() {
  let newState = store.getState();
  component.setState(newState);   
}
```