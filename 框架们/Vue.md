# Vue

TODO:
编辑 数组直接赋值 ok
用下标修改数组 无效?
set 修改数组 无效?

## 路由

### 多层路由嵌套, 子路由不显示

每一层路由都需要`<router-view></router-view>`将组件传递下去

```js
// 下面三种写法都 ok
component: {
  render(c) {
    return c('router-view')
  }

  render (createElement) {
     return createElement('router-view')
  }

  template: `<router-view></router-view>`
}
```

### 传参方式: query, params+动态路由

1. query 通过 path 切换路由, params 通过 name 切换路由
2. query 通过`this.$route.query`接收参数, params 通过`this.$route.params`接收参数
3. 传参的展现方法: query url 后缀 `/detail?id=1&user=123&identity=1`; params 动态路由 `/detal/123`
4.

## 组件

父组件 异步更改传递给子组件的 data 触发子组件更新 使用.sync 修饰符

### props

`$listeners` 和 `$attr` 可以透传 props 和 事件
`<wrapped v-on="$listeners" v-bind="$attrs"/>`

等价于在 React 中透传 props：

`<WrappedComponent {...this.props}/>`

#### props 写法 默认值, 带验证功能

> 当 prop 验证失败的时候，(开发环境构建版本的) Vue 将会产生一个控制台的警告。
> 注意那些 prop 会在一个组件实例创建之前进行验证，所以实例的属性 (如 data、computed 等) 在 default 或 validator 函数中是不可用的。

```js
props: {
  // 带有默认值的对象
  title:{
      type: String,
       // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
  // 自定义验证函数
  author:{
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['au', 'th', 'or'].indexOf(value) !== -1
      }
    }
}
```

#### 属性事件传递

高阶组件，属性向下传递。如果属性较多，需要一个个去传递，不友好且费时。可使用`v-bind` 和 `v-on`实现类似`{...this.props}`的一次性传递。

```js
// 高阶组件 SortList
<template>
  <BaseList v-bind="$props" v-on="$listeners"></BaseList>
</template>
```

### 封装可以使用 api 调用的组件

类似: `this.$message()`

1. 写一个组件
2. 写一个 js 使用`Vue.extend`继承上述组件，实现调用的 api，注意组件的出现 消失 和 多次调用时的定位。暴露出 api。在合适处销毁
3. 在调用处 引入 js 使用 api 调用

```js
import Vue from 'vue'
import imageCover from './imageCover'
// 继承组件
const imageCoverConstructor = Vue.extend(imageCover)
// 出现api
function open(options) {
  // 创建新的实例
  const instance = new imageCoverConstructor({ data: options })
  instance.vm = instance.$mount() //$mount生成虚拟dom
  instance.vm.visible = true

  document.body.appendChild(instance.vm.$el) //插入
}
```

---

## 函数式组件

函数式组件，即无状态，无法实例化，内部没有任何生命周期处理方法，非常轻量，因而渲染性能高，特别适合用来只依赖外部数据传递而变化的组件。

写法:

1. 在`template`标签里注明`functional`
2. 只接受`props`值
3. 不需要`script`标签

```js
// List.vue 函数式组件
<template functional>
  <div>
    <p v-for="item in props.items" @click="props.itemClick(item);">
      {{ item }}
    </p>
  </div>
</template>
```

### 监听子组件的生命周期

常规写法: 在子组件的生命周期函数中`$emit`触发父组件监听事件
简单写法: 在父组件中使用`@hook`来监听。

```js
<Child @hook:mounted="doSomething"/>
```

## 模板语法

渲染 html `v-html` 或用 `{{{` 包裹

## watch 侦听器

用 watch 监听对象内部的变动

```js
watch: {
  obj: {
    handler: function(new, old) {
      // 操作
    },
    deep: true, // 监听对象内某一个属性变化
    immediate: true, // 当值第一次绑定的时候不会触发 watch 监听, 用 immediate 可以在最初绑定的时候执行
  }
}
```

## 数据响应失效

**单独**对数组的 index 和 length 赋值时, 不会响应

> 由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
> 当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
> 当你修改数组的长度时，例如：vm.items.length = newLength
> 用 this.\$set

在项目中很少会单独改变数组的 index , 通常会有其他对数组或其中对象的一些操作, 而这些操作会触发响应

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c', { money: 1 }]
  }
})
vm.items[1] = 'x' // 不是响应性的
vm.items.length = 2 // 不是响应性的
vm.items[3].money = 10 // 响应
```

> Vue 不能检测对象属性的添加或删除。 对于已经创建的实例，Vue 不能动态添加根级别的响应式属性。要使用`Vue.set(object, key, value)`

```js
var vm = new Vue({
  data: {
    a: 1
  }
})
vm.a = 3 // 响应
vm.b = 2 // 不响应
```

## 小技巧

可以用一个函数来代替循环中的 list, 以免每次修改数据

```js
<li v-for="n in evenNumbers">{{ n }}</li>
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

### 获取事件默认回调参数

arguments 获取作用域下可用变量 (ES6 废弃, 不推荐使用)

大多数情况 \$event = arguments[0]

当 v-on 的参数是 dom 事件时, \$event 代表的是原生的 event 事件.
原生 event 事件可以获取到, 事件类型, offset, 目标元素(target), stopPropagation, preventDefault

```js
// 下面几种方法都可以获取到默认的回调参数
<el-radio-group
  v-model="params.isPublish"
  @change="e => changeSearchStatus(e, 'msg')"
  @change="changeSearchStatus($event, 'msg')"
  @change="changeSearchStatus('msg', arguments)"
  @change="changeSearchStatus('msg', ...arguments)"
  size="medium"
>
</el-radio-group>
```

### 修饰符

- `.number` 自动将用户输入的值转为数值类型

```js
// type="number" 时, 也会返回 字符串
<input v-model.number="age" type="number">
```

- `.trim` 自动过滤用户输入的首尾空白字符

- `.lazy` 在change 时更新而非 input

- `@touchmove.prevent` 阻止页面滑动

- `.stop` 阻止事件冒泡

## elementUi

表单 resetFields , `el-form-item` 需要`props`字段

## vue-loader

> 处理`.vue`文件的 webpack loader.

vue 组件细则

语言块:
1. template: 最多一个, 内容会被提取为字符串, 将编译并用在 Vue 组件的 template 选项中
2. script: 最多一个, 必须导出对象, 也可以导出由 Vue.extend() 创建的扩展对象
3. style: 可以包含多个. scope 使得 css 只作用于当前组件中的元素. css 作用域, 用 postCss 来转换

```js
<template></template>

<script></script>

<style></style>
```

1. transformToRequire

图片无需先传给变量

```html
<template>
  <div>
    <avatar :default-src="DEFAULT_AVATAR"></avatar>
  </div>
</template>
<script>
  export default {
    created () {
      this.DEFAULT_AVATAR = require('./assets/default-avatar.png')
    }
  }
</script>
```

## 项目搭建

坑

全局引入 scss 文件报错. `Invalid options object. Sass Loader has been initialized using an options object that does not match the API schema.`

解决
```shell
npm uninstall --save-dev sass-loader
npm install --save-dev sass-loader@7.1.0
```
If you use vue cli 3, it works with sass-loader v7 not with v8 if you use vue cli 3 and sass-loader v7 the previous configuration still works.
