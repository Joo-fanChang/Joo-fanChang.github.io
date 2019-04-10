---
title: redux实现
date: 2019-04-09 21:40:40
tags: js react redux
categories: web
---

# 面试之redux的实现

> 为啥使用redux略过... <br/>
> [demo地址](https://github.com/Joo-fanChang/react-app/tree/redux)

一般项目中使用redux时，会和react-redux配合使用，如果不使用react-redux时，redux也可以单独工作，使用react-redux会简化一些操作

reudx包暴露了几个方法
- createStore
- combineReducer
- applyMiddleware
- bindActionCreators

## createStore
其中createStore是其中的核心方法，只使用其中一个就可以单独使用

{% asset_img redux.png %}

根据上面的使用流程可以实现以下方法
```js
function createStore(reducer, prePayload) {

  let currentState = prePayload;
  let listeners = [];

  function getState() {
    return currentState;
  }

  function dispatch(action) {
    currentState = reducer(currentState, action);

    for(let i = 0; i < listeners.length; i++) {
      listeners[i]();
    }
  }

  function subscribe(func) {
    let isSubscribed = false;

    if (typeof func === 'function') {
      if (!listeners.includes(func)) {
        listeners.push(func);
        isSubscribed = true;
      }
    }

    return function unSubscribe() {
      if (!isSubscribed) {
        return;
      }

      let index = listeners.indexOf(func);
      listeners.splice(index, 1);
    }
  }

  dispatch({type: 'INIT'});

  return {
    getState,
    dispatch,
    subscribe,
  }
}
```

## combineReducer
这个函数的意义是可以整合reducer函数，这样可以方便分模块定义redux
实现原理就是定义一个顶级对象，使用`key`来区分（key是传入`combineReducers`的对象的key），当派发一个`action`时，循环所有的`reducer`函数，更新所有的模块

```js
export default function combineReducers(reducers) {
  const reducerKeys = Object.keys(reducers);

  return function combine(state = {}, action) {

    let nextState = {};
    let isChanged = false;

    for(let i = 0; i < reducerKeys.length; i++) {
      let key = reducerKeys[i];
      let reducer = reducers[key];
      let stateForKey = state[key];

      nextState[key] = reducer(stateForKey, action);
      isChanged = isChanged || nextState !== state;
    }

    return isChanged ? nextState : state;
  }
}
```