# Vue

## 组件

父组件 异步更改传递给子组件的data 触发子组件更新 使用.sync修饰符

**属性事件传递**

高阶组件，属性向下传递。如果属性较多，需要一个个去传递，不友好且费时。可使用`v-bind` 和 `v-on`实现类似`{...this.props}`的一次性传递。

```js
<!-- 高阶组件 SortList  -->
<template>
  <BaseList v-bind="$props" v-on="$listeners"></BaseList>
</template>
```

### 封装可以使用api调用的组件

类似: `this.$message()`

1. 写一个组件
2. 写一个js 使用`Vue.extend`继承上述组件，实现调用的api，注意组件的出现 消失 和 多次调用时的定位。暴露出api。在合适处销毁
3. 在调用处 引入js 使用api调用


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

- - - - -

## 函数式组件

函数式组件，即无状态，无法实例化，内部没有任何生命周期处理方法，非常轻量，因而渲染性能高，特别适合用来只依赖外部数据传递而变化的组件。

写法:

1. 在`template`标签里注明`functional`
2. 只接受`props`值
3. 不需要`script`标签

```js
<!-- List.vue 函数式组件 -->
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

阻止页面滑动 `@touchmove.prevent`

阻止事件冒泡 `.stop`修饰符

## elementUI

elementUI事件传参 不覆盖第一个参数 可以用`$event`

由于 JavaScript 的限制，Vue 不能检测以下变动的数组：
当你利用索引直接设置一个项时，例如：vm.items[indexOfItem] = newValue
当你修改数组的长度时，例如：vm.items.length = newLength
用this.$set

## 模板语法

 渲染html `v-html` 或用 `{{{` 包裹
