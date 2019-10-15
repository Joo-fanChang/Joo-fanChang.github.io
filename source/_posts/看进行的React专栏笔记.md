---
title: 看进击的React专栏笔记
date: 2019-10-15 15:02:38
tags: web react
---

> 进击的React是[知乎进击的React](https://zhuanlan.zhihu.com/advancing-react)的专栏

## 1. setState：这个API设计到底怎么样

[地址链接](https://zhuanlan.zhihu.com/p/25954470)

### 1.1 为什么要设计setState这个API

state本质是js对象，可以直接通过`state.x = 1`来修改值，但不会触发页面刷新等一系列操作，这些个逻辑直接封装到`setState`中；
虽然可以通过Object setter来完成
> 那样的API设计会更让人晕头转向，因为不管是谁，第一眼也看不出来修改一个this.state对象居然会引发重新渲染的副作用。

### 1.2 state在什么时候更新

```js
this.setState({ count: this.state + 1 });
this.setState({ count: this.state + 1 });
this.setState({ count: this.state + 1 });
```

在调用`setState`后，会触发
- shouldComponentUpdate
- componentWillUpdate
- render
- componentDidUpdate

1. state在shouldComponentUpdate和componentWillUpdate中都会进行改变，在调用`render`的时候才会进行更新；
2. 但是如果在`shouldComponentUpdate`中`return false`，虽然页面上不会进行更新，但是state对象依然会更新。也就是说下例中，第一次点击update不会有效果，第二次点击update，页面上直接显示`3`；

```tsx
class App extends React.Component {
  public flag: boolean = false;

  public state = {
    count: 1,
  }

  public shouldComponentUpdate() {
    if (!this.flag) {
      this.flag = true;
      return false;
    }
    return true;
  }

  public update = () => {
    this.setState({
      count: this.state.count + 1,
    })
  }

  public render() {
    return (
      <div>
        { this.state.count }
        <div>
          <button onClick={this.update}>update</button>
        </div>
      </div>
    )
  }
}
```

### 1.3 setState的函数参数用法

setState方法可以接收一个回调函数，这个回调函数有两个参数1. state 2. props，返回值应该为修改的state对象；
```js
this.setState((state, prop) => ({
  count: state.count + 1,
}))
```

如果连续调用

```js
// count初始值为0
 this.setState(state => ({
  count: state.count + 1,
}));
this.setState(state => ({
  count: state.count + 1,
}));
this.setState(state => ({
  count: state.count + 1,
}));
```

则可以达成想要的效果，即最终效果为3

如果这样

```js
 this.setState(state => ({
  count: state.count + 1,
}));
this.setState(state => ({
  count: state.count + 1,
}));
this.setState({
  count: this.state + 1,
});
this.setState(state => ({
  count: state.count + 1,
}));
```

则最终的结果是2，因为一次正常调用会将前面的结果都清空，最后执行的两次，所以结果是2

## 2. setState为什么不会同步更新组件状态

[地址链接](https://zhuanlan.zhihu.com/p/25990883)

### 2.1 改成同步更新的阻碍
1. 因为`shouldComponentUpdate(nextProps, nextState) { /* this.state此时还不能更新 */ }`这个生命周期的存在，即使使用了`setState`它在这个生命周期之前仍然不能更新，所以只要这个生命周期还存在，就不能立即更新；
  `componentWillUpdate`这个生命周期有同样的问题，最早的更新时间是在`componentWillUpdate`和`render`之间；
2. 如果`setState`这个api设计只更新`state`不触发更新操作， 更新操作交由其他方法来执行，这样要`setState`就没有意义了；
```js
this.state.count = this.state.count + 1;
this.state.count = this.state.count + 1;
this.state.count = this.state.count + 1;
this.setState();
```
> 如果用这种方法来写code，那么React也就不够React了，算不上Reactive。
> Reactive Programming通俗说就是这样的编程风格：改变一个东西，另一个东西会做出响应发生改变，而不用我们的Code去主动让另一个东西做出改变。
> 还记得关于React的那个公式吗？`UI = f(state)` ，我们的代码就是那个f，和Excel表格中的公式一个性质。在React中，当我们通过setState改变了组件状态，那组件的UI就会自动发生变化，这就是Reactive Programming的体现。


## 3. setState何时同步更新状态

[地址链接](https://zhuanlan.zhihu.com/p/26069727)

以上的代码成立的前提条件是
- react的生命周期中执行
- react事件中执行

不成立的情况
- addEventListener绑定的事件
- setTimout/setInterval

原因是react的更新策略中的一个布尔值`isBatchingUpdates`，显然，在成立的情况下是`true`其他情况是`false`
至于为何如此，还需要从源码中挖掘

TODO:
