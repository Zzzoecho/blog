# es6

## ECMAScript

ECMAScript -- 规范, 规格
JavaScript -- 实现

## let 和 const

ES6 引入了块级作用域, let 和 const 不存在变量提升
在声明前, 变量都是不可用的 (ReferenceError), 称为 暂时性死区
for 循环内部会形成单独的子作用域
不允许重复声明
块级作用域内部优先使用函数表达式 `let f = function(){}`

ES5 只有全局作用域和函数作用域, 缺点

1. 内层变量可能会覆盖外层变量,
2. 变量泄露

const 声明一个只读的常量, 一旦声明必须立即赋值

const 保证的是变量指向的地址不可更改, 对于对象来说, 不可把对象指向另一个地址, 但可以为其添加属性和方法

如果想保证对象内容不可更改, 可以用`Object.freeze`

### 顶层对象

> 浏览器环境: window 对象, node 环境: global 对象

ES5 中, 顶层对象的属性和全局变量是等价的. JavaScript 最大的设计败笔之一.

1. 没法在编译时就报出变量未声明的错, 因为全局变量可能是顶层对象的属性创造的, 而属性的创造是动态的
2. 程序员很可能不知不觉就创建了全局变量
3. 顶层对象的属性到处可以读写, 不利于模块化编程
4. window 对象有实体含义

ES6 为了兼容性, var 和 function 声明的全局变量依旧是顶层对象的属性, 而 let, const, class 声明的全局变量则不属于

```js
window.a = 1
a // 1

var a = 2
window.a // 2

let b = 1
window.b // undefined
```

#### 获取顶层对象

为了能在各种环境都能取到顶层对象, 一般是使用 this 变量

- 全局环境中, this 会返回顶层对象. 但 Node 模块和 ES6 模块中, this 返回当前模块
- 函数里的 this, 作为对象的方法返回当前对象, 作为单纯函数运行, 指向顶层对象, 但严格模式下会返回 undefined

globalThis 提案, 任何环境都可以从它拿到顶层对象. global-this 库模拟了这个提案

## 变量的解构赋值

```js
let [a, b, c] = [1, 2, 3]

let { foo, obj } = { foo: 1, obj: 2 }
```

## 字符串的扩展

字符串可以被 for...of 遍历

### 标签模板

> 函数([模板字符串中没有变量替换的部分], ...模板字符串各个变量被替换后的值)

```js
let a = 5
let b = 10

tag`Hello ${a + b} world ${a * b}`
// 等同于
tag(['Hello ', ' world ', ''], 15, 50)
```

应用场景:

1. 过滤 HTML 字符串, 防止恶意内容
2. 多语言转换

## 数值的扩展

- Number.isFinite()
- Number.isNaN()
- Number.parseInt()
- Number.parseFloat()
- Number.isInteger() 判断是否为整数
- Number.isSafeInteger() 判断是否在安全范围内
- Number.MAX_SAFE_INTEGER & Number.MIN_SAFE_INTEGER
- Number.trunc() 去除小数部分, 返回整数
- Number.sign() 判断一个数是正数(+1), 负数(-1)还是 0(0), -0(-0), 其他值(NaN)
- Number.cbrt() 立方根

## Class

语法糖，简化`prototype`的写法。类中的所有方法都定义在`prototye`属性上

```js
class Point {
  //类的默认方法，默认返回实例对象(this)
  constructor(x, y) {
    this.x = x
    this.y = y
  }

  toString() {
    return '(' + this.x + ', ' + this.y + ')'
  }
}
```

以上写法等于

```js
Point.protype = {
  constructor() {},
  toString() {}
}
```

类和模块内部默认就是严格模式
不存在变量提升
name 属性总是返回紧跟在 class 关键字后面的类名
类的内部所有定义的方法都不可枚举
实例的属性除非显式定义在其本身, 否则都是定义在原型上
类的所有实例共享一个原型对象

```js
var p1 = new Point(2, 3)
var p2 = new Point(3, 2)

p1.__proto__ === p2.__proto__
```

类的属性名可以采用表达式

```js
let methodName = 'getArea'

class Square {
  constructor(length) {
    // ...
  }

  [methodName]() {
    // ...
  }
}
```

`__proto___`是各大厂商具体实现时添加的私有属性, 并不是语言本身的特性. 所以虽然大多数浏览器的 JS 引擎中都提供了这个私有属性, 但依旧不建议在生产环境中使用.
使用`__proto__`改写原型要相当谨慎, 因为会改变类的原始定义, 影响到所有实例
可以使用`Object.getPrototypeOf`来获取实例对象的原型, 之后为原型添加方法/属性

### constructor

默认返回 this (实例对象)

constructor 方法可以不写, 默认会添加一个空的 constructor

prototype 对象的 constructor 属性，直接指向“类”的本身

### super

### getter 和 setter

可以使用 get 和 set 关键字, 设置某个属性的存值函数和取值函数, 拦截该属性的存取行为

```js
class MyClass {
  constructor() {}
  get prop() {
    return 'getter'
  }
  set prop(value) {
    console.log('setter: ' + value)
  }
}

let inst = new MyClass()

inst.prop = 123 // setter: 123

inst.prop // 'getter'
```

### 静态方法

类中定义的方法, 默认都会被实例继承
在一个方法前, 加上 static 关键字, 就表示该方法不会被实例继承
静态方法中的 this 指向类, 而不是实例
父类的静态方法可以被子类继承 extends

### 实例属性与静态属性

> 实例属性: 定义在实例对象上的属性
> 静态属性: Class 本身的属性. class 内部只有静态方法,没有静态属性

