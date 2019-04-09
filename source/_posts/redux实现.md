---
title: redux实现
date: 2019-04-09 21:40:40
tags: js react redux
categories: web
---

# redux的实现

> 为啥使用redux略过...

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