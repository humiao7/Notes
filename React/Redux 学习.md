# Redux 学习

## 一、Redux 简介

Redux 的官方介绍：React 用于构建用户界面的 Javascript 库。可以简单理解为 React 是一个**视图层框架**，使用 React 可以很轻松构建组件式 UI 界面，但他有两个方面没有深入涉及

* 代码结构
* 组件数据通信

而对于一些大型项目来说，以上两点非常关键。因此，仅仅使用 React，不足以应对所有大型项目的开发需求。Redux 是一个 JavaScript 状态管理工具，提供可预测化的状态管理，也可以简单理解为 Redux是一个**数据层框架**。

使用 React 进行 UI 界面的开发，并结合 Redux 进行状态管理，有利于项目模块化拆分和管理，便于应对大型项目需求。

## 二、配置 Redux

### 1、安装

```bash
npm install redux --save
```

### 2、配置 store

在项目 ```src``` 目录下新建 ```/store/index.js``` 文件编写项目 **store** 信息。

```js
import { createStore } from 'redux'
import reducer from './reducer'
// state 初始值
const initState = {
    count: 0
}
// 创建 store 实例
const store = createStore(reducer, initState)
export default store;
```

### 3、配置 Reducer 处理规则

```reducer.js``` 为一个纯函数，接收 **store** 传入 action 以及当前的 state 状态信息，并返回更新后的 state 给 **store**.

```js
export default (state, action) => {
    let newState = { ...state }
    switch (action.type) {
        case 'reduceCount':
            newState.count--;
            return newState;
        case 'addCount':
            newState.count++;
            return newState;
        default:
            return state;
    }
}
```

store 创建完成后，只需要在要使用的界面，将 ```/store/index.js```进行导入即可对 store 进行操作了。

### 4、Redux DevTools 

