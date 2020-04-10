# React

## API

`React.createElement(type, [props], [...children])`

type -- 标签名, React 组件
children

## propTypes

`npm install --save prop-types`
production 不会使用 prop-types

```js
Icon.propTypes = {
  name: PropTypes.string.isRequired,
  size: PropTypes.number,
}

Icon.defaultProps = {
  size: 30,
}
```

原理

```js
SayHello.proptypes = {
  firstName: (props, propName, componentName) {
    if (typeof props[propName] !== 'string) {
      return new Error(`hey, you should pass a string for ${propName} in ${componentName} buy you passed a ${typeof props[propName]}`)
    }
  }
}

// 提取出类型检测
const PorpTypes = {
  string(props, propName, componentName) {
    if (typeof props[propName] !== 'string) {
      return new Error(`hey, you should pass a string for ${propName} in ${componentName} buy you passed a ${typeof props[propName]}`)
    }
  },
}

SayHello.propTypes = {
  firstName: PropTypes.string,
  lastName: PropTypes.string,
}

// 使用 prop-types 一般放在类中的静态方法
class SayHello extends React.Component {
  static propTypes = {
    firstName: PropTypes.string.isRequired,
    lastName: PropTypes.string.isRequired,
  }

  render() {

  }
}

```

```js
//指定枚举类型：你可以把属性限制在某些特定值之内
optionalEnum: PropTypes.oneOf(['News', 'Photos']),

// 指定多个类型：你也可以把属性类型限制在某些指定的类型范围内
optionalUnion: PropTypes.oneOfType([
  PropTypes.string,
  PropTypes.number,
  PropTypes.instanceOf(Message)
]),

// 指定某个类型的数组
optionalArrayOf: PropTypes.arrayOf(PropTypes.number),

// 指定类型为对象，且对象属性值是特定的类型
optionalObjectOf: PropTypes.objectOf(PropTypes.number),

```

## 生命周期

挂载
constructor
getDerivedStateFromProps
render
componentDidMount

更新
getDerivedStateFromProps
showComponentUpdate
render
getSnapshotBeforeUpdate
componentDidUpdate

卸载
componentWillUnmount

错误处理
getDeviredStateFromError
componentDidCatch

## 虚拟 DOM

广度优先算法 (复杂度 O(n) 线性，性能好)
Diff 算法: 从根节点开始分层比较。
两个假设:

1. 组件的 DOM 结构相对稳定
2. 类型相同的兄弟节点可以被唯一标识 (用于兄弟节点位置顺序发生变化，即循环生成的相同类型节点需要指定唯一的 key，可提高性能)

## 高阶组件

接受组件做为参数，返回新的组件。实现通用逻辑，本身并没有 ui 展现。

```js
/// 高阶组件 实现了定时器的逻辑
export default function withTimer(WrappedComponent) {
  return class extends React.Component {
    state = { time: new Date() };
    componentDidMount() {
      this.timerID = setInterval(() => this.tick(), 1000);
    }

    componentWillUnmount() {
      clearInterval(this.timerID);
    }

    tick() {
      this.setState({
        time: new Date()
      });
    }
    render() {
      // 返回传入的组件 并在props上注入time属性。
      return <WrappedComponent time={this.state.time} {...this.props} />;
    }
  };
}

// chatApp.js
// 调用
withTimer(chatApp)

render() {
    {this.props.time}
}
```

函数作为子组件。像 vue 中的插槽，使用者来决定渲染出什么。

```js
// TabSelect.js
<TabSelect>{name => <span>{name}</span>}</TabSelect>
```

## Context Api

创建一个上下文环境，在任意节点都可以访问。不需要通过 props 一层层传递。
Context 的根节点 -- Provider ；子节点 -- Customer

```js
// React.createContext 创建上下文 参数任意。
const LocaleContext = React.createContext(enStrings)

class LocaleProvider extends React.Component {
  render() {
    return (
      // 根节点 必须用创建的上下文 Provider 包裹起来
      <LocaleContext.Provider value={this.state.locale}>
        <button onClick={this.toggleLocale}>切换语言</button>
        {this.props.children}
      </LocaleContext.Provider>
    )
  }
}

class LocaledButtons extends React.Component {
  render() {
    return (
      // 子节点 必须用创建的上下文 Consumer 包裹起来
      <LocaleContext.Consumer>
        {locale => (
          <div>
            <button>{locale.cancel}</button>
            &nbsp;<button>{locale.submit}</button>
          </div>
        )}
      </LocaleContext.Consumer>
    )
  }
}
```