```js
class IncreasingCounter {
  // 2.直接写在类的最顶层
  _count = 0
  // 1. 定义在 constructor 里的 this 上
  constructor() {
    this._count = 0
  }

  // b. 提案的新写法
  static prop = 1

  get value() {
    console.log('Getting the current value!')
    return this._count
  }
  increment() {
    this._count++
  }
}

// a. 定义静态属性的唯一办法
IncreasingCounter.prop = 1
```

### 私有方法和私有属性

只能在类的内部访问的方法和属性, 外部不能访问. ES6 不提供

```js
// 1. 在命名上加以区分, 但没有限制外部不可调用
class Widget {
  // 公有方法
  foo(baz) {
    this._bar(baz)
  }

  // 私有方法
  _bar(baz) {
    return (this.snaf = baz)
  }
}

// 2. 将私有方法移除模块
class Widget {
  foo(baz) {
    bar.call(this, baz)
  }
}

function bar(baz) {
  return (this.snaf = baz)
}

// 3. 利用 Symbol 的唯一性, 将私有方法命名为一个 Symbol 值
const bar = Symbol('bar')
const snaf = Symbol('snaf')

export default class myClass {
  // 公有方法
  foo(baz) {
    this[bar](baz)
  }

  // 私有方法
  [bar](baz) {
    return (this[snaf] = baz)
  }
}
```

### new

```js
function newFunc(father, ...rest) {
  var result = {};
  result.__proto__ = father.prototype;
  var result2 = father.apply(result, rest);
  if (
    (typeof result2 === 'object' || typeof result2 === 'function') &&
    result2 !== null
  ) {
    return result2;
  }
  return result;
}
```

## Set 和 Map 数据结构

### Set 集合

> 类似数组, 但成员的值唯一, 不允许重复. Set 本身是一个构造函数可以用来生成 Set 数据结构

可以接受一个数组作为参数, 用来初始化

向 set 加入值的时候不会发生类型转换, 5 和 "5" 是两个不同的值.
set 判断相等的逻辑类似`===`, 但在 set 内部认为 `NaN`是相等的.

Tips:

数组去重 `[...new Set(array)]`
字符串去重 `[...new Set('ababbac')].join('')`
Array.from 可以将 Set 转为数组, 所以数组去重也可以是 `Array.from(new Set(array))`

#### 属性和方法

属性:

- constructor: 构造函数
- size: 返回成员总数

操作方法:

- add(): 添加某个值, 返回 Set
- delete(): 删除某个值, 返回 Boolean, 是否删除成功
- has(): 返回 Boolean, 判断该值是否为 Set 的成员
- clear(): 清除所有成员, 无返回值

遍历方法:

> Set 的遍历顺序就是插入顺序, 能保证**顺序**
> 由于 Set 没有键名只有键值(或者说键名和键值是同一个值), 所以 keys 和 values 方法行为一致

- keys(): 返回键名遍历
- values(): 返回键值遍历
- entries(): 返回键值对的遍历
- forEach(): 使用回调函数遍历每个成员

应用:

1. 去重
2. 实现并集, 交集, 差集

```js
let set1 = new Set([1, 2, 3])
let set2 = new Set([4, 3, 2])

let intersect = new Set([...set1].filter(value => set2.has(value)))
let union = new Set([...set1, ...set2])
let difference = new Set([...set1].filter(value => !set2.has(value)))

console.log(intersect) // Set {2, 3}
console.log(union) // Set {1, 2, 3, 4}
console.log(difference) // Set {1}
```

### WeakSet

> 类似 Set, 也是不重复的值集合.但 WeakSet 的成员只能是对象

弱引用, 垃圾回收机制不考虑 WeakSet 对该对象的引用. 如果其他对象都没有引用该对象那么就会自动回收该对象占用的内存.

所以, WeakSet 的成员不适合引用, 也不可遍历. 成员个数不定, 取决于垃圾回收机制何时运行.

方法:

- add(): 添加某个值, 返回 WeakSet
- delete(): 删除某个值, 返回 Boolean, 是否删除成功
- has(): 返回 Boolean, 判断该值是否在 WeakSet 实例之中

### Map 字典

> Object, 键值对集合, 但只能用字符串当做键.
>
> Map 类似对象, 但键可以用各种类型.

Map 构造函数接受数组作为参数, 指定了键和值

- 对同一个键多次赋值，后面的值将覆盖前面的值。
- 读取一个未知的键，则返回 undefined。
- 键和内存地址绑定, 如果键是值类型则判断是否严格相等, 只要严格相等, Map将其视为一个键
- 虽然NaN不严格相等于自身, 但Map还是将其视为同一个键

#### 属性和方法

- size
- set()
- get()
- has()
- delete()
- clear()

遍历

- keys()
- values()
- entries()
- forEach()

### WeakMap

> 只接受对象作为键名(null 除外), 不可遍历, 没有 size 属性

应用场景:

以 DOM 节点作为键名, 一旦 DOM 节点删除, WeakMap 中不存在内存泄露风险

总结:

- Set和Map内部判断严格相等的逻辑, 都会判断 NaN === NaN
- WeakSet: 只接受对象做成员; 成员弱引用. WeakMap: 只接受对象作键名; 键名弱引用.
- WeakSet 和 WeakMap 都不可遍历, 无size属性, 大小不定

---

## 装饰器

> 装饰器: 对类进行处理的函数, 第一个参数是要装饰的目标类.如果一个参数不够用, 可以在装饰器外层再封装一层函数

### 方法的装饰

参数: 3 个

- target: 目标类的原型, 本来是想修改类的实例的, 但那时候实例还没生成.
- name: 要装饰的属性名
- descriptor: 该属性的描述对象

类似于`Object.defineProperty(obj, prop, descriptor)`