Redux 为开发者提供了 [Redux DevTools Chrome 插件](https://chrome.google.com/webstore/detail/lmhkpmbekcpmknklioeibfkpmmfibljd)，启用该插件后可以在控制台直观的追踪到 state 的变化，需要在代码中进行配置。

1. 首先需要从 Chrome 应用商店安装该插件 [Redux DevTools](https://chrome.google.com/webstore/detail/lmhkpmbekcpmknklioeibfkpmmfibljd)，并在浏览器扩展中启用；

2. 如果 store 实例不包含其他中间件的，直接在 createStore 时中加入以下配置即可：

   ```js
   const store = createStore(reducer, initState, window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__())
   ```

3. 如果 store 实例同时使用了其他一些中间件，请使用以下方法进行封装：

   ```js
   import { createStore, applyMiddleware, compose } from 'redux';
   const composeEnhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ || compose;
   const store = createStore(reducer, initState, composeEnhancers(applyMiddleware(...middleware)));
   ```

   以  ```redux-thunk``` 为例

   ```js
   import { createStore, applyMiddleware, compose } from 'redux'
   import reducer from './reducer'
   import thunk from 'redux-thunk'
   
   const initState = {}
   
   const composeEndhancers = window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__ ?
       window.__REDUX_DEVTOOLS_EXTENSION_COMPOSE__({}) : compose
   const enhancer = composeEndhancers(applyMiddleware(thunk))
   
   const store = createStore(reducer, enhancer)
   export default store;
   ```

   

## 三、Redux 的三大原则

### 1、单一数据源

整个应用的 state 被储存在一棵 object tree 中，并且这个 object tree 只存在于唯一一个 store 中。

> Tips：使用单一数据源的好处在于整个应用状态都保存在一个对象中，这样我们随时可以提取出整个应用的状态进行持久化。缺点在于，强制使用单一数据源会产生一个特别庞大的 Javascript  对象

### 2、store 只读

改变 state 的唯一方法就是触发 action，action 是一个用于描述已发生事件的普通对象（使用 redux 中间件可对 action 功能进行扩展）。

### 3、state 修改均由纯函数完成

**reducer** 用来描述 action 如何改变 state，**reducer** 函数接收 action 和旧的 state，并返回新的 state 给 Store。**reducer** 必须是一个纯函数（输入必须对应着唯一的输出），并且不能直接对 state 进行修改。**reducer** 返回 state 的值后，**store** 会自动对 state 的值进行修改，并且调用 store 的 subscribe 函数，完成界面数据的刷新。

## 四、Redux 的工作原理

Redux 的触发环节主要由以下几个部分组成：

![](./assets/redux.jpg)

1. 首先，用户操作 UI 引发视图层数据变化，视图层向 **Store** 发出 action:

   ```js
   store.dispatch(action);
   ```

2. 然后 **Store** 自动调用 **Reducer**，并且传入两个参数：当前 state 和收到的 action ，**Reducer** 解析当前 state 和传入的 action，并返回新的 state 状态给 **Store**

   ```js
   function Reducer(previousState, action) {
       let nextState = { ...previousState }
       // TODO: 解析 action 得到更新后的状态
       return nextState
   }
   ```

3. **Store** 一旦监听到 state 变化，就会调用监听函数 subscribe 绑定的方法，完成界面数据的刷新

   ```js
   store.subscribe(listener); // 设置监听函数
   ```

## 五、基本概念和 API

### 1、Store

Store 就是保存数据的地方，你可以把它看成一个容器，redux 提供了 createStore 方法来创建 Store 实例，遵循单一数据原则，整个应用只能有一个 Store。

Store 提供了三个基本方法

```js
store.getState(); // 获取某一时刻，state 的值
store.dispatch(); // 向 store 发出 action
store.subscribe(); // 设置监听函数，state 变化时自动执行
```

### 2、Reducer

Store 收到 Action 以后，必须给出一个新的 State，这样 View 才会发生变化。这种 State 的计算过程就叫做 Reducer。Reducer 是一个纯函数，它接受 Action 和当前 State 作为参数，返回一个新的 State。

Reducer 函数不用手动调用，`store.dispatch`方法执行时会自动触发 Reducer 的执行。而 Store 识别 Reducer 函数内容，是在生成 Store 的时候，通过`createStore`方法传入的 Reducer  来实现的。

### 3、Action

State 的变化，会导致 View 的变化。但是，用户接触不到 State，只能接触到 View。所以，State 的变化必须是 View 导致的。Action 就是 View 发出的通知，表示 State 应该要发生变化了。

Action 是一个对象。其中的 `type` 属性是必须的，表示 Action 的名称。其他属性可以自由设置，社区有一个[规范](https://github.com/acdlite/flux-standard-action)可以参考

```js
const action = {
  type: 'ADD_TODO',
  payload: 'Learn Redux'
};
```

### 4、Action Creator

View 要发送多少种消息，就会有多少种 Action。如果都手写，会很麻烦。可以定义一个函数来生成 Action，这个函数就叫 Action Creator

```js
const ADD_TODO = '添加 TODO';

function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}

const action = addTodo('Learn Redux');
```

上面代码中，`addTodo`函数就是一个 Action Creator。

### 5、store.dispatch()

`store.dispatch()` 是 View 发出 Action 的唯一方法。

```js
store.dispatch(action);
```

上面代码中，`store.dispatch`接受一个 Action 对象作为参数，将它发送出去。

### 6、store.subscribe()

Store 允许使用 `store.subscribe` 方法设置监听函数，一旦 State 发生变化，就自动执行这个函数。

```js
store.subscribe(listener);
```

所以只要把 View 的更新函数（对于 React 项目，就是组件的`render`方法或`setState`方法）放入`listener`，就会实现 View 的自动渲染。

`store.subscribe`方法执行后会返回一个函数，调用这个函数就可以解除监听

```js
let unsubscribe = store.subscribe(() =>
  console.log(store.getState())
);

unsubscribe();
```

## 六、Reducer 的拆分

Reducer 函数负责生成 State。由于整个应用只有一个 State 对象，包含所有数据，对于大型应用来说，这个 State 必然十分庞大，导致 Reducer 函数也十分庞大。

如下面的例子：

```js
import { ADD_CHAT, CHANGE_STATUS, CHANGE_USERNAME } from './actionTypes'

const chatReducer = (state = defaultState, action = {}) => {
  const { type, payload } = action;
  switch (type) {
    case ADD_CHAT:
      return Object.assign({}, state, {
        chatLog: state.chatLog.concat(payload)
      });
    case CHANGE_STATUS:
      return Object.assign({}, state, {
        statusMessage: payload
      });
    case CHANGE_USERNAME:
      return Object.assign({}, state, {
        userName: payload
      });
    default: return state;
  }
};
```

上面代码中，三种 Action 分别改变 State 的三个属性。

- ADD_CHAT：`chatLog` 属性
- CHANGE_STATUS：`statusMessage` 属性
- CHANGE_USERNAME：`userName` 属性

这三个属性之间没有联系，这提示我们可以把 Reducer 函数拆分。不同的函数负责处理不同属性，最终把它们合并成一个大的 Reducer 即可

```js
const chatReducer = (state = defaultState, action = {}) => {
  return {
    chatLog: chatLog(state.chatLog, action),
    statusMessage: statusMessage(state.statusMessage, action),
    userName: userName(state.userName, action)
  }
};
```

上面代码中，Reducer 函数被拆成了三个小函数，每一个负责生成对应的属性

这样一拆，Reducer 就易读易写多了。而且，这种拆分与 React 应用的结构相吻合：一个 React 根组件由很多子组件构成。这就是说，子组件与子 Reducer 完全可以对应。

Redux 提供了一个 `combineReducers` 方法，用于 Reducer 的拆分。你只要定义各个子 Reducer 函数，然后用这个方法，将它们合成一个大的 Reducer。

```js
import { combineReducers } from 'redux';

const chatReducer = combineReducers({
  chatLog,
  statusMessage,
  userName
})

export default todoApp;
```

上面的代码通过 `combineReducers` 方法将三个子 Reducer 合并成一个大的函数。

这种写法有一个前提，就是 State 的属性名必须与子 Reducer 同名。如果不同名，就要采用下面的写法。

```js
const reducer = combineReducers({
  a: doSomethingWithA,
  b: processB,
  c: c
})

// 等同于
function reducer(state = {}, action) {
  return {
    a: doSomethingWithA(state.a, action),
    b: processB(state.b, action),
    c: c(state.c, action)
  }
}
```

总之，`combineReducers()` 做的就是产生一个整体的 Reducer 函数。该函数根据 State 的 key 去执行相应的子 Reducer，并将返回结果合并成一个大的 State 对象。