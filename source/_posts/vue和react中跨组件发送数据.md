---
title: vue和react中跨组件发送数据
date: 2018-04-24 19:20:09
tags: vue react 跨组件
categories: web
---

vue和react都可以通过`props`来向子组件传递数据，或者回调函数，也都可以通过回调函数来拿到子组件的数据。
如果层次过多，就可以使用下面的方法。
因为只是演示，数据只在父子两层组件中传递，而不写过深的组件。
而且是不带`vuex`和`redux`两个状态管理库。

## 1 子 -> 父
### 1.1 react -> 发布-订阅
或者组件没有嵌套关系的也同样适用。
使用node的`events`模块，使用影响全局机制去做。发布订阅事件。

单独写一个模块：
``` js
import {EventEmitter} from 'events'
let emitter = new EventEmitter();
export default emitter;
```
在子组件中发布事件
``` js
import emitter from './events'
handleClick = () => {
    emitter.emit('something', {msg: '发布事件'})
}
```

在父组件中
``` js
import emitter from './events'

componentDidMount() {
    this.xx = emitter.addListener('something', payload => {
        console.log(payload)
    })
}

componentWillUnmount() {
    emitter.removeListener(this.xx)
}
```
### 1.2 vue -> 发布-订阅

vue可以同样使用这个机制来发送数据

vue除了可以使用`node`模块外，还自带了一个事件订阅-发布机制

单独写一个模块
``` js
import Vue from 'vue'
export default new Vue()
```

子组件中
``` js
import Hub from './events'

handleClick() {
    Hub.$emit('something', {msg: '发布事件'})
}
```
在父组件中
``` js
created() {
    Hub.$on('something', payload => {
        console.log(payload)
    })
}

beforeDestory() {
    Hub.$off('something');
}
```
## 2 父 -> 子

### 2.1 react -> context

SuperType
``` js
class Super extends Component {
    static childContextTypes = {
        name: PropTypes.string
    }

    getChildContext() {
        return {
            name: 'changzhn'
        }
    }
}
```

SubType
``` js
class SubType extends Component {
    static contextTypes = {
        name: PropTypes.string
    }

    render() {
        return (
            <div>{this.context.name}</div>
        )
    }
}
```

建议使用 `high-order componet`高阶组件

## 2.2 vue $dispatch $broadcast
> $dispatch 和 $broadcast 已经被弃用。请使用更多简明清晰的组件间通信和更好的状态管理方案，如：Vuex。

官方已经废弃了。建议使用`vuex`或者`Hub`形式。