## Redux

UI用户操作 -> 形成Actions -> Dispatch分发 -> Reducer处理数据 -> 更新State 单向数据流

```js
// action
{
  type: 'ADD_COUNT',
  text: 'add count', // action描述
  payload: {}, // 额外参数
}

// reducer
(state, action) => {
  switch (action.type) {
    case '':
      return {}
    default:
      break;
  }
}

// store
createStore()
bindActionCreators(actionCreators, dispatch) // 封装action为拥有同名key的对象 同时用dispatch包装 以便直接调用
combineReducers({
  reducer1, reducer2
}) // 合并多个reducers

```

### connect的工作原理 - 高阶组件

```js
function mapStateToProps(state) {
  // 为了性能 能访问的state必须限制在最小范围
  return {}
}

function mapDispatchToProps(dispatch) {
  return bindActionCreators()
}

const Connect = connect(mapStateToProps, mapDispatchToProps)(component)
```

### 异步action

多个同步action的组合使用。返回promise，中间件截获等待promise返回值。

```js
FETCH_LIST_BEGIN
FETCH_LIST_SUCCESS
FETCH_LIST_FAILURE
```

### 如何组织Action和Reducer

> 标准形式：所有Action放一个文件，Action和Reducer分开

问题：

1. 所有Action在一份文件，会无限扩展
2. Actiion，Reducer分开，实现业务逻辑时需要来回切换
3. 不能直观显示系统中有哪些Actions

> 新形式：每个文件一个Action，文件名就是action名
每个文件里同时包含Action 和 Reducer

### 不可变数据

Redux中的store每个节点都是不可变数据，只要判断引用是否相等就可以判断是否更新

1. 性能优化 判断引用
2. 易于调试和跟踪
3. 易于推测

操作不可变数据

1. 原生，扩展运算符，Object.assign
2. immutability-helper
3. immer

## React Route

1. 声明式
2. 动态路由

三种方式

1. URL路径(BrowerRoute)
2. hash路由(HashRoute) 低版本浏览器不支持路由变更不刷新页面
3. 内存路由(MemoryRoute)

基于路由配置进行资源组织

1. 实现业务逻辑的松耦合
2. 易于扩展，重构和维护
3. 路由可实现Lazy Load

### API

```html
import { Link } from 'react-router-dom'
import { Prompt, Redirect, Switch } from 'react-router'

<!-- 普通链接 不会触发浏览器刷新 -->
<Link to="/about">About</Link>

<!-- 类似Link 但是当浏览器地址匹配 NavLink 中的地址时 会添加当前选中状态 -->
<NavLink to="/about" activeClassName="selected">About</NavLink>

<!-- 满足条件时提示用户是否离开当前页 -->
<Prompt when={formIsHalfFilledOut} message="确定离开？"/>

<!-- 重定向当前页 常用于登录判断 -->
<Redirect to="/dashboard"/>

<!-- 路径匹配时显示对应组件 -->
<Route exact path="/" component={Home} /> <!-- exact 是否精确匹配 有该属性则要求完全匹配 -->
<Route path="/news" component={NewsFeed} />

<!-- 同时匹配多个Route 可同时显示多个组件 -->
<!-- 只显示第一个匹配的路由 -->
<Switch>
   <Route exact path="/" component={Home} />
</Switch>
```

### 传递参数

> path-to-regexp: 将字符串路径转换为正则表达式

页面状态尽量通过URl参数传递： 用于链接分享后 可直接定位状态

### 使用 history 控制路由跳转

在使用 react-router v3 的时候,跳转路由一般:

1. 从 react-router 导出 browserHistory
2. 使用 browserHistory.push()等方法操作路由跳转

```js
import browserHistory from 'react-router';

export function addProduct(props) {
  return dispatch =>
    axios.post(`xxx`, props, config)
      .then(response => {
        browserHistory.push('/cart'); //这里
      });
}
```

但是!在 react-router v4 中, 不再提供好 browserHistory 等的导出

解决方法:

#### 1. 使用 withRouter

withRouter高阶组件，提供了history让你使用~

官方推荐做法, 但是这种方法用起来有点难受，比如我们想在redux里面使用路由的时候，我们只能在组件把history传递过去。。

```js
import React from "react";
import {withRouter} from "react-router-dom";

class MyComponent extends React.Component {
  ...
  myFunction() {
    this.props.history.push("/some/Path");
  }
  ...
}
export default withRouter(MyComponent);
```

