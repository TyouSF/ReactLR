# ReactLR

Learning record for React

# 关于 ReactJS

React.js 是一个帮助你构建页面 UI 的库。如果你熟悉 MVC 概念的话，那么 React 的组件就相当于 MVC 里面的 View。如果你不熟悉也没关系，你可以简单地理解为，React.js 将帮助我们将界面分成了各个独立的小块，每一个块就是组件，这些组件之间可以组合、嵌套，就成了我们的页面。

一个组件的显示形态和行为有可能是由某些数据决定的。而数据是可能发生改变的，这时候组件的显示形态就会发生相应的改变。而 React.js 也提供了一种非常高效的方式帮助我们做到了数据和组件显示形态之间的同步。

React.js 不是一个框架，它只是一个库。它只提供 UI （view）层面的解决方案。在实际的项目当中，它并不能解决我们所有的问题，需要结合其它的库，例如 Redux、React-router 等来协助提供完整的解决方法。

# 环境准备

更多信息，可参阅官网：[React 环境准备](https://zh-hans.reactjs.org/tutorial/tutorial.html#setup-for-the-tutorial)

# 评论示例练习

项目整体存放文件夹在：

> `./reactlr`

# 第一阶段示例

切换至 tag：`step1`

进入项目文件夹根目录下，使用：`npm start` 即可启动项目查看

## 第一阶段知识点梳理

### JSX 语法

> 在 JavaScript 写的标签的”语法叫 JSX

JSX 原理：

用 JavaScript 对象来描述所有能用 HTML 表示的 UI 信息
编译的过程会把类似 HTML 的 JSX 结构转换成 JavaScript 的对象结构

**所谓的 JSX 其实就是 JavaScript 对象**

### 组件的 render 方法

`render` 方法必须要返回一个 JSX 元素

但这里要注意的是，必须要用一个外层的 JSX 元素把所有内容包裹起来。返回并列多个 JSX 元素是不合法的，下面是错误的做法：

```jsx
...
render () {
  return (
    <div>第一个</div>
    <div>第二个</div>
  )
}
...
```

# 第二阶段示例

切换至 tag：`step2`

## 第二阶段知识梳理

### 挂在阶段的组件生命周期

> React.js 将组件渲染，并且构造 DOM 元素然后塞入页面的过程称为组件的挂载

- `componentWillMount`：组件挂载开始之前，也就是在组件调用 render 方法之前调用
- `componentDidMount`：组件挂载完成以后，也就是 DOM 元素已经插入页面后调用
- `componentWillUnmount`：组件对应的 DOM 元素从页面中删除之前调用

我们一般会把组件的 `state` 的初始化工作放在 `constructor` 里面去做；
在 `componentWillMount` 进行组件的启动工作，例如 Ajax 数据拉取、定时器的启动；组件从页面上销毁的时候，有时候需要一些数据的清理，例如定时器的清理，就会放在 `componentWillUnmount` 里面去做。一般来说，有些组件的启动工作是依赖 DOM 的，例如动画的启动，而 `componentWillMount` 的时候组件还没挂载完成，所以没法进行这些启动工作，这时候就可以把这些操作放在 componentDidMount 当中

### 更新阶段的生命周期

就是 setState 导致 React.js 重新渲染组件并且把组件的变化应用到 DOM 元素上的过程，这是一个组件的变化过程。而 React.js 也提供了一系列的生命周期函数可以让我们在这个组件更新的过程执行一些操作

- `shouldComponentUpdate(nextProps, nextState)`：你可以通过这个方法控制组件是否重新渲染。如果返回 false 组件就不会重新渲染。这个生命周期在 React.js 性能优化上非常有用。
- `componentWillReceiveProps(nextProps)`：组件从父组件接收到新的 props 之前调用。
- `componentWillUpdate()`：组件开始重新渲染之前调用。
- `componentDidUpdate()`：组件重新渲染并且把更改变更到真实的 DOM 以后调用

### props.children 和容器类组件

使用自定义组件的时候，可以在其中嵌套 JSX 结构。嵌套的结构在组件内部都可以通过 props.children 获取到，这种组件编写方式在编写容器类型的组件当中非常有用。而在实际的 React.js 项目当中，我们几乎每天都需要用这种方式来编写组件

### style 属性

style 接受一个对象，这个对象里面是这个元素的 CSS 属性键值对，原来 CSS 属性中带 - 的元素都必须要去掉 - 换成驼峰命名，如 font-size 换成 fontSize，text-align 换成 textAlign

```jsx
<h1 style={{fontSize: '12px', color: this.state.color}}>React.js LR</h1>
```

### PropTypes 和组件参数验证

React 提供的第三方库 `prop-types`

通过 PropTypes 给组件的参数做类型限制，可以在帮助我们迅速定位错误，这在构建大型应用程序的时候特别有用；另外，给组件加上 propTypes，也让组件的开发、使用更加规范清晰

```jsx
import React, { Component } from 'react'
import PropTypes from 'prop-types'

class Comment extends Component {
  static propTypes = {
    comment: PropTypes.object.isRequired
  }

  render () {
    const { comment } = this.props
    return (
      <div className='comment'>
        <div className='comment-user'>
          <span>{comment.username} </span>：
        </div>
        <p>{comment.content}</p>
      </div>
    )
  }
}
```

### 组件编写的一些常规习惯

组件的内容编写顺序如下：

- static 开头的类属性，如 defaultProps、propTypes。
- 构造函数，constructor。
- getter/setter（还不了解的同学可以暂时忽略）。
- 组件生命周期。
- _ 开头的私有方法。
- 事件监听方法，handle*。
- render*开头的方法，有时候 render() 方法里面的内容会分开到不同函数里面进行，这些函数都以 render* 开头。
- render() 方法。

如果所有的组件都按这种顺序来编写，那么维护起来就会方便很多，多人协作的时候别人理解代码也会一目了然
