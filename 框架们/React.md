# React

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

### 如何组织Acton和Reducer

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

### Route


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