#### 2. 使用 Context

react-router v4 在 Router 组件中通过 Context 暴露了一个router对象~

在子组件中使用Context，我们可以获得router对象，如下面例子~

```js
import React from "react";
import PropTypes from "prop-types";

class MyComponent extends React.Component {
  static contextTypes = {
    router: PropTypes.object
  }
  constructor(props, context) {
     super(props, context);
  }
  ...
  myFunction() {
    this.context.router.history.push("/some/Path");
  }
  ...
}
```

当然，这种方法慎用~尽量不用。因为react不推荐使用context哦。在未来版本中有可能被抛弃哦。

#### 3. hack

v3中把我们传递给Router组件的history又暴露出来，让我们调用了, 而 v4 的BrowserRouter自己创建了 history, 并且不暴露出来.可以依旧 hi 使用 Router 组件, 自己创建 history

```js
// 1. 自己创建一个 history.js
// src/history.js
import createHistory from 'history/createBrowserHistory';
export default createHistory();

// 2. 使用 Router 组件
import { Router, Link, Route } from 'react-router-dom';
import history from './history';

ReactDOM.render(
  <Provider store={store}>
    <Router history={history}>
      ...
    </Router>
  </Provider>,
  document.getElementById('root'),
);

// 3. 其他地方使用
import history from './history';

export function addProduct(props) {
  return dispatch =>
    axios.post(`xxx`, props, config)
      .then(response => {
        history.push('/cart'); //这里
      });
}
```

#### 4. 自己实现一个 BrowserRouter

---

参考 repo class 普通 hook hook 三种写法
https://github.com/Lucifier129/react-hooks-demo

https://usehooks.com/

https://github.com/rematch/rematch
https://github.com/Astrocoders/epitath

---

## this.props.children

```js
// render
<CustomComponent>
    <p>121212</p>
</CustomComponent>

// CustomComponent.js
return (
    {this.props.children} // 返回JSX <p>121212</p>
)

```

## 脚手架

Create-React-App 适合构建练手项目
Rekit 自带全家桶和Scss/ less
在线： [CodeSandbox](http://codesandbox.io)

- React全家桶 React | React Route | Redux
- Webpack打包
- babel 兼容
- eslint 代码规范


create-react-app 不暴露配置文件，想要自定义修改

1. reject 出一份配置文件
2. 使用 `react-app-rewired`，修改 npm script。新建`config-overrides.js`覆盖原有配置。

react-router

- BrowserRouter 常用
- hashRouter 带`#`号的路由，常用于静态页面

## 全家桶

react-dom

- react-router React Router 核心
- react-router-dom 用于 DOM 绑定的 React Router
- react-router-native 用于 React Native 的 React Router
- react-router-redux React Router 和 Redux 的集成
- react-router-config 静态路由配置的小助手

babel

- @babel/core-babel核心模块
- @babel/preset-env 编译ES6等
- @babel/preset-react 转换JSX
- @babel/plugin-transform-runtime: 避免 polyfill 污染全局变量，减小打包体积
- @babel/polyfill: ES6 内置方法和函数转化垫片

## 单元测试

1. Jest: Facebook 开源的 JS 单元测试框架
2. JS DOM: 浏览器环境的 NodeJS 模拟
3. Enzyme: React 组件渲染和测试 (airbnb)
4. nock: 模拟 HTTP 请求
5. sinon: 函数模拟和调用跟踪
6. istanbul: 单元测试覆盖率

## 前端应用

可维护, 可扩展, 可测试, 易开发, 易构建

易于测试: 功能分层是否清晰, 副作用少, 尽量使用纯函数

### 项目拆分

1. 按领域模型(feature)组织代码, 降低耦合度
2.

## 优秀文章

[React ES5、ES6+ 常见用法对照表](https://github.com/carlleton/reactjs101/tree/zh-CN/Appendix01)

## 项目搭建

脚手架

- npx create-react-app my-app

react-router

- react-router-dom

代码规范

- husky: commit 时校验, 未通过不可提交

```js
"script": {
  "precommit": "lint-staged" // commit 前执行 'lint-staged'
}
```

- lint-staged: 只校验改动的文件

```js
"lint-staged": {
    "src/**/*.{js,jsx}": [ // 校验文件后缀
      "eslint --fix", // 代码规范 --fix 自动修复 (规范上前面带小扳手的就是可以自动fix的)
      "prettier --write", // 代码格式化 --write 自动改写文件
      "git add"
    ]
},
```

- eslint
- prettier



