# new

1. 熟悉快捷键和键盘操作
2. 写代码前先想清楚逻辑
3. 多写注释, 注意命名
4. 调试技巧

## QA

ES6 class

基础类型

浏览器的事件循环: 执行一个宏任务，然后执行清空微任务列表，循环再执行宏任务，再清微任务列表

- 微任务 microtask(jobs): promise / ajax / Object.observe(该方法已废弃)
- 宏任务 macrotask(task): setTimout / script / IO / UI Rendering


vue 生命周期: beforeCreate -> created -> beforeMount -> mounted -> beforeUpdate -> updated -> beforeDestroy -> destroyed

created 和 mounted 的区别

vue 响应式原理:

vuex 的流程: state -> actions -> mutations

vue 和 react 的异同

相同点：

1. 数据驱动页面，提供响应式的试图组件
2. 都有virtual DOM,组件化的开发，通过props参数进行父子之间组件传递数据，都实现了webComponents规范
3. 数据流动单向，都支持服务器的渲染SSR
4. 都有支持native的方法，react有React native， vue有wexx

不同点：

1. 数据绑定：Vue实现了双向的数据绑定，react数据流动是单向的
2. 数据渲染：大规模的数据渲染，react更快
3. 使用场景：React配合Redux架构适合大规模多人协作复杂项目，Vue适合小快的项目
4. 开发风格：react推荐做法jsx + inline style把html和css都写在js了

手动修改 this 的指向
call, apply, bind 的不同

call: fn.call(target, 1, 2)
apply: fn.apply(target, [1, 2])
bind: fn.bind(target)(1,2)
