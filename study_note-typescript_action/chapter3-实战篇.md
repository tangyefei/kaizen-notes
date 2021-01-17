# 实战篇

## 01 两种方式搭建项目

作者基于自己设想的简单需求，通过两种方式搭建了项目。

### 第一种 手动配置

直接使用TypeScript，以`.ts`文件做后缀即可，但是如果希望在`ts`中使用`jsx `，我们要把文件后缀改为`tsx`。

首先,要在配置文件中对`.tsx`格式的resolve，因为引用到其他的类型所以一旦写了extensions `.js`也要写上

```
// config/webpack.base.js

module.exports = {
  resolve: {
    extensions: ['.tsx', '.ts', '.js']
  }
}  
```


然后，需要在配置文件中增加对`jsx `的编译方式，如下是能运行的最小配置：

```
// tsconfig.js
{
  "compilerOptions": {
    "esModuleInterop": true,
    "jsx": "react"
  },
}
```
注，如果加入`"noEmit":true`会导致编译失败

最后，实际引用`jsx`后缀的组件的时候，直接引入即可：

```
// src/index.jsx

import Hello from './components/Hello';
```
## 通过RCA创建


第二种，通过react-create-app创建项目，然后作者引入了antd，另外引入了一些开发工具。

下述为创建命令：

```
npx create-react-app hello-tsx --typescript
```

## 02 函数组件和类组件

作者尝试在React的函数组件和类组件中引入TypeScript的类型。

包括给props和state指定类型，顺带帮忙复习了下React的知识点（state 和 defaultProps的用法）：

```
import React, { Component } from 'react';

interface Greeting {
  firstName: string,
  lastName: string,
  name: string
}

interface State {
  count: 0
}

class HelloComponent extends Component<Greeting, State> {
  state: State = {
    count: 0
  }
  static defaultProps = {
    firstName: '',
    lastName: '',
  }

  render () {
    return <div>Hello {this.props.name}</div>
  }
}

export default HelloComponent;
```

## 03 高阶组件和hooks

![书写组件方式](https://notes-1258030304.cos.ap-shanghai.myqcloud.com/books/typescript-action/react-component-in-4-ways.jpeg)

### 高阶组件

例子使用泛型来应用了高阶组件：

```
import React, { Component } from 'react';
import Hello from './Hello';

interface Loading  {
  loading: boolean
}

function HelloHOC<P>(WrappedComponent: React.ComponentType<P>) {
  return class extends Component<P & Loading> {
    render () {
      let { loading, ...props } = this.props;
      return loading ? <div>Loading ...</div>: <WrappedComponent {...(props as P)}></WrappedComponent>
    }
  }
}

export default HelloHOC(Hello);
```

### 高阶组件

例子使用高阶组件达成同上例的类似效果

```
import React, { useState, useEffect } from 'react';

interface Greeting {
  firstName: string,
  lastName: string,
  name: string,
}
const HelloHooks = (props: Greeting) => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState<string | null>(null);
  useEffect(() => {
    if(count > 5) setText('休息一下')
  })
  return (
    <div>
      <p>你点击了{count}次，{text}</p>
      <button onClick={() => {setCount(count + 1)}}>hello, {props.name}</button>
    </div>
  )
}
HelloHooks.defaultProps = {
  firstName: '',
  lastName: '',
}

export default HelloHooks;
```

## 04 事件处理和数据请求