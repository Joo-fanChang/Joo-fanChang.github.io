---
title: react高阶组件
date: 2018-07-11 13:29:44
tags: js react hoc　高阶组件
categories: web
---
> 高阶组件(high order component) 的本质就是一个高阶函数，高阶函数是接收一个函数作为参数或者把一个函数作为返回值；同样高阶组件就是接收一个`react`组件，返回一个`react`组件。

## 哪些地方用到了高阶组件
- `react-redux`
- `antd -> Form.create`

-> antd
```js
import React, { Component } from 'react';
import { Form } from 'antd';

const FormItem = Form.Item;

class AddUser extends Component {
    render() {
        const { getFieldDecorator } = this.props.form;
        return(
            <Form className="modal-form">
                <FormItem
                  label="业务对象"
                  labelCol={{span: 6}}
                  wrapperCol={{span: 18}}>
                  {getFieldDecorator('bocode', {
                    rules: [{ required: true, message: '请输入业务对象名称' },
                      { whitespace: true, message: '请输入业务对象名称' }],
                    initialValue: this.props.boInfoName
                  })(
                    <Input placeholder="请输入业务对象名称" />
                  )}
                </FormItem>
            <Form/>
        )
    }
}

export default Form.create()(AddUser);
```
在使用`AddUser`组件的时候，并没有传递名为`form`的`props`,但是却可以使用`this.props.form.getFieldDecorator`方法。
这是因为在导出的时候并不是导出原来的`AddUser`组件，而是导出了一个包装后的高阶组件，在这个HOC中不仅将原来的`props`传递进去，还为这个`AddUser`组件添加了一些新的方法。

同样`react-redux`的`connect`方法也是一个高阶组件。


## 怎么判断组件是作为高阶组件导出的
-_-
1. 使用了不是你自己传的`props`
2. 组件不是直接导出，而是作为一个函数的参数
3. `decorator`装饰器


## 为什么要使用高阶组件
TODO:


## 自己封装一个简单的高阶组件

-> hello
```js
import React from 'react';
import testHOC from '../HOC/TestHOC';

const Hello = props => {

  return (
    <div>
      {
        `hello, ${props.name}`
      }
    </div>
  )
}

export default testHOC('changzhn')(Hello);


// -> hoc
import React, { Component } from 'react';

const TestHOC = words => {
  return WrappedComponent => {
    return class HOC extends Component {
      render() {
        return (
          <div>
            <WrappedComponent name={words} {...this.props}/>
          </div>
        )
      }
    }
  }
}

export default TestHOC;
```

->_->直接传props就行了

TODO:

自已封装一个`react-redux`

